---
name: freddie-mac-prequalify-affordable
description: Pre-qualify a borrower for Freddie Mac affordable mortgage products before a full Loan Product Advisor submission, using Income Limits, Affordable Check and Loan Look Up.
api: freddie-mac:affordable-check
operations:
  - incomeLimitsData
  - requestEligibility
  - LoanLookup
  - requestMortgageData
generated: '2026-09-10'
method: generated
source: openapi/freddie-mac-income-limits-openapi.json, openapi/freddie-mac-affordable-check-openapi.json, openapi/freddie-mac-loan-look-up-openapi.json, openapi/freddie-mac-current-mortgage-snapshot-openapi.json
---

# Pre-qualify a borrower for Freddie Mac affordable products

All four operations here are read-only assessments. Nothing in this flow writes, commits or can be
undone — you are asking Freddie Mac what it thinks, not telling it anything.

## Before you start

- You need an OAuth 2.0 bearer token minted for an app whose Apigee API product covers these APIs.
  A token scoped to a different product returns `401.003 API Product mismatch for token`, and an app
  without the entitlement returns `401.006 Insufficient scope for Application`. Neither is fixable in
  code — a Developer Administrator has to add the product to the app.
- Set `Content-Type: application/json` explicitly. Several operations declare it as a required header
  parameter and return `400.006 Content-type must be application/json` without it.
- The base URLs in the published contracts are the test hosts. The production base path is issued
  inside the Developer Portal at app-promotion time; do not guess it.

## Steps

1. **Establish the income ceiling for the property location.**
   `POST /incomeLimitsData` (`incomeLimitsData`, Income Limits v2.1.0) with the property state, zip
   and census tract. Returns area median income values and the income-to-AMI percentage. As of the
   2026-08-06 release it also accepts an optional loan application date to support Mission Indication
   scoring — send it if you have it, because it changes which AMI vintage applies.

2. **Check affordable product eligibility.**
   `POST /requestBorrowerEligibilityData` (`requestEligibility`, Affordable Check v2) with borrower
   income, loan purpose, loan amount or down payment, estimated value or purchase price, and property
   state and zip. Returns preliminary eligibility for Home Possible, HFA Advantage, Refi Possible and
   HomeOne, plus messaging for Duty to Serve Manufactured Housing, Duty to Serve High Needs Rural
   Regions, and the LIP / VLIP / LIR subgoal categories.
   Only v2 exists — v1 and its SDKs were retired on 2025-10-29.

3. **If this is a refinance, check ownership first.**
   `POST /loanlookup` (`LoanLookup`, Loan Look Up v2) with the subject property address and borrower
   SSN. Returns a Freddie Mac ownership indicator, the Freddie Mac loan number, note date, mortgage
   type and payoff-type detail, including loans paid off in the last 90 days. A borrower whose loan
   Freddie Mac owns may be eligible for Refi Possible, which is what step 2 will have flagged.

4. **Pull the current mortgage picture when you need terms, not just ownership.**
   `POST /requestMortgageData` (`requestMortgageData`, Current Mortgage Snapshot v1).

## Handling the answer

- These are *preliminary* assessments. None of them is a Loan Product Advisor decision, and none of
  them binds Freddie Mac. Present them as an early read, not an approval.
- The response is a single complete document. There is no pagination anywhere in this catalog, so do
  not look for a cursor.

## Errors

- `400.002` — your body does not match the schema. Validate against the operation's `requestBody`
  before sending; these specs ship worked examples for exactly this.
- `401.002` / `401.010` — access token or refresh token expired. Renew and retry.
- `429.001` — you exceeded the per-second rate. `429.002` — you exceeded the per-minute quota.
  No `Retry-After` or `X-RateLimit-*` header is published, so back off exponentially with jitter and
  treat 429.001 as clearing within a second and 429.002 as clearing on the quota window.
- The error envelope is `{code, message, details[]}` on these APIs. It is not RFC 9457 and it is not
  the same shape the Committing or Resolve APIs return — see `errors/freddie-mac-problem-types.yml`.
