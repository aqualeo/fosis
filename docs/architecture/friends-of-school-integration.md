# Friends of School ↔ FOSIS Integration

## Objective
FOSIS remains the authoritative SIS while Friends of School (FoS), including the Backpack parent experience, consumes and contributes permitted community/parent-facing information through APIs and events.

## Pattern
```mermaid
flowchart LR
 FOS[Friends of School / Backpack] <--> GW[FOSIS API Gateway]
 GW <--> D[FOSIS Domain Services]
 D --> E[Integration Events]
 E --> FOS
```

## Candidate data flows
FOSIS -> FoS:
- School/tenant identity and permitted branding references
- Student/parent linkage needed for authorised parent experiences
- Calendar/timetable/event-relevant information
- Approved communications/report availability signals
- Club/booking eligibility context where required

FoS -> FOSIS:
- Parent-originated actions supported by a FOSIS workflow
- Configured acknowledgements/selections
- Integration events relevant to the authoritative student record

## Rules
- No shared database.
- Stable FOSIS identifiers plus explicit external mappings.
- Parent/student data minimisation.
- Every write-back is validated by FOSIS policy/workflow.
- Delivery channels are school-configurable and FoS/Backpack may be one of several channels.
- Idempotency and event replay handling are required for reliable synchronisation.

## Open design work
The exact endpoint/event catalogue and field-level data contract should be derived from each epic's approved user stories and API requirements.
