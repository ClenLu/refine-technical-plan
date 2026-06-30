# Expert Plan Quality Gates

Use these gates to decide whether a software engineering plan, or an implementation produced from an approved plan, can pass expert-style review.

## Severity

- `Blocker`: The plan is likely to fail, create severe technical debt, break users, corrupt data/state, violate security/privacy/compliance expectations, or cannot be implemented safely without resolving this.
- `Major`: The plan may work but leaves meaningful ambiguity, delivery risk, operational risk, evidence gaps, repository mismatch, external dependency risk, or maintainability debt.
- `Minor`: Useful improvement that should not block implementation.
- `Accepted Risk`: A known tradeoff with explicit owner, impact, mitigation, validation or monitoring path, trigger/expiry, and reason for acceptance.

## Status Model

- `Pass`: no `Blocker` or `Major` remains; important claims are evidence-backed, repository-backed, validation-backed, or owner-confirmed as needed; the plan is executable by someone without the prior discussion.
- `Pass under assumptions`: no `Blocker` or `Major` remains if explicitly listed assumptions hold, but one or more important facts were not independently verified.
- `Pass with notes`: only `Minor` issues or explicit `Accepted Risk` items remain.
- `Revise`: any `Blocker` or unresolved `Major` remains, or the plan would likely create avoidable debt.
- `Blocked`: essential input is unavailable and further refinement would require guessing.


## Approval-to-Action Semantics

A gate decision must constrain the next agent action:

| Decision | Allowed Next Action | Not Allowed |
| --- | --- | --- |
| `Pass` | Full implementation may start; required validation, rollback, observability, and cleanup evidence must be preserved. | Ignoring gates during implementation. |
| `Pass with notes` | Implementation may start; notes must be tracked or converted to accepted risks. | Dropping notes that affect tests, ownership, cleanup, docs, or validation. |
| `Pass under assumptions` | Discovery, spike, validation, owner confirmation, or reversible guarded implementation. | Full production rollout, irreversible migration, public contract change, or high-risk implementation before assumptions are verified or explicitly accepted. |
| `Revise` | Plan revision or targeted risk-reduction work. | Implementing the full plan as approved. |
| `Blocked` | Evidence collection, owner confirmation, or scope reduction. | Continuing by guessing. |

For high-risk plans, `Pass under assumptions` is not implementation approval. It only permits bounded evidence collection or reversible validation work unless the assumptions are verified or formally accepted.

## Decision Rules

- Any unresolved `Blocker` means `Revise` or `Blocked`.
- Any unresolved `Major` means the plan cannot be `Pass`.
- Important unverified contracts, caller inventory, data shape, production behavior, permissions, rollback behavior, business constraints, or compliance constraints prevent absolute `Pass`.
- Only `Minor` issues or explicit `Accepted Risk` items may remain for `Pass with notes`.
- Use `Pass under assumptions` when the plan is coherent but depends on material assumptions that must be verified before or during implementation.
- Use `Blocked` only when essential input is unavailable and continuing would require guessing.
- Do not upgrade a plan to `Pass` merely because a rewrite sounds clearer. Evidence, repository alignment, validation, and gates must improve.


## Mandatory Repo-Aware Evidence Rules

When repository context is available and material, inspection is mandatory before final high-impact findings, implementation-readiness decisions, pass-like decisions, or implementation-conformance decisions.

Inspect relevant:

- code, docs, configs, tests, schemas, routes, imports, generated files, migrations, prompts, tools, workflows, deployment files, runbooks, and CI commands;
- caller/consumer inventory and public contract surfaces;
- existing permission checks, error semantics, observability, rollback, and cleanup conventions.

The reviewer must produce or summarize a repository inspection ledger:

| Area | Paths / Commands / Search Terms | Why Inspected | Finding | Confidence |
| --- | --- | --- | --- | --- |

Rules:

- Prefer repository evidence over plan claims when they conflict.
- Mark conflicts as `Blocker` or `Major` depending on impact.
- Do not ask the user for information that can reasonably be discovered from the repository.
- If repository evidence is incomplete, state the exact missing artifact, path, search area, command, or owner confirmation needed.
- Do not treat lack of search as lack of evidence. If the agent did not inspect relevant repository areas, it must not claim repository-backed confidence.
- If repository tools are unavailable, mark related conclusions as assumption-backed.

## Evidence Rules

For each `Blocker` or `Major`, identify at least one of:

- exact plan text that creates the risk;
- repository, documentation, schema, config, test, log, metric, trace, eval, or prior-discussion evidence;
- missing business, production, external contract, compliance, or domain-owner fact;
- concrete failure scenario that makes the risk material.

Before `Pass`, `Pass under assumptions`, or `Pass with notes`, produce a gate evidence matrix covering:

- Scope.
- Boundary.
- Contracts.
- State/migration.
- Failure behavior.
- Observability.
- Validation.
- Rollout/rollback.
- Security/privacy.
- Technical debt control.


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

For high-risk plans, absolute `Pass` requires repository evidence plus validation evidence or explicit owner confirmation.

Treat a plan as high-risk when it involves any of:

- irreversible data or state changes;
- public API, event, schema, storage, prompt/tool, permission, or workflow contract changes;
- security, privacy, compliance, billing, payment, authentication, authorization, access-control, or user-data impact;
- production migration or high-blast-radius rollout;
- autonomous agent actions with real-world side effects;
- user-visible workflow changes that are hard to roll back;
- cross-team or external dependency coordination;
- performance or reliability assumptions that could affect SLOs.

For high-risk plans, require at least one of:

- executed validation evidence;
- migration dry-run or rollback rehearsal;
- contract tests or compatibility checks;
- production smoke, replay, or staged-rollout evidence when available;
- domain owner, operator, security, compliance, or product confirmation;
- explicit `Accepted Risk` with owner, trigger, mitigation, monitoring, and expiry.

Without this, use `Pass under assumptions`, `Revise`, or `Blocked`, not absolute `Pass`.


## Implementation Conformance Gate

Use after a coding agent or engineer produces code, config, migration, tests, prompt/tool, docs, or deployment changes from an approved plan.

Implementation cannot pass unless:

- actual changes map to approved plan sections, gates, or accepted risks;
- unplanned APIs, schemas, files, routes, permissions, configs, prompts, tools, migrations, jobs, side effects, public behavior, or operational procedures are listed and reviewed;
- tests and validation artifacts target the real risk, not only changed lines;
- rollout, rollback, observability, cleanup, owner, and accepted-risk triggers remain valid after the diff;
- temporary adapters, feature flags, facades, dual paths, eval fixtures, prompts, queues, dashboards, or runbooks have owner, metric/signal, and deletion or review trigger;
- implementation shortcuts do not create new unreviewed debt.

Implementation decisions:

- `Implementation Pass`: matches the approved plan and has required validation evidence or explicitly accepted deferrals.
- `Implementation Pass under assumptions`: matches the approved plan, but material validation, owner confirmation, or production evidence remains pending.
- `Revise implementation`: deviates from the approved plan, lacks required evidence, or introduces unreviewed debt.
- `Implementation Blocked`: the diff or evidence cannot be inspected safely.

A normal test pass is not sufficient for implementation pass if the tests do not cover the approved gates or riskiest failure modes.

## Pre-Pass Stress Test Gate

Before `Pass`, `Pass under assumptions`, or `Pass with notes`, check at least three failure scenarios:

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

For each prior finding, classify current status as:

- `Resolved`: the new plan directly fixes the issue with evidence or a concrete mechanism.
- `Partial`: the plan improves the issue but leaves material ambiguity.
- `Open`: the issue still exists.
- `Regressed`: the new plan makes the issue worse or reintroduces a resolved risk.

A plan cannot pass while a prior `Blocker` or unresolved `Major` remains open or regressed.

## Pass Criteria

A plan passes only when:

- Problem, goals, non-goals, and success criteria are explicit.
- Ownership boundaries are clear: which module/service/component/artifact owns which responsibility.
- Public or critical contracts are named: APIs, events, DTOs, schemas, storage formats, permissions, UI states, CLI behavior, workflow states, prompts/tools, docs contracts, or operator procedures as applicable.
- Compatibility and migration are addressed when existing users/data/callers/workflows are affected.
- Failure modes are handled: timeout, partial failure, retries, idempotency, degraded behavior, rollback, and cleanup where relevant.
- Observability is sufficient to answer what changed, who or what was affected, where it failed, and why.
- Validation strategy matches risk: unit, integration, contract, e2e, migration dry-run, manual QA, load, security, eval, replay, or production smoke checks as applicable.
- Rollout and rollback are credible, or the plan explains why an atomic direct change is safer.
- Security, privacy, permissions, compliance, and data exposure are considered when users, secrets, auth, tools, models, or cross-boundary access are involved.
- The implementation sequence is clear enough that an engineer or coding agent can execute without inventing architecture mid-flight.
- Remaining risks are explicit, owned, monitored or validated, and acceptable.

## Accepted Risk Requirements

A risk can be marked `Accepted Risk` only when all are explicit:

- owner;
- impact;
- mitigation;
- validation or monitoring path;
- trigger for revisit, removal, or escalation;
- expiry condition or review condition;
- reason acceptance is better than immediate resolution.

Do not mark unresolved ambiguity as `Accepted Risk`. Do not use `Accepted Risk` to hide a missing decision, missing owner, or missing validation path.

## Common Technical Debt Traps

- A facade accumulates product rules, mapping logic, retries, auth quirks, and downstream-specific branching without clear ownership.
- Local validation duplicates the real source of truth and drifts from the true contract.
- "Temporary compatibility" has no removal trigger or owner.
- A slice-based rollout preserves the worst old abstraction and spreads adapter logic across new code.
- A direct refactor is proposed without an inventory of callers, state, migrations, rollback, and acceptance tests.
- Error semantics are inconsistent across layers, causing consumers to depend on accidental status codes or message text.
- Shared DTOs, shared state, or shared prompts become dumping grounds for unrelated use cases.
- Observability is added as generic logging but cannot answer which user/request/workflow/component failed and why.
- Tests verify happy paths while missing contract, migration, permission, failure, or eval behavior.
- The plan uses broad terms such as "unified", "standardized", or "decoupled" without naming the new boundary.

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

## Minimal Safe Plan Gate

When the proposed plan is unsafe but the goal is valid, the reviewer should propose the smallest safe path that:

- avoids irreversible changes;
- preserves existing contracts until migration is proven;
- validates the riskiest assumption early;
- includes rollback or cleanup;
- avoids long-lived compatibility debt.

Use this instead of only returning critique when enough context exists to produce a safer plan.

## Approach Selection

Choose slice-based delivery when:

- The blast radius is high and compatibility must be proven gradually.
- Old and new paths can coexist without duplicating complex business rules.
- Feature flags, routing rules, or workflow gates can safely separate traffic or usage.
- Each slice removes risk or validates an assumption.

Choose direct refactor when:

- The existing boundary is the primary source of debt.
- Maintaining two paths would duplicate complex rules or increase drift.
- Callers and contracts are known and can be updated together.
- Rollback is feasible through deployment/version rollback, data is unchanged or reversible, and acceptance checks are strong.

Choose a hybrid when:

- The internal structure should be replaced directly, but external behavior must remain compatible.
- A stable facade can protect callers while internals are rewritten.
- Migration can be staged by state/domain/workflow boundary rather than arbitrary file or UI slices.

## Disagreement Resolution Gate

When expert roles disagree, the decision must not average opinions.

The final decision must state:

- conflicting recommendations;
- which risk or invariant each recommendation protects;
- blast radius;
- reversibility;
- validation strength;
- delivery complexity;
- long-term debt impact;
- chosen path;
- rejected options and accepted risks.

If the disagreement affects safety, data, contracts, migration, or user impact and cannot be resolved with available evidence, mark `Revise` or `Blocked`.

## Implementation Readiness Score

Readiness score is optional and must not override severity.

A plan with unresolved `Blocker` or `Major` cannot be marked implementation-ready regardless of score.

Use the score only to communicate execution readiness:

- 90-100: ready to implement; remaining work is minor or accepted.
- 70-89: conditionally ready; implementation may start only if listed assumptions, validations, or confirmations are handled.
- 40-69: not ready; revise before implementation.
- 0-39: blocked or unsafe.

Readiness should consider:

- repository alignment, inspected artifacts, and repository inspection ledger quality;
- clarity of scope and contracts;
- migration and rollback safety;
- validation strength;
- observability and operational readiness;
- security/privacy/compliance risk;
- technical debt containment;
- owner confirmation for high-risk assumptions;
- implementation conformance when code, config, migration, prompt/tool, tests, docs, or deployment changes already exist.
