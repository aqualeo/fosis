# FOSIS — Build Prompt Index

Every build prompt MUST consult these documents and the relevant GitHub issues:
- [Whole-Platform Solution Architecture](../01-solution-architecture.md)
- [Detailed Domain/Application Design](../02-domain-design.md)
- [Logical/Proposed Physical Data Model](../03-data-model.md)
- [Whole-Project Development Plan](../04-development-plan.md)

**Readiness definitions:** R0 = safe foundations after ADR/tech approvals; R1 = approved low-risk sub-slice with explicit domain stop gates; R2 = needs more detailed acceptance/ownership decisions. **None** of these prompts should be treated as blanket authorisation to implement an entire unfinished epic.

| Prompt | Order | Initial scope | Readiness | Blocking cautions |
|---|---|---|---|---|
| [BP-00](BP-00-engineering-foundation.md) | 1 | Native engineering foundation | R0 | Architecture placement, stack, DB and isolation ADRs to approve |
| [BP-01](BP-01-tenancy-and-config.md) | 2 | Tenant + global/school settings | R0/R1 | Need approved physical tenant isolation and lock rules |
| [BP-02](BP-02-authz-audit.md) | 2 | Scoped RBAC and audit | R0 | Identity provider/MFA/permissions require design controls |
| [BP-03](BP-03-person-and-student-identity.md) | 3 | Person and tenant profile, gated Student identity | R1 | ADR-006 conflict: Student ID creation timing |
| [BP-04](BP-04-guardian-links-and-access.md) | 4 | Guardian verification/child scope | R1 | Person → Student link after approved Student creation |
| [BP-05](BP-05-school-structure-calendar.md) | 4 | School structure and multi-calendar | R1 | Advanced capacity/rollover/timetable deferred |
| [BP-06](BP-06-admission-prejoin-enrolment.md) | 5 | Accepted application → confirmed-awaiting-joining | R1 | **No physical arrival activation** until ADR-006 |
| [BP-07](BP-07-curriculum-structure-and-demand.md) | 5 | Versioned curriculum catalogue and #109 teaching time | R1 | Not whole Epic 6, options allocation or timetable |

**Stop rules:** If a prompt conflicts with an approved GitHub story, the story takes precedence and the code author raises a proposed design revision. Missing business decisions are escalated instead of invented. Every change must preserve the existing Ed-Fi OneRoster tests and pass tenant/permission negative tests. No real student data or production secrets in prompts, test fixtures or logs.

**Use:** Copy one prompt into your coding agent with repo access. Require it to report preflight findings, propose blocked ADRs, identify the intended PR scope, implement only authorised changes, run tests, and provide a changed-file/test/issue matrix. Do not mark an epic complete because its first build prompt passed.
