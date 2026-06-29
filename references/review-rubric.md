# Technical Plan Review Rubric

Use this rubric to sharpen expert-panel reviews without assuming a specific technology domain. Apply only the lenses that fit the user's artifact, codebase, and stated context.

Do not assume a web, backend, frontend, data, infrastructure, AI, or product domain unless the plan or repository evidence proves it. Derive domain-specific checks from the plan under review.

## Universal Review Lenses

- Problem fit: the plan solves the stated problem instead of optimizing adjacent symptoms.
- Scope control: goals, non-goals, deferred work, and success criteria are explicit.
- Boundary clarity: ownership, responsibilities, dependencies, public contracts, side effects, and lifecycle boundaries are named.
- Contract safety: callers, consumers, inputs, outputs, compatibility requirements, versioning, and deprecation behavior are covered.
- State correctness: source of truth, invariants, consistency, ordering, idempotency, drift, and recovery are addressed where state exists.
- Failure behavior: timeout, retry, cancellation, partial failure, fallback, rollback, cleanup, and user/consumer-visible degradation are explicit where relevant.
- Validation strategy: tests, simulations, contract checks, manual checks, migration checks, observability, evals, and acceptance criteria match the blast radius.
- Operability: configuration, deployment or release mechanism, ownership, monitoring, alerts, runbooks, support burden, and cleanup are realistic.
- Security and abuse resistance: authentication, authorization, secrets, privacy, tenant or actor isolation, data exposure, prompt/tool misuse, and abuse cases are considered when applicable.
- Evidence and assumption traceability: material decisions are backed by artifact evidence, named assumptions, or concrete validation steps.
- Reversibility: rollout, rollback, cleanup, and irreversible decisions are explicit.
- Evolution and maintainability: the plan reduces meaningful complexity or risk, names tradeoffs, and avoids long-lived ambiguity.
- Cost and complexity: runtime cost, operational cost, implementation complexity, and coordination cost are proportionate to the value and risk.
- Non-expert legibility: a user outside the domain can identify the red lines, accepted tradeoffs, required confirmations, and implementation evidence to preserve.

## Context Intake Questions

Ask or infer these before deep critique. Ask the user only when the missing answer materially changes the design or pass/fail status.

- What artifact is being reviewed: draft plan, PRD, architecture proposal, migration plan, code diff, rollout plan, or docs update?
- What change type is this: feature, refactor, migration, integration, platform/runtime, data/state, UI/workflow, AI/agent behavior, or documentation-only?
- What surfaces are affected: users, callers, APIs, events, database, UI states, permissions, jobs, configs, deployment, monitoring, support, or docs?
- What evidence is available: plan text, code, docs, schemas, logs, tests, configs, prior discussion, or user-confirmed facts?
- What facts are unknown but materially affect safety?
- What is the blast radius: low, medium, or high?
- Is the change reversible, partially reversible, or irreversible?
- What are the critical contracts and invariants?

## Change-Type Lenses

### Architecture / Boundary Change

- The new boundary is named and justified by responsibilities, not by file layout or team preference alone.
- Responsibilities move to the layer or owner that can enforce the relevant invariant.
- Cross-cutting behavior is centralized only when it is truly cross-cutting.
- The plan states what becomes easier and what becomes harder after the change.
- The plan names which coupling is removed and which coupling is intentionally retained.
- The transition path prevents half-old/half-new ownership ambiguity.

### Refactor / Replacement / Migration

- The target design is defined before implementation sequencing.
- Compatibility is preserved until affected callers, users, data, configuration, or workflows are migrated.
- Any adapter, dual path, compatibility shell, feature flag, or temporary bridge has an owner, metric, and deletion trigger.
- Direct refactor is preferred when the old boundary is the debt source, dual paths would create drift, callers/contracts are known, and rollback/acceptance checks are credible.
- Incremental migration is preferred when blast radius is high, assumptions need production evidence, or external consumers cannot be coordinated atomically.
- Behavioral equivalence or intentional behavior change is verifiable before and after the refactor.
- Migration steps are ordered by dependency, reversibility, and ability to validate risk early.

### New Capability / Feature

- The plan covers the full workflow, not only the happy path.
- Actor-visible states are explicit: unavailable, loading/progress, empty, permission denied, invalid input, partial success, retry, and recovery where applicable.
- Edge cases are tied to domain invariants instead of listed generically.
- The acceptance criteria prove user or consumer value, not just implementation completion.
- The plan names which user, caller, or operator behavior is intentionally changed.
- The plan defines how the capability is disabled, rolled back, or degraded if it causes harm.

### Integration / External Dependency

- Trust boundaries, ownership boundaries, and failure boundaries are explicit.
- Input shaping, policy decisions, capability selection, and side effects are owned by the correct layer.
- Error semantics preserve meaningful distinctions instead of collapsing distinct failures into vague responses.
- Versioning, compatibility, rate limits, quotas, timeout budgets, retries, and degradation behavior are addressed when relevant.
- Correlation identifiers, audit signals, or traceability exist when cross-boundary debugging matters.
- The plan states what happens when the dependency is slow, wrong, unavailable, partially successful, or returns unexpected data.

### State, Data, and Migration

- State ownership and source of truth are explicit.
- Transitions are backward-compatible or sequenced with expand-migrate-contract when existing consumers or persisted state are affected.
- Migration steps are idempotent, restartable, or explicitly bounded by a safe operational procedure.
- Rollback behavior is realistic, especially after irreversible transformation.
- Reconciliation, data quality checks, and cleanup ownership are included when state can drift.
- Data shape assumptions are backed by schema, query, sample, inventory, or dry-run evidence.
- The plan distinguishes reversible rollout from irreversible data transformation.

### Human-Facing Experience / Consumer Workflow

- The plan names the actors and their critical journeys.
- The experience is resilient to slow, failed, partial, denied, or stale responses.
- Accessibility, localization, responsiveness, text overflow, input ergonomics, and keyboard/screen-reader behavior are considered when humans directly use the output.
- Documentation, discoverability, and support burden are covered when the change affects operators, developers, or end users.
- UI or workflow states are not inferred from happy-path backend behavior alone.
- User-visible degradation and recovery paths are explicit.

### Backend / API / Service Behavior

Apply this lens only when the plan affects backend, API, service, worker, or server-side behavior.

- Caller inventory is known or discovery work is explicitly planned.
- Public behavior is specified through request/response examples, events, schemas, permissions, status codes, error bodies, or CLI/tool output.
- Idempotency, retries, ordering, concurrency, and duplicate handling are addressed where relevant.
- Authorization and actor/tenant isolation are checked at the boundary that can enforce them.
- Error semantics are compatible with existing clients or the migration plan covers the change.
- Observability can identify caller, request, path, downstream, failure class, and impact.

### Frontend / UI / Client Workflow

Apply this lens only when the plan affects frontend, UI, native app, CLI, or human-facing client behavior.

- State ownership is clear: local state, server state, cache, URL state, form state, optimistic state, or persisted state.
- Loading, empty, error, permission denied, partial success, stale data, retry, cancellation, and recovery states are explicit.
- Accessibility, responsiveness, localization, text overflow, input validation, and focus behavior are considered where relevant.
- Client-side validation does not drift from the true server or domain contract.
- The plan covers analytics, support/debug signals, and manual QA for critical journeys when relevant.
- The UX can degrade safely if backend, network, feature flag, or dependency behavior changes.

### Platform / Runtime / Operations

- Runtime assumptions are explicit: resource limits, concurrency, scheduling, deployment, startup/shutdown, configuration, and environment differences where relevant.
- Observability can answer what changed, who or what was affected, where it failed, and why.
- Rollout, rollback, smoke checks, and cleanup match the production or release risk.
- Operational ownership after release is clear.
- The plan names runbook steps or incident response actions for high-risk operations.
- Capacity, cost, rate limits, and failure isolation are considered when traffic or workload can change.

### AI / Agent / Model Behavior

Apply this lens only when the plan affects AI behavior, agent workflows, LLM tool use, automated decisions, model outputs, or prompt-driven systems.

- The plan distinguishes deterministic software contracts from probabilistic model behavior.
- Evaluation data, success metrics, failure examples, and regression checks are explicit.
- Prompt injection, tool misuse, unauthorized action, data exfiltration, and hallucinated output are considered where relevant.
- Tool boundaries are concrete: allowed tools, denied tools, input/output schema, side effects, permissions, and audit trail.
- Human override, escalation, fallback, or safe refusal behavior is defined for high-impact failures.
- The plan states how model or prompt changes are rolled out, monitored, rolled back, and compared against baseline behavior.
- Privacy, retention, logging, and user-data exposure are covered when model or tool calls cross boundaries.

### Documentation-Only / Planning Artifact

Apply this lens when the output is a plan document rather than executable code.

- The document is actionable enough that implementers do not need hidden conversation context.
- It names validation strategy even if tests are not run yet.
- It records decisions, tradeoffs, rejected alternatives, accepted risks, and unresolved assumptions concisely.
- It avoids preserving every intermediate critique when only final decisions matter.
- It includes enough evidence and acceptance criteria for a later agent or engineer to implement safely.

## Domain Pack Generation

After context intake, derive a temporary checklist. Use this to avoid cargo-cult critique.

```markdown
Domain Pack:
- Applicable lenses: <backend/api, frontend/ui, data/migration, infra/runtime, AI/agent, product/ops, etc.>
- Critical contracts/invariants: ...
- Highest-risk failure modes: ...
- Evidence needed to pass: ...
- Checks intentionally skipped: ...
```

Examples of evidence needed by domain:

- Frontend/UI: user journeys, state matrix, API contract examples, visual/manual QA, accessibility checks, error/loading/empty states.
- Backend/API: caller inventory, API examples, schema/event contracts, auth rules, idempotency behavior, contract tests.
- Data/Migration: source-of-truth definition, schema, data sample/inventory, dry-run result, reconciliation, rollback/cleanup plan.
- Infra/Runtime: deployment mechanism, config, resource constraints, observability, runbook, smoke checks.
- AI/Agent: eval set, tool schema, policy boundaries, human override, prompt/tool attack scenarios, audit logs, rollback path.

## Evidence Matrix Questions

Ask these when deciding whether findings are evidence-backed, assumption-backed, or validation-backed:

- What exact text, code, schema, config, test, log, or user-provided fact supports the conclusion?
- Which important facts are still assumptions?
- What validation would turn the assumption into evidence?
- Which assumption, if false, would invalidate the design?
- Which missing fact is severe enough to mark `Blocked`?
- Which claims sound plausible but are not actually grounded in the artifact?

## Adversarial Review Prompts

Use these before final pass-like decisions:

- What assumption would invalidate the plan?
- What hidden coupling could make implementation unsafe?
- What temporary compatibility path could become permanent debt?
- What test would falsely pass while production still breaks?
- What would a future maintainer misunderstand?
- What rollback path only works in theory but not after data or external behavior changes?
- What user, caller, or operator impact is easy to miss because the plan is written from the implementer's perspective?
- What would an agent hallucinate because the plan lacks concrete contracts?

## Domain Adaptation Questions

Ask these before adding domain-specific critique:

- What kind of system, artifact, workflow, or contract is being changed?
- Who are the primary actors, consumers, operators, and maintainers?
- What are the critical contracts or invariants in this domain?
- What state can become corrupt, stale, incompatible, duplicated, or hard to reconcile?
- What failure mode would be most expensive, dangerous, or hard to detect?
- What would make this change hard to modify, remove, or operate later?
- What evidence would prove the plan works in this specific domain?
- Which checks are irrelevant and should be skipped to avoid cargo-cult review?
- What must a non-domain user monitor so they are not misled by a confident agent-generated implementation?

## Passing Criteria

Mark `Pass` only if the plan:

- can be implemented without relying on unstated discussion context;
- has no unresolved high-impact ambiguity;
- names the important boundaries, contracts, states, and ownership model;
- has a credible validation path for the riskiest behavior;
- includes rollout, rollback, and cleanup where user, consumer, state, or production impact exists;
- makes tradeoffs explicit enough that a future maintainer can understand why the design exists;
- ties material conclusions to evidence or concrete validation, not just plausible reasoning;
- has survived a concrete pre-pass stress test for likely, expensive/user-visible, and hard-to-detect failures.

Mark `Pass under assumptions` when no blocker or major issue remains if explicitly listed assumptions are true, but one or more important facts are not independently verified.

Mark `Pass with notes` when only minor improvements remain and they do not affect implementation safety.

Mark `Revise` when the plan has any unclear boundary, unvalidated assumption, unsafe migration path, weak validation strategy, likely long-lived debt, missing stress-test handling, or failed gate.

Mark `Blocked` only when essential input is unavailable and reviewing further would require guessing.

## Non-Expert Decision Brief

When the user is likely outside the domain, provide a short control summary:

- Top 3 things to watch.
- Red lines that must not be accepted.
- Tradeoffs that are acceptable if owned and bounded.
- Questions requiring real owner, code, production, or business confirmation.
- Evidence a later agent implementation must preserve: tests, contracts, screenshots, dry-runs, dashboards, logs, eval results, rollback proof, or acceptance artifacts.

Keep this brief separate from the technical plan so it helps the user steer future agent work without pretending to be an expert.
