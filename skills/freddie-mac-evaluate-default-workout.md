---
name: freddie-mac-evaluate-default-workout
description: Get an instant Freddie Mac workout decision for a delinquent loan — retention options, liquidation/short-sale, and valuation and pricing — using the Resolve servicing APIs.
api: freddie-mac:resolve-workout-options
operations:
  - evaluateWorkoutUsingPOST
  - requestWorkoutV3UsingPOST
  - requestWorkoutUsingPOST
  - ShortSaleMNPUsingPOST
generated: '2026-09-10'
method: generated
source: openapi/freddie-mac-resolve-workout-options-openapi.json, openapi/freddie-mac-resolve-retention-openapi.json, openapi/freddie-mac-resolve-liquidation-openapi.json, openapi/freddie-mac-resolve-valuation-pricing-openapi.json
---

# Evaluate a Freddie Mac default workout

Four separate APIs, four separate contracts, four different path versions. All four are POST-shaped
decisioning calls that return terms and rationale and change nothing.

## The four surfaces

| API | Path | operationId | Version in path |
| --- | --- | --- | --- |
| Resolve Workout Options | `/api/evaluateWorkout` | `evaluateWorkoutUsingPOST` | v1 |
| Resolve Retention | `/api/v3/requestWorkout` | `requestWorkoutV3UsingPOST` | v3 |
| Resolve Liquidation | `/shortsale` | `requestWorkoutUsingPOST` | v1 |
| Resolve Valuation & Pricing | `/rvpService` | `ShortSaleMNPUsingPOST` | v2 |

They sit on three different base paths under `singlefamily/` — the workout service is versioned in
the path (`/singlefamily/v1/servicing/workoutservice/…` for options, `/singlefamily/v3/…` for
retention), so upgrading Retention to v3 means changing the host path, not a header.

Resolve Retention carries 74 schema definitions and Resolve Workout Options 54 — these are the
richest request models in the Freddie Mac catalog. Budget real time for the mapping.

## Steps

1. **Start with options.** `POST /api/evaluateWorkout` (`evaluateWorkoutUsingPOST`) to find out which
   workout paths the loan qualifies for at all. Requests are batch-shaped: a
   `requestBatchIdentifier` wraps one or more `requestTransactionIdentifier` entries, each keyed on
   `servicerLoanIdentifier` and `servicerAccountIdentifier`.

2. **Retention path.** If the borrower is staying, `POST /api/v3/requestWorkout`
   (`requestWorkoutV3UsingPOST`) for modification, forbearance and repayment terms.

3. **Liquidation path.** If they are not, `POST /shortsale` (`requestWorkoutUsingPOST`) on Resolve
   Liquidation.

4. **Valuation and pricing.** `POST /rvpService` (`ShortSaleMNPUsingPOST`) on Resolve Valuation &
   Pricing for the minimum net proceeds and valuation inputs that a short-sale decision depends on.

## Reading the response

- Errors on this surface use their own envelope —
  `{workoutErrorCode, workoutErrorDescription, workoutErrorType}` — which is not the
  `{code, message, details[]}` shape the origination APIs use and not the `{Error}` shape the
  committing APIs use. Write a Resolve-specific parser.
- Responses are per-transaction inside the batch. A batch can partially succeed; check each
  transaction, not just the HTTP status.

## What is coming

Freddie Mac has announced a **Resolve Default Event Reporting API for November 2026**: event-based
default reporting submitted by servicers with near real-time acceptance or error responses, replacing
reliance on monthly EDR action-code submissions. If you are building EDR automation now, build it so
the reporting leg can be swapped. See `lifecycle/freddie-mac-lifecycle.yml`.

## Errors

- Standard dotted codes apply on the gateway (`401.001`–`401.010`, `429.001`, `429.002`) plus the
  Resolve-specific workout error triple above.
- No `Retry-After` or `X-RateLimit-*` header is published. Back off exponentially.
- Check <https://sf.freddiemac.com/tools-learning/system-status> before escalating a 500 — the status
  page is per-system and covers the servicing tools individually.
