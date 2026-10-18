# BP-00 — Native FOSIS Engineering Foundation
**Read before coding:** `docs/fosis-blueprint/01-solution-architecture.md` §§A1–A4, A9–A12; `docs/fosis-blueprint/02-domain-design.md` §§D1, D3, D7; `docs/fosis-blueprint/03-data-model.md` §§M1, M5, M8–M10; `docs/fosis-blueprint/04-development-plan.md` §§P4–P10. These source paths are mandatory implementation inputs, not optional references. Relevant epics: #24, #25, #26, #31, #32 and #249.

## Goal
Create a safe, testable native-FOSIS application boundary and CI/test infrastructure **without re-engineering or conflating** the existing Ed-Fi ODS → OneRoster 1.2 service in the repository root.

## Preflight and stop conditions
1. Inspect repository structure, `README.md`, `package.json`, `docs/architecture/*`, existing builds/tests, CI, licence and runtime scripts. Record the existing baseline and run current checks **before** changes.
2. Present short ADR-001/002/003/004 implementation proposal: code location (separate `apps/fosis-api` or repo), modular monolith module boundaries, database/dialect and tenant isolation. Obtain authorised approval **before** committing irreversible scaffolding or schema changes. If approval is unavailable, create a non-executable ADR proposal and test-plan only; do not silently select a stack.
3. Do not touch production secrets, Ed-Fi schema, OneRoster projections or deployed configuration. Do not copy student data.

## Implementation once approved
- Establish native API module/package layout with domain boundaries for tenancy, identity, school, academic, admissions/enrolment, policy, audit, configuration and integrations; packages must not cross-write one another's state.
- Add health/version endpoint (non-sensitive), OpenAPI schema and consistent structured error contract; validated request context with correlation and immutable event IDs.
- Add deterministic test fixtures for two different school tenants, cross-school guardian, school staff and revoked grants. Provide contract tests for absence of tenant context and denial for wrong tenant.
- Create migration runner, schema lint and reversible expansion/deployment conventions, not an unapproved final domain schema. Define outbox/inbox/idempotency primitives; no live delivery dependencies yet.
- CI: clean install; lint/type/static checks if selected; unit, API contract, negative tenancy, vulnerability and migration smoke tests. Explicitly run existing Ed-Fi OneRoster tests to prove no regression.
- Provide local mock identity implementation limited to non-production and fail-closed authorisation; no hardcoded administrator bypass.
- Instrument structured logs, secret masking, health/latency and safe error handling; all logs omit PII.

## Acceptance criteria / validation
- Fresh checkout can start native app and run CI with documented commands; current Ed-Fi service still builds/tests exactly as baseline.
- Cross-tenant reads/writes and missing auth/tenant claims are denied, with at least two synthetic tenants; no unauthenticated sensitive endpoints.
- Schema/migration test can apply to empty non-prod DB and rollback/forward with documented restrictions.
- API OpenAPI contract, error format, idempotent request IDs, outbox schema/interface and ADR references are documented.
- No manual shell/database bypass or developer-only auth path is enabled in non-dev environments.

## Output
PR containing structure, tooling, synthetic tests, README, API contract, ADR decisions/gaps, CI evidence and explicit issue traceability. Record unresolved approvals and stop work requiring them. Do not mark any epic completed or alter GitHub Project status without verifying approval and the user's project-update rule.