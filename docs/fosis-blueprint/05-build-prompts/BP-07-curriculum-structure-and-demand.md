# BP-07 — Curriculum Reference Catalogue and Teaching Demand Contract
**Read in full:** `docs/fosis-blueprint/01-solution-architecture.md` §§A2–A5, A7, A9, A12; `docs/fosis-blueprint/02-domain-design.md` §§D2 (Epics 6,7,8), D3, D4.3, D6–D8; `docs/fosis-blueprint/03-data-model.md` §§M1–M3, M5, M8–M11; `docs/fosis-blueprint/04-development-plan.md` §§P2–P5, P9–P11. Issues #6, #109, #8, #106, #7; dependencies BP-00/01/02/05. This is a **conditional subset**, not full Epic 6 or Epic 7.

## Goal
Implement a versioned curriculum/subject/learning-objective reference catalogue and an **authoritative teaching-time requirement** interface to support future timetabling, without inventing selection/placement logic.

## Scope
- Curriculum and version, key stage/phase/year group, subject and subject offering categories core/mandatory, optional, elective; school/academic effective periods. Subjects may span year groups; do not encode 'learning objective' as a subject category.
- LearningObjective as curriculum-owned record; allow authorised assessment mapping later but do **not** create marks/assessment flows.
- TeachingTimeRequirement per curriculum/year/subject/session cycle with source/provenance of current curriculum, approved school template, previous year or manual HOD/admin. Unit may be hours or periods; derived conversion uses published bell-period duration from Epic 8 #96, otherwise mark calculation pending.
- Current authoritative curriculum requirement takes precedence over earlier carry-forward, with impact/diff rather than automatic overwrite. **Do not generate invented hour recommendations.**
- Mandatory subject tolerance defaults to **±10%** as explicitly agreed in #109 and is configurable through Epic 31 settings. Outside tolerance requires HOD review; optional/elective not subjected to the mandatory tolerance unless configured.
- HOD/admin view with version, provenance, effective period, variance and confirmation. Expose `TeachingDemandConfirmed` data contract consumed by future timetable planner; no timetable algorithm or subject-choice allocation.
- Group structure may be represented in schema/contract after approval; do not auto-create teacher assignments or full teaching groups before explicit design.

## Acceptance
- Source and approved version visible on each requirement; older approved values preserved.
- Calculation reflects period duration and surfaces fractional variance rather than silently rounding outside tolerance.
- School A and B maintain independent curriculum customisation without affecting global template or each other.
- HOD approval required for configured mandatory variance breaches; no curriculum change auto-mutates an approved school version.
- API contract consumed in a synthetic timetabling-demand test with no accidental timetable writes; RBAC, audit, migrations and policy negative tests pass.

## Stop/Deliverable
If exact curriculum/source/mapping semantics or stakeholder approval is unresolved, deliver stable reference schema/contract and test fixtures only; log open question in GitHub issue and do not implement workflow-dependent selection. Reviewed PR with lineage, approvals where agreed, versioning tests and a formal list of deferred Epic 6/7 features.