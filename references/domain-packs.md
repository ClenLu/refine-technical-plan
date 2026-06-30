# Domain Packs

Use these packs only when the artifact, repository, or user context proves the domain is relevant. Do not apply all packs blindly.

A domain pack adds depth to the review without replacing universal gates.

## Frontend / UI

Use when the plan affects UI, frontend state, user workflows, design systems, accessibility, browser behavior, or client-side data.

Check:

- State ownership: server state, local state, derived state, cached state, optimistic state, and URL state are separated.
- Loading, empty, error, denied, stale, partial success, retry, and recovery states are covered.
- User-visible errors are useful without leaking sensitive details.
- Accessibility: keyboard navigation, focus management, ARIA semantics, contrast, screen reader behavior.
- Responsive behavior: layout degradation, text overflow, small screens, large data lists.
- Forms: validation source of truth, dirty state, submit race, duplicate submit, partial failure.
- Client caching: invalidation, stale data, optimistic update rollback, cross-tab behavior.
- Performance: bundle impact, rendering cost, hydration or initial load when relevant.
- Internationalization: text expansion, locale formatting, timezone, pluralization when relevant.
- The UI does not duplicate business rules that should be enforced by the backend or source of truth.

## Backend / API / Service

Use when the plan affects services, APIs, jobs, RPC, queues, webhooks, permissions, or backend workflows.

Check:

- API/request/response contracts, error semantics, versioning, and backward compatibility.
- Caller inventory and migration path for breaking or semantic changes.
- Idempotency, retries, timeouts, cancellation, deduplication, and partial failure.
- Authentication, authorization, tenant/actor isolation, rate limits, quotas, and abuse cases.
- Transaction boundaries, consistency model, concurrency, ordering, and race conditions.
- Background jobs: retry policy, poison messages, dead-letter handling, scheduling, backpressure.
- Observability: request IDs, correlation IDs, caller/user/tenant/workflow dimensions, dashboards, alerts.
- Deprecation and cleanup triggers for temporary compatibility.
- Runbook or operator action when production behavior degrades.

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
- Cleanup of temporary infra, flags, queues, dashboards, and alerts is owned.

## Security / Privacy / Compliance

Use when the plan touches identity, permissions, secrets, user data, sensitive data, audit, compliance, abuse, or external access.

Check:

- Threat model: actors, assets, trust boundaries, attack paths.
- Authentication and authorization are enforced at the correct layer.
- Least privilege, tenant isolation, data minimization, and purpose limitation.
- Secrets handling: storage, rotation, exposure risk, logging, access scope.
- Sensitive data: collection, retention, deletion, masking, export, audit.
- Abuse cases, rate limits, anomaly detection, and monitoring.
- Compliance or policy owner confirmation when relevant.
- Security tests, review, or signoff required for high-risk changes.

## AI / Agent / Model Behavior

Use when the plan involves LLMs, agents, prompts, retrieval, tools, model behavior, evals, or autonomous decisions.

Check:

- Desired and prohibited behaviors have concrete examples.
- Prompt injection, tool misuse, hallucination, unsafe autonomy, data leakage, and overconfidence are handled.
- Tool permissions, side effects, rate limits, and human approval boundaries are explicit.
- Retrieval sources, freshness, authority, and fallback behavior are defined.
- Evals cover riskiest behaviors, adversarial cases, and regression cases.
- Outputs are auditable, replayable, and traceable.
- Human override, escalation, and failure containment are present for real-world impact.
- The plan distinguishes "model should" from enforceable guardrails.

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

## Developer Experience / Internal Tooling

Use when the plan affects internal developer workflows, CLIs, CI, build systems, code generation, test utilities, or developer-facing docs.

Check:

- Command behavior, flags, inputs, outputs, exit codes, and error messages are explicit.
- Backward compatibility for existing scripts, CI jobs, or local workflows is considered.
- Setup and troubleshooting paths are documented.
- Failure modes are actionable rather than noisy.
- Changes reduce repeated manual work without hiding important state.
- Rollback or compatibility path exists for team workflows.

## Documentation / Process

Use when the plan mainly changes docs, ADRs, runbooks, onboarding, process, or handoff material.

Check:

- The document is executable: a future engineer or agent can act without hidden conversation context.
- Claims are separated from assumptions, decisions, and open questions.
- Ownership, review cadence, and update triggers are explicit.
- Examples match real repository paths, commands, APIs, or workflows when possible.
- The doc states what evidence implementation must produce.
- Obsolete docs or conflicting guidance are updated or linked.

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

Evidence needed to pass:

- Repository inspection ledger.
- Caller/contract/schema/config/test inventory when material.
- Diff-to-plan mapping after implementation.
- Validation evidence for the riskiest gate.
- Cleanup/rollback/observability evidence for production-impacting work.
