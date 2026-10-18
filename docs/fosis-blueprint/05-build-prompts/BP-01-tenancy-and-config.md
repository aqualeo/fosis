# BP-01 — Multi-Tenancy and Configuration Registry
**Mandatory references:** `docs/fosis-blueprint/01-solution-architecture.md` §§A3–A5, A7, A9, A12; `docs/fosis-blueprint/02-domain-design.md` §§D1–D3, D4.7, D7; `docs/fosis-blueprint/03-data-model.md` §§M1, M3–M5, M8–M10; `docs/fosis-blueprint/04-development-plan.md` §§P3–P5, P9–P10. Issues #31, #198, #199, #249. This is a **subfeature**, not closure of either epic.

## Objective and prerequisites
Implement tenant onboarding boundaries plus platform-vs-school configuration, lock evaluation, versioned values and impact-aware edits. BP-00 baseline and approved ADR-001–004 required. Issue #249 has no formal epic number yet; do not invent one.

## Requirements / design
- Tenant identity, jurisdiction, region policy, timezone, school active/suspended marker and independent branding/configuration. Deny missing/inactive/wrong tenant context.
- Platform configuration owner defines setting key, owning functional epic, type, default, admissible constraints, `school_override_allowed`, hard lock, version and effective period. The **registry is centrally governed**.
- School override exists only for school-editable settings. Compute effective value by platform lock → approved current school override → platform default. Never weaken platform security, privacy or safeguarding via school settings. Validate server-side and audit both allowed changes and rejected override attempts where appropriate.
- Simple School Settings UI/API organised by owning epic; display inherited vs local value, lock reason, permitted range, effective date and history. Diff/impact preview for consequential changes; no silent mutation of already published grade/timetable/academic records.
- Enforce tenant-scoped FK/unique and database/access policy chosen by ADR-004. Tenant administrators must not enumerate another school or edit global registry. Platform operations require distinct elevated principal and separate audit.
- Model catalogue can have a *catalogue schema and admin-only API* but **do not integrate paid/unapproved model endpoints** until AI governance/residency details are approved.

## Acceptance
- Two synthetic tenants can set distinct allowed school values without cross-reading them.
- Global default inheritance works; hard-locked config cannot be overridden; default update never silently changes audited published records.
- Version history/provenance of actor and setting is preserved and time-based lookup deterministically returns applicable value.
- School admin cannot publish global settings; platform admin cannot silently change school settings bypassing tenant audit.
- REST contract, migration, rollback/test fixture, policy negative tests, version conflict test and impact preview documented.

## Output
Reviewed PR containing migrations, policy tests, settings registry/evaluator/API/basic UI, OpenAPI, docs and traceability. Block any incomplete jurisdiction rule rather than fabricate defaults. Preserve the existing OneRoster component.