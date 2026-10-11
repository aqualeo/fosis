# FOSIS Enterprise Architecture

## Purpose
FOSIS is an AI-first, multi-tenant Student Information System supporting nursery through Year 13 and multiple curricula within a school.

## Logical layers
1. Experience: school admin, staff/teacher, parent/guardian, student where permitted, Friends of School/Backpack and external applications.
2. API and integration: authenticated REST APIs, OneRoster 1.2 interfaces, events/webhooks and integration adapters.
3. Domain services: identity/person, admissions and enrolment, curriculum, subjects/groups, timetabling, attendance/leave, assessment/reporting, pastoral/safeguarding/SEND, medical, facilities, library integration, finance-related workflows and marketplace.
4. Workflow and policy: configurable approvals, SLAs, task pools, notifications and school-level policy.
5. AI services: recommendations, summaries, classification/reason coding and optimisation. AI proposes; authorised users approve where decisions require human authority.
6. Data: relational system of record, tenant isolation, audit history, document references and integration metadata.
7. Platform: authentication, authorisation, observability, configuration, secrets, deployment and resilience.

## Core boundaries
Person is distinct from Student. Parent/guardian is represented separately and related to the student. Enrolment is annual and lifecycle-based. Curriculum, year, form and teaching-group structures are versionable/configurable.

## Multi-tenancy
Every school is a tenant boundary. Group-level configuration may provide templates that schools clone/customise. Tenant context must be enforced at API and persistence layers.

## Standards
OneRoster 1.2 alignment is retained for interoperable roster/exchange use cases. Internal domain modelling is not constrained to OneRoster where FOSIS requirements are richer.

## Related documents
See integration-architecture.md, logical-data-model.md, security-rbac-architecture.md and ai-architecture.md.
