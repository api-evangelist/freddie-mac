---
name: freddie-mac-submit-closing-dataset
description: Submit a single-loan Uniform Closing Dataset (UCD) file to Freddie Mac Loan Closing Advisor for assessment, and retrieve the resulting feedback documents.
api: freddie-mac:loan-closing-advisor-loan-submission
operations:
  - submitLoan
  - getLCLAdocumentsPOST
generated: '2026-09-10'
method: generated
source: openapi/freddie-mac-loan-closing-advisor-loan-submission-openapi.yaml, openapi/freddie-mac-data-share-openapi.json
---

# Submit a UCD closing dataset to Loan Closing Advisor

One operation, one loan, one synchronous assessment — and no way to withdraw it.

## Before you act

`submitLoan` is a write with **no reversal**. There is no cancel, no delete and no withdraw operation
in the Loan Closing Advisor contract or anywhere else in the Freddie Mac catalog, and there is no
Idempotency-Key header, so a retried submission is a second submission. If a call times out, treat the
outcome as unknown and confirm through Loan Closing Advisor before resending.

OAuth was deployed to production for this API on 2025-09-25; the contract's `bearerAuth` scheme
documents its `bearerFormat` as "Token acquired from OAuth API for the user credentials".

## The payload is MISMO, not JSON-shaped

`POST /EWSG_LCLALoanEvaluationServiceV2.0` (`submitLoan`) takes a single-loan Uniform Closing Dataset
file. The contract's own request examples are MISMO 3.3 XML:

```xml
<DOCUMENT MISMOReferenceModelIdentifier="3.3.0299"
          xmlns:mismo="http://www.mismo.org/residential/2009/schemas"
          xmlns:gse="http://www.freddiemac.com">
```

Party relationships are expressed with MISMO relationship URNs — for example
`xlink:arcrole="urn:fdc:mismo.org:2009:residential/ROLE_IsEmployedBy_ROLE"` linking a `PARTY…_ROLE1`
to another. If your LOS already produces UCD for Loan Closing Advisor, you already have this file;
generating it from scratch out of a flat loan record is not a mapping exercise you want to do by hand.

The contract points at downloadable schemas via `externalDocs`
(`https://developer.freddiemac.com/devportal/documents/download//685de1428f40401bb68c26c0`), which is
inside the authenticated portal.

## Steps

1. **Produce a valid MISMO 3.3 UCD file** for the single loan.
2. **`POST /EWSG_LCLALoanEvaluationServiceV2.0`** with a bearer token. The service is synchronous with
   a single operation: it accepts one loan and returns the assessment results.
3. **Read the assessment.** Findings come back in `LCLALoanSubmissionResponse`.
4. **Retrieve the artefacts later** with Data Share: `POST /getLCLAdocuments`
   (`getLCLAdocumentsPOST`) returns the UCD XMLs, closing disclosure PDFs and feedback response PDFs
   and XMLs for the loan. Check the `DSA1xx` business-message codes in that 200 body — `DSA102` means
   the UCD XML was not found.

## Errors

- `500.001 Malformed XML` and `500.004 Schema Validation Error` are the two you will actually hit.
  Note that both are 500s, not 400s — a schema problem in your file surfaces as a server error here,
  which is unusual and worth special-casing.
- `500.002 Target server not reachable` / `500.003 Failed to establish a backside connection` are
  genuine infrastructure faults; these are safe to retry, unlike the malformed-XML cases.
- `431 Request Header Fields Too Large` is declared on this API — the only one in the catalog that
  anticipates it. UCD payloads are large; keep headers lean.
- Standard `401.001`–`401.010` OAuth codes apply.
