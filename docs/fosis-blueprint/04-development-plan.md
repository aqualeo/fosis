# FOSIS — Whole-Project Development and Delivery Plan (Draft v1.0)
**Status:** Planning baseline, not a committed schedule/budget. Covers Epics **1–41** plus the unnumbered Platform Configuration Management epic (#249). **Inputs:** [Architecture](01-solution-architecture.md), [Domain design](02-domain-design.md), [Data model](03-data-model.md), GitHub issues as of October 2026.

## P1. Delivery objective and strategy
Deliver an operable privacy-first, AI-assisted SIS with a secure vertical slice of tenancy → verified identities → academic setup → enrolment → teaching → operational attendance → assessed learning → reporting, then extend to specialist, partner and commercial domains. Use **rolling-wave elaboration**: build only slices that satisfy Definition of Ready (DoR), while analysts complete remaining user stories. Do not equate “issue created” or “epic closed for requirements” with implemented, tested, released capability. The new FOSIS platform must not be confused with the existing Ed-Fi OneRoster bridge at the repository root.

**Technical baseline is provisional:** native modular monolith, relational tenant-aware system of record, API-first, approved model gateway, async outbox; confirm ADRs in [A3](01-solution-architecture.md) before production coding.

## P2. Prioritisation labels and criteria
- **R0 — Infrastructure/build-ready slice:** architecture contract and robust generic scaffolding are sufficiently bounded; no unresolved product policy is silently implemented. Examples BP-00 to BP-03.
- **R1 — Conditional slice:** implement only the specifically approved low-risk domain subfeatures with explicit decision gates; not the whole epic (BP-04 to BP-07).
- **R2 — Needs refinement:** scope and stories exist but cross-epic state/authority/exception/integration details still require completion.
- **R3 — Later:** roadmap coverage established; detailed story/UI/security contracts needed before build.

At every feature PR, recheck GitHub issue body for newer acceptance criteria and store a requirements traceability reference in code/tests.

## P3. Project-wide epic delivery map
| Epic | Capability | Planned wave | Entry/readiness status and governing dependency |
|---|---|---|---|
| 1 | Student Master | W1 | R1; core person/identifier groundwork now; Student creation blocked by ADR-006 |
| 2 | Guardian/Household | W1 | R1; verified relationships and denial scopes, cross-tenant privacy |
| 3 | Admissions | W2 | R2; applications/offers, duplicate and prejoin policies |
| 4 | Academic Structure | W1 | R1; academic calendar/school structure now, advanced rollover later |
| 5 | Annual Enrolment/Lifecycle | W2 | R1 prejoin framework; arrival/activation blocked by ADR-006 and downstream contracts |
| 6 | Curriculum/Teaching Groups | W2 | R2 except approved subject-teaching-time #109; full decomposition and versioning |
| 7 | Options/Subject Choice | W3 | R2/R3; needs detailed acceptance, allocation and approvals |
| 8 | Timetabling | W3 | R2; requirements documented, but needs 4/6/7/19/20 data and optimiser design |
| 9 | Attendance | W4 | R2; calendar+enrolment+published timetable+leave, correction policy gaps |
| 10 | Leave | W4 | R2; approval workflows and evidence, coordinates with 9/30 |
| 11 | Assessment | W5 | R2; Epic 11 index outdated; template/marks/AI scope needs 12 reconciliation |
| 12 | Gradebook | W5 | R2; single evidence/release authority shared with 11 |
| 13 | Reports & Report Cards | W6 | R2; depends on approved attendance/results and publishing policy |
| 14 | Behaviour/Pastoral | W6 | R2; sensitive record segregation, role policies |
| 15 | Safeguarding | W6 | R2/high risk; restricted security and incident workflows before implementation |
| 16 | Learning Support | W6 | R2; neutral terminology, controlled support plans |
| 17 | Health/Medical | W6 | R2/high risk; medical need-to-know, consent and emergency action |
| 18 | Student Documents | W2 | R2; required for enrolment, scoped document storage first |
| 19 | Staff/Teacher | W1–W3 | R2; minimum operational worker/assignment before timetable |
| 20 | Rooms/Resources | W2–W3 | R2; room capacity/availability before timetabling |
| 21 | Communication/Notification | W2–W4 | R2; outbox and delivery policy before parent launch |
| 22 | Parent/Student Self-Service | W3–W6 | R2; Backpack integration, scoped child access and consent |
| 23 | Import/Migration | W2–W4 | R2; early controlled imports, mature onboarding later |
| 24 | Integrations/API | W0 onward | R1 basic contracts; R2 partner APIs/OneRoster native mapping |
| 25 | Identity/RBAC | W0–W1 | R0 generic policy skeleton; RBAC per epic before live feature |
| 26 | Audit/Privacy/Governance | W0 onward | R0 audit metadata; R2 legal class retention/DSAR decisions |
| 27 | Reporting/Analytics/Regulatory | W7 | R2; source data completeness and law/rules |
| 28 | AI Intelligence | W4–W7 | R2; AI only after privacy gateway and human-review patterns |
| 29 | AI Privacy Gateway | W2 then all AI | R1 architectural skeleton; residency/provider rules must be approved |
| 30 | Workflow/Automation | W1–W3 | R1 basic auditable approval engine; domain definitions later |
| 31 | Multi-tenancy/School Settings | W0–W1 | R0 core tenant/settings registry; parent epic currently closed for requirements despite extension |
| 32 | Reliability/Observability | W0 onward | R0 telemetry/test harness; SLOs and recovery targets to approve |
| 33 | Marketplace/Extensions | W8 | R3; security scopes and billing |
| 34 | Fees/Finance | W3–W5 | R2; early enrolment fee consequence events, finance rules later |
| 35 | On-site Services | W8 | R3; optional order/entitlement modules |
| 36 | Transport | W7 | R2; external provider receives/authoritative status only |
| 37 | Trips/Off-site | W7 | R2; consent, health alerts, staff duty |
| 38 | Learning Resources | W7 | R2; digital references and physical issue/return |
| 39 | Portfolio/Awards | W7 | R2; approved awards and selective FoS exposure |
| 40 | Library | W8 | R3; licence/security feasibility and connector first |
| 41 | Staff CPD | W8 | R2/R3; credential verification and reminders |
| Unnumbered #249 | Platform Configuration | W0–W1 | R1; approved global defaults, model catalogue and lock governance; assign formal epic number |

**W0–W8 are delivery dependency waves, not strictly sequential calendar phases.** Platform and security remain active across all waves.

## P4. Phases, demonstrable outputs and gates
| Wave | Target outcome | Deliverable evidence | Gate to advance |
|---|---|---|---|
| W0 Engineering foundation | Native SIS skeleton, CI, non-prod isolated environments, contracts, logging, multi-tenant isolation test harness | Architecture ADRs reviewed; secure API endpoint, dual-tenant negative tests, migration/rollback demo | No untrusted tenant header, no student PII, no existing OneRoster test regressions |
| W1 School/person setup | Onboard school, configure allowed settings, authenticate staff, create/read global Person mapping in authorised context, Guardian linkage, calendars | Synthetic walkthrough school A/B, guardian with child in two schools without cross-leakage | ADR-006 resolved before Student ID issuance; admin/guardian least privilege |
| W2 Admissions/prejoin and master setup | Applications and pending/confirmed enrolments, documents, staff/rooms, curriculum metadata, safe imports | Accepted application to confirmed-awaiting-joining, audit and prerequisite checks | No activated student/timetable without arrival workflow and approved contracts |
| W3 Student activation and academic operations | Arrival event, option allocation, timetable draft/approval, relevant integrations/notifications | End-to-end enrolment activation to approved staff timetable with compensations | Hard timetable clashes blocked, signed-off versioned approvals |
| W4 Daily operations | Attendance/leave workflow, parent alerts, operational actions | Timetable-based register, holiday suppression, human leave approval and correct prefill | Attendance correction and partial-day policy confirmed, no silent submitted register rewrite |
| W5 Assessment and gradebook | Templates, marking, moderation, scheduled results publication, approved AI marking | One class completes assessment → teacher/HOD review → scheduled parent release | Epic 11/12 ownership reconciled, no AI guess, publish rights tests |
| W6 Reports and student support | Approved reports, pastoral, safeguarding, learning support, medical | Restricted specialist journeys and parent-safe reports | Privacy/safeguarding regulatory review and sensitive-access audit |
| W7 Ecosystem and insight | BI/regulatory, transport, trips, materials, verified portfolio | Partner simulations, scoped FoS export, school analytics | Signed integrations and source ownership contract tests |
| W8 Commercial/operations expansion | Marketplace, finance depth, services, library, CPD; rollout to additional regions | Vendor approval, billing/reconciliation, OSS library evaluation, CPD workflows | Operational SLOs, licensing, accessibility, privacy approvals and pilot sign-off |

## P5. First build packages and issue traceability
| Prompt | What can be started safely | Stories and limits |
|---|---|---|
| BP-00 | Native SIS repo boundary, CI, test harness, contract/migration conventions | Cross-cutting 24,25,26,31,32; **do not rewrite existing OneRoster component** |
| BP-01 | Tenant context + school/platform configuration registry and locked default evaluation | #31, #198, #199, #249; UI later where policy undefined |
| BP-02 | Authenticated request context, role/permission enforcement, audit foundation | #25, #175, #176, #26, #179; no unreviewed specialist grants |
| BP-03 | Global Person + tenant Person profile + student identity interface (ID issue behind gate) | #1 #33–40, #2 #45–48; **no premature Student ID** |
| BP-04 | Verified guardian relationship and restricted child-scoped read model | #2 #45–55; require parent verification policy; no cross-school discovery |
| BP-05 | School structure, multi-calendar/academic periods; timetable-ready reference API | #4 #65–73; no full rollover without further decisions |
| BP-06 | Admissions/prejoining Enrolment ID, prerequisites and `Awaiting Joining` workflow | #3, #5, #82–83; **do not implement arrival activation** before ADR-006 |
| BP-07 | Approved curriculum subject/time-allocation data contract, provenance and safe tooling | #6, #109, #8 dependency; **not full Epic 6, options allocation or timetabling** |

Prompts live in [05-build-prompts/](05-build-prompts/). Each prompt names **exact** architecture, design and data-model paths and a fail-closed preflight.

## P6. Suggested parallel workstreams
- **A — Platform/security:** tenant onboarding, global/school configuration, SSO, RBAC, audit, privacy gateway, reliable jobs.
- **B — Identity and school master:** Person/Guardian, calendars, Student Master, admissions/prejoin/enrolment.
- **C — Academic operations:** curriculum/subjects, room/staff, options, timetable, attendance, leave.
- **D — Learning and school support:** assessment/gradebook, reports, safeguarding/health/support, self-service.
- **E — Partner/marketplace:** integration adapters, FoS, library/transport/finance, data exports, marketplace; this runs later with architecture scaffolding early.

With 2–6 developers as previously discussed, **do not run five independent implementation teams immediately**. Start one platform-oriented vertical team and add a second functional stream when automated isolation/security tests are stable. Appoint named product owner/education SME, architecture lead, QA automation lead, privacy/security reviewer and delivery owner; one person may cover multiple roles in early stages, but independent security approval should remain explicit.

## P7. Planning scenarios (non-committal)
A basic identity/academic/enrolment pilot is a *different scope* from delivering all 41 production-grade epics. Initial foundation and limited school-master demonstration may take multiple 2-week iterations **after** staffing/ADRs/environment are approved. A broad production SIS covering all 41 epics, three jurisdictions, specialist healthcare/safeguarding, integrations, marketplace and full security/compliance should be planned as a **multi-year programme**, with delivery dates governed by team capability and third-party contracts, not inferred from epic counts. Avoid publishing an 18-month finish date without bottom-up story estimation and pilot validation.

Suggested planning cadence: two-week development iterations; monthly technical-risk review; six-week product/release demo; quarterly product/school pilot decision and reforecast. Estimate per *vertical end-to-end story* including UX, API, persistence, policy, tests, migration and deployment rather than per CRUD screen.

## P8. Backlog and dependency management
Maintain explicit requirements statuses `Concept → Eliciting → Specified → Reviewed → ReadyForBuild → InBuild → Tested → Piloted → Released` **separately** from GitHub issue Open/Closed and Epic Completed for requirements. Record dependencies as `blocked by` relationships, not only textual links. For each epic track: approved stories/total, acceptance completeness, UI designs, domain contract, data schema, RBAC, dependencies, ADR closure, test coverage and pilot acceptance. Populate GitHub Project fields if and only if API access/permissions are verified; don't claim status sync before actual update.

**User's workflow rule:** *When an epic is completed, update the relevant GitHub Project before proceeding to the next epic.* The current documents do **not** close or complete any epic. Before marking an epic decomposition complete, update GitHub issues and the Project board and verify placement/status.

## P9. Definition of Ready (DoR)
- Approved, traceable story and acceptance tests; dependencies and out-of-scope explicit; user journeys, errors and rollback/retry.
- Architecture/Design/Data Model latest versions linked in issue and build prompt; ADRs signed off where blocking.
- Data fields/classification, migrations, tenant/RBAC field constraints and consent/legal basis reviewed.
- Cross-epic API/event contracts and ownership agreed; security and failure-mode testing planned.
- Story split small enough for testable demonstration without inventing open domain policy.

## P10. Definition of Done (DoD)
- Code/config committed with review; security/architecture/test checks pass; module API and migrations documented.
- Unit, integration, dual-tenant denial, privilege escalation, negative-state, race/idempotency, audit, accessibility and baseline performance tests pass as applicable.
- Traceability from GitHub issue → code → tests → demo evidence; changes recorded in related docs and ADRs.
- Synthetic UAT accepted by authorised product owner; release flags/rollback/recovery and monitoring demonstrated; no premature user-facing publication.
- Existing Ed-Fi/OneRoster build and integration tests remain passing if touched. **“Requirements completed” does not mean build completed.**

## P11. Specific open decisions and risks to resolve early
1. **ADR-006:** Student ID before or on arrival (#33 vs #82/#83); highest-priority domain blocker.
2. **ADR-001–004:** modular-monolith approach, native SIS folder/repo, Postgres/SQL model, tenant/RLS isolation.
3. Legal/privacy: region-specific residency, retention by record type, consent and safeguarding data boundaries.
4. Global identity + FoS FOS ID life-cycle, identity reconciliation, parent access across schools.
5. UI, document storage, IAM/SSO, background worker/queue and infrastructure vendor selection.
6. Inconsistent epic indexes and cross-epic overlaps: Epic 11 #247/#248 vs Epic 12, Epic 31 newly expanded while parent closed, #249 missing epic number.
7. Attendance correction exceptions and register partial-day handling, curriculum/option allocations and future external integration contracts.
8. Existing Ed-Fi adapter's OneRoster IDs should not be assumed to map one-to-one to a new native SIS Student ID; mapping/collision testing essential.
9. Capacity/response SLO and DR RPO/RTO targets, third-party SLA and estimated workload still need measurement and approval.
10. No real student data or production keys in demo, prompt/tool output or GitHub issues.
