# FOSIS Epic Workshop Reconciliation: Epics 6–10

**Date:** 6 October 2026  
**Purpose:** Record workshop decisions and correct epic numbering against the existing FOSIS GitHub issue catalogue. This is a discussion addendum; the epic issues remain the source of truth and should be updated with any approved story changes.

## Source issues

- [Epic 6 — Curriculum, Subjects & Teaching Groups](https://github.com/aqualeo/fosis/issues/6)
- [Epic 7 — Options & Subject Selection](https://github.com/aqualeo/fosis/issues/7)
- [Epic 8 — Timetabling](https://github.com/aqualeo/fosis/issues/8)
- [Epic 9 — Attendance](https://github.com/aqualeo/fosis/issues/9)
- [Epic 10 — Leave & Absence Management](https://github.com/aqualeo/fosis/issues/10)
- [Epic 11 — Assessment Framework](https://github.com/aqualeo/fosis/issues/11)

## Numbering correction

The workshop initially treated teaching groups as Epic 7. The GitHub catalogue places teaching groups under **Epic 6**; **Epic 7 is Options & Subject Selection**. The earlier teaching-group discussion should therefore be reconciled into Epic 6, not recorded as Epic 7 decisions.

Epic 6 already includes group membership, teacher assignment, secondary/support assignments, effective dates, set/band changes, capacity, and historical membership. Workshop decisions to reconcile there include a designated teaching-group leader, role templates for schools of different sizes, HOD review of subject/session requirements, and reasoned approval of changes. The precise boundary between Epic 6 membership and Epic 7 option allocation remains to be mapped so that the two do not duplicate student-to-group assignments.

## Epic 7 — Options & Subject Selection

**Purpose:** Manage student subject choices and allocate them into available options/groups under school-defined eligibility, prerequisites, capacity, approval, and timetable constraints.

The GitHub issue identifies option windows, eligibility, ranked preferences, prerequisites, option blocks, capacity, student/parent submission, approval, allocation, clash detection, alternatives/waitlists, change requests, and finalisation/publication.

**Architecture touchpoint:** Epic 7 consumes the curriculum and subject catalogue from Epic 6 and produces approved student choices/allocations for Epic 6 group membership and Epic 8 timetabling. The data model should preserve submitted preferences separately from the final allocation and retain allocation/change history.

## Epic 8 — Timetabling

**Purpose:** Create and maintain feasible timetables for students, teaching groups, staff, and rooms.

**Agreed direction:** Generate a draft from curriculum/session requirements and group/staff inputs; allow academic review and final school approval according to configured roles; publish a new timetable version rather than silently changing a published one. Timetable generation may suggest and explain options, but authorised staff approve them.

Previously agreed priority order for allocating subjects:
1. Core subjects (non-negotiable).
2. Continuity of prior electives.
3. Core/elective combination suitability.
4. Student choice within availability.
5. Remaining-capacity optimisation.

Mid-term joiners may receive a single elective where a clash prevents a full elective combination. Staff absence/substitution is a separate workflow and should not require republishing the full timetable.

## Epic 9 — Attendance

**Purpose:** Capture daily or session-level attendance quickly and reliably, with school configuration determining the register pattern.

**Agreed direction:** Configure attendance granularity through School Setup/the setup wizard. Primary schools may use one or two registers per day; middle/high schools may take attendance per teaching session. Support the selected school pattern and link session registers to timetable/teaching-group data where applicable. Attendance records capture what actually happened; an approved leave request does not overwrite the recorded attendance.

**Data/integration touchpoint:** Attendance draws its expected student list from active enrolments and, for session registers, the timetable/group allocation. Preserve correction history and who recorded each entry.

## Epic 10 — Leave & Absence Management

**Purpose:** Manage planned leave requests and unplanned absences and link their approved/explained status to attendance.

**Default school workflow:** Parents submit planned leave requests; staff record unplanned absences (for example, illness). Schools may configure an alternative workflow. Preserve request/decision history and route approval according to school settings. Epic 10 supplies the relevant leave/absence context to Epic 9 but does not rewrite actual attendance.

## Cross-epic data flow (provisional)

Academic structure/curriculum and teaching groups (Epic 6) → subject choices and approved allocations (Epic 7) → timetable versions and scheduled sessions (Epic 8) → daily/session attendance (Epic 9). Planned/unplanned leave cases (Epic 10) link to, and inform, attendance without replacing it.

## Next reconciliation

Before drafting the full data model, confirm the ownership and lifecycle of student-to-teaching-group membership across Epics 6–8, and reconcile the workshop notes against the detailed GitHub stories. Epic 11 is **Assessment Framework**; it is the next epic to review after this correction.
