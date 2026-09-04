---
name: company-knowledge-onboarding
description: Run the explicit installation, onboarding, connection, identity, capability, seven-receipt, snapshot, preview/cancel, confirmed-rehearsal, permission, and sensitive-data self-check for Memova's 公司知识助手. Use only when the user asks to onboard, set up, diagnose readiness, or run the self-check; never trigger it for an ordinary company question.
---

# Company Knowledge Onboarding

Run a bounded, fail-closed readiness workflow for the dedicated
`company_knowledge_assistant` MCP. Read `references/self-check-contract.json` before the first check
and treat its gate IDs, retry classifications, receipt types, and completion rule as authoritative.
This Skill orchestrates checks; it does not implement authentication, infer authorization, or
change server policy.

## Entry boundary

- Trigger only after an explicit onboarding, setup, migration, connection-diagnosis, or self-check
  request. A company question, bare Plugin invocation, or query failure must not start a write
  rehearsal.
- Explain that installation, Plugin updates, or first-time MCP login require a full Codex restart
  and a new conversation before a reliable tool check. Inspect only the current Plugin version and
  dedicated MCP host status; do not install, upgrade, distribute, authenticate, or edit
  marketplace/workspace settings without the employee's explicit request or approval.
- P0 checks no GitHub, Figma, Feishu, Azure, Google, Slack, or other third-party connection and must
  not request per-platform OAuth. SharePoint access is mediated by the dedicated Company MCP; do
  not bypass it with a direct employee connector.

## OAuth bootstrap

Run this bootstrap before evaluating any required gate. The host can withhold protected MCP tools
until the server is authenticated, so the workflow must not fail `MCP_TOOL_SURFACE` before offering
the employee OAuth path.

1. Run the local, read-only `codex mcp list` status check and inspect only the row for
   `company_knowledge_assistant`.
2. If the server is absent or disabled, stop with `host_binding_missing` or
   `host_binding_disabled`. Do not substitute SharePoint or another MCP.
3. If the server is enabled and reports `Not logged in`, explain that
   `codex mcp login company_knowledge_assistant` opens the Microsoft authorization flow for this
   dedicated MCP. Run it only when the employee's current installation/onboarding request already
   authorizes first-time login or after obtaining explicit approval. The employee completes the
   Microsoft browser flow using their own `@memova.ai` account; never operate the identity-provider
   page for them.
4. Yield control while the employee enters any password or MFA only on Microsoft's page. Never ask
   them to copy any credential, code, token, cookie, URL query, or callback content into Codex.
   `codex mcp logout company_knowledge_assistant` is the local connection rollback.
5. After the employee reports completion, run `codex mcp list` again. If it still reports
   `Not logged in`, stop with `oauth_login_incomplete` and the exact retry command; do not loop or
   reinstall. If authenticated, require the employee to fully quit and reopen Codex and start a new
   conversation. The current run remains incomplete because its tool surface was created before
   login.
6. In the fresh conversation, if the server is authenticated but the seven required tools are
   still absent, fail with `authenticated_host_binding_missing`. Only an authenticated server with
   the exact seven tools may continue to the required gates.

## Authentication and profile

1. After the OAuth bootstrap, verify that the current session exposes the exact required Company
   MCP tools from the contract. Missing or unexpected tools fail the relevant gate; never
   substitute another endpoint.
2. Call `get_profile`. If the host unexpectedly requires Microsoft authorization again, return to
   the bounded bootstrap classification instead of assuming that a tool call will open login.
   Never ask the employee to paste or dictate a password, access/refresh token, cookie, client
   secret, private key, MFA code, recovery code, or session export. Never place any such value in a
   tool argument, file, log, report, or conversation summary. If one appears, stop handling it and
   ask the employee to revoke/rotate it through the proper administrator path.
3. Apply the contract retry budget: one initial attempt plus at most two additional guided attempts.
   Retry only a transient network, dependency, or connection failure. Do not retry an unauthorized
   employee, administrator-disabled application, wrong tenant, missing site permission, identity
   mismatch, policy denial, or consent requiring administrator action.
4. Show only the safe profile fields needed for the employee to verify that the signed-in identity
   is their own. Ask for a yes/no identity confirmation; never ask for identity documents. Stop on
   mismatch and never continue using another employee's account.
5. Read returned job assignments only as explanatory metadata. Current explicit capability grants
   are the sole authorization input. Job, title, department, manager, and historical contribution
   types never restrict questions or grant submission permission.

## Read and receipt readiness

1. Confirm the contract's seven receipt types and required MCP tool surface. A local declaration is
   package readiness, not proof of a live backend binding; mark a live check `not_run` rather than
   claiming success when no server response exists.
2. Read `answer_probe` from the machine contract. Generate one fresh UUID v4 and substitute it into
   the exact `question_template`; send only that resulting identifier as the question with the
   frozen time scope, filters, and selected IDs. Do not add explanatory words or choose a company
   question. Pass only when the response exactly preserves the expected insufficient-evidence
   status, empty citations/conflicts, gap reason, freshness labels, and a non-empty MCP
   `policy_version`. This probe is a transport/contract canary, not a company-fact evaluation.
3. Verify that `search`, `fetch`, `prepare_knowledge_submission`,
   `submit_knowledge_candidate`, and `get_publication_status` are present. Do not call write tools
   merely to prove that they exist.

## Controlled preview and cancel rehearsal

- A remote preview can create employee-owned ephemeral server state. Before calling
  `prepare_knowledge_submission`, state the exact Company MCP target, synthetic or explicitly
  approved evidence, expected ephemeral effect, 30-minute expiry, and absence of a Published item.
  Obtain explicit user approval for that exact remote action.
- Use only synthetic non-sensitive `general_knowledge` content unless the user separately approves
  another evidence item and receipt type. For the synthetic rehearsal, call
  `prepare_knowledge_submission` with only the `current_task_general_knowledge` input defined by
  `preview_cancel_rehearsal`; generate fresh valid request/task/selection identifiers and supply the
  bounded synthetic topic, claim, applicability, observation time and selection reason. Never
  provide or invent `source_locator`, source identity, revision, evidence hash, immutable locator,
  or knowledge-owner identity: the server derives them from the authenticated employee and bounded
  current-task selection. Do not upload or copy real company secrets into a test.
- Present the returned target, normalized payload, exclusions, warnings, capability decision,
  policy version, and expiry. For a cancel rehearsal, discard the preview and do **not** call
  `submit_knowledge_candidate`; cancellation means allowing the preview to expire.

## Single final confirmation rehearsal

- A confirmation rehearsal is a real remote write: it creates Published Knowledge plus an
  immutable SubmissionReceipt and can enqueue an index projection. Never treat the intent or selection that authorized
  the preview as approval to publish.
- Before calling `submit_knowledge_candidate`, show the exact preview ID, destination, effect,
  audit identity, and correction/forward-recovery path; obtain the single final explicit publication
  confirmation. Never promise deletion as rollback. After confirmation, use
  `get_publication_status` for bounded status inspection and report Published, Receipt, and Index
  states separately.
- Verify that the SharePoint author, operation submitter, and Receipt submitter all resolve to the
  current employee. An app-only or shared Publisher identity is a failure, not a fallback.

## Negative checks and completion

- Permission checks use only synthetic IDs and require server denial for another employee's
  preview, unauthorized destination, absent/revoked submit capability, app-only publication, or a
  delegated principal mismatch. Do not impersonate an employee.
- Sensitive-data checks use inert markers named in the contract, never real credentials, personal
  data, production identifiers, or customer content. The preview must reject or exclude them.
- Report every required gate as `pass`, `fail`, `not_run`, or `not_applicable`, with a safe error
  classification and next action. Do not include raw tokens, stack traces, internal ACL detail, or
  hidden identifiers.
- Complete onboarding only when the contract completion rule is satisfied. `not_run` is never
  success. Every active employee uses the same submit rehearsal contract; Chenchen may choose not
  to run it in ordinary work, but is not a role-based denial case. Completion requires both the
  approved cancel rehearsal and the single-final-confirmation rehearsal. If approval is withheld,
  report onboarding as incomplete, not failed.
- Never persist the profile, employee identifiers, tokens, or check transcript in Plugin files.
  Stable Plugin, policy, mapping, source-registry, terminology-registry, or workflow-version changes
  require a new conversation and a fresh self-check.
