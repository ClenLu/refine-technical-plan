# Expert Plan Quality Gates

Use these gates to decide whether a software engineering plan, or an implementation produced from an approved plan, can pass expert-style review.

## Severity

- `Blocker`: The plan is likely to fail, create severe technical debt, break users, corrupt data or state, violate security/privacy/compliance expectations, or cannot be implemented safely without resolving this issue.
- `Major`: The plan may work but leaves meaningful ambiguity, delivery risk, operational risk, evidence gaps, repository mismatch, external dependency risk, or maintainability debt.
- `Minor`: A useful improvement that should not block implementation.
- `Accepted Risk`: A known tradeoff with explicit owner, impact, mitigation, validation or monitoring path, trigger or expiry, and reason for acceptance.

## Status Model

- `Pass`: no `Blocker` or `Major` remains; important claims are evidence-backed, repository-backed, validation-backed, or owner-confirmed as needed; the plan is executable by someone without prior discussion.
- `Pass under assumptions`: no `Blocker` or `Major` remains if explicitly listed assumptions hold, but one or more important facts were not independently verified.
- `Pass with notes`: only `Minor` issues or valid `Accepted Risk` items remain.
- `Revise`: any `Blocker` or unresolved `Major` remains, or the plan would likely create avoidable debt.
- `Blocked`: essential input is unavailable and further refinement would require guessing.

## Source of Truth Hierarchy

When evidence conflicts, prefer sources in this order unless a newer authoritative artifact explicitly supersedes an older one:

1. Current production/runtime facts: logs, metrics, traces, incidents, dashboards, staged rollout results, and rollback rehearsal evidence.
2. Current repository facts: code, tests, schemas, migrations, configs, prompts, tool definitions, CI, deployment files, runbooks, pinned dependency versions, lockfiles, and generated artifacts.
3. Official version-specific external technical authorities when external dependencies are material: vendor docs, API references, changelogs, migration guides, deprecation notices, security advisories, compatibility matrices, provider limits, and standards.
4. Official owner-maintained internal contracts and documents: API specs, ADRs, runbooks, compliance docs, security docs, support docs, and migration guides.
5. Explicit owner confirmations: product, domain, security, compliance, data, operator, or external partner confirmations.
6. User-provided constraints in the current task.
7. Prior conversation.
8. Model reasoning.

Model reasoning may identify risk but cannot prove safety. If source conflicts materially affect safety, mark the issue `Blocker` or `Major` until resolved, scoped away, or converted into a valid `Accepted Risk`.

## Approval-to-Action Semantics

Every decision constrains what a later engineer or coding agent may do.

| Decision | Allowed Next Action | Not Allowed |
| --- | --- | --- |
| `Pass` | Full implementation may start; required validation, rollback, observability, and cleanup evidence must be preserved. | Ignoring gates during implementation or release. |
| `Pass with notes` | Implementation may start; notes must be tracked or converted into valid accepted risks. | Dropping notes that protect tests, ownership, cleanup, docs, or validation. |
| `Pass under assumptions` | Discovery, spike, validation, owner confirmation, or reversible guarded implementation. | Full production rollout, irreversible migration, public contract change, or high-risk implementation before assumptions are verified or formally accepted. |
| `Revise` | Plan revision or targeted risk-reduction work. | Implementing the full plan as approved. |
| `Blocked` | Evidence collection, owner confirmation, or scope reduction. | Continuing by guessing. |

For high-risk plans, `Pass under assumptions` is not implementation approval. It permits bounded evidence collection or reversible validation work only unless assumptions are verified or formally accepted.

## Decision Rules

- Any unresolved `Blocker` means `Revise` or `Blocked`.
- Any unresolved `Major` means the plan cannot be `Pass`.
- Important unverified contracts, caller inventory, data shape, production behavior, permissions, rollback behavior, business constraints, external technical facts, or compliance constraints prevent absolute `Pass`.
- Material third-party, framework, platform, provider, SDK, LLM/API, security-advisory, or compliance-rule claims require current authoritative evidence, pinned-version repository evidence, owner confirmation, or an explicit assumption; otherwise use `Pass under assumptions`, `Revise`, or `Blocked`.
- Only `Minor` issues or valid `Accepted Risk` items may remain for `Pass with notes`.
- Use `Pass under assumptions` when the plan is coherent but depends on material assumptions that must be verified before or during implementation.
- Use `Blocked` only when essential input is unavailable and continuing would require guessing.
- Do not upgrade a plan to `Pass` merely because a rewrite sounds clearer. Evidence, repository alignment, validation, and gates must improve.
- Do not downgrade a `Blocker` or `Major` unless the revised plan directly removes the risk, proves it irrelevant, reduces scope so the risk no longer applies, or converts it into a valid `Accepted Risk`.


## Reviewer Independence Gate

Use role separation to reduce agent self-approval, especially when a plan was generated or heavily rewritten by an agent.

For automatic multi-round review, separate these passes:

1. Proposal pass: improve the plan only; do not approve it in this pass.
2. Red-team pass: review the plan as if another agent wrote it; look for hidden assumptions, missing evidence, false confidence, over-broad abstractions, implementation traps, and repository conflicts.
   Do not defend prior decisions; classify regressions or newly discovered evidence even when they invalidate the proposal pass or rewrite.
3. Gatekeeper pass: decide status only from severity rules, source-of-truth evidence, repository alignment, validation, owner confirmation, accepted risks, and explicit assumptions.

Do not mark `Pass` merely because the plan improved or all written TODOs were addressed. For high-risk, irreversible, security-sensitive, data-changing, public-contract, billing, compliance, autonomous-agent, or production-impacting plans, absolute `Pass` requires repository evidence plus validation evidence, owner confirmation, production observation, or a valid accepted risk.

## Evidence Rules

Do not treat plausible reasoning or simulated expert consensus as evidence.

For each `Blocker` or `Major`, identify at least one of:

- exact plan text that creates the risk;
- repository, documentation, schema, config, test, log, metric, trace, eval, or prior-discussion evidence;
- missing business, production, external contract, compliance, or domain-owner fact;
- concrete failure scenario that makes the risk material.

For each pass-like decision, state whether the decision is:

- evidence-backed;
- repository-backed;
- validation-backed;
- owner-backed or production-backed when applicable;
- assumption-backed.

A validation plan is not validation evidence. Evidence-backed confidence requires inspected artifacts, executed validation, owner confirmation, or production observation as appropriate.

## Mandatory Repo-Aware Evidence Rules

When repository context is available and material, inspection is mandatory before material review, rewrite, high-impact findings, implementation-readiness decisions, pass-like decisions, or implementation-conformance decisions.

Inspect relevant:

- code, docs, configs, tests, schemas, routes, imports, generated files, migrations, prompts, tools, workflows, deployment files, runbooks, and CI commands;
- caller/consumer inventory and public contract surfaces;
- existing permission checks, error semantics, observability, rollback, and cleanup conventions.

Rules:

- Prefer repository evidence over plan claims when they conflict.
- Mark conflicts as `Blocker` or `Major` depending on impact.
- Do not ask the user for information that can reasonably be discovered from the repository.
- If repository evidence is incomplete, state the exact missing artifact, path, search area, command, or owner confirmation needed.
- Do not treat lack of search as lack of evidence. If relevant areas were not inspected, do not claim repository-backed confidence.
- If repository tools are unavailable, mark related conclusions as assumption-backed.

## Evidence Collection Gate

When evidence gaps block or condition approval, produce an evidence collection plan instead of absolute `Pass`.

| Missing Evidence | Why It Matters | How To Collect | Owner / Source | Decision Impact | Can Implementation Start? |
| --- | --- | --- | --- | --- | --- |

Evidence collection must separate:

- repo-discoverable facts, such as callers, schemas, routes, tests, configs, migrations, prompts, tools, and existing docs;
- validation artifacts, such as CI, dry-runs, contract tests, evals, smoke checks, rollback rehearsals, dashboards, screenshots, or logs;
- external facts, such as production traffic, historical incidents, partner contracts, compliance requirements, business priority, operator constraints, or owner confirmation.

A missing material fact can be downgraded only when it is verified, made irrelevant by scope reduction, or converted into a valid `Accepted Risk`.

## High-Risk External Validation Gate

Treat a plan as high-risk when it involves any of:

- irreversible data or state changes;
- public API, event, schema, storage, prompt/tool, permission, or workflow contract changes;
- security, privacy, compliance, billing, payment, authentication, authorization, access-control, or user-data impact;
- production migration or high-blast-radius rollout;
- autonomous agent actions with real-world side effects;
- user-visible workflow changes that are hard to roll back;
- cross-team or external dependency coordination;
- performance or reliability assumptions that could affect service objectives.

For high-risk plans, absolute `Pass` requires repository evidence plus at least one appropriate external or executed validation signal:

- executed validation evidence;
- migration dry-run or rollback rehearsal;
- contract tests or compatibility checks;
- production smoke, replay, or staged-rollout evidence when available;
- domain owner, operator, security, compliance, or product confirmation;
- explicit `Accepted Risk` with owner, trigger, mitigation, monitoring, and expiry.

Without this, use `Pass under assumptions`, `Revise`, or `Blocked`, not absolute `Pass`.


## Freshness / External Technical Authority Gate

Use this gate when a plan depends on current external technical facts, including third-party APIs, frameworks, SDKs, cloud services, browser or mobile platform behavior, operating systems, databases, queues, search engines, payment providers, identity providers, LLM/model APIs, model/tool capabilities, security advisories, compliance rules, partner contracts, rate limits, quotas, or provider lifecycle policies.

Do not rely on model memory, old examples, blog posts, or plausible reasoning when a material external fact may have changed.

Check the most authoritative source justified by risk:

- official documentation or API reference;
- version-specific changelog, release notes, migration guide, or breaking-change notice;
- deprecation, end-of-life, or compatibility notice;
- security advisory or vulnerability notice when security is material;
- compatibility matrix, provider limits, rate limits, quota policy, runtime/platform support table, or SLA/SLO statement;
- repository-pinned versions, lockfiles, generated clients, SDK versions, or deployment/runtime configuration;
- owner, vendor, partner, compliance, or domain confirmation when official docs are insufficient for local behavior.

Classify each material external claim as one of:

- `Official-current`: verified against current official source for the relevant version or platform.
- `Official-versioned`: verified for the pinned version, but not necessarily latest behavior.
- `Repo-pinned`: inferred from lockfile, config, generated client, or deployment version.
- `Owner-confirmed`: confirmed by a real owner, partner, operator, or domain expert.
- `Assumption-backed`: plausible but not verified against an authoritative source.
- `Stale / unverified`: source is outdated, unofficial, conflicting, or unavailable.

Decision impact:

- For low-risk reversible work, stale or missing external authority may remain an explicit assumption with bounded validation.
- For high-risk, public-contract, security-sensitive, compliance-sensitive, billing/payment, production-impacting, or hard-to-rollback work, stale or missing material external authority prevents absolute `Pass`.
- When official sources conflict with repository behavior, mark the conflict `Major` or `Blocker` until reconciled, scoped away, or accepted by a real owner.
- If freshness cannot be verified in the current environment, produce an evidence collection task naming the exact official source, version, changelog, advisory, provider doc, or owner confirmation needed.


## Real Expert / Owner Escalation Trigger

This skill simulates expert review; it must surface when real human or organizational authority is required.

Require real owner, domain SME, operator, security/privacy/compliance reviewer, data owner, product owner, finance/billing owner, legal/policy owner, or external partner confirmation when any of these are material:

- data can be lost, corrupted, exposed, duplicated, irreversibly migrated, or made hard to reconcile;
- authentication, authorization, tenant isolation, billing, payment, financial state, privacy, compliance, audit, secrets, or sensitive user data is affected;
- public APIs, events, schemas, storage formats, prompts/tools, permissions, workflow contracts, mobile/desktop client compatibility, or external integrations change;
- rollback is not trivial after deployment, migration, prompt/tool change, model/provider change, or external behavior change;
- production behavior, traffic shape, historical incident context, operator constraints, or business rules are not discoverable from repository evidence;
- a high-impact autonomous or agentic system can take real-world actions, call tools with side effects, or influence user/business outcomes;
- the reviewing agent cannot inspect key repository, production, validation, external-authority, or owner evidence needed for a safe decision;
- legal, compliance, security, policy, accessibility, finance, support, or customer-commitment interpretation is required.

Without the required confirmation, use `Pass under assumptions`, `Revise`, or `Blocked`; do not issue absolute `Pass`. If bounded work is allowed, state exactly what is allowed, what is forbidden, who must confirm, and what artifact will prove confirmation.

## Pass Criteria

A plan passes only when:

- Problem, goals, non-goals, and success criteria are explicit.
- Ownership boundaries are clear: which module, service, component, artifact, owner, or process owns which responsibility.
- Public or critical contracts are named: APIs, events, DTOs, schemas, storage formats, permissions, UI states, CLI behavior, workflow states, prompts/tools, docs contracts, or operator procedures as applicable.
- Compatibility and migration are addressed when existing users, data, callers, workflows, configs, or docs are affected.
- Failure modes are handled: timeout, partial failure, retries, idempotency, cancellation, degraded behavior, rollback, and cleanup where relevant.
- Observability can answer what changed, who or what was affected, where it failed, and why.
- Validation strategy matches risk: unit, integration, contract, end-to-end, migration dry-run, manual QA, load, security, accessibility, eval, replay, or smoke checks as applicable.
- Rollout and rollback are credible, or the plan explains why an atomic direct change is safer.
- Security, privacy, permissions, compliance, abuse, and data exposure are considered when users, secrets, auth, tools, models, or cross-boundary access are involved.
- Material external technical facts are current, version-specific, repository-pinned, owner-confirmed, or explicitly assumption-backed with bounded validation.
- Real expert, owner, operator, security/privacy/compliance, product, or external partner escalation has been handled when triggers apply.
- Review coverage exposes material blind spots and no uninspected material area is silently treated as safe.
- The implementation sequence is clear enough that an engineer or coding agent can execute without inventing architecture mid-flight.
- Remaining risks are explicit, owned, monitored or validated, and acceptable.

## Gate Evidence Matrix

Use this matrix before pass-like decisions.

| Gate | Evidence / Assumption / Validation | Status |
| --- | --- | --- |
| Scope |  | Pass/Revise/N/A |
| Boundary |  | Pass/Revise/N/A |
| Contracts |  | Pass/Revise/N/A |
| State/Migration |  | Pass/Revise/N/A |
| Failure behavior |  | Pass/Revise/N/A |
| Observability |  | Pass/Revise/N/A |
| Validation |  | Pass/Revise/N/A |
| Rollout/Rollback |  | Pass/Revise/N/A |
| Security/Privacy |  | Pass/Revise/N/A |
| External authority / freshness |  | Pass/Revise/N/A |
| Real owner / SME escalation |  | Pass/Revise/N/A |
| Review coverage |  | Pass/Revise/N/A |
| Technical debt control |  | Pass/Revise/N/A |

Use `N/A` only when the gate truly does not apply. If a gate is unknown and material, mark `Revise` or `Blocked`, not `N/A`.

## Pre-Pass Stress Test Gate

Before `Pass`, `Pass under assumptions`, or `Pass with notes`, test at least three failure scenarios:

1. Most likely failure.
2. Most expensive or user-visible failure.
3. Hardest-to-detect technical-debt failure.

Each scenario must include:

- trigger;
- affected user, caller, component, or data;
- detection signal;
- mitigation;
- rollback or cleanup path;
- whether the final plan handles it.

If a severe or likely scenario has no credible detection, mitigation, or rollback path, mark the plan `Revise` or `Blocked`.

## Regression Gate

When reviewing an updated plan, prior `Blocker` and `Major` findings must be checked before introducing new critique.

| Prior Finding | Previous Severity | Current Status | Evidence | New Severity |
| --- | --- | --- | --- | --- |

Status meanings:

- `Resolved`: the new plan directly fixes the issue with evidence or a concrete mechanism.
- `Partial`: the plan improves the issue but leaves material ambiguity.
- `Open`: the issue still exists.
- `Regressed`: the new plan makes the issue worse or reintroduces a resolved risk.

A plan cannot pass while a prior `Blocker` or unresolved `Major` remains open, partial, or regressed unless it is converted into a valid `Accepted Risk`.

## Review Issue IDs

Assign stable IDs to material findings:

- `B-001`: Blocker.
- `M-001`: Major.
- `A-001`: Assumption.
- `E-001`: Missing Evidence.
- `R-001`: Accepted Risk.

Keep the same ID when severity changes. Record why it changed. Do not rename an issue to make it appear resolved.

## Accepted Risk Requirements

A risk can be marked `Accepted Risk` only when all are explicit:

- owner;
- affected users, callers, data, components, or operators;
- impact;
- reason for accepting it now;
- mitigation;
- validation or monitoring path;
- trigger for revisit, removal, or escalation;
- expiry condition or review condition.

Do not mark unresolved ambiguity as `Accepted Risk`. Do not use `Accepted Risk` to hide a missing decision, missing owner, or missing validation path.

## Common Technical Debt Traps

- A facade accumulates product rules, mapping logic, retries, auth quirks, and downstream-specific branching without clear ownership.
- Local validation duplicates the real source of truth and drifts from the true contract.
- Temporary compatibility has no removal trigger or owner.
- A slice-based rollout preserves the worst old abstraction and spreads adapter logic across new code.
- A direct refactor is proposed without an inventory of callers, state, migrations, rollback, and acceptance tests.
- Error semantics are inconsistent across layers, causing consumers to depend on accidental status codes or message text.
- Shared DTOs, shared state, or shared prompts become dumping grounds for unrelated use cases.
- Observability is added as generic logging but cannot answer which user, request, workflow, component, or dependency failed and why.
- Tests verify happy paths while missing contract, migration, permission, failure, rollback, or eval behavior.
- Broad terms such as unified, standardized, or decoupled appear without naming the new boundary.

## Agent Self-Review Traps

- The same model invents the plan, reviews it, fixes it, and approves it without external evidence.
- The plan treats plausible architecture reasoning as proof.
- Tests or evals are described but do not actually cover the riskiest behavior.
- Missing facts are silently converted into assumptions.
- The final answer sounds more confident after review even though evidence did not improve.
- Repository context exists but was not inspected before claiming implementation readiness.

## Over-Engineering Traps

- The plan adds abstraction, process, or infrastructure without naming the risk it reduces.
- Low-risk changes are forced through high-ceremony rollout that creates more coordination debt than safety.
- Optional future flexibility dominates current correctness and maintainability.
- A generic framework replaces a small explicit design without evidence that variation is real.



## Reference Loading Gate

Progressive loading reduces context bloat, but it must not hide mandatory gates.

- Before any pass-like decision that materially constrains implementation, load or apply this `quality-gates.md` file.
- Before any repository-backed claim or repository-dependent rewrite, load or apply `repo-aware-review.md`.
- Before any implementation-conformance decision, load or apply `implementation-conformance.md`.
- Before high-risk or non-expert output, load or apply `output-templates.md` so allowed/forbidden next actions, review coverage, and continuation packet are explicit.
- Before durable document creation or update, load or apply `document-template.md`.
- If a required reference cannot be loaded, state the limitation and mark affected conclusions assumption-backed.

## Review Discipline Gate

Use this gate to prevent review theater and cargo-cult planning.

- Do not accept vague phrases such as `enhance validation`, `improve architecture`, `standardize`, `decouple`, or `handle edge cases` unless the plan names the concrete mechanism, invariant, owner, contract, or validation artifact.
- Do not add process, abstraction, rollout stages, feature flags, dashboards, queues, docs, or owner handoffs unless each one reduces a named risk or preserves a specific invariant.
- Do not require local validation beyond what the risk justifies. For docs-only planning, specify the validation strategy and evidence required during implementation instead of pretending tests were run during planning.
- Do not over-index on incremental slicing. Slicing is useful only when each slice reduces risk, proves compatibility, validates an assumption, or limits blast radius.
- Do not skip direct refactor when the old boundary is the source of debt and dual paths would create more drift than safety.
- Do not apply domain packs because of keywords alone. Apply a domain pack only when the artifact, repository, user context, or affected surface makes that domain material.

## Approach Selection

Choose slice-based delivery when:

- blast radius is high and compatibility must be proven gradually;
- old and new paths can coexist without duplicating complex business rules;
- feature flags, routing rules, or workflow gates can safely separate traffic or usage;
- each slice removes risk or validates an assumption.

Choose direct refactor when:

- the existing boundary is the primary source of debt;
- maintaining two paths would duplicate complex rules or increase drift;
- callers and contracts are known and can be updated together;
- rollback is feasible through deployment/version rollback, data is unchanged or reversible, and acceptance checks are strong.

Choose a hybrid when:

- internal structure should be replaced directly, but external behavior must remain compatible;
- a stable facade can protect callers while internals are rewritten;
- migration can be staged by state, domain, or workflow boundary rather than arbitrary file or UI slices.

## Minimal Safe Plan Gate

When the proposed plan is unsafe but the goal is valid, propose the smallest safe path that:

- avoids irreversible changes;
- preserves existing contracts until migration is proven;
- validates the riskiest assumption early;
- includes rollback or cleanup;
- avoids long-lived compatibility debt;
- states what evidence would allow the larger plan to continue.


## Disagreement Resolution Gate

When expert roles disagree, the decision must not average opinions or silently choose the most convenient path.

State:

- conflicting recommendations;
- which risk or invariant each recommendation protects;
- blast radius;
- reversibility;
- validation strength;
- delivery complexity;
- long-term debt impact;
- chosen path;
- rejected options and accepted risks.

If the disagreement affects safety, data, contracts, migration, security/privacy, compliance, user impact, or production behavior and cannot be resolved with available evidence, mark `Revise` or `Blocked`.


## Review Coverage Score

Review coverage is separate from implementation readiness. It describes how much of the decision surface was actually inspected or verified. It is non-authoritative and must not override severity rules.

Include this score for high-risk, unfamiliar-domain, repository-backed, pass-like, `Pass under assumptions`, `Blocked`, `Revise`, or implementation-conformance decisions.

Use these dimensions:

| Coverage Area | Rating | Evidence / Gap | Decision Impact |
| --- | --- | --- | --- |
| Repository coverage | High / Medium / Low / N/A | inspected paths, commands, search terms, uninspected material areas |  |
| Domain coverage | High / Medium / Low | selected domain packs, skipped checks, missing expertise |  |
| External authority / freshness coverage | High / Medium / Low / N/A | official docs, pinned versions, changelogs, advisories, owner/vendor confirmation |  |
| Validation coverage | High / Medium / Low / None | tests, CI, dry-run, evals, smoke, replay, dashboards, rollback rehearsal |  |
| Owner / production confirmation coverage | Present / Partial / Missing / N/A | owner, operator, product, security/compliance, production observation |  |
| Implementation conformance coverage | High / Medium / Low / N/A | diff-to-plan mapping, unplanned changes, validation evidence, rollback/cleanup review |  |

Always state:

- Biggest blind spot.
- What would most improve confidence.
- Whether the decision is repository-backed, validation-backed, owner-backed, production-backed, external-authority-backed, or assumption-backed.

Decision impact:

- Low coverage in a material area does not automatically block low-risk reversible discovery work.
- Low coverage in a material high-risk area prevents absolute `Pass` unless a valid accepted risk or owner-confirmed exception exists.
- A high implementation readiness score is misleading when review coverage is low; explain the gap rather than raising the decision label.

## Implementation Readiness Score

A readiness score is optional and non-authoritative. It must not override severity.

- 90-100: ready to implement; remaining work is minor or accepted.
- 70-89: conditionally ready; implementation may start only if listed assumptions, validations, or confirmations are handled.
- 40-69: not ready; revise before implementation.
- 0-39: blocked or unsafe.

A plan with unresolved `Blocker` or `Major` cannot be marked implementation-ready regardless of score.

Readiness should consider:

- repository alignment, inspected artifacts, and repository inspection ledger quality;
- clarity of scope, ownership boundaries, contracts, and non-goals;
- migration, compatibility, rollout, rollback, and cleanup safety;
- validation strength and whether validation evidence exists or is only planned;
- observability and operational readiness;
- security, privacy, compliance, billing, permission, and abuse risk where applicable;
- technical debt containment and temporary-path removal triggers;
- owner confirmation for high-risk assumptions or external facts;
- implementation conformance when code, config, migration, prompt/tool, tests, docs, or deployment changes already exist.
