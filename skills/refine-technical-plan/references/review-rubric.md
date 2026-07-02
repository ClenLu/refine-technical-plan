# Technical Plan Review Rubric

Use this rubric to sharpen expert-panel reviews and plan-to-code conformance reviews without assuming a specific technology domain. Apply only the lenses that fit the user's artifact, codebase, implementation diff, and stated context.

Do not assume a web, backend, frontend, data, infrastructure, AI, product, or operations domain unless the artifact or repository evidence proves it.

## Context Intake Questions

Before deep review, identify:

- What artifact is being reviewed: plan, PRD, architecture proposal, code diff, migration plan, rollout plan, docs update, or agent-generated proposal?
- What kind of change is this: feature, refactor, migration, integration, platform/runtime, state/data, UI/workflow, AI/agent behavior, documentation/process, or mixed?
- Who are the affected users, callers, consumers, operators, and maintainers?
- What contracts, state, permissions, workflows, deployments, prompts/tools, or docs are affected?
- What evidence is available: plan text, code, docs, schemas, configs, logs, tests, metrics, evals, traces, validation artifacts, owner confirmations, or prior discussion?
- In repo-aware mode, which files, directories, tests, schemas, configs, docs, routes, prompts/tools, migrations, runbooks, or CI commands were inspected?
- Which repository areas were not inspected but are material?
- What facts are unknown but materially affect pass/fail status?
- What is the blast radius?
- Is the change reversible, partially reversible, or irreversible?

## Universal Review Lenses

- Problem fit: the plan solves the stated problem instead of optimizing adjacent symptoms.
- Scope control: goals, non-goals, deferred work, and success criteria are explicit.
- Boundary clarity: ownership, responsibilities, dependencies, public contracts, side effects, and lifecycle boundaries are named.
- Contract safety: callers, consumers, inputs, outputs, compatibility requirements, versioning, and deprecation behavior are covered.
- Evidence quality: major claims are backed by plan text, repository facts, docs, schemas, configs, logs, tests, evals, validation results, owner confirmation, production observation, external authority, or explicitly named assumptions.
- External authority and freshness: material third-party, framework, SDK, cloud, platform, LLM/API, security, compliance, or provider facts are verified against current or version-specific authoritative sources, pinned repository versions, or owner confirmation.
- Real owner escalation: decisions that require domain, operator, security/privacy/compliance, data, billing, product, legal/policy, or external partner authority are surfaced instead of being absorbed into simulated expert consensus.
- Review coverage: the reviewer names which repository, domain, external-authority, validation, owner/production, and conformance areas were covered or left blind.
- State correctness: source of truth, invariants, consistency, ordering, idempotency, drift, and recovery are addressed where state exists.
- Failure behavior: timeout, retry, cancellation, partial failure, fallback, rollback, cleanup, and user/consumer-visible degradation are explicit where relevant.
- Validation strategy: tests, simulations, contract checks, manual checks, migration checks, observability, evals, and acceptance criteria match the blast radius.
- Operability: configuration, deployment or release mechanism, ownership, monitoring, alerts, runbooks, support burden, and cleanup are realistic.
- Security and abuse resistance: authentication, authorization, secrets, privacy, actor isolation, tenant isolation, data exposure, prompt/tool misuse, and abuse cases are considered when applicable.
- Evolution and maintainability: the plan reduces meaningful complexity or risk, names tradeoffs, and avoids long-lived ambiguity.
- Cost and complexity: runtime cost, operational cost, implementation complexity, and coordination cost are proportionate to value and risk.
- Non-expert legibility: a user outside the domain can identify red lines, accepted tradeoffs, required confirmations, and implementation evidence to preserve.


## Review Discipline Lens

Apply this lens to keep reviews concrete and proportional.

- Replace vague claims with mechanisms: what boundary changes, what invariant is protected, what evidence proves it, and what rollback or cleanup exists.
- Treat phrases such as `enhance validation`, `improve architecture`, `standardize`, `decouple`, and `handle edge cases` as incomplete until the concrete mechanism is named.
- Prefer the least ceremony that controls the risk. A longer rollout, extra abstraction, or extra process is only useful when it protects a specific user, caller, contract, state, security, operations, or maintainability invariant.
- For docs-only or planning-only work, require a validation strategy and acceptance evidence to preserve, not local test execution unless the user provides runnable artifacts and the risk justifies it.
- Avoid cargo-cult domain checks. State which domain checks are intentionally skipped and why.

## Change-Type Lenses

### Architecture / Boundary Change

- The new boundary is named and justified by responsibilities, not file layout or team preference alone.
- Responsibilities move to the layer or owner that can enforce the relevant invariant.
- Cross-cutting behavior is centralized only when it is truly cross-cutting.
- The plan states what becomes easier and what becomes harder after the change.
- The plan names which coupling is removed and which coupling is intentionally retained.
- The transition path prevents half-old/half-new ownership ambiguity.

### Refactor / Replacement / Migration

- The target design is defined before implementation sequencing.
- Compatibility is preserved until affected callers, users, data, configuration, or workflows are migrated.
- Any adapter, dual path, compatibility shell, feature flag, or temporary bridge has an owner, metric or signal, and deletion trigger.
- Direct refactor is preferred when the old boundary is the debt source, dual paths would create drift, callers/contracts are known, and rollback/acceptance checks are credible.
- Incremental migration is preferred when blast radius is high, assumptions need production evidence, or external consumers cannot be coordinated atomically.
- Behavioral equivalence or intentional behavior change is verifiable before and after the refactor.
- Migration steps are ordered by dependency, reversibility, and ability to validate risk early.

### New Capability / Feature

- The plan covers the full workflow, not only the happy path.
- Actor-visible states are explicit: unavailable, loading/progress, empty, permission denied, invalid input, partial success, retry, and recovery where applicable.
- Edge cases are tied to domain invariants instead of listed generically.
- Acceptance criteria prove user or consumer value, not just implementation completion.
- The plan names which user, caller, or operator behavior is intentionally changed.
- The plan defines how the capability is disabled, rolled back, or degraded if it causes harm.

### Integration / External Dependency

- Trust boundaries, ownership boundaries, and failure boundaries are explicit.
- Input shaping, policy decisions, capability selection, and side effects are owned by the correct layer.
- Error semantics preserve meaningful distinctions instead of collapsing distinct failures into vague responses.
- Versioning, compatibility, rate limits, quotas, timeout budgets, retries, and degradation behavior are addressed when relevant.
- Correlation identifiers, audit signals, or traceability exist when cross-boundary debugging matters.
- The plan states what happens when the dependency is slow, wrong, unavailable, partially successful, or returns unexpected data.


### External Technical Dependency / Platform Authority

Apply this lens when the plan depends on third-party APIs, SDKs, frameworks, cloud services, browsers, mobile/desktop platforms, databases, queues, search engines, payment providers, identity providers, LLM/model APIs, provider contracts, security advisories, or compliance rules.

- The plan names the specific dependency, version, provider, runtime, platform, or contract that matters.
- Repository-pinned versions, lockfiles, generated clients, deployment runtime, or SDK configuration are checked when available.
- Current official docs, API references, changelogs, migration guides, deprecation notices, security advisories, compatibility matrices, or provider limits are identified when risk justifies them.
- The plan distinguishes latest-provider behavior from behavior of the version actually deployed.
- Stale or unofficial sources are not treated as proof.
- If authority cannot be verified, the affected decision is marked assumption-backed and bounded by discovery, spike, validation, owner confirmation, or reversible guarded work.
- High-risk external dependencies have rollback, fallback, rate-limit, quota, timeout, degradation, observability, and owner-confirmation paths when relevant.

### State, Data, and Migration

- State ownership and source of truth are explicit.
- Transitions are backward-compatible or sequenced with expand-migrate-contract when existing consumers or persisted state are affected.
- Migration steps are idempotent, restartable, or explicitly bounded by a safe operational procedure.
- Rollback behavior is realistic, especially after irreversible transformation.
- Reconciliation, data quality checks, and cleanup ownership are included when state can drift.
- Data shape assumptions are backed by schema, query, sample, inventory, or dry-run evidence.
- The plan distinguishes reversible rollout from irreversible data transformation.

### Human-Facing Experience / Consumer Workflow

- The plan names actors and critical journeys.
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
- Rollout, rollback, smoke checks, and cleanup match production or release risk.
- Operational ownership after release is clear.
- The plan names runbook steps or incident response actions for high-risk operations.
- Capacity, cost, rate limits, and failure isolation are considered when traffic or workload can change.

### Security / Privacy / Compliance

- Threat model is proportional to risk: actors, assets, trust boundaries, and attack paths are named when relevant.
- Authentication and authorization are enforced at the correct layer.
- Least privilege, tenant isolation, data minimization, and purpose limitation are considered.
- Secrets handling, sensitive data logging, retention, deletion, masking, export, and audit needs are covered where relevant.
- Compliance, policy, or domain-owner confirmation is required for high-risk changes.

### AI / Agent / Model Behavior

Apply this lens only when the plan involves LLMs, agents, model behavior, prompts, tools, retrieval, evals, or autonomous workflows.

- Desired and prohibited behaviors are specified with examples or eval cases.
- Hallucination, prompt injection, tool misuse, unsafe autonomy, data leakage, and overconfident answers are considered.
- Human override, auditability, replayability, traceability, and failure containment are covered when actions have real impact.
- Evals or test scenarios cover the riskiest behaviors, not just happy-path responses.
- Model assumptions, tool permissions, retrieval sources, and fallback behavior are explicit.
- The plan distinguishes model preferences from enforceable guardrails.

### Documentation-Only / Planning Artifact

Use when the artifact is a plan, ADR, PRD-to-engineering proposal, or implementation-ready document.

- The document can guide implementation without hidden conversation context.
- Claims are separated from assumptions, decisions, and open questions.
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

After answering the domain adaptation questions, select only relevant packs from `references/domain-packs.md`.

For each selected pack, state:

- why it applies;
- which checks are material to this plan;
- which checks are intentionally skipped;
- what evidence is needed to pass those checks.

Do not apply a domain pack just because a keyword appears. Apply it only when the artifact, codebase, user context, or affected surface makes the domain relevant.

## Evidence Collection Questions

Use these when a plan is blocked or conditional because evidence is missing:

- Is the missing fact discoverable from the repository, validation artifact, production observation, external technical authority, or owner confirmation?
- What exact path, command, search term, official doc, changelog, migration guide, advisory, compatibility matrix, provider limit, test, dry-run, eval, smoke check, dashboard, log, screenshot, or owner is needed?
- Can implementation start only as discovery, spike, or reversible guarded work?
- What evidence would upgrade `Pass under assumptions` to absolute `Pass`?
- Which assumption, if false, would invalidate the implementation?
- Which evidence must a later coding agent preserve in the final diff or pull request?

## Evidence Matrix Questions

For material findings and pass-like decisions, ask:

- Which claim is evidence-backed?
- Which claim is repository-backed?
- Which claim is assumption-backed?
- Which claim is validation-backed?
- Which claim is external-authority-backed or version-pinned?
- Which claim requires real owner, operator, security/privacy/compliance, data, billing, product, legal/policy, or partner confirmation?
- Which claim lacks evidence but affects design safety?
- Which missing fact is serious enough to mark `Blocked`?
- Which findings are based only on plausible reasoning?
- What evidence would upgrade `Pass under assumptions` to absolute `Pass`?


## Review Coverage Questions

Use these before pass-like, high-risk, unfamiliar-domain, repository-backed, or implementation-conformance decisions:

- Repository coverage: which paths, commands, search terms, callers, tests, schemas, configs, prompts/tools, docs, CI, migrations, or runbooks were inspected?
- Domain coverage: which domain packs or change-type lenses were applied, and which were intentionally skipped?
- External authority / freshness coverage: which official docs, versioned changelogs, advisories, compatibility matrices, provider limits, pinned versions, or owner confirmations were checked?
- Validation coverage: which tests, CI logs, dry-runs, evals, smokes, screenshots, dashboards, traces, production observations, or rollback rehearsals exist, and what do they not validate?
- Owner / production confirmation coverage: which real owner, operator, domain, product, security/privacy/compliance, data, billing, legal/policy, or partner confirmations are present or missing?
- Implementation conformance coverage: if a diff exists, how completely were actual changes mapped to the approved plan, gates, accepted risks, and validation evidence?
- Biggest blind spot: what uninspected area could most likely change the decision?
- What would most improve confidence without adding unnecessary ceremony?

## Adversarial Review Prompts

Use before final pass-like decisions:

- What assumption would invalidate the plan?
- What hidden coupling could make implementation unsafe?
- What temporary compatibility path could become permanent debt?
- What test would falsely pass while production still breaks?
- What would a future maintainer misunderstand?
- What rollback path only works in theory but not after data or external behavior changes?
- What user, caller, or operator impact is easy to miss because the plan is written from the implementer's perspective?
- What would an agent hallucinate because the plan lacks concrete contracts?
- What repository fact would most quickly disprove the plan?

Use adversarial review to strengthen the final plan, not to create endless critique.

## Non-Expert Decision Brief

When the user is likely outside the domain, provide a short control summary:

- Top 3 things to watch.
- Red lines that must not be accepted.
- Tradeoffs that are acceptable only if owned and bounded.
- Questions requiring real owner, code, production, business, security/privacy/compliance, data, billing, legal/policy, or external-partner confirmation.
- Whether `Pass under assumptions` means discovery/validation only rather than full implementation approval.
- Evidence a later agent implementation must preserve: repository inspection ledger, diff-to-plan mapping, tests, contracts, screenshots, dry-runs, dashboards, logs, eval results, rollback proof, cleanup trigger, or acceptance artifacts.
- The simplest next instruction the user should give to a later implementation agent.

Keep this brief separate from the technical plan so it helps the user steer future agent work without pretending to be an expert.
