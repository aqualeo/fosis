# FOSIS — Document Access Control Matrix (Draft v0.1)

**Status:** Working RBAC/ABAC baseline for user review, 21 October 2026. **Applies to:** Student and school documents. **Related:** Epic 18, Epic 26, domain-specific medical/inclusion/safeguarding rules. This is a proposed default matrix, not approval of every cell.

## Permission vocabulary
V=view metadata/content where authorised; U=upload/create; R=replace/update by new version; A=administrative intake/legibility acceptance; S=specialist substantive verification; X=download/export/share subject to separate export approval; D=request disposal (not hard delete); —=denied by default. No role automatically receives all permissions. All permissions require tenant and relevant student/case/workflow relationship, document type, classification and purpose checks.

## Proposed student-document matrix
| Document type | Parent/guardian (own child) | Admissions/registrar | General school admin | Health team | Inclusion/SEN | Safeguarding/pastoral case team | Finance |
|---|---|---|---|---|---|---|---|
| Identity/legal (passport, visa, ID) | V,U,R; X permitted | V,U,R,A,X | Assigned intake V,U,R,A | — unless justified | — | — | — |
| Admission/prior-school | V,U,R for parent-visible records | V,U,R,A,X | Assigned intake V,U,R,A | — | Case-specific V | — | — |
| School-issued official certificates | V,X once published | V,U,R,A,X | Authorised issuer V,U,R,A,X | — | — | — | — |
| Consent evidence | V,U where appropriate | Programme-specific V | Programme-specific V,A | Programme-specific V,S | Programme-specific V,S | Case-specific | — |
| Medical/health | Permitted V,U,R; restricted case exceptions | Intake metadata only if assigned | Intake metadata/legibility only if authorised | Case-scoped V,U,R,S,X | — unless explicitly permitted | Emergency/case-specific only | — |
| Inclusion/SEN | Permitted V,U,R; restricted case exceptions | Intake metadata only if assigned | Intake metadata/legibility only if authorised | Case-specific only | Case-scoped V,U,R,S,X | Case-specific only | — |
| Safeguarding/confidential pastoral | No default visibility; case/legal policy | — | No default content access | Explicit case access only | Explicit case access only | Case-scoped V,U,R,S,X | — |
| Financial/administrative evidence | Permitted V,U,R for own child | Assigned admission-finance subset | Assigned intake subset | — | — | — | Assigned case V,U,R,A,X |

**Important:** School administrative legibility checks must not automatically disclose sensitive document contents. For sensitive uploads, a domain-authorised intake reviewer or a safe redacted/metadata workflow must be used. Critical specialist notifications occur immediately even if intake is pending. Specialist verification remains separate from official receipt.

## School-level documents — proposed baseline
| Classification | School admin | Principal/delegate | Teacher/staff | Finance | Safeguarding lead |
|---|---|---|---|---|---|
| Public school policies / parent publications | V,U,R if document owner; publish permission separately | V,approve publication | V | V | V |
| Internal operational documents | Assigned V,U,R | Assigned V,U,R,approve | Need-to-know V | Need-to-know V | Need-to-know V |
| HR/personnel evidence | Assigned HR role only (not general admin) | Authorised HR scope | Own records as policy permits | Payroll subset only | — |
| Finance/contracts | Authorised finance/contract role | Authorised oversight | — | V,U,R,verify,X within role | — |
| Restricted safeguarding / incident evidence | — | Explicit case access only | — | — | Case-scoped V,U,R,S,X |

## Common controls
- School-configured types can narrow or delegate permissions within platform policy, never override safeguarding, medical, inclusion or legal restrictions.
- **Update means new version**, with provenance; prior accepted version remains governed and retrievable to authorised reviewers.
- **Deletion:** no ordinary user hard-delete. Authorised users may submit a reasoned disposal request; Epic 26 controls retention, legal hold, approval, cryptographic/object disposal, backups and immutable audit. Emergency takedown/quarantine is a distinct restricted action.
- **Exports:** separate X grant, scoped recipient/purpose, approved destination, secure delivery, expiry, watermark/password protection where policy requires; audit all downloads/shares.
- **Visibility:** parents see only expressly parent-visible documents for an authorised child; no default visibility of safeguarding or restricted clinical/pastoral evidence.
- **Deny by default:** cross-tenant, cross-region without approved transfer, unrelated child, unauthorised role, revoked access, or unapproved recipient.
- **Audit:** sensitive views, upload, version changes, verification, export, denied access and disposal request/decision; capture actor, time, purpose and object version.
- **Implementation:** combine RBAC (role/action) with ABAC (tenant, child relationship, classification, domain, case assignment, state); enforce at API and storage access layer.

## Decisions still to confirm
1. Which independent approver roles are eligible for disposal requests, including when a principal is the requester? Are additional domain-specialist approvals needed for sensitive records?
2. Which document categories are parent-downloadable versus view-only by default?
3. Which school-level document classes require a separate publisher/approver?
4. Should the platform provide an emergency break-glass document-access process, and for which categories? Existing safeguarding break-glass policy must take precedence.

## Approved retention floor — 21 October 2026
- Once approved, a student or school document **must not be permanently deleted before the applicable statutory/regulatory minimum retention period** has elapsed. This applies regardless of school role, including principals and administrators.
- Determine the retention rule by jurisdiction, document category and any applicable sector/child-protection obligations; calculate an earliest permissible disposal date. Do **not** invent a universal number of years; legal validation and Epic 26 policy configuration are required.
- Superseding, invalidating, correcting or restricting visibility of a document must not destroy its approved original/version or provenance during mandatory retention.
- Once the minimum period has elapsed, disposal still requires an authorised, auditable workflow and checks for legal hold, active investigation, contractual or other longer obligations. Retention expiry is not automatic deletion.
- A lawful exception or mandatory erasure obligation must be assessed through central legal/governance policy, not ad hoc admin override.

## Approved segregation of duties (SoD) — 21 October 2026
- RBAC grants eligibility to perform an action; **SoD is a separate workflow control** preventing incompatible actions by the same individual on the same document/request. Enforce on backend identity and audit, not merely UI role labels. Holding multiple roles does not bypass SoD.
- After the legally required retention period, a **school administrator or principal** may submit a reasoned disposal request. The requester **cannot approve their own request**. Require approval by a separately authorised individual and confirm retention, legal holds and other constraints before disposal. If no eligible independent approver exists, keep the request pending and route to an authorised governance/escalation role; never self-approve.
- Record requester identity, approval/denial identity, reasons, timestamps, document versions, policy checks and eventual disposal outcome. Specialist consultation/additional approval for sensitive documents remains subject to domain rules and further design.
- SoD also applies to other high-risk workflows as configured; specific maker/checker pairs beyond disposal require separate product decisions. Routine authorised school upload-and-accept remains a permitted one-step workflow, while substantive specialist verification stays independent of administrative intake.

### Approved independent approver for principal requests — 21 October 2026
- If a principal requests disposal, another appropriately authorised **senior school official** may approve; the approver need not be higher in the organisational hierarchy. They must be a distinct individual with explicit disposal-approval permission and no conflict of interest.
- For small schools without an eligible second person, retain the request pending and use an approved external/governance escalation route; never waive independent approval.

### Approved parent view/download parity — 21 October 2026
- For a parent/guardian authorised to view a specific student document, **download is also permitted**; do not implement a parent 'view-only' document permission. This is document-level permission parity, not universal parent access.
- Parent-supplied documents remain accessible to the submitting parent/authorised guardian subject to the child's relationship and applicable legal/case restrictions. School-created routine documents that are parent-visible are likewise viewable and downloadable.
- Document origin (parent versus school) alone does not determine visibility; classification, parent/child relationship and domain-specific restrictions decide whether access is granted at all. Confidential safeguarding or other restricted documents remain hidden unless explicitly authorised.
- Downloads remain authenticated, access-checked and audited, with applicable secure-export controls. Do not claim downloaded copies can be revoked once distributed.
