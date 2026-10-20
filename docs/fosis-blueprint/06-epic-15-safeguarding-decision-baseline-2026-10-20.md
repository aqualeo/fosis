<!-- SPDX-License-Identifier: Apache-2.0 -->
# Epic 15 — Safeguarding & Student Welfare
## Approved decisions, detailed workflows and acceptance-test baseline
**Product:** FOSIS · **Decision capture:** 20 October 2026 · **Document status:** Requirements/solution-design baseline; not a completed or implemented epic.

**Canonical requirements:** [Epic #15](https://github.com/aqualeo/fosis/issues/15) and [US 15.1 #130](https://github.com/aqualeo/fosis/issues/130), [US 15.2 #131](https://github.com/aqualeo/fosis/issues/131), [US 15.3 #132](https://github.com/aqualeo/fosis/issues/132), [US 15.4 #133](https://github.com/aqualeo/fosis/issues/133). If this document and an approved user story differ, the story's latest approved acceptance criteria govern. This document consolidates the **20 October 2026 conversation**; it does **not** assert code, database schemas, tests or release are complete.

## 1. Security boundary and actors
- **Pastoral care and safeguarding are separate security domains.** A pastoral concern may be escalated or linked, but pastoral permissions never expose a safeguarding case, chronology or evidence.
- Any authorised school staff member can report a safeguarding concern. **Reporting confers no safeguarding case access.**
- Only explicitly designated, school/tenant/case-scoped safeguarding users may manage cases. A general school administrator or teacher role does not implicitly permit viewing, approval, export, referral, transfer or reassignment.
- A designated safeguarding lead (DSL), designated deputies and other case-authorised safeguarding officers have permissions based on school policy and current case scope. School governance maintains the authorised coverage/hierarchy.
- The principal has **break-glass approval authority, not standing access to case content**. The only eligible alternate emergency approver is a **formally nominated and active acting principal or vice-principal**. No delegation below that level. No approver can authorise their own case access.
- Appointment, promotion, role change and removal of safeguarding-privileged users require **two distinct authorised persons**. Pending privileges are ineffective. Coverage must not be orphaned; exceptional urgent security suspension is separately controlled, reasoned and retrospectively reviewed.

## 2. Concern submission and responsibility (US 15.1)
**Story:** As a staff member I need to report a concern securely, receive acknowledgement and supply later information without seeing confidential case details.

**Confirmed flow**
1. Authorised reporter selects the school/student, enters concern, urgency and relevant evidence, and submits.
2. FOSIS creates a reference and immutable original submission with actor/time; it enters the restricted safeguarding queue.
3. Route to the DSL or, on known absence/unavailability, the next eligible authorised deputy, preserving the original submission time, urgency and deadline.
4. An authorised officer **explicitly acknowledges and takes responsibility**; notification delivery/read does not count.
5. **Routine** acknowledgement defaults to the **same school day** (school-configurable, always subject to stricter law/policy). Urgent concerns alert appropriate authorised staff immediately and follow the school's emergency pathway. Overdue unacknowledged reports escalate only within the explicitly authorised safeguarding hierarchy; failures remain visible to safeguarding administrators.
6. Reporter tracking shows only **Submitted, Acknowledged, Closed**, the report reference and allowable timestamps. It never leaks case chronology, evidence, investigation, other people's reports, referrals, restricted officer identity or details through screens, API, notifications or export.
7. Original submission remains immutable to the reporter. A reporter may append a separate timestamped **Add information** statement and evidence linked to their original reference. Corrections use a new linked statement, never rewriting the original. Follow-ups notify the current authorised officer; urgent information follows urgent escalation.

**Acceptance-test baseline**
- [ ] Tenant, student and reporter validation; submit persists a reference, original text, actor, evidence link and timestamp in restricted records.
- [ ] Acknowledgement is explicit, identifies a case owner and stops the unresolved-acknowledgement escalation. Read/delivered alerts do not acknowledge.
- [ ] Same-school-day routine deadline and faster applicable school/jurisdiction rules are enforced; urgent risks do not wait for routine timers.
- [ ] DSL absence, deputy routing, failure/retry/escalation and single ownership are enforced without making generic administrators case viewers.
- [ ] Reporter view and APIs contain only the three authorised statuses/reference/timestamps.
- [ ] Reporter can add append-only linked statements; cannot edit/delete original or read restricted chronology.
- [ ] Every notification, submission, follow-up, routing, acknowledgement and failure has a restricted audit record.

## 3. Case assessment, chronology and lifecycle (Epic 15 / US 15.2)
**Risk:** Default levels **Routine**, **Elevated**, **High** (product labels, not universal statutory classifications). School-approved thresholds, jurisdictional guidance and safe escalation rules govern local configuration. FOSIS/AI may *suggest* risk, but an authorised safeguarding officer must confirm/change it with reason/time/history. Indicators of immediate danger trigger protective action independently of classification.

**Case record:** Stable case ID; student/tenant scope; restricted owner; risk with changes; linked pastoral escalation (without breaking separation); original report; append-only chronology; actions and due dates; follow-ups; referral organisation, timestamp, reference, owner, state/outcome and communications; metadata for immutable evidence; review and closure records; related-case links; full access audit.

**Closure decisions**
- **Routine:** authorised safeguarding lead can close with documented outcome/rationale under school policy.
- **Elevated:** authorised safeguarding lead may close **without a second approver** only after every mandatory action is actually completed and checked, with any required evidence, verifying user and timestamp. Scheduled/pending/overdue/unchecked actions block closure; rationale and checklist must be retained.
- **High:** closure requires an **independent second authorised review**, in addition to completion of relevant safeguards.
- If a fresh concern appears after closure, create a **new case with a new ID**, optionally linked by an authorised officer with reason/actor/timestamp. **Do not silently reopen or rewrite** the former case, closure checklist or history. Each case keeps separate risk, owner, chronology, SLA, referrals, actions and closure. Distinct administrative correction/reopening for a genuine error, if ever implemented, needs explicit governance and audit.
- Related-case history is visible only to case-authorised safeguarding officers; no unauthorised cross-school or cross-tenant access.

**Acceptance-test baseline**
- [ ] Validate mandatory risk level; human sign-off and reasoned updates; urgent warning/escalation doesn't wait for sign-off.
- [ ] Case chronology preserves decisions, evidence metadata, referrals, follow-ups, view events and original ownership.
- [ ] Routine, elevated and high closure paths enforce the appropriate rationale/checklist/independent review rules.
- [ ] Elevated closure is blocked by any required incomplete/unchecked/overdue action; completion actors/evidence/timestamps remain attributable.
- [ ] New post-closure concerns create independent IDs and records; prior decisions remain intact; links require restricted permission and an audit record.
- [ ] Authorised reopening/correction is distinguishable from a new incident; unknown reopening details are not inferred.

## 4. Evidence ingestion, correction and secure exports (Epic 15 / US 15.2)
- Use **encrypted access-controlled object storage** for binaries; database stores restricted metadata and references, **not evidence bytes in PostgreSQL**.
- Evidence captures uploader/source, timestamps, case/event link, processing status, version history and permitted accesses.
- **48-hour default uploader correction/deletion window**, school-configurable only within stricter evidence-preservation, child-protection, legal and retention obligations.
- Uploader may remove/replace incorrectly submitted evidence within the allowed window only when **not reviewed, relied on, referenced in a decision, included in an external referral, or under hold**. A formal use/review instantly locks ordinary uploader deletion, even before 48 hours. Expiry also locks it.
- Following lock, authorised officer can mark attachment **invalid/superseded** with recorded reason but cannot ordinarily erase the original. Lawful independently authorised disposal remains a separate audited exception.
- Permitted deletion removes the active encrypted object through verified storage/version/replica lifecycle while retaining a **minimal tamper-resistant deletion audit stub**, without retaining unnecessary sensitive content. Case text/chronology never disappears with an attachment.
- **Standard export** is a **strongly encrypted, password-protected PDF** or other policy-approved protected package. No plain-data, CSV or unprotected original download by default. Include reference/evidence ID, time and controlled-handling notice; authoritative stored evidence is unchanged.
- Export requires **explicit safeguarding role AND case-specific export permission**. Record actor, case/evidence IDs, time, purpose, intended recipient and approvals/outcome. Audit denied attempts.
- Separate password/secret channel from file delivery; never put a secret into the same message or ordinary logs. Use secure transport and short-lived links/revocation where feasible; password protection does not replace access control.
- **Exceptional original-format export** only where evidential integrity, forensic use or recipient technical rules require it: documented reason, recipient, lawful/policy basis, independent authorised approval where appropriate and encrypted secure transfer. Routine external evidence sharing may be authorised by the DSL; particularly sensitive/original disclosures require a **second distinct authorised safeguarding officer**, subject only to a documented lawful emergency exception with minimum necessary disclosure and retrospective review.
- No workflow must prevent lawful, immediate protective disclosure when a child faces serious/immediate danger.

**Acceptance-test baseline**
- [ ] Separate encrypted evidence store; access-controlled reference and event metadata in transactional DB.
- [ ] Allow original uploader correction only while unreviewed/unreferenced, before expiry and absent hold; deny otherwise.
- [ ] Keep original chronology and minimal audit stub across permitted deletion; verify object lifecycle and no plain logging of sensitive content.
- [ ] Enforce encrypted protected PDF/package by default, no raw/CSV exports, and preserve originals.
- [ ] Deny and audit case-unauthorised downloads; enforce reason/recipient and additional approval where needed.
- [ ] Keep encryption secret delivery separate and audit successful/failed external disclosures.
- [ ] Enforce second safeguarding-authorised approver for exceptional/original exports except documented lawful emergency exception.

## 5. Principal-authorised emergency access (US 15.3)
**Strict use:** Serious **immediate child-protection risk** and the normal DSL **and** authorised deputy are unable to act in time. Illustrative exceptional situations: immediate serious-harm disclosure, an endangered missing child, or police/child-protection notification of an immediate risk. These are examples, not automatic permissions or a fixed legal classification.

**Approved workflow**
1. School sets its principal, formally nominated active acting-principal/vice-principal alternate, approved urgent notification channels and eligible requesters. No lower role can become an approver by configuration.
2. Requester chooses **one specific case/student** and describes urgency, necessary scope/duration, why access is necessary, explicit **DSL-and-deputy unavailability attestation**, and attempted contacts or why contact is unsafe/impracticable. Incomplete or generic justifications are blocked.
3. Notify principal immediately with **non-sensitive alert** directing them to authenticated secure details.
4. Principal explicitly accepts/rejects. Default response window **5 minutes**, configurable by school and applicable policy. At timeout, alert **only** the active, formally delegated acting principal/vice-principal. This timer is for **notification escalation**, not an access token or automatic grant; never escalate to teachers/HOD/admins. If no qualified approver is available, leave access denied and display the school's outside-system emergency procedure.
5. FOSIS atomically enforces **explicit eligible approval**, different requester/approver, valid active delegation if alternate, correct school and single-case scope, and a bounded grant. Approval power itself does not disclose case content.
6. Grant expires **after four hours by default** (unless stricter policy); extensions need a fresh reason and a new explicit eligible approval; automatically revoke at expiry or earlier revocation.
7. Capture request/notification/attempt/timeout/escalation/approval/grant/views/changes/revocation in prominent restricted audit; notify DSL/deputy and require retrospective safeguarding review.
8. **Never delay emergency services, reporting to authorities or immediate safety measures pending FOSIS access.**

**Acceptance-test baseline**
- [ ] Request requires attestation, necessity, contact attempts or safety explanation and case-specific scope.
- [ ] Principal can approve/reject; principal cannot self-approve; acting principal/vice-principal is alternate only with active formal designation.
- [ ] Generic school admins, HODs, teachers and all lower roles cannot approve, even when principal is unavailable.
- [ ] At 5 minutes default, unanswered principal request produces a fallback **alert**, not automatic access; resolved requests do not re-escalate; concurrent approver decisions are safe.
- [ ] Explicit approved case-scoped access expires after 4 hours default; extension is independently reauthorised and reasons/audit preserved.
- [ ] No secret details in notification payloads; all positive and denied access attempts and actions audited.
- [ ] If eligible approvers unavailable, no authorisation bypass; emergency actions outside FOSIS remain available.

## 6. Case handover and reassignment (US 15.2)
**Ordinary handover:** Existing owner selects a suitably authorised safeguarding officer, records reason and reviews restricted summary of risk, outstanding actions, deadlines and pending referrals. **Recipient must explicitly accept**. Until acceptance, current owner retains accountability and case access. Rejection/expiry leaves current ownership untouched and notifies initiator. Acceptance **atomically transfers** ownership with full immutable trail and notifies both officers.

**Exceptional reassignment:** Where the outgoing officer suddenly becomes unavailable/leaves, the designated safeguarding lead may reassign to an eligible authorised recipient without outgoing-officer participation, with urgent justification, recipient validation, notification and full audit. Recipient acknowledgement should not block urgent continuity.

**Non-negotiable invariants:** no unowned case; no creation of a second case or reset of due dates; no lost evidence, chronology, risk assessment, referrals, actions or historic owner; no cross-school transfer without separate explicit governance; no access for an unauthorised recipient.

**Acceptance-test baseline**
- [ ] Pending handover does not transfer ownership; acceptance does so atomically.
- [ ] Reject/expire safely, retaining the existing accountable officer and all deadlines.
- [ ] Exceptional DSL reassignment verifies eligibility, logs reason and preserves continuity without requiring unavailable outgoing owner's approval.
- [ ] Both types retain case ID, evidence, referral/chronology, SLA, open actions, risk and previous ownership log.
- [ ] All attempted and effective transfers are case-scoped, role-scoped and timestamped.

## 7. Proposed domain model and UI/API design (implementation design, **not** independently approved schema)
The following names are *design guidance* for a later engineering review; they do **not** represent migrations or completed implementations.

**Logical entities:** SafeguardingConcern; ConcernFollowUp; SafeguardingCase; CaseLink; RiskDecision; CaseChronologyEvent; CaseAction/ActionVerification; SafeguardingReferral; EvidenceAsset/EvidenceVersion/EvidenceDeletionAudit; CaseAccessGrant; EmergencyAccessRequest/Approval/Notification; RoleChangeRequest/IndependentApproval; CaseTransferRequest/Acceptance; ExportRequest/DisclosureApproval; RestrictedAuditEvent; SafeguardingCoverage/DelegationPolicy.

**Important fields/relationships:** tenant/school and opaque student IDs on every controlled aggregate; original event and author/timestamps; case-specific permission subject; classification effective history; prior/new owner; signed-off actions; referral recipient/reference/communications; evidence storage key without logging binary; access reason, request/approver IDs and expiry; explicit intended export recipient; immutable event references. Put case-specific authorisation at every API boundary and at retrieval time, not merely in UI. Prevent IDs and tokens in URLs/logs from becoming access channels.

**UI views:** (a) secure concern report with urgent-action guidance; (b) reporter minimal-status and append-only follow-up; (c) restricted safeguarding queue/acknowledgement; (d) case workspace, chronology, evidence and referrals; (e) risk/closure checklist; (f) evidence correction/protected export with approval; (g) principal emergency-approval screen plus short-lived access screen; (h) principal/alternate delegation setup; (i) case handover summary/recipient acceptance and exceptional DSL reassignment; (j) separate safeguarding role governance and restricted audit/review dashboards.

**API/behaviour patterns:** immutable reports and follow-up events; explicit acknowledgement transition; state-machine guarded risk/closure and transfer; authorised protected export issuance; self-approval prevention; idempotent timeout/notification jobs; atomic approvals and ownership updates; deny-by-default on absent tenant, school, case scope, delegation or coverage. Tests must include concurrency, request replay, expired grants, invalid alternates, unavailability, retries, audit integrity and unauthorised API access.

## 8. AI and jurisdiction controls
- Safeguarding AI must go through the FOSIS Epic 29-approved gateway/policy, with deliberate human review. **No autonomous safeguarding judgement or closure.**
- Local child-protection, evidence/retention, incident response, lawful sharing and age-specific procedures must be configured and reviewed by authorised policy/legal/safeguarding owners. This document sets **product defaults**, not national legal advice.
- Alerts to parents/staff/external parties should expose only the minimum permitted information. School-specific safe-notification rules may restrict who is informed in active safeguarding situations.
- All data access, denial, export, alteration and emergency overrides remain auditable without placing the confidential narrative into ordinary logs.

## 9. Consolidated decision register — as at 20 October 2026
| Area | Approved decision | Primary issue |
|---|---|---|
| Reporting | Explicit acknowledgement, restricted queue, deputy routing, three reporter statuses, append-only follow-up | [#130](https://github.com/aqualeo/fosis/issues/130) |
| Risk | Routine/Elevated/High; authorised human classification, independent emergency escalation | [#15](https://github.com/aqualeo/fosis/issues/15) |
| Closure | Routine lead+rationale; Elevated fully checked mandatory actions + lead; High independent second review | [#15](https://github.com/aqualeo/fosis/issues/15) |
| Closed cases | New concern creates new separately assessed case; optionally link, preserve closed original | [#15](https://github.com/aqualeo/fosis/issues/15) |
| Evidence | Object store, 48-hour conditional correction, no ordinary deletion after review/hold/expiry | [#15](https://github.com/aqualeo/fosis/issues/15) |
| Protected exports | Protected encrypted PDF/package by default; exceptional original with separate authorisation | [#15](https://github.com/aqualeo/fosis/issues/15) |
| External sharing | DSL authorises routine lawful sharing; second safeguarding authoriser for highly sensitive/original except lawful emergency | [#131](https://github.com/aqualeo/fosis/issues/131) |
| Break-glass | Principal approves, formally delegated acting principal/vice-principal only alternate; no self-approval | [#132](https://github.com/aqualeo/fosis/issues/132) |
| Emergency timers | 5-minute default unanswered-request **notification escalation**; 4-hour default approved **access expiry** | [#132](https://github.com/aqualeo/fosis/issues/132) |
| Case transfers | Recipient accepts ordinary handover; DSL may exceptionally reassign; preserve responsibility/history | [#131](https://github.com/aqualeo/fosis/issues/131) |
| Role governance | Second-person approval; separate role approval from case content access | [#133](https://github.com/aqualeo/fosis/issues/133) |
| AI | Policy-gated and human decision; no automated safeguarding conclusion | [#133](https://github.com/aqualeo/fosis/issues/133) |

## 10. Deferred verification / implementation gates
No additional product decision is silently inferred by this summary. Before implementation or declaring Epic 15 complete, independently confirm:
1. Jurisdiction-specific safeguarding policy, exact urgency thresholds, mandated records retention, and school-specific approval/notification policy.
2. Role-to-privilege and per-case authorisation matrix, including formally delegated principal coverage and independent approval governance.
3. Emergency procedure when DSL/deputy and principal/alternate are unavailable; software must not replace urgent protective action.
4. The scheme for protected documents (strong PDF/package encryption), out-of-band secret delivery, exceptional forensic-original handling and verified object-storage disposal.
5. The draft logical data model/API/UI with engineering/security reviewers; integration and Epic 29 AI gateway.
6. Automated tests for denied access, tenant isolation, evidence holds, export policy, timers/concurrency, atomic ownership and integrity of audit.
7. GitHub Project board synchronisation / lifecycle state, final story grooming and technical sign-off.

**Session disposition:** Requirements conversation concluded **20 October 2026**. The *discussion* is closed; **Epic 15 remains open** pending product/technical verification, implementation and acceptance. This document is a traceable consolidation, not a claim of delivery.
