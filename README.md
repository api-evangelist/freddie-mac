# Freddie Mac (freddie-mac)

<!-- API-EVANGELIST-PROVENANCE:BEGIN -->
> ### About this repository
>
> **This is not our API.** This repository is an independent, third-party profile of a company's
> **publicly available** API surface, maintained by [API Evangelist](https://apievangelist.com).
> API Evangelist does not operate, host, resell, or support this company's APIs, and is not
> affiliated with or endorsed by the company unless stated on the profile.
>
> **Where the information came from.** Everything here is assembled from material a member of the
> public can reach with a browser and no credentials — the company's own website, developer portal
> and documentation, the specifications it publishes for public use (OpenAPI, AsyncAPI, JSON Schema,
> `apis.json`, `llms.txt` and similar), its public repositories, and its public status, pricing and
> changelog pages. **Nothing here is obtained by breaching a system, defeating an access control, or
> using credentials of any kind.**
>
> **The rating is an independent assessment.** The Kin Score and Agent Readiness rating are
> independently calculated scores of a company's *public* API artifacts, produced by API Evangelist
> against a published rubric. They are not certifications, endorsements, security assessments, or
> audits, and they score published artifacts — not the quality, safety, or security of the software.
>
> **Corrections, re-scores, and removal are free.** No partnership, contract, or purchase is
> required, and you do not need to justify the request.
>
> - **Something wrong?** Open an issue on this repository, or email
>   [info@apievangelist.com](mailto:info@apievangelist.com).
> - **Published something new?** Ask for a re-score and we will re-run the rating.
> - **Want the listing taken down?** Say so and we will honor it. The profile is reduced to your
>   company name, a factual description, and a link to your own site, and the company is recorded as
>   **unrated** — never scored zero for having asked.
>
> **Response times.** Acknowledgement within **one business day**; removal or restriction within
> **two business days**; corrections and re-scores within **five business days**.
>
> **Not from the company, and here with a question?** You are welcome here — we would rather be the
> front line and point you the right way than have a good report go nowhere. What this repository
> can answer is narrow, though, so it is worth knowing who you are actually looking for:
>
> - **A question about how the API works, an account, billing, or a bug in the service** — that is
>   the company's own support, not us. We profile this API; we do not operate it and cannot see
>   your account.
> - **A bug in an open-source project we only catalog** — file it on that project's own repository.
>   This has happened with a real and correct bug report that reached us instead of the people who
>   could fix it, which helped nobody.
> - **Anything about this listing itself** — the description, the tags, the rating, a missing or
>   wrong artifact — is ours. Open an issue here.
> - **Not sure, or something general about API Evangelist or APIs.io** — open an issue on the
>   [APIs.io Inbox](https://github.com/api-search/inbox) and we will route it.
>
> **This repository contains no software, and we will never ask you to download anything.** There is
> no build, release, installer, or binary here — only text and machine-readable API descriptions, so
> there is nothing here that can be "corrupt" or need "repairing". Any issue, comment, or email
> claiming otherwise and offering a download link is not from us and is hostile. Do not follow the
> link; it is a lure. Report it to GitHub and, if you like, tell us at
> [info@apievangelist.com](mailto:info@apievangelist.com) so we can take it down.
>
> **On a security or compliance team?** Email
> [info@apievangelist.com](mailto:info@apievangelist.com) with *security* in the subject line and
> you will get a person, not a form. We will tell you exactly which public URLs this profile was
> built from so your team can see the same surface we did, and we will take the listing down on
> request while you work through it.
>
> Full detail: **[Where this data comes from](https://apievangelist.com/about/where-our-data-comes-from)**
<!-- API-EVANGELIST-PROVENANCE:END -->

Freddie Mac (Federal Home Loan Mortgage Corporation) is the government-sponsored enterprise that provides liquidity, stability and affordability to the U.S. housing market by purchasing and securitizing single-family and multifamily mortgages. Its Single-Family API programme is published through the Freddie Mac Developer Portal and spans the whole mortgage lifecycle: origination (Affordable Check, Income Limits, Loan Look Up, Property Insights, Current Mortgage Snapshot, Beyond ACE), closing (Loan Closing Advisor Loan Submission), selling and delivery (Cash and Guarantor Pricing, Committing and Settlement Purchase Statement, Loan Import into Loan Selling Advisor), post-close data retrieval (Loan Advisor Data Share, Data Share Bid Tape) and default servicing (the Resolve workout, liquidation and valuation/pricing decisioning services). The public catalog publishes 20 OpenAPI 3.0 contracts covering 45 operations; the request and response payloads are modelled on the MISMO reference model and the GSE Uniform Mortgage Data Program datasets (UAD, ULAD, ULDD, UCD).

**URL:** [Visit APIs.json URL](https://raw.githubusercontent.com/api-evangelist/freddie-mac/refs/heads/main/apis.yml)

## Scope

- **Type:** Index
- **Position:** Producing
- **Access:** 3rd-Party

## Tags

- Federal Government, Housing, Mortgage, Lending, Servicing, Origination, Secondary Market, MISMO, Fortune 100

## Timestamps

- **Created:** 2024-12-03
- **Modified:** 2026-09-10

## APIs

20 APIs are published in the Freddie Mac Developer Portal public catalog, covering 45 operations across five mortgage-lifecycle categories. Every OpenAPI below was harvested verbatim from the provider on 2026-09-10.

### Freddie Mac Affordable Check

Help low-income borrowers choose the optimal mortgage path with an early look at Freddie Mac affordable product eligibility prior to submitting a full application to Loan Product Advisor®.

- **Human URL:** https://developer.freddiemac.com/public/#/api-info/overview/62858cdd06bca128fb37ce71
- **Base URL:** https://api-test.freddiemac.com/single-family/affordable-check-api/v2
- **Version:** 2.0.0

#### Tags

- Mortgage, Origination

#### Properties

- [OpenAPI](openapi/freddie-mac-affordable-check-openapi.json)
- [Documentation](https://developer.freddiemac.com/public/#/api-info/details/62858cdd06bca128fb37ce71)
- [Overlay](overlays/freddie-mac-affordable-check-overlay.yaml)

### Freddie Mac Beyond ACE

Submit property data report (PDR) data and photos to receive feedback messages and speed up your underwriting process.

- **Human URL:** https://developer.freddiemac.com/public/#/api-info/overview/6197ea655032a264da85a08c
- **Base URL:** https://api-test.freddiemac.com/single-family/loan-advisor-suite/las-beyondace-api/v2
- **Version:** 3.0.0

#### Tags

- Mortgage, Origination

#### Properties

- [OpenAPI](openapi/freddie-mac-beyond-ace-openapi.yaml)
- [Documentation](https://developer.freddiemac.com/public/#/api-info/details/6197ea655032a264da85a08c)
- [Overlay](overlays/freddie-mac-beyond-ace-overlay.yaml)

### Freddie Mac Cash Committing

Quickly streamline secondary market processes with an easy-to-use, integrated technology solution that allows you to execute and modify mandatory cash contracts without the need to manually do so in Loan Selling Advisor®.

- **Human URL:** https://developer.freddiemac.com/public/#/api-info/overview/623d0765fddb9917ea999f3b
- **Base URL:** https://lassvcs-uat.fmrei.com/ESO/rest/lsa
- **Version:** 1.0.0

#### Tags

- Mortgage, Selling

#### Properties

- [OpenAPI](openapi/freddie-mac-cash-committing-openapi.json)
- [Documentation](https://developer.freddiemac.com/public/#/api-info/details/623d0765fddb9917ea999f3b)
- [Overlay](overlays/freddie-mac-cash-committing-overlay.yaml)

### Freddie Mac Cash Pricing

Quickly streamline secondary market processes with easier, integrated access to cash pricing information without the need to manually retrieve it from Loan Selling Advisor®.

- **Human URL:** https://developer.freddiemac.com/public/#/api-info/overview/623d0826c48acc2401ba22da
- **Base URL:** https://lassvcs-uat.fmrei.com/ESO/rest
- **Version:** 1.0.0

#### Tags

- Mortgage, Selling

#### Properties

- [OpenAPI](openapi/freddie-mac-cash-pricing-openapi.json)
- [Documentation](https://developer.freddiemac.com/public/#/api-info/details/623d0826c48acc2401ba22da)
- [Overlay](overlays/freddie-mac-cash-pricing-overlay.yaml)

### Freddie Mac Cash Settlement Purchase Statement

Streamline your post funding reconciliation process by eliminating manual processes with fast, easy access to cash purchase statement data for the loans you sold to Freddie Mac.

- **Human URL:** https://developer.freddiemac.com/public/#/api-info/overview/624f7c78b1e1c3419ba58885
- **Base URL:** https://api-test.freddiemac.com/single-family/css/v1/cashsettlement
- **Version:** 1.0.1

#### Tags

- Mortgage, Selling

#### Properties

- [OpenAPI](openapi/freddie-mac-cash-settlement-purchase-statement-openapi.json)
- [Documentation](https://developer.freddiemac.com/public/#/api-info/details/624f7c78b1e1c3419ba58885)
- [Overlay](overlays/freddie-mac-cash-settlement-purchase-statement-overlay.yaml)

### Freddie Mac Current Mortgage Snapshot

Easy access to original loan terms as well as current servicing data, on Freddie Mac owned loans to help validate information provided by the borrower or obtained from other sources (e.g., credit report).

- **Human URL:** https://developer.freddiemac.com/public/#/api-info/overview/6197b94aa8adda2fb7323736
- **Base URL:** https://api-test.freddiemac.com/single-family/current-mortgage-snapshot-api/v1
- **Version:** 1.0.0

#### Tags

- Mortgage, Origination

#### Properties

- [OpenAPI](openapi/freddie-mac-current-mortgage-snapshot-openapi.json)
- [Documentation](https://developer.freddiemac.com/public/#/api-info/details/6197b94aa8adda2fb7323736)
- [Overlay](overlays/freddie-mac-current-mortgage-snapshot-overlay.yaml)

### Freddie Mac Data Share

Get fast easy access to key data, documents, and loan-level assessment results from Freddie Mac tools.

- **Human URL:** https://developer.freddiemac.com/public/#/api-info/overview/6197eb69a8adda2fb732376d
- **Version:** 1.0.0

#### Tags

- Mortgage, Post-Close

#### Properties

- [OpenAPI](openapi/freddie-mac-data-share-openapi.json)
- [Documentation](https://developer.freddiemac.com/public/#/api-info/details/6197eb69a8adda2fb732376d)
- [Overlay](overlays/freddie-mac-data-share-overlay.yaml)

### Freddie Mac Data Share Bid Tape

Aggregators can have greater confidence when pricing and valuing correspondent loans by getting easy access to key loan data and loan-level summary assessment results at the point of bid.

- **Human URL:** https://developer.freddiemac.com/public/#/api-info/overview/6233cd01689af970cd9a0b43
- **Version:** 1.0.0

#### Tags

- Mortgage, Post-Close

#### Properties

- [OpenAPI](openapi/freddie-mac-data-share-bid-tape-openapi.json)
- [Documentation](https://developer.freddiemac.com/public/#/api-info/details/6233cd01689af970cd9a0b43)
- [Overlay](overlays/freddie-mac-data-share-bid-tape-overlay.yaml)

### Freddie Mac Guarantor Committing

Quickly streamline secondary market processes with an easy-to-use, integrated technology solution that allows you to execute and modify Guarantor contracts without the manual processes in Loan Selling Advisor®.

- **Human URL:** https://developer.freddiemac.com/public/#/api-info/overview/623d087efddb9917ea999f5a
- **Base URL:** https://lassvcs-uat.fmrei.com/ESO/rest/lsa
- **Version:** 1.0.0

#### Tags

- Mortgage, Selling

#### Properties

- [OpenAPI](openapi/freddie-mac-guarantor-committing-openapi.json)
- [Documentation](https://developer.freddiemac.com/public/#/api-info/details/623d087efddb9917ea999f5a)
- [Overlay](overlays/freddie-mac-guarantor-committing-overlay.yaml)

### Freddie Mac Guarantor Pricing

Quickly streamline secondary market processes with easier, integrated access to guarantor pricing information without the need to manually retrieve it from Loan Selling Advisor®.

- **Human URL:** https://developer.freddiemac.com/public/#/api-info/overview/623d08d5fddb9917ea999f68
- **Base URL:** https://lassvcs-uat.fmrei.com/ESO/rest
- **Version:** 1.0.0

#### Tags

- Mortgage, Selling

#### Properties

- [OpenAPI](openapi/freddie-mac-guarantor-pricing-openapi.json)
- [Documentation](https://developer.freddiemac.com/public/#/api-info/details/623d08d5fddb9917ea999f68)
- [Overlay](overlays/freddie-mac-guarantor-pricing-overlay.yaml)

### Freddie Mac Guarantor Settlement Purchase Statement

Obtain Guarantor Settlement Purchase Statement data for loans settled in a security by Freddie Mac.

- **Human URL:** https://developer.freddiemac.com/public/#/api-info/overview/6531c4a3276bea497cf1cad4
- **Base URL:** https://api-test.freddiemac.com/single-family/guarantor-settlement-service/v1
- **Version:** 1.0.0

#### Tags

- Mortgage, Selling

#### Properties

- [OpenAPI](openapi/freddie-mac-guarantor-settlement-purchase-statement-openapi.json)
- [Documentation](https://developer.freddiemac.com/public/#/api-info/details/6531c4a3276bea497cf1cad4)
- [Overlay](overlays/freddie-mac-guarantor-settlement-purchase-statement-overlay.yaml)

### Freddie Mac Income Limits

Receive fast, reliable data you can use to evaluate potential eligibility for affordable lending solutions.

- **Human URL:** https://developer.freddiemac.com/public/#/api-info/overview/6197b8395032a264da85a01b
- **Base URL:** https://api-test.freddiemac.com/single-family/income-limits-api/v2
- **Version:** 2.1.0

#### Tags

- Mortgage, Origination

#### Properties

- [OpenAPI](openapi/freddie-mac-income-limits-openapi.json)
- [Documentation](https://developer.freddiemac.com/public/#/api-info/details/6197b8395032a264da85a01b)
- [Overlay](overlays/freddie-mac-income-limits-overlay.yaml)

### Freddie Mac Loan Closing Advisor Loan Submission

Provides access to Loan Closing Advisor, through your own system or a software provider LOS. This enables loan access and receiving actionable feedback directly through your system on the accuracy of your UCD XML file to assist you in identifying and correcting errors before you close a loan.

- **Human URL:** https://developer.freddiemac.com/public/#/api-info/overview/660f427090f1397952c8a9a0
- **Base URL:** https://api-test.freddiemac.com/eso/v2
- **Version:** 3.2.0

#### Tags

- Closing, Mortgage

#### Properties

- [OpenAPI](openapi/freddie-mac-loan-closing-advisor-loan-submission-openapi.yaml)
- [Documentation](https://developer.freddiemac.com/public/#/api-info/details/660f427090f1397952c8a9a0)
- [Overlay](overlays/freddie-mac-loan-closing-advisor-loan-submission-overlay.yaml)

### Freddie Mac Loan Import

Loan Import provides sellers and integrated technology providers the capability to create or modify loans, evaluate purchase eligibility, and provide decisions on representation and warranty relief through Loan Selling Advisor®.

- **Human URL:** https://developer.freddiemac.com/public/#/api-info/overview/68df12b88875c078cd3faf22
- **Base URL:** https://api-test.freddiemac.com/single-family/lsa
- **Version:** 4.0.0

#### Tags

- Mortgage, Selling

#### Properties

- [OpenAPI](openapi/freddie-mac-loan-import-openapi.json)
- [Documentation](https://developer.freddiemac.com/public/#/api-info/details/68df12b88875c078cd3faf22)
- [Overlay](overlays/freddie-mac-loan-import-overlay.yaml)

### Freddie Mac Loan Look Up

Look up if Freddie Mac owns the mortgage for a specific subject property.

- **Human URL:** https://developer.freddiemac.com/public/#/api-info/overview/6197bab05032a264da85a02b
- **Base URL:** https://api-test.freddiemac.com/single-family/loan-advisor-suite/v2
- **Version:** 2.0.0

#### Tags

- Mortgage, Origination

#### Properties

- [OpenAPI](openapi/freddie-mac-loan-look-up-openapi.json)
- [Documentation](https://developer.freddiemac.com/public/#/api-info/details/6197bab05032a264da85a02b)
- [Overlay](overlays/freddie-mac-loan-look-up-overlay.yaml)

### Freddie Mac Property Insights

Get fast, early access to Freddie Mac property data to help identify and overcome property-related pain points.

- **Human URL:** https://developer.freddiemac.com/public/#/api-info/overview/619720835032a264da859ff4
- **Base URL:** https://api-test.freddiemac.com/single-family/property-insights-api/v2
- **Version:** 2.0.0

#### Tags

- Mortgage, Origination

#### Properties

- [OpenAPI](openapi/freddie-mac-property-insights-openapi.json)
- [Documentation](https://developer.freddiemac.com/public/#/api-info/details/619720835032a264da859ff4)
- [Overlay](overlays/freddie-mac-property-insights-overlay.yaml)

### Freddie Mac Resolve Liquidation

Request short sale approvals to Resolve, the integrated default management platform. Directly integrate while maintaining your user experience and receive rapid, rules-based eligibility decisions and approvals.

- **Human URL:** https://developer.freddiemac.com/public/#/api-info/overview/6198033aa8adda2fb732385d
- **Base URL:** https://api-test.freddiemac.com/singlefamily/servicing/v1/resolve/liquidation-service/api
- **Version:** 1.0.0

#### Tags

- Mortgage, Servicing

#### Properties

- [OpenAPI](openapi/freddie-mac-resolve-liquidation-openapi.json)
- [Documentation](https://developer.freddiemac.com/public/#/api-info/details/6198033aa8adda2fb732385d)
- [Overlay](overlays/freddie-mac-resolve-liquidation-overlay.yaml)

### Freddie Mac Resolve Retention

Request relief options and retention workouts via the API to Resolve, the integrated default management platform. Directly integrate to maintain your user experience and receive rapid, rules-based eligibility decisions without leaving your existing default management tool, enabling faster resolution for homeowners.

- **Human URL:** https://developer.freddiemac.com/public/#/api-info/overview/619801965032a264da85a168
- **Base URL:** https://api-test.freddiemac.com/singlefamily/v3/servicing/workoutservice/workout-service
- **Version:** 3.0.0

#### Tags

- Mortgage, Servicing

#### Properties

- [OpenAPI](openapi/freddie-mac-resolve-retention-openapi.json)
- [Documentation](https://developer.freddiemac.com/public/#/api-info/details/619801965032a264da85a168)
- [Overlay](overlays/freddie-mac-resolve-retention-overlay.yaml)

### Freddie Mac Resolve Valuation & Pricing

Request Minimum Net Proceeds (MNP) value via the API to Resolve, the integrated default management platform. Directly connect while maintaining your user experience and receive MNP values for short sales.

- **Human URL:** https://developer.freddiemac.com/public/#/api-info/overview/619803ca5032a264da85a185
- **Base URL:** https://api-test.freddiemac.com/singlefamily/servicing/v2/rvp-service/rvpInfo
- **Version:** 2.0.0

#### Tags

- Mortgage, Servicing

#### Properties

- [OpenAPI](openapi/freddie-mac-resolve-valuation-pricing-openapi.json)
- [Documentation](https://developer.freddiemac.com/public/#/api-info/details/619803ca5032a264da85a185)
- [Overlay](overlays/freddie-mac-resolve-valuation-pricing-overlay.yaml)

### Freddie Mac Resolve Workout Options

Request real-time eligibility decisions for multiple workout options in a single response.

- **Human URL:** https://developer.freddiemac.com/public/#/api-info/overview/6198025c5032a264da85a176
- **Base URL:** https://api-test.freddiemac.com/singlefamily/v1/servicing/workoutservice/workout-service
- **Version:** 1.3.0

#### Tags

- Mortgage, Servicing

#### Properties

- [OpenAPI](openapi/freddie-mac-resolve-workout-options-openapi.json)
- [Documentation](https://developer.freddiemac.com/public/#/api-info/details/6198025c5032a264da85a176)
- [Overlay](overlays/freddie-mac-resolve-workout-options-overlay.yaml)

## Common Properties

- [Website](https://www.freddiemac.com/)
- [DeveloperPortal](https://developer.freddiemac.com/public/)
- [Documentation](https://sf.freddiemac.com/tools-learning/apis/our-api-solutions)
- [APIReference](https://developer.freddiemac.com/public/#/api-catalog)
- [GettingStarted](https://sf.freddiemac.com/tools-learning/apis/getting-started-with-apis)
- [SignUp](https://developer.freddiemac.com/public/#/contact-us)
- [Support](https://developer.freddiemac.com/public/#/contact-us)
- [Blog](https://sf.freddiemac.com/news-insights)
- [TermsOfService](https://www.freddiemac.com/terms)
- [PrivacyPolicy](https://www.freddiemac.com/terms/privacy)
- [LinkedIn](https://www.linkedin.com/company/freddie-mac)
- [StatusPage](https://sf.freddiemac.com/tools-learning/system-status)
- [ChangeLog](https://sf.freddiemac.com/tools-learning/technology-tools/releases)
- [Roadmap](https://developer.freddiemac.com/public/#/text/68ff8f5955b3873d79a0ff2b/6901115355b3873d79a10296)
- [Security](https://api.freddiemac.com/.well-known/security.txt)
- [SecurityTxt](well-known/freddie-mac-api-security.txt)
- [WellKnown](well-known/freddie-mac-well-known.yml)
- [VulnerabilityDisclosure](security/freddie-mac-vulnerability-disclosure.yml)
- [DomainSecurity](security/freddie-mac-domain-security.yml)
- [Authentication](authentication/freddie-mac-authentication.yml)
- [Conventions](conventions/freddie-mac-conventions.yml)
- [ErrorCatalog](errors/freddie-mac-problem-types.yml)
- [Lifecycle](lifecycle/freddie-mac-lifecycle.yml)
- [Deprecation](lifecycle/freddie-mac-lifecycle.yml)
- [Conformance](conformance/freddie-mac-conformance.yml)
- [DataModel](data-model/freddie-mac-data-model.yml)
- [Examples](examples/freddie-mac-examples.yml)
- [Sandbox](sandbox/freddie-mac-sandbox.yml)
- [ChangeLog](changelog/freddie-mac-changelog.yml)
- [Packages](packages/freddie-mac-packages.yml)
- [MCPServer](mcp/freddie-mac-mcp.yml)
- [AgentSkill](skills/_index.yml)
- [LLMsTxt](llms/freddie-mac-llms.txt)
- [RateLimits](rate-limits/freddie-mac-rate-limits.yml)
- [Plans](plans/freddie-mac-plans-pricing.yml)
- [FinOps](finops/freddie-mac-finops.yml)
- [Rules](rules/freddie-mac-rules.yml)

## Maintainers

**FN:** Kin Lane

**Email:** kin@apievangelist.com
