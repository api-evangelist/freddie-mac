---
name: freddie-mac-deliver-and-commit-a-loan
description: Price, commit, deliver and settle a loan sale to Freddie Mac through Loan Selling Advisor, using the quote-then-decide pattern on Cash and Guarantor Committing and the Loan Import system-to-system interface.
api: freddie-mac:cash-committing
operations:
  - price
  - price-decision
  - extension
  - extension-decision
  - pairoff
  - pairoff-decision
  - guarantor-price
  - guarantor-price-decision
  - guarantor-modify
  - guarantor-modify-decision
  - guarantor-pairoff
  - guarantor-pairoff-decision
  - importLoanS2S
  - importLoanS2SStatus
  - getPurchaseStatement
generated: '2026-09-10'
method: generated
source: openapi/freddie-mac-cash-committing-openapi.json, openapi/freddie-mac-guarantor-committing-openapi.json, openapi/freddie-mac-cash-pricing-openapi.json, openapi/freddie-mac-guarantor-pricing-openapi.json, openapi/freddie-mac-loan-import-openapi.json, openapi/freddie-mac-cash-settlement-purchase-statement-openapi.json
---

# Deliver and commit a loan to Freddie Mac

This is the only write surface in the Freddie Mac catalog with a rehearsal step and the only one with
a reversal. Read the safety notes before you act.

## The two credential regimes

The Committing and Pricing services run on Loan Selling Advisor at `lassvcs-uat.fmrei.com/ESO/rest`
and use **HTTP Basic**, not the bearer token the rest of the catalog uses. Loan Import and the
Settlement Purchase Statement APIs are on the Apigee gateway and use **OAuth 2.0 bearer**. Crossing
from committing to delivery means changing credential model mid-flow.

Loan Import additionally requires vendor attribution headers on every call:
`X-LIS-VENDOR-IDENTIFIER`, `X-LIS-VENDOR-NAME`, `X-LIS-VENDOR-SOFTWARE`,
`X-LIS-VENDOR-SOFTWARE-VERSION`. The Settlement Purchase Statement API requires the `X-CSS-VENDOR-*`
equivalents. Omitting them is a 400.

## Every committing action is two calls

Freddie Mac splits each commitment action into a quote and a decision, bound by a `ResponseToken`:

| Rehearse (commits nothing) | Commit |
| --- | --- |
| `price` | `price-decision` |
| `extension` | `extension-decision` |
| `pairoff` | `pairoff-decision` |
| `guarantor-price` | `guarantor-price-decision` |
| `guarantor-modify` | `guarantor-modify-decision` |
| `guarantor-pairoff` | `guarantor-pairoff-decision` |

The quote call returns a `ResponseToken` in hex plus the resulting terms — contract duration day
count, prices, SRP values and, since the 2026-03-26 release, Servicing Released attributes and the
Contract Asset Price when a designated Servicer is set. The contract is explicit that the decision
call must send back "the same as the ResponseToken sent in price operation respectively".

**Use the quote call as your dry run.** It is the only place in this catalog where an agent can see
the consequence of an action before taking it.

## Steps

1. **Price the execution.**
   Cash sheet: `POST /EWSG_PricingCashSheetV1.0` (`price`, Cash Pricing).
   Guarantor sheet: `POST /EWSG_PricingGuarantorSheetV1.0` (`price`, Guarantor Pricing).
   Note that both operations are literally named `price` — operationIds are unique inside a document
   but not across this catalog. Key on (spec, operationId).

2. **Quote the commitment.** `POST /cash-contracts/price-v1` (`price`) or
   `POST /guarantor-contracts/price-v1` (`guarantor-price`). Keep the `ResponseToken`.
   Every request needs `CommitmentRequestMetaData` carrying a `CorrelationIdentifier` UUID and a
   `RequestTime`; a missing or wrongly-typed one is `400.006` / `400.007`.

3. **Decide.** `POST /cash-contracts/price-decision-v1` (`price-decision`) with the
   `ResponseToken`, `RequestingPartyAccountIdentifier`, `CounterPartyType`, `SellerIdentifier` and
   `ContractIdentifier`. This is the point of no return for the quote. The response carries
   `PurchaseContractStatusType`, `AcceptedDateTime` and `DecisionStatus`.
   Best-efforts contracts without an associated loan have been supported since the 2025-07-24 release.

4. **Deliver the loan.** `POST /v1/loans/import` (`importLoanS2S`, Loan Import v4.0.0) with the
   `SellerIdentifier`, `SellerOrganizationBranchIdentifier`, `FileIdentifier` and
   `CorrelationIdentifier`. Then poll `POST /v1/loans/import-status` (`importLoanS2SStatus`).
   Evaluation findings come back with `errorType`, `criticalityType` (Warning / …) and compliance
   detail — a Warning is not a rejection.

5. **Reconcile settlement.** `POST /purchase_statement` (`getPurchaseStatement`) on Cash Settlement
   Purchase Statement or Guarantor Settlement Purchase Statement. Rate fields here carry their ULDD
   data point names in the schema descriptions (Ceiling Rate Percent, First Rate Change Payment
   Effective Date, Per Change Maximum Increase Rate Percent), so map by ULDD name, not by JSON key.

## Reversal and retry — read this before writing

- **Pair-off is the reversal.** `pairoff` → `pairoff-decision` offsets an accepted cash commitment,
  and `guarantor-pairoff` → `guarantor-pairoff-decision` does the same for guarantor execution.
  Freddie Mac does not publish a window inside which a pair-off is available. Do not assume one; the
  commitment deadlines live in the Seller/Servicer agreement and the Guide.
- **`importLoanS2S` has no reversal at all.** There is no delete, cancel or rollback operation
  anywhere in the Freddie Mac catalog — zero DELETE operations across all 20 contracts. An import you
  did not mean to send has to be unwound by a human through Loan Selling Advisor.
- **There is no Idempotency-Key.** The only replay protection is the ResponseToken binding on the six
  decision operations, which stops a replayed decision creating a second contract. `importLoanS2S`
  has none. If it times out, do not blind-retry — call `importLoanS2SStatus` with the same
  `FileIdentifier` and `CorrelationIdentifier` first and find out whether it landed.

## Errors

- These services **do not return 429**. "Request limit exceeded" arrives as a bare `500`, alongside
  "Message too large", "Invalid JSON format", "Connection timeout from backend" and "Failed to
  establish backside connection". You cannot distinguish throttling from a server fault by status
  code — read `ErrorMessage`.
- `401` / `403` on the Committing services is literally "Rejected by policy" — an Apigee policy
  refusal, not a credential detail.
- If a 500 says "The Loan Selling Advisor API being requested is currently unavailable", retry; the
  contract's own remediation is to call 1-800-FREDDIE if it persists. Check
  <https://sf.freddiemac.com/tools-learning/system-status/loan-selling-advisor> first.
