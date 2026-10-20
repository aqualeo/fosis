# FOSIS — Architecture, Data and Development Blueprint (v1.0 draft)

**Project:** [aqualeo/fosis](https://github.com/aqualeo/fosis). **Coverage:** Product Epics 1–41 and [Platform Configuration Management #249](https://github.com/aqualeo/fosis/issues/249). **Date:** October 2026. **Status:** Draft for technical/product approval, not an automatically authorised implementation.

## Core documents
1. [01 — Whole-Platform Solution Architecture](01-solution-architecture.md): capabilities, boundaries, target architecture, security, AI, deployment and ADR gates.
2. [02 — Detailed Domain/Application Design](02-domain-design.md): ownership and design for all 41 epics, shared workflows, API and event contracts, UI/RBAC patterns and cross-epic invariants.
3. [03 — Conceptual, Logical and Proposed Physical Data Model](03-data-model.md): domain entities for 41 epics, ER relationship view, cross-tenant keys/RLS design proposal and draft PostgreSQL examples.
4. [04 — Whole-Project Development Plan](04-development-plan.md): wave/dependency plan, requirements/build readiness, risk, team/cadence assumptions and DoR/DoD.
5. [05 — Individual Build Prompts](05-build-prompts/README.md): eight bounded build-ready/conditionally ready packages BP-00 to BP-07, each with direct reference to Architecture, Design, Data Model and Development Plan.
6. [06 — Epic 15 Safeguarding Decision Baseline (20 October 2026)](06-epic-15-safeguarding-decision-baseline-2026-10-20.md): consolidated approved safeguarding discussions, case/role/incident/asset workflows, acceptance-test baseline and open implementation gates. The epic's GitHub issues remain canonical.

## How to use this set
- Read **all three primary documents** before planning a build prompt. Then consult relevant GitHub story/epic issues; issue acceptance criteria remain canonical where design documents are more general.
- Confirm blocking Architecture Decision Records in [A3](01-solution-architecture.md) with the product/architecture owners, especially **ADR-006 on Student ID creation**. Do not silently infer a missing rule.
- Implementation priority: BP-00 → BP-01/BP-02 → BP-03 → BP-04/BP-05 → BP-06 and BP-07 where dependencies and gates are signed off.
- Each completed build produces tests, traceable PR, reviewed migrations and API contract, updated documentation and demo evidence.
- This blueprint did not change any GitHub issue's status or GitHub Project board. User rule remains: **on completing an epic's requirements, update the relevant GitHub Project before proceeding**.

## Existing repository baseline that must be preserved
This repository currently contains the upstream-derived **Ed-Fi ODS → OneRoster 1.2 Node.js/Express adapter** at its root (per `README.md` and `package.json`). Its Ed-Fi ODS is authoritative for *that adapter*. It is **not** evidence that a native FOSIS SIS database, workflow engine, student master and RBAC already exist. Existing FOSIS baseline design is in [../architecture/](../architecture/README.md); it is retained. A native SIS application boundary is proposed but not yet approved.

## Explicit limitations
This is a **whole-project design baseline**, not 41 finished low-level design specifications. Later epic details and country-specific regulations must be elaborated during rolling-wave development. Physical schema examples are *design sketches*, not approved migrations. Technical stack/hosting and authorisation matrix require owner approval. No code was built or deployed by creation of these documents.
