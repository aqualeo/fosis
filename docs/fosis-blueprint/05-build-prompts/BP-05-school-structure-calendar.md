# BP-05 — Academic Structure and Multiple Calendars
**Mandatory reading:** `docs/fosis-blueprint/01-solution-architecture.md` §§A2–A5, A7, A9, A12; `docs/fosis-blueprint/02-domain-design.md` §§D1–D3, D4.3, D5–D7; `docs/fosis-blueprint/03-data-model.md` §§M1–M3, M5–M6, M8–M10; `docs/fosis-blueprint/04-development-plan.md` §§P3–P5, P9–P11. Epic #4 and issues #65–73; Epic 31 configuration. Requires BP-00/01/02.

## Goal
Create the school/campus, curriculum-neutral academic structural hierarchy and effective-dated calendar service consumed later by enrolment, attendance, timetable and reporting. The system MUST support **more than one academic calendar per school**, never one universal calendar.

## Scope
- Tenant-scoped campus/site, phase/division, year group, form/class, house, academic year, calendar version, period and period applicability to campus/cohort/curriculum.
- Teaching versus non-instructional periods (holiday, closure, staff-only/inset, exam) and nested terms/semesters with configurable labels; date/time semantics use school's configured IANA timezone.
- Calendar/version history: edits of published periods preserve previous effective records and do not retrospectively alter attendance/report history.
- APIs to resolve calendar for given school/student placement, date, phase and year group. For pre-enrolment, resolve candidate calendar based on programme/placement (not speculative Student creation).
- Tenant admin UI to create/edit/preview calendar and structural rules; display effective date, version, source and consequent impact before publishing.
- Provide school-specific capacity metadata for forms but avoid automated balancing, teacher allocation, complex rollover and government holiday subscription until appropriate dependencies/decision reviews.

## Acceptance
- One school can publish separate programmes/campus calendars, sharing periods where explicitly configured; no duplication forced.
- Holiday/closure is distinguishable from a teaching date, and downstream simulation expects no attendance register on noninstructional day.
- Cross-tenant structure lookup denied; admin-edit permissions separate from read; published history immutable/auditable.
- Invalid overlapping periods or ambiguous applicability surface validation rather than silent selection. Rule resolution documented.
- Test boundary dates, school timezone, multiple curricula/campuses and effective version updates.

## Deliverable
Academic reference schema/services/API/limited admin UI, contract tests consumed by Epics 5/8/9, versioning, audit/permission tests and issue traceability. Do not implement timetable scheduling or promotion in this prompt.