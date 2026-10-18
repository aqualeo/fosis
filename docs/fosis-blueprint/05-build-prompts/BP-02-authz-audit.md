# BP-02 — Authentication Context, Scoped RBAC and Audit
**Read/obey:** `docs/fosis-blueprint/01-solution-architecture.md` §§A3, A5–A6, A9–A12; `docs/fosis-blueprint/02-domain-design.md` §§D1, D3, D5–D7; `docs/fosis-blueprint/03-data-model.md` §§M1, M3, M5, M7–M10; `docs/fosis-blueprint/04-development-plan.md` §§P3–P5, P9–P10. Issues #25, #175, #176, #26, #179; dependencies BP-00 and tenant context BP-01.

## Goal
Build a generic **deny-by-default** authz/audit foundation before exposing Person/Student or school data. Do **not** invent specialist medical, safeguarding or principal powers: functional epics own those policy details.

## Implementation
- Authenticated principal and validated tenant-resolution interface (federated OIDC/SAML provider selection pending approved ADR); allow synthetic dev/test identity only in isolated development.
- A versionable `role,permission,grant,scope` model: action and module, school/child/group/student relation, field sensitivity and effective period. Multiple roles without automatic union to forbidden data; server-resolved relationships.
- Explicit permission checks in handler and persistence; no authority inferred from a route parameter or client UI visibility. Include delegated grants with expiry/revocation, teacher-to-assigned-group constraints, guardian child context as future extensibility.
- Separation-of-duties interface for approval commands; deny self-approval where policy demands. Require MFA/step-up proof for privileged actions using identity assurance claim; never invent OTP/security provider.
- Append-only change audit plus *sensitive-read audit API* with protected review privileges, actor/time/purpose/correlation and classification-safe differences. Audit remains available when downstream integrations fail.
- Policy decision tracing for tests/support with masked details; do not expose full grant graph or other students via API errors.

## Acceptance
- Synthetic accounts with staff roles for school A cannot read/write school B; unlinked guardian has no access; generic teacher cannot see restricted health/safeguarding; forged tenant claim fails.
- Expired/revoked grant ceases to authorise; privileged operation requires step-up; sensitive self-approval blocked in a configured scenario.
- Sensitive read produces secure audit; ordinary user cannot tamper/delete/rewrite audit history.
- Authenticated negative tests cover list/detail/search/export and batch endpoints; no `WHERE tenant_id`-only policy on sensitive data.
- Integration/API contract tests run via CI; log redaction and documented threat model/test cases.

## Output
Policy engine, interfaces/fixtures, append-only audit primitives, tests and OpenAPI/ADRs in a reviewed PR. Only then begin scoped identity data; never claim the full Epic 25/26 is finished.