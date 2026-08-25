# 公司知识助手 Plugin

This is the independent, thin Plugin package for the Memova Company Knowledge Platform. The
authoritative product decisions remain in the Obsidian Master PRD; this directory is a derived
installable artifact.

The package contains separate universal-query, explicit onboarding/self-check, and explicit
current-task submission Skills plus the dedicated `company_knowledge_assistant` MCP declaration.
It intentionally contains no authentication code, tokens, employee or job allowlists, server
policy, retrieval implementation, third-party connector, Hook, background collector, custom UI, or
Memova product endpoint.

S10-03 adds the general P0 submission Skill. It starts only from an explicit employee request,
assesses at most three current-task candidates, routes each fact to one of seven receipt types, and
requires separate exact approvals for ephemeral preparation and durable publication. The ordinary
query Skill still cannot call publication tools. A check request, candidate selection, earlier
approval, ambiguous confirmation, or changed preview never authorizes publication.

The production URL in `.mcp.json` is the current Pilot MCP binding. The independently validated
public distribution repository is `gxyfred/memova-company-knowledge-plugin`; employees can read it
without GitHub login and should follow its fixed installation prompt.

Version `0.4.1` adds a deterministic first-login bootstrap before protected-tool discovery. It
classifies an absent, disabled, unauthenticated, or authenticated-but-unbound MCP separately and
directs an unauthenticated employee through `codex mcp login company_knowledge_assistant` using
their own Microsoft browser session. It never receives credentials and requires a full Codex
restart and new conversation after first login.

Version `0.4.2` exposes the complete, dereferenced `ReceiptCandidateV1` contract on the MCP prepare
tool and bundles the same generated schema with the submission Skill. Invalid candidates now return
safe field paths, so employees can correct the candidate without revealing rejected content or
guessing server fields.

Version `0.4.3` adds a bounded `current_task_business_status` prepare input. For status facts
selected from the current Codex task, the employee and model provide only the selected semantic
fields; the server derives the authenticated knowledge owner and constructs the fixed Codex source
route, locator, revision, immutable anchor, evidence manifest and evidence hash. Employees no
longer need an unrelated HTTPS source link for this route.

Version `0.4.4` fixes the submit confirmation binding. Codex must copy the exact
`client_request_id` from the prepared `normalized_candidate` together with the same preview ID and
preview hash; generating a new request ID at submit is explicitly forbidden. The server's existing
fail-closed idempotency check is unchanged.

Every active employee publication still uses that employee's request-scoped Microsoft OBO
identity. The Plugin does not permit a shared Publisher identity or job-based submission allowlist;
Chenchen may normally query without submitting by choice.

The repository-level [employee operations](../../docs/employee-operations.md) and
[administrator operations](../../docs/admin-operations.md) guides define the S10-04 usage and
release boundaries. The current manifest is `0.4.4`; OAuth behavior remains unchanged from
`0.4.1`, current-task status provenance remains server-owned, and submit identity now stays bound
to the prepared candidate.

Validate locally from the repository root:

```bash
.venv/bin/python /Users/gxyfred/.codex/skills/.system/plugin-creator/scripts/validate_plugin.py \
  plugins/memova-company-knowledge
.venv/bin/python -m pytest tests/contract/test_plugin_contract.py \
  tests/contract/test_plugin_onboarding_contract.py \
  tests/contract/test_plugin_submission_contract.py \
  tests/acceptance/test_plugin_onboarding_acceptance.py \
  tests/acceptance/test_plugin_submission_acceptance.py \
  tests/security/test_plugin_security.py
```
