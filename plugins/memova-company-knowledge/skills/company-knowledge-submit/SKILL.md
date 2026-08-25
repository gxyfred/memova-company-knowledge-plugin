---
name: company-knowledge-submit
description: Check and explicitly submit current-task knowledge to Memova's company public knowledge base when an employee deliberately invokes @公司知识助手 or clearly asks to submit or publish selected evidence. Use for candidate assessment, receipt routing, preparation, preview, cancellation, confirmation, publication status, and correction; never trigger for an ordinary question, task completion, commit, merge, meeting end, deployment, file change, or bare mention.
---

# Company Knowledge Submit

Provide the P0 employee-initiated submission workflow through the dedicated
`company_knowledge_assistant` MCP. Before assessing candidates, read
`references/submission-workflow-contract.json`, `references/trigger-routing-v1.json`, and
`references/receipt-candidate-v1.schema.json`. For a `business_status` selected from the current
Codex task, also read `references/current-task-business-status-selection-v1.schema.json`. Treat
their outcomes, gate order, receipt routes, candidate fields, enum values, trigger IDs, exclusion
rules, approval boundaries, and stop conditions as authoritative. The Company MCP remains
authoritative for identity, capabilities, source/target registries, validation, idempotency, audit,
publication, and index state.

## Explicit entry only

- Start only when the employee deliberately invokes `@公司知识助手` or clearly asks to check or
  submit selected current-task evidence to the company public knowledge base. The standard check
  prompt starts assessment only; it is not approval to prepare or publish.
- A normal company question, answer, task completion, commit, merge, meeting end, deployment, file
  change, third-party event, bare `@公司知识助手`, or a request to summarize must never prepare or
  publish knowledge. Keep those paths in the universal query Skill or return `NO_SUBMIT`.
- P0 performs no background scan, Hook, polling, third-party OAuth, Connector, full mirror, or
  automatic login. Do not request passwords, tokens, cookies, MFA data, connection strings, or
  private keys.

## Assess current-task candidates

1. Use only the current task and evidence the employee explicitly selects: named fields, one
   allowlisted Git revision range, one allowlisted file/export/image, one sanitized log, a selected
   readable object, or a bounded employee attestation when the receipt contract permits it. Never
   submit the full conversation, repository, diff, directory, design file, transcript, raw log,
   platform export, private message, or unrelated workspace context.
2. Apply the trigger catalog and no-submit rules. Default to at most three highest-value candidates.
   Assign exactly one trigger ID, one decision outcome, and one primary receipt type per candidate.
   Split code, design, business status, deployment/incident, document/meeting, file/media, and
   general knowledge facts into separate candidates; one receipt must not imply another state.
3. Evaluate the five common gates: material change, reusable or cross-role value, complete stable
   evidence, company-public-internal distribution, and employee selection confirmation. The
   employee's ability to read an object does not prove company-wide publication rights.
4. Return exactly one of `PREPARE_NOW`, `PREPARE_BEFORE_TASK_END`, `CORRECTION_REQUIRED`,
   `NEED_MORE_EVIDENCE`, `SENSITIVE_OR_RESTRICTED`, or `NO_SUBMIT`. Show the proposed receipt type,
   evidence included, evidence excluded, snapshot time, missing requirements, and reason. A link,
   account, filename, commit, merge, or CI success alone is not evidence of the claimed fact.
5. Never invent or model-fill a locator, revision, hash, environment, health result, approval,
   owner, publication right, capability, or correction chain. If required evidence is absent, stop
   at `NEED_MORE_EVIDENCE`. If restricted material cannot be safely excluded, stop at
   `SENSITIVE_OR_RESTRICTED`.

## Select collector and build one candidate

- Route the chosen trigger through `trigger-routing-v1.json`. Use the corresponding deterministic
  collector semantics and one frozen receipt type. Source ID, access basis, target, and allowed
  receipt type must agree with server registries; the Skill never overrides or invents them.
- `code_change` proves code/revision/test state, not deployment. `design_change` proves approved
  design intent, not implementation. `deployment_or_incident` needs environment, immutable runtime
  version, status and health/recovery evidence. `business_status` must keep discussion, development,
  merge, test, deployment and user availability distinct.
- Correction and source-invalid cases inherit the original receipt type and require
  `previous_receipt_id`, `supersedes_knowledge_id`, and a substantive correction reason. Create a
  forward superseding record; never edit, overwrite, hide, or delete history.
- For a `business_status` selected from the current Codex task, construct only the bounded
  `CurrentTaskBusinessStatusSelectionV1` fields and pass them as the
  `current_task_business_status` tool argument. The Skill must not provide `source_locator`, source
  ID/system/object/access basis, revision, immutable locator, evidence manifest/hash, or
  `knowledge_owner_upn`; those fields belong to the server-owned current-task route. The test marker
  or business object ID belongs in `business_object_id`. It must not ask for an HTTPS link merely
  because the evidence is the current Codex task.
- Construct one `ReceiptCandidateV1` from the selected bounded evidence using only the fields,
  required sets, formats, and enum values in `references/receipt-candidate-v1.schema.json`. Do not
  guess missing fields, source identifiers, locators, revisions, hashes, or evidence-manifest
  values. If the current task cannot supply a fully valid candidate, stop at `NEED_MORE_EVIDENCE`
  before asking for prepare approval. Never expose employee IDs, ACLs, arbitrary targets, hidden
  server fields, or raw sensitive material. Process multiple candidates sequentially, each with its
  own preview and confirmation.

## Approval-gated preparation

Before calling `prepare_knowledge_submission`, show the exact candidate or current-task status
selection, receipt type, selected evidence, exclusions, Company MCP target, and expected effect: a
30-minute employee-owned ephemeral preview, not Published Knowledge. Ask for explicit approval for this exact remote prepare action. Candidate selection or the initial `@公司知识助手` invocation is not preparation approval.

After approval, call `prepare_knowledge_submission` once with either `candidate` or
`current_task_business_status`, never both. Do not automatically retry, switch sources, broaden
evidence, or prepare other candidates. If blocked, present safe field errors and return to
assessment; do not weaken a gate. If ready, show all of:

- the exact Published content preview and destination;
- normalized receipt type, source locator/revision/hash, snapshot time, owner and policy result;
- evidence and attachments included, excluded evidence, redactions, limitations and warnings;
- `preview_id`, `preview_payload_sha256`, expiry, and a clear “not yet published” label.

Cancel means do not call `submit_knowledge_candidate` and allow the preview to expire. Any content,
evidence, target, owner, receipt type, or correction-chain change invalidates the displayed preview
for confirmation and requires a newly approved prepare call.

## Separate final confirmation and direct publication

- Ask for a second, explicit confirmation only after displaying the complete server preview. The
  employee must clearly confirm publishing that exact preview; earlier approval, “continue”, silence,
  or confirmation of a different preview is insufficient.
- Then call `submit_knowledge_candidate` exactly once with only the four confirmation fields. Copy
  the exact `client_request_id` from `normalized_candidate`; use that same
  `preview_id`, the same `preview_payload_sha256`, and `user_confirmed: true`. Do not generate a
  fresh request ID for submit. Never resend or
  reconstruct the candidate payload at submit time.
- A successful call directly creates Published Knowledge plus an immutable SubmissionReceipt; no
  Draft or human review queue follows. Report `knowledge_id`, `receipt_id`, Published target URL,
  audit reference, source/submission snapshot times, and `search_visibility`.
- `index_pending` means the records exist but are not yet searchable. Use
  `get_publication_status` only for bounded read-only status checks allowed by the contract; never
  claim visibility early.

## Failure and unknown outcome

- Do not blindly retry submit after a timeout, dependency failure, unknown outcome, one-sided write,
  hash mismatch, expired preview, idempotency conflict, or reconciliation requirement. Preserve any
  returned operation ID and inspect it with `get_publication_status`; otherwise stop with a safe
  recovery instruction. Never create a replacement preview to hide an unknown operation.
- Authentication, authorization, source, target, sensitivity, distribution, evidence,
  currentness, type-completion, correction-chain, and policy errors are non-retryable until the
  employee or administrator resolves the stated cause. Do not use another employee's account.
- Never delete or silently overwrite a Published item, Receipt, preview, operation, or audit record.
  Recovery and corrections are forward-only.
