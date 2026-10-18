# BP-06 — Admissions to Confirmed Pre-Joining Enrolment (No Arrival Activation)
**Required references:** `docs/fosis-blueprint/01-solution-architecture.md` §§A2–A5, A7, A9, A12 (ADR-006); `docs/fosis-blueprint/02-domain-design.md` §§D2 (Epics 3,5), D3–D4.1, D7–D8; `docs/fosis-blueprint/03-data-model.md` §§M1–M6, M8–M11; `docs/fosis-blueprint/04-development-plan.md` §§P3–P5, P9–P11. Issues #3, #56–63; #5, #82–83. Prerequisites BP-00/01/02/03/05 and verified guardian applicant contact (BP-04).

## Goal and hard boundary
Implement **only** applicant → accepted offer → pending enrolment → confirmation / awaiting joining, including enrolment ID and prerequisites. Per #82, permanent Student ID must **not** be issued on offer acceptance or enrolment confirmation. The conflicting Epic 1 #33 is ADR-006 and MUST be formally reconciled before extending this prompt into joining/arrival.

## Work
- Admissions-side applicant/Application aggregate with school, intended programme/year, pre-joining Person + verified guardian contacts, offer reference and audit. Detect duplicates only within authorised identity-resolution policy; never auto-merge.
- On accepted offer, create a unique Enrolment ID and a **Pending** prejoining enrolment with academic-year and school tenant reference; Student ID nullable/not minted.
- Configurable school-specific prerequisites: required documents, terms/consent, fee confirmation/reference and other approved checks; support incomplete/action required, validated proof and provenance. Do not make finance calculations inside enrolment domain.
- Registrar-controlled confirm only after mandatory checks; persist **Confirmed/AwaitingJoining** state and approval record. Failure returns actionable information, not a hidden partial commit.
- Withdrawn before arrival preserves all authoritative records and leaves Student ID unissued. Audit lifecycle, prerequisites, approval and decision reason. Outbox signal for later notifications without assuming those channels are already online.
- Permission matrices for registrar/school admin, not blanket staff; domain API/CLI admin minimal UI with prereq status and safe reviewer actions.

## Acceptance
- Accepted offer without all required prerequisites remains pending, with outstanding evidence displayed; no bypass without approved workflow.
- Confirmation can succeed exactly once and is idempotent under duplicate requests; unique enrollment ID remains durable.
- No path issues Student ID or sets Active before physical-arrival confirmation and ADR-006 resolution.
- Two tenants cannot access or write each other's application/enrolment, student/guardian contacts or prerequisites.
- WithdrawnBeforeJoining retains approval and reference history; transaction audit and event outbox reconcile after simulated downstream failure.
- Test fee dependency as *verified external/finance outcome*, not a hard-coded payments integration.

## Deliverable
API/domain workflow, migration, synthetic UI/testing, prejoining state diagram, outbox and security evidence. Stop before Student arrival activation, class placement, timetable publication or live payment capture.