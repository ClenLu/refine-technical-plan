# Domain Packs

Use these packs only when the artifact, repository, or user context proves the domain is relevant. Do not apply all packs blindly. A domain pack adds depth to the review without replacing universal gates.

For each selected pack, state why it applies, which checks are material, which checks are intentionally skipped, and what evidence is needed to pass.

## Frontend / UI

Use when the plan affects UI, frontend state, user workflows, design systems, accessibility, browser behavior, or client-side data.

Check:

- State ownership: server state, local state, derived state, cached state, optimistic state, form state, and URL state are separated.
- Loading, empty, error, denied, stale, partial success, retry, cancellation, and recovery states are covered.
- User-visible errors are useful without leaking sensitive details.
- Accessibility: keyboard navigation, focus management, ARIA semantics, contrast, and screen reader behavior.
- Responsive behavior: layout degradation, text overflow, small screens, large data lists, and touch targets.
- Forms: validation source of truth, dirty state, submit race, duplicate submit, partial failure, and recovery.
- Client caching: invalidation, stale data, optimistic update rollback, cross-tab behavior, and refresh behavior.
- Performance: bundle impact, rendering cost, hydration or initial load when relevant.
- Internationalization: text expansion, locale formatting, timezone, pluralization, and right-to-left support when relevant.
- The UI does not duplicate business rules that should be enforced by the backend or source of truth.

Evidence needed:

- user journey or state matrix;
- API contract examples or schema;
- accessibility, visual, or manual QA evidence for critical journeys;
- tests for error/loading/empty/permission states when risk justifies them.

## Backend / API / Service

Use when the plan affects services, APIs, jobs, RPC, queues, webhooks, permissions, or backend workflows.

Check:

- API/request/response contracts, error semantics, versioning, and backward compatibility.
- Caller inventory and migration path for breaking or semantic changes.
- Idempotency, retries, timeouts, cancellation, deduplication, ordering, and partial failure.
- Authentication, authorization, tenant/actor isolation, rate limits, quotas, and abuse cases.
- Transaction boundaries, consistency model, concurrency, race conditions, and lock behavior.
- Background jobs: retry policy, poison messages, dead-letter handling, scheduling, backpressure, and replay.
- Observability: request IDs, correlation IDs, caller/user/tenant/workflow dimensions, dashboards, and alerts.
- Deprecation and cleanup triggers for temporary compatibility.
- Runbook or operator action when production behavior degrades.

Evidence needed:

- caller/consumer inventory;
- API, event, or schema examples;
- contract tests or compatibility checks;
- permission and error-semantic tests when relevant;
- observability plan or existing conventions.

## Data / Migration

Use when the plan affects persisted data, schemas, storage formats, pipelines, search indexes, caches, analytics, or data quality.

Check:

- Source of truth and ownership are explicit.
- Expand-migrate-contract sequencing is used when consumers exist.
- Backfill is idempotent, restartable, observable, and bounded.
- Dual-write, dual-read, reconciliation, and cutover are justified and temporary.
- Data quality checks exist before and after migration.
- Rollback is realistic after irreversible transformation.
- Historical data, nulls, corrupted rows, duplicates, ordering, and timezone are considered.
- Cleanup ownership and removal trigger are explicit.
- Reporting, analytics, audit, and downstream consumers are included when relevant.

Evidence needed:

- schema, migration files, sample query, inventory, or dry-run;
- reconciliation or data quality checks;
- rollback or forward-fix plan;
- ownership and cleanup trigger.

## Infrastructure / SRE / Runtime

Use when the plan affects deployment, runtime behavior, infrastructure, configuration, capacity, reliability, or operations.

Check:

- Resource limits, concurrency, scheduling, startup, shutdown, and environment assumptions.
- Config rollout, secrets, feature flags, and environment differences.
- Capacity, performance budget, load shedding, backpressure, retry storms, and failure isolation.
- SLO/SLA impact, alerting, dashboards, runbooks, and on-call ownership.
- Rollout strategy, rollback strategy, smoke checks, and incident response.
- Dependency failure and degraded mode are explicit.
- Cost impact is proportional and monitored.
- Cleanup of temporary infrastructure, flags, queues, dashboards, and alerts is owned.

Evidence needed:

- deployment/config files;
- runbook or operational checklist;
- smoke check or rollback rehearsal plan;
- dashboard/alert signal definition;
- capacity or cost estimate for material changes.

## Security / Privacy / Compliance

Use when the plan touches identity, permissions, secrets, user data, sensitive data, audit, compliance, abuse, or external access.

Check:

- Threat model: actors, assets, trust boundaries, and attack paths.
- Authentication and authorization are enforced at the correct layer.
- Least privilege, tenant isolation, data minimization, and purpose limitation.
- Secrets handling: storage, rotation, exposure risk, logging, access scope, and incident response.
- Sensitive data: collection, retention, deletion, masking, export, audit, and support access.
- Abuse cases, rate limits, anomaly detection, and monitoring.
- Compliance or policy owner confirmation when relevant.
- Security tests, review, or signoff required for high-risk changes.

Evidence needed:

- current permission checks and ownership boundary;
- data classification or privacy impact statement when relevant;
- security/privacy owner confirmation for high-risk changes;
- tests or review evidence for authorization and data exposure.

## AI / Agent / Model Behavior

Use when the plan involves LLMs, agents, prompts, retrieval, tools, model behavior, evals, or autonomous decisions.

Check:

- Desired and prohibited behaviors have concrete examples.
- Prompt injection, tool misuse, hallucination, unsafe autonomy, data leakage, and overconfidence are handled.
- Tool permissions, side effects, rate limits, and human approval boundaries are explicit.
- Retrieval sources, freshness, authority, and fallback behavior are defined.
- Model, provider, SDK, tool API, and retrieval-system behavior is tied to pinned versions or current official authority where material.
- Evals cover riskiest behaviors, adversarial cases, and regression cases.
- Outputs are auditable, replayable, and traceable.
- Human override, escalation, and failure containment are present for real-world impact.
- The plan distinguishes model instructions from enforceable guardrails.

Evidence needed:

- prompt/tool definitions and schemas;
- eval set or adversarial examples;
- audit/logging plan;
- human escalation policy for high-impact failures;
- rollback path for model, prompt, retrieval, or tool changes.

## Mobile / Desktop Client

Use when the plan affects native mobile, desktop apps, offline behavior, app-store release, local storage, permissions, or client version fragmentation.

Check:

- Version compatibility and upgrade path.
- Offline behavior, local cache, sync conflict, and retry behavior.
- OS permissions, background execution, push notifications, and platform differences.
- App-store rollout, phased release, rollback limits, and forced update policy.
- Crash reporting, telemetry, and user support path.
- Battery, network, storage, and performance impact.
- Local data privacy and secure storage.

Evidence needed:

- version support matrix;
- offline/sync scenarios;
- crash/telemetry signal;
- staged rollout and rollback limits;
- platform permission review.

## Developer Experience / Internal Tooling

Use when the plan affects internal developer workflows, CLIs, CI, build systems, code generation, test utilities, or developer-facing docs.

Check:

- Command behavior, flags, inputs, outputs, exit codes, and error messages are explicit.
- Backward compatibility for existing scripts, CI jobs, or local workflows is considered.
- Setup and troubleshooting paths are documented.
- Failure modes are actionable rather than noisy.
- Changes reduce repeated manual work without hiding important state.
- Rollback or compatibility path exists for team workflows.

Evidence needed:

- existing commands and CI jobs;
- compatibility inventory;
- docs updates;
- before/after workflow examples;
- failure-mode examples.

## Documentation / Process

Use when the plan mainly changes docs, ADRs, runbooks, onboarding, process, or handoff material.

Check:

- The document is executable: a future engineer or agent can act without hidden conversation context.
- Claims are separated from assumptions, decisions, and open questions.
- Ownership, review cadence, and update triggers are explicit.
- Examples match real repository paths, commands, APIs, or workflows when possible.
- The doc states what evidence implementation must produce.
- Obsolete docs or conflicting guidance are updated or linked.

Evidence needed:

- target doc path and related docs inventory;
- ownership and update trigger;
- examples grounded in real project artifacts;
- explicit implementation evidence requirements.

## Repository-Aware Coding Agent / Plan-to-Code

Use when Codex, Claude Code, Cursor Agent, Devin, or another coding agent reviews or implements a plan inside a repository.

Check:

- The agent inspected repository evidence before pass-like decisions and listed concrete paths, commands, or search terms.
- The plan does not invent APIs, files, schemas, commands, owners, prompts/tools, flags, jobs, migrations, permissions, product behavior, or deployment assumptions.
- Caller/consumer inventory is searched before approving breaking or semantic changes.
- Diff-to-plan mapping exists after implementation, and every material change maps to an approved plan section, gate, or accepted risk.
- Tests validate the real contract, migration, permission, failure, UI, or agent-behavior risk rather than only the changed code.
- Temporary compatibility paths, feature flags, adapters, facades, dual paths, prompts, eval fixtures, queues, dashboards, or runbooks have owner, metric/signal, deletion trigger, and cleanup task.
- The agent distinguishes repository-backed facts from assumptions caused by context-window limits, tool failures, or inaccessible files.
- Implementation conformance review is run after code/config/migration/prompt/tool/tests/docs/deployment changes unless the user explicitly opts out.

Common failure modes:

- The agent says it inspected the repo but provides no inspectable ledger.
- The plan is internally consistent but conflicts with existing code or tests.
- The implementation silently changes public behavior beyond the approved plan.
- CI passes because tests cover the new code path but not the real caller, contract, data, permission, or rollback risk.
- A compatibility shim becomes permanent because no owner, metric, or deletion trigger was created.
- Context-window limits hide a relevant caller, migration, prompt/tool, route, or config.

Evidence needed:

- repository inspection ledger;
- caller/contract/schema/config/test inventory when material;
- diff-to-plan mapping after implementation;
- validation evidence for the riskiest gate;
- cleanup/rollback/observability evidence for production-impacting work.


## External API / Third-Party Platform Authority

Use when the plan depends on third-party APIs, SaaS platforms, cloud services, SDKs, frameworks, browsers, mobile/desktop platforms, databases, queues, search providers, identity providers, payment providers, LLM/model APIs, or partner contracts.

Check:

- The dependency, provider, API version, SDK version, runtime, platform, region, account feature, or contract variant is named.
- Repository-pinned versions, lockfiles, generated clients, deployment runtime, and environment config are inspected when available.
- Current official docs, API references, changelogs, migration guides, deprecation notices, security advisories, compatibility matrices, provider limits, rate limits, quotas, and lifecycle notices are checked when material.
- The plan distinguishes latest provider behavior from behavior of the pinned or deployed version.
- Fallback, timeout, retry, degradation, rate-limit, quota, partial failure, idempotency, and support/escalation behavior are explicit when relevant.
- Breaking changes, provider outages, contract ambiguity, and vendor-specific edge cases have detection and rollback or mitigation.
- External facts are not treated as repository-backed unless the repo contains a pinned version, generated client, recorded contract, or owner-maintained integration doc.
- A real owner, partner, vendor, security/compliance, or operator confirmation is required when external behavior can affect production, users, data, billing, auth, compliance, or irreversible work.

Evidence needed:

- official docs or API reference for the relevant version;
- changelog, migration guide, deprecation notice, compatibility matrix, or security advisory when material;
- repo-pinned version, lockfile, generated client, SDK config, or deployment runtime evidence;
- contract tests, provider sandbox/replay, smoke test, or staged rollout evidence for high-risk integrations;
- owner, partner, vendor, security/compliance, or operator confirmation when local behavior depends on non-repo facts.

## High-Risk Subdomain Packs

Use these when the affected surface makes the subdomain material.

### Auth / Permission / Access Control

Check:

- authentication and authorization are enforced at the boundary that has the required context;
- actor, tenant, role, resource, and delegation semantics are explicit;
- default-deny behavior and permission-denied UX/API semantics are defined;
- privilege escalation, confused deputy, replay, audit, and support-access paths are considered;
- tests cover both allowed and denied access.

Evidence needed:

- existing permission checks;
- policy source of truth;
- audit/logging requirement;
- security or domain owner confirmation for high-risk changes.

### Payment / Billing / Financial State

Check:

- idempotency, duplicate handling, retries, reconciliation, refund/void behavior, and ledger integrity;
- external provider contracts and webhook semantics;
- state transitions are auditable and reversible only when truly possible;
- customer-visible and support-visible failure modes are explicit;
- finance, compliance, or product owner confirmation is required for material behavior changes.

Evidence needed:

- provider contract or existing integration code;
- idempotency and reconciliation tests;
- audit trail and support runbook;
- owner confirmation for high-impact changes.

### Multi-Tenant SaaS

Check:

- tenant isolation, actor isolation, data partitioning, admin delegation, and cross-tenant support actions;
- migration or cache changes do not leak data across tenants;
- observability includes tenant and actor dimensions where safe;
- rate limits, quotas, and noisy-neighbor behavior are covered.

Evidence needed:

- tenant boundary code or schema;
- permission tests;
- sample queries or migrations scoped by tenant;
- monitoring dimensions.

### Search / Indexing

Check:

- source of truth, index freshness, backfill, reindex, ranking changes, partial indexing, and stale results;
- query semantics, filters, pagination, permissions, and tenant isolation;
- fallback behavior when indexing fails or lags;
- analytics impact and user-visible relevance changes.

Evidence needed:

- index schema and pipeline;
- reindex/dry-run plan;
- relevance or acceptance examples;
- permission-filter tests.

### Realtime / Streaming / WebSocket

Check:

- ordering, duplication, reconnect, replay, backpressure, throttling, and partial delivery;
- authorization across connection lifetime;
- degraded mode when connection drops or server pushes stale data;
- observability for dropped, delayed, duplicated, or unauthorized messages.

Evidence needed:

- protocol contract;
- reconnect/replay scenarios;
- load or backpressure test plan;
- authorization and observability checks.

### Workflow / State Machine

Check:

- states, transitions, guards, side effects, retries, cancellation, and compensation are explicit;
- illegal transitions are rejected consistently;
- partial failure and manual override paths are defined;
- state migration and historical records are handled.

Evidence needed:

- state transition table;
- invariant tests;
- audit and manual recovery path;
- migration/dry-run when existing state changes.

### Retrieval / Prompt Injection / Tool-Use Security

Check:

- retrieval source authority, freshness, and untrusted-content boundaries;
- prompt injection containment and tool permission boundaries;
- tool input/output schemas, side effects, approval gates, rate limits, and audit logs;
- safe refusal, escalation, and human override for high-impact actions.

Evidence needed:

- retrieval source inventory;
- adversarial eval cases;
- tool schema and permission evidence;
- audit/replay plan;
- rollback path for prompt/model/tool configuration.
