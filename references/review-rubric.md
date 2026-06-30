# Technical Plan Review Rubric

Use this rubric to sharpen expert-panel reviews and plan-to-code conformance reviews without assuming a specific technology domain. Apply only the lenses that fit the user's artifact, codebase, implementation diff, and stated context.

Do not assume a web, backend, frontend, data, infrastructure, AI, or product domain unless the plan or repository evidence proves it. Derive domain-specific checks from the plan under review.

## Context Intake Questions

Before applying review lenses, identify:

- What artifact is being reviewed: plan, PRD, architecture proposal, code diff, migration plan, docs update, or agent-generated proposal?
- What kind of change is this: feature, refactor, migration, integration, platform/runtime, state/data, UI/workflow, AI/agent behavior, or process/docs?
- Who are the affected users, callers, consumers, operators, and maintainers?
- What contracts, state, permissions, workflows, deployments, prompts/tools, or docs are affected?
- What evidence is available: plan text, code, docs, schemas, configs, logs, tests, metrics, evals, traces, or prior discussion?
- In repo-aware mode, which files, directories, tests, schemas, configs, docs, routes, prompts/tools, migrations, or CI commands were inspected before review?
- Which repository areas were not inspected but are material?
- What facts are unknown but materially affect pass/fail status?
- What is the blast radius?
- Is the change reversible, partially reversible, or irreversible?


## Repository-Aware Coding Agent Lens

Use this lens whenever the reviewer runs inside Codex, Claude Code, Cursor Agent, Devin, or another repository-aware coding agent.

- Repository inspection is mandatory before pass-like, implementation-ready, or implementation-conformance decisions.
- The reviewer lists inspected paths, commands, search terms, and material areas not inspected.
- Repo-discoverable facts are not asked from the user unless tools or permissions prevent inspection.
- Plan claims are checked against code, schemas, configs, tests, routes, migrations, prompts/tools, docs, runbooks, deployment files, and CI commands.
- Caller inventory is searched before approving breaking, semantic, permission, schema, prompt/tool, or workflow changes.
- Existing tests are inspected to determine what behavior is already protected and what false confidence they may create.
- If context-window limits prevent full inspection, the reviewer narrows scope, states what was not inspected, and marks related conclusions as assumptions.
- Repository evidence conflicts override plan text unless an owner decision or newer artifact proves the plan intentionally changes behavior.

Use this compact ledger when repo evidence matters:

| Area | Paths / Commands / Search Terms | Why Inspected | Finding | Confidence |
| --- | --- | --- | --- | --- |

## Universal Review Lenses

- Problem fit: the plan solves the stated problem instead of optimizing adjacent symptoms.
- Scope control: goals, non-goals, deferred work, and success criteria are explicit.
- Boundary clarity: ownership, responsibilities, dependencies, public contracts, side effects, and lifecycle boundaries are named.
- Contract safety: callers, consumers, inputs, outputs, compatibility requirements, versioning, and deprecation behavior are covered.
- Evidence quality: major claims are backed by plan text, repository facts, docs, schemas, configs, logs, tests, evals, validation results, owner confirmation, or explicitly named assumptions.
- State correctness: source of truth, invariants, consistency, ordering, idempotency, drift, and recovery are addressed where state exists.
- Failure behavior: timeout, retry, cancellation, partial failure, fallback, rollback, cleanup, and user/consumer-visible degradation are explicit where relevant.
- Validation strategy: tests, simulations, contract checks, manual checks, migration checks, observability, evals, and acceptance criteria match the blast radius.
- Operability: configuration, deployment or release mechanism, ownership, monitoring, alerts, runbooks, support burden, and cleanup are realistic.
- Security and abuse resistance: authentication, authorization, secrets, privacy, actor isolation, data exposure, and misuse cases are considered when applicable.
- Evolution and maintainability: the plan reduces meaningful complexity or risk, names tradeoffs, and avoids long-lived ambiguity.
- Cost and complexity: runtime cost, operational cost, implementation complexity, and coordination cost are proportionate to the value and risk.

## Change-Type Lenses


### Implementation Conformance / Diff Review

Apply this lens when code, config, migration, prompt/tool, tests, docs, or deployment changes have been produced from an approved plan.

- Every material diff maps to an approved plan section, gate, accepted risk, or explicitly reviewed deviation.
- The implementation does not invent unreviewed APIs, files, schemas, commands, owners, prompts/tools, flags, jobs, migrations, permissions, or product behavior.
- Public contracts, caller behavior, permissions, error semantics, data shape, rollout, rollback, observability, and cleanup still match the approved plan.
- Tests prove the approved risk controls and not only the happy path or changed lines.
- CI, dry-run, eval, smoke, screenshot, dashboard, rollback rehearsal, or owner-confirmation evidence is linked where required.
- Temporary compatibility paths have owner, metric/signal, deletion trigger, and cleanup task.
- Any deviation is classified as `Minor`, `Major`, `Blocker`, or `Accepted Risk` instead of silently becoming the new plan.

Use this mapping:

| Changed Area / File | Approved Plan Section / Gate | Match? | Evidence | Notes |
| --- | --- | --- | --- | --- |

### Architecture / Boundary Change

- The new boundary is named and justified by responsibilities, not by file layout or team preference alone.
- Responsibilities move to the layer or owner that can enforce the relevant invariant.
- Cross-cutting behavior is centralized only when it is truly cross-cutting.
- The plan states what becomes easier and what becomes harder after the change.

### Refactor / Replacement / Migration

- The target design is defined before implementation sequencing.
- Compatibility is preserved until affected callers, users, data, configuration, or workflows are migrated.
- Any adapter, dual path, compatibility shell, feature flag, or temporary bridge has an owner, metric, and deletion trigger.
- Direct refactor is preferred when the old boundary is the debt source, dual paths would create drift, callers/contracts are known, and rollback/acceptance checks are credible.
- Incremental migration is preferred when blast radius is high, assumptions need production evidence, or external consumers cannot be coordinated atomically.
- Behavioral equivalence or intentional behavior change is verifiable before and after the refactor.

### New Capability / Feature

- The plan covers the full workflow, not only the happy path.
- Actor-visible states are explicit: unavailable, loading/progress, empty, permission denied, invalid input, partial success, retry, and recovery where applicable.
- Edge cases are tied to domain invariants instead of listed generically.
- The acceptance criteria prove user or consumer value, not just implementation completion.

### Backend / API / Service

- API contracts, request/response schemas, error semantics, and versioning are explicit.
- Idempotency, retries, timeouts, cancellation, deduplication, and partial failure are handled where relevant.
- Authorization, authentication, rate limits, quotas, tenant isolation, and actor isolation are considered.
- Database or state changes preserve invariants and have migration/rollback strategy.
- Observability can identify which caller, request, user, tenant, workflow, or dependency failed.
- Backward compatibility and caller migration are addressed before breaking existing consumers.

### Frontend / UI / Human Experience

- State ownership is explicit: server state, local state, derived state, optimistic state, cached state, and URL state.
- Loading, empty, error, permission denied, stale data, partial success, retry, and recovery states are covered.
- Accessibility, keyboard behavior, focus management, screen reader semantics, and color/contrast are considered where relevant.
- Responsive behavior, text overflow, localization, and layout degradation are considered.
- User-visible error messages preserve useful meaning without leaking sensitive details.
- The plan avoids duplicating domain rules in UI when the backend or source of truth should enforce them.

### Integration / External Dependency

- Trust boundaries, ownership boundaries, and failure boundaries are explicit.
- Input shaping, policy decisions, capability selection, and side effects are owned by the correct layer.
- Error semantics preserve meaningful distinctions instead of collapsing distinct failures into vague responses.
- Versioning, compatibility, rate limits, quotas, timeout budgets, retries, and degradation behavior are addressed when relevant.
- Correlation identifiers, audit signals, or traceability exist when cross-boundary debugging matters.

### State, Data, and Migration

- State ownership and source of truth are explicit.
- Transitions are backward-compatible or sequenced with expand-migrate-contract when existing consumers or persisted state are affected.
- Migration steps are idempotent, restartable, or explicitly bounded by a safe operational procedure.
- Rollback behavior is realistic, especially after irreversible transformation.
- Reconciliation, data quality checks, and cleanup ownership are included when state can drift.

### Human-Facing Experience / Consumer Workflow

- The plan names the actors and their critical journeys.
- The experience is resilient to slow, failed, partial, denied, or stale responses.
- Accessibility, localization, responsiveness, text overflow, or ergonomics are considered when humans directly use the output.
- Documentation, discoverability, and support burden are covered when the change affects operators, developers, or end users.

### Platform / Runtime / Operations

- Runtime assumptions are explicit: resource limits, concurrency, scheduling, deployment, startup/shutdown, configuration, and environment differences where relevant.
- Observability can answer what changed, who or what was affected, where it failed, and why.
- Rollout, rollback, smoke checks, and cleanup match the production or release risk.
- Operational ownership after release is clear.

### Security / Privacy / Compliance

- Threat model is proportional to the risk: actors, assets, trust boundaries, and attack paths are named when relevant.
- Authentication and authorization are enforced at the correct layer.
- Least privilege, tenant isolation, data minimization, and purpose limitation are considered.
- Secrets handling, sensitive data logging, retention, deletion, masking, export, and audit needs are covered where relevant.
- Compliance, policy, or domain-owner confirmation is required for high-risk changes.

### AI / Agent / Model Behavior

Use only when the plan involves LLMs, agents, model behavior, prompts, tools, retrieval, evals, or autonomous workflows.

- Desired and prohibited behaviors are specified with examples or eval cases.
- Hallucination, prompt injection, tool misuse, unsafe autonomy, data leakage, and overconfident answers are considered.
- Human override, auditability, replayability, traceability, and failure containment are covered when actions have real impact.
- Evals or test scenarios cover the riskiest behaviors, not just happy-path responses.
- Model assumptions, tool permissions, retrieval sources, and fallback behavior are explicit.
- The plan distinguishes "model should" from enforceable guardrails.

### Documentation-only / Planning Artifact

Use when the artifact is a plan, ADR, PRD-to-engineering proposal, or implementation-ready document.

- The document can guide implementation without relying on hidden conversation context.
- Claims are separated from assumptions and unknowns.
- Decisions are concrete enough to constrain future agent or engineer behavior.
- Validation strategy is specified even if tests are not run during planning.
- Review history preserves material decisions and accepted risks without bloating the document.
- The document states what evidence must be produced during implementation.

## Domain Adaptation Questions

Ask these before adding domain-specific critique:

- What kind of system, artifact, workflow, or contract is being changed?
- Who are the primary actors, consumers, operators, and maintainers?
- What are the critical contracts or invariants in this domain?
- What state can become corrupt, stale, incompatible, duplicated, or hard to reconcile?
- What failure mode would be most expensive, dangerous, or hard to detect?
- What would make this change hard to modify, remove, or operate later?
- What evidence would prove the plan works in this specific domain?
- What assumption would invalidate the plan if false?
- Which checks are irrelevant and should be skipped to avoid cargo-cult review?

## Domain Pack Selection

After answering the domain adaptation questions, select only the relevant domain packs from `references/domain-packs.md`.

For each selected pack, state:

- why it applies;
- which checks are material to this plan;
- which checks are intentionally skipped;
- what evidence is needed to pass those checks.

Do not apply a domain pack just because a keyword appears. Apply it only when the artifact, codebase, user context, or affected surface makes the domain relevant.


## Evidence Collection Questions

Use these when a plan is blocked or conditional because evidence is missing:

- Is the missing fact discoverable from the repository, validation artifact, production observation, or owner confirmation?
- What exact path, command, search term, test, dry-run, eval, smoke check, dashboard, log, screenshot, or owner is needed?
- Can implementation start only as discovery, spike, or reversible guarded work?
- What evidence would upgrade `Pass under assumptions` to absolute `Pass`?
- Which assumption, if false, would invalidate the implementation?
- Which evidence must a later coding agent preserve in the final diff or PR?

## Evidence Matrix Questions

For material findings and pass-like decisions, ask:

- Which claim is evidence-backed?
- Which claim is repository-backed?
- Which claim is assumption-backed?
- Which claim is validation-backed?
- Which claim lacks evidence but affects design safety?
- Which missing fact is serious enough to mark `Blocked`?
- Which findings are based only on plausible reasoning?
- What evidence would upgrade `Pass under assumptions` to absolute `Pass`?

## Adversarial Review Prompts

Use before final pass-like decisions:

- What assumption would invalidate the plan?
- What hidden coupling could make implementation unsafe?
- What temporary compatibility path could become permanent debt?
- What test would falsely pass while production still breaks?
- What would a future maintainer misunderstand?
- What rollback path only works in theory but not after data or external behavior changes?
- What would an agent hallucinate because the plan lacks concrete contracts?
- What repository fact would most quickly disprove the plan?

## Passing Criteria

Mark `Pass` only if the plan:

- can be implemented without relying on unstated discussion context;
- has no unresolved high-impact ambiguity;
- names the important boundaries, contracts, states, and ownership model;
- aligns with inspected repository evidence when available;
- has a credible validation path for the riskiest behavior;
- includes rollout, rollback, and cleanup where user, consumer, state, or production impact exists;
- makes tradeoffs explicit enough that a future maintainer can understand why the design exists.

Mark `Pass under assumptions` when the plan is coherent only if explicitly listed material assumptions hold.

Mark `Pass with notes` when only minor improvements remain and they do not affect implementation safety.

Mark `Revise` when the plan has any unclear boundary, unvalidated assumption, unsafe migration path, weak validation strategy, repository conflict, or likely long-lived debt.

Mark `Blocked` only when essential input is unavailable and reviewing further would require guessing.

## Non-Expert Decision Brief

When the user is likely outside the domain, provide a short control summary:

- Top 3 things to watch.
- Red lines that must not be accepted.
- Tradeoffs that are acceptable only if owned and bounded.
- Questions requiring real owner, code, production, or business confirmation.
- Evidence a later agent implementation must preserve: repository inspection ledger, diff-to-plan mapping, tests, contracts, screenshots, dry-runs, dashboards, logs, eval results, rollback proof, cleanup trigger, or acceptance artifacts.
- The simplest next instruction the user should give to a later implementation agent.

Keep this brief separate from the technical plan so it helps the user steer future agent work without pretending to be an expert.
