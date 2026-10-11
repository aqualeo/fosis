# FOSIS Security and RBAC Architecture

## Security model
Authorisation combines tenant boundary, role, domain, record sensitivity and workflow authority.

## Roles already identified
School Admin, Finance Reviewer, Principal Approver, Teacher/HOD or Teacher Lead, Secondary Lead, External Provider, Parent/Guardian, Student where permitted, Super User and specialist roles such as nurse/doctor. Exact RBAC matrices remain domain-specific and must be completed before implementation.

## High-sensitivity controls
- Safeguarding: confidential, need-to-know access with audit trail.
- Break-glass: emergency access only for the highest authorised role, fully audited.
- Medical: restricted visibility; emergency medication administration by nurse/doctor with recorded parent notification after emergency use.
- History: role-restricted visibility.

## Approval controls
Two-person/higher-authority approval is required for agreed high-impact cases such as certain large-school workflows, disciplinary withdrawal and specified exceptional changes.

## Technical controls
- Tenant context on every authenticated request.
- Least-privilege scopes for APIs/plugins.
- Row-level/record-level enforcement in the physical data layer where appropriate.
- Immutable or tamper-evident audit history for sensitive actions.
- Secrets separated from source/configuration.
- Authentication and session controls integrated with existing auth design.

## TBD
A complete permission matrix by epic/action remains to be finalised; this document intentionally does not invent permissions not yet agreed.
