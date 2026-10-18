# BP-03 — Global Person and Student Identity Foundations
**Must read:** `docs/fosis-blueprint/01-solution-architecture.md` §§A2–A4, A5, A9, A12 (especially ADR-006); `docs/fosis-blueprint/02-domain-design.md` §§D2 (Epics 1–2), D3, D4.1–D4.2, D7–D8; `docs/fosis-blueprint/03-data-model.md` §§M1–M5, M7–M11; `docs/fosis-blueprint/04-development-plan.md` §§P3–P5, P9–P11. Issues #1, #33–44, #2, #45–48, #41, #82–83. Dependencies BP-00/01/02.

## Critical stop rule
**Issue conflict**: #33 describes Student identity creation in admissions; #82 explicitly says permanent Student ID is issued only upon physical arrival. This is ADR-006. **Do not implement Student ID minting, merge of activated students, or arrival transition until owner resolves it.** Build global Person and tenant profile groundwork and an interface with safe inactive placeholders only.

## Scope
- Global Person UUID and opaque, immutable platform-wide FOS Person ID; distinct tenant-scoped Person profiles and Guardian roles; no implicit user login. Person may be both guardian and staff.
- Identity field model for legal/preferred/middle names, provenance, verification timestamps and versioning; sensitive legal attributes controlled by RBAC. Students' minimum agreed identity fields include legal first/last name, DOB and sex, with linked guardian during actual Student creation; phone optional for Guardian, email mandatory for agreed onboarding flow (#34).
- StudentIdentity interface and ID uniqueness/alias specifications, but issuance feature disabled behind explicit ADR resolution; no prejoin accidental ID creation. External IDs are scoped by issuer/type and tenant, not primary identity.
- Duplicate candidate workflow: propose, preview and require human confirmation; no automatic merges. Design 30-day reversible retirement as documented in #41, but do not implement full merge until cross-record references and approval bounds are established.
- Add safe search endpoints with only entitled tenant profiles, no platform-wide enumeration or cross-tenant name/email lookups for school actors.

## Acceptance
- One global Person can have two tenant profiles with independent scoped visibility; school A sees none of school B's children or relationships.
- Names and identifiers preserve legal/preferred display distinction; opaque FOS ID cannot be reused; email is not assumed unique human identity.
- No Student ID can be generated via hidden student create path pending ADR-006; unauthorised user sees no identity matches outside their tenant.
- Data migrations, API, domain tests, identity collision/concurrency, audit and no-PII-in-logs tests pass.

## Deliverable
PR with global Person/profile model and APIs, issued-ID interface (inactive where blocked), synthetic two-tenant tests, threat model and explicit ADR-006 unresolved report. Do not create activated Student records or close Epic 1.