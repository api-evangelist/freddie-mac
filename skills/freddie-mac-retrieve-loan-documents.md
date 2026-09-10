---
name: freddie-mac-retrieve-loan-documents
description: Retrieve the source and feedback documents a loan generated across the Freddie Mac Loan Advisor Suite — LPA request/response XML, ULDD XML from Loan Quality Advisor, UCD XML and closing disclosures from Loan Closing Advisor, and UAD appraisal XML from Loan Collateral Advisor — using Data Share.
api: freddie-mac:data-share
operations:
  - getDocumentsUsingPOST
  - getSummaryReultsUsingPOST
  - getLQAdocumentsPOST
  - getLCLAdocumentsPOST
  - getLCAdocumentsPOST
  - getbidsummaryPOST
generated: '2026-09-10'
method: generated
source: openapi/freddie-mac-data-share-openapi.json, openapi/freddie-mac-data-share-bid-tape-openapi.json
---

# Retrieve post-close loan documents from Freddie Mac

Data Share is a read-only document retrieval surface. Everything here is a POST, but nothing here
writes.

## What you get back

The documents are the GSE uniform datasets, carried as base64 XML and PDF inside a JSON body:

| Operation | Source system | Documents |
| --- | --- | --- |
| `getDocumentsUsingPOST` | Loan Product Advisor | LPA request and response XML, feedback response PDF |
| `getLQAdocumentsPOST` | Loan Quality Advisor | **ULDD** XML, feedback response PDF |
| `getLCLAdocumentsPOST` | Loan Closing Advisor | **UCD** XML, closing disclosure PDFs, feedback response PDFs and XMLs |
| `getLCAdocumentsPOST` | Loan Collateral Advisor | appraisal (**UAD**) XML, appraisal PDFs, feedback response XML |
| `getSummaryReultsUsingPOST` | all four | key assessment results only |

Note the operationId `getSummaryReultsUsingPOST` — the typo is the provider's and it is what you must
send.

## Steps

1. **Decide what you actually need.** Build a `documentRequests[]` list naming the document types you
   want. This is the closest thing to field selection in the catalog; there is no `?fields=` or
   `?expand=`, and there is no pagination.

2. **Identify the loan.** Each request entry carries `loanIdentifiers` with a `loanIdentifier`, a
   `lenderLoanIdentifier` and optionally a `propertyAddress` and a per-loan `correlationID`.
   `loanIdentifierType` takes a value from a MISMO prescribed list — `LenderLoan` is one of them —
   so the same number means different things depending on the type you send.

3. **Post the request** to the matching operation.

4. **Read the DSA business-message codes in the 200 body.** This is the part that catches people:
   document availability is reported *inside a successful response*, not as an HTTP status.
   `DSA100` means all available LCLA documents were found; `DSA102` means the requested UCD_XML
   documents were **not** found; `DSA103` means closing-disclosure PDFs were not found. The vocabulary
   spans DSA001–DSA061 (request and validation), DSA100–DSA109 (availability), DSA201–DSA204,
   DSA300–DSA304, DSA400, DSA700–DSA701, DSA800 and DSA900–DSA903.
   A 200 with `DSA102` is a miss. Treat it as one.

5. **For bid-tape work, use Data Share Bid Tape.** `POST /getbidsummary` (`getbidsummaryPOST`) returns
   appraisal and party detail for secondary-market evaluation — `AppraisalDocumentType` identifies
   which of the three UCDP-submitted evaluation files a record refers to, and
   `LoanCollateralAdvisorSubmissionDatetime` is when the appraisal reached UCDP.

## Working with the payloads

- The contracts publish full worked scenarios you can develop against — Data Share ships named
  examples such as `5.0AllDocumentsfoundRequest` / "Scenario 2 All Documents Found Request for ULAD
  5.0" with complete ULAD 5.0 payloads, and matching response examples.
- `Document.MISMOVersionNumber` tells you which MISMO version the returned document conforms to.
  Do not assume; different loans in the same response can differ.
- `partyRoleType` is "a value from a MISMO defined list that identifies the role that the party plays
  in the transaction" — parties can be people or legal entities.

## Errors

- Envelope on these APIs is `{code, message, details[]}` with the standard dotted codes:
  `400.001`, `400.002`, `400.005`, `401.001`–`401.010`, `404.001`, `429.001`, `429.002`, `500`.
- `429.001` is the per-second rate limit and `429.002` the per-minute quota. Data Share responses are
  large — a full document set per loan — so quota, not rate, is the ceiling you will meet first.
- No `Retry-After` header is published anywhere in this catalog. Back off exponentially with jitter.
