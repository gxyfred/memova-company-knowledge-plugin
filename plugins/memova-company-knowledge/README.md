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
uses an exact submission request or exact candidate selection to authorize one ephemeral preview.
It does not ask for a redundant preview-creation confirmation. The ordinary query Skill still
cannot call publication tools, and durable publication requires the single final confirmation after
the complete server preview. A check request, ambiguous confirmation, or changed preview never
authorizes publication.

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

Version `0.4.5` removes the redundant confirmation before ephemeral preview creation. An explicit
request to submit exact selected content or an exact candidate selection authorizes one prepare
call; only the complete server preview receives the single final publication confirmation. Preview
hash, request identity, audit, delegated ownership, fail-closed validation and no-auto-publish
protections remain unchanged.

Version `0.4.7` makes onboarding checks deterministic. The read-only answer probe uses a frozen
pre-platform empty-snapshot boundary and receives the effective policy version on the MCP result. Synthetic preview
cancellation and ordinary current-task `general_knowledge` now use a typed server-owned route that
derives source locator, revision, evidence anchors, hashes and owner without an employee-provided
HTTPS URL.

Version `0.4.8` makes ordinary employee onboarding a read-only readiness check and moves synthetic
preview/negative checks behind an explicitly requested administrator/QA deep mode. Protected MCP
calls are strictly sequential with bounded refresh-boundary recovery. The server owns onboarding
rehearsal inputs, binds submit request identity from the authenticated preview, rejects all reserved
sensitive markers, and returns safe SharePoint Author readback for new publications. Synthetic
onboarding content is never durably published or added to normal company search.

Every active employee publication still uses that employee's request-scoped Microsoft OBO
identity. The Plugin does not permit a shared Publisher identity or job-based submission allowlist;
Chenchen may normally query without submitting by choice.

The repository-level [employee operations](../../docs/employee-operations.md) and
[administrator operations](../../docs/admin-operations.md) guides define the S10-04 usage and
release boundaries. Version `0.4.9` removes the redundant employee identity reconfirmation: the
delegated Microsoft token and active employee-directory resolution are authoritative, while the
safe UPN/display name is shown only for transparency and server-detected mismatch still fails
closed.

Version `0.4.10` preserves the employee's natural-language question at the query boundary. The
query Skill no longer expands a question with guessed code, test, deployment, or evidence terms,
and it sends conversation/parent identifiers only as a valid pair. The server classifies natural
completion and ETA wording, keeps business status additive to delivery facets, and uses inferred
receipt routes as evidence priorities instead of destructive search filters. The current manifest
is `0.4.10`; OAuth bootstrap remains unchanged from
`0.4.1`, current-task status and general-knowledge provenance remain server-owned, submit identity
stays bound to the prepared candidate, and only the final server preview requires a publication
confirmation.

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
