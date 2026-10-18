# BP-04 — Guardian Relationships and Child-Scoped Authorisation
**Read these exact sources:** `docs/fosis-blueprint/01-solution-architecture.md` §§A2, A5–A6, A9, A12; `docs/fosis-blueprint/02-domain-design.md` §§D2 (Epics 1–2), D3, D4.2, D5, D7–D8; `docs/fosis-blueprint/03-data-model.md` §§M1, M3–M5, M7, M10–M11; `docs/fosis-blueprint/04-development-plan.md` §§P3–P5, P9–P11. Issues #2, #45–55; #1 and #82 for Student lifecycle. Requires BP-01/02/03 plus ADR-006 for production Student link.

## Goal
Model and enforce tenant-scoped guardian, custody/responsibility, household and contact relationships without accidental global Person visibility. Begin with Person → Applicant authorised contact links if Student ID issuance is still pending. Do not create actual Student-to-Guardian relationship before that identity legally exists.

## Work
- Provide versioned relationship aggregate with tenant ID, guardian Person/profile, child Student (after ADR) or admissions-side applicant link (prejoining), relationship type, separately verified legal responsibility, access restrictions, provenance, validity and revocation.
- Do **not** automatically equate `Mother`/`Father` to legal access, collection authorisation or emergency-contact ordering. These are separate grants or business facts.
- Create guardian eligibility service: given authenticated actor + tenant + child + action/field/instant, return allowed field list or deny; verify before exposing child data to FoS/Backpack.
- Configurable permitted contact preferences and authorised pickup/emergency fields must follow original issues; leave undecided priority/conflict outcomes pending rather than fabricate.
- Allow one Person linked to children in multiple schools, with separate scoped authorisation. Add minimal API `guardian-relationships` CRUD under review workflow; maintain read and change audit.
- UI for registrar: authorised Person lookup in same tenant, relationship detail and access preview; capture revocation reason.

## Acceptance tests
- Guardian A with child X at tenant A and child Y at tenant B sees only the selected child after relevant tenant context and verification; staff tenant A cannot discover Y.
- Unverified/restricted/revoked guardian cannot read student marks, health, timetable or personal fields solely from kinship; picker/search/list/export enforce same rule.
- Changing responsibility does not rewrite historical access evidence; case history includes actor/time/source.
- Staff cannot silently override restricted-contact flag; ambiguous custody decision is queued for authorised review.
- Negative tests include forged child IDs, sibling inference and cross-tenant ID enumeration; synthetic data only.

## Deliverable
Scoped API/model/UI slice, tests and permission tables, OpenAPI, migration/rollback, audit and unresolved guardian-verification policy note. Do not implement an entirely new parent portal or full self-service Epic 22.