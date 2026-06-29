# Expert Plan Quality Gates

Use these gates to decide whether a software engineering plan can pass expert-style review. These gates are designed to prevent false confidence when a user is relying on an agent outside their own domain.

## Severity

- `Blocker`: The plan is likely to fail, create severe technical debt, break users, expose data, or cannot be implemented safely without resolving this.
- `Major`: The plan may work but leaves meaningful ambiguity, delivery risk, operational risk, compatibility risk, security/privacy risk, or maintainability debt.
- `Minor`: Useful improvement that should not block implementation.
- `Accepted Risk`: A known tradeoff with explicit owner, impact, mitigation, reason for acceptance, and review/removal trigger.

## Decision Outcomes

- `Pass`: No blocker or major issue remains; material claims are evidence-backed or validation-backed; the plan is executable by someone without the prior discussion.
- `Pass under assumptions`: No blocker or major issue remains if the explicitly listed assumptions are true, but one or more important facts were not independently verified.
- `Pass with notes`: Only minor issues or explicit accepted risks remain.
- `Revise`: At least one blocker or major issue remains, or the plan is likely to create avoidable technical debt.
- `Blocked`: Essential input is unavailable and further refinement would require unsafe guessing.

## Decision Rules

- Any unresolved `Blocker` means `Revise` or `Blocked`.
- Any unresolved `Major` means the plan cannot be `Pass`.
- Only `Minor` issues or explicit `Accepted Risk` items may remain for `Pass with notes`.
- Use `Blocked` only when essential input is unavailable and further refinement would require guessing.
- Use `Pass under assumptions` when the plan is safe only if named assumptions are true.
- Use absolute `Pass` only when no blocker or major issue remains, material assumptions have been verified or bounded by validation, and the plan is executable by someone without the prior discussion.
- Do not mark `Pass` merely because a plan improved across review rounds.
- Do not downgrade a blocker or major issue unless the revised plan directly removes the risk, proves it irrelevant, or converts it into an explicit accepted risk with owner, mitigation, and trigger.

## Evidence Rules

Do not treat plausible reasoning or simulated expert consensus as evidence.

For every `Blocker` or `Major` finding, record at least one of:

- plan text that creates the risk;
- repository, documentation, schema, config, test, log, metric, or prior-discussion evidence;
- missing business, production, external contract, data, or domain fact;
- concrete failure scenario that makes the risk material.

For every passing decision, record whether it is:

- evidence-backed;
- assumption-backed;
- validation-backed.

Absolute `Pass` is not allowed when important contracts, caller inventory, production data shape, permissions, rollout behavior, rollback behavior, or business constraints are only assumed.

## Pass Criteria

A plan passes only when:

- Problem, goals, non-goals, and success criteria are explicit.
- Ownership boundaries are clear: which module/service/component/team/process owns which responsibility.
- Public contracts are named: APIs, events, DTOs, schemas, storage formats, permissions, UI states, CLI behavior, job behavior, or external provider behavior.
- Compatibility and migration are addressed when existing users/data/callers/workflows are affected.
- Failure modes are handled: timeout, partial failure, retries, idempotency, cancellation, degraded behavior, rollback, and cleanup.
- Observability is sufficient for the change: logs, metrics, traces, request ids, dashboards, alerts, audit events, or smoke signals where relevant.
- Validation strategy matches risk: unit, integration, contract, e2e, migration dry-run, manual QA, load, security, accessibility, evals, or production smoke checks.
- Rollout and rollback are credible, or the plan explains why an atomic direct refactor is safer.
- Security, privacy, permissions, actor isolation, and data exposure are considered when user data, auth, tokens, cross-boundary access, AI/tool use, or external dependencies are involved.
- The implementation sequence is clear enough that an engineer can execute without inventing architecture mid-flight.
- Remaining risks are explicit, owned, acceptable, mitigated, and bounded by follow-up or removal triggers.
- The plan states which conclusions are evidence-backed and which are assumption-backed.

## Gate Evidence Matrix

Use this matrix before any passing decision:

| Gate | Evidence / Assumption | Status |
| --- | --- | --- |
| Scope |  | Pass/Revise/Blocked |
| Boundary |  | Pass/Revise/Blocked |
| Contracts |  | Pass/Revise/Blocked/N/A |
| State/Migration |  | Pass/Revise/Blocked/N/A |
| Failure behavior |  | Pass/Revise/Blocked |
| Observability |  | Pass/Revise/Blocked/N/A |
| Validation |  | Pass/Revise/Blocked |
| Rollout/Rollback |  | Pass/Revise/Blocked/N/A |
| Security/Privacy |  | Pass/Revise/Blocked/N/A |
| Technical debt control |  | Pass/Revise/Blocked |

Use `N/A` only when the gate truly does not apply. If a gate is unknown and material, mark `Revise` or `Blocked`, not `N/A`.

## Pre-Pass Stress Test

Before marking `Pass`, `Pass under assumptions`, or `Pass with notes`, test the plan against at least three scenarios:

1. Most likely failure.
2. Most expensive or user-visible failure.
3. Hardest-to-detect technical-debt failure.

For each scenario, identify:

- trigger;
- affected user/caller/component/data;
- detection signal;
- mitigation;
- rollback or cleanup path;
- whether the final plan handles it.

A plan cannot receive an absolute `Pass` if the stress test reveals an unresolved blocker or major issue.

## Regression Gate

When reviewing an updated plan, prior blockers and major findings must be checked before new critique is introduced.

| Prior Finding | Previous Severity | Current Status | Evidence | New Severity |
| --- | --- | --- | --- | --- |
|  | Blocker/Major/Minor | Resolved/Partial/Open/Regressed |  |  |

Status meanings:

- `Resolved`: the new plan directly removes the risk or adds credible evidence/validation.
- `Partial`: the plan mentions the issue or reduces impact but leaves a material gap.
- `Open`: the issue remains unaddressed.
- `Regressed`: the issue became worse, broader, less observable, less reversible, or less testable.

A prior `Blocker` or `Major` with `Partial`, `Open`, or `Regressed` status must keep the final decision at `Revise` or `Blocked`, unless it is converted into a valid `Accepted Risk`.

## Accepted Risk Requirements

An accepted risk must include:

- risk description;
- owner;
- affected users/callers/data/components;
- reason for accepting it now;
- mitigation;
- monitoring or validation;
- review date, metric threshold, deletion trigger, or follow-up condition.

A risk without owner, mitigation, and trigger is not an accepted risk; it is unresolved debt.

## Common Technical Debt Traps

- A gateway or facade accumulates product rules, mapping logic, retries, auth quirks, and downstream-specific branching without clear ownership.
- Local validation duplicates downstream rules and drifts from the true contract.
- "Temporary compatibility" has no removal trigger or owner.
- A slice-based rollout preserves the worst old abstraction and spreads adapter logic across new code.
- A direct refactor is proposed without an inventory of callers, state, migrations, rollback, and acceptance tests.
- Error semantics are inconsistent across layers, causing clients to depend on accidental status codes or message text.
- Shared DTOs become a dumping ground for unrelated use cases.
- Observability is added as generic logging but cannot answer "which user/request/path/downstream failed and why".
- Tests verify happy paths while missing contract, migration, rollback, and failure behavior.
- The plan uses broad terms such as "unified", "standardized", or "decoupled" without naming the new boundary.
- The plan improves local structure while preserving the real source of debt at a boundary, contract, or ownership seam.
- Feature flags, adapters, compatibility shells, or dual paths are introduced without ownership, metrics, deletion trigger, and cleanup plan.
- The plan proposes broad abstraction before proving there are multiple real use cases.
- The plan treats a generated implementation plan as evidence instead of verifying it against code, contracts, tests, data, or operations.
- The plan adds validation or logging generically but cannot prove the riskiest assumption.
- The plan makes irreversible data, contract, or user-facing changes before proving rollback or compatibility.

## Agent-Generated Plan Traps

Use these traps when the proposal was generated or heavily shaped by an agent:

- The plan invents APIs, files, schemas, commands, constraints, owners, or product behavior not present in evidence.
- The plan is internally consistent but unsupported by repository or production facts.
- The plan uses confident language to hide unknown caller inventory, data shape, permissions, or operational behavior.
- The validation plan checks what the agent changed rather than the risk that could break production.
- Tests are proposed at the wrong level and would pass even if the real contract is broken.
- The plan overfits to the prompt and misses hidden constraints from existing code, data, users, or deployment process.
- The plan collapses expert disagreement into a smooth compromise without naming tradeoffs.
- The final review declares success because all written TODOs were addressed, not because the gates passed.

## Overengineering Traps

- The plan adds a framework, platform, abstraction, event bus, plugin system, or policy engine before the need is proven.
- The plan creates generic extension points but cannot name the next concrete extension.
- The proposed architecture increases operational surface area more than it reduces product or engineering risk.
- The migration plan is safer on paper but creates long-lived parallel systems that will be hard to remove.
- The plan spreads ownership across many components when a local, reversible change would solve the problem.
- The plan adds process ceremony that does not produce evidence, reduce blast radius, or improve rollback.

## Approach Selection

Choose slice-based delivery when:

- The blast radius is high and compatibility must be proven gradually.
- Old and new paths can coexist without duplicating complex business rules.
- Feature flags or routing rules can safely separate traffic.
- Each slice removes risk or validates an assumption.
- A slice has a clear deletion or convergence path; otherwise it may create permanent drift.

Choose direct refactor when:

- The existing boundary is the primary source of debt.
- Maintaining two paths would duplicate complex rules or increase drift.
- Callers and contracts are known and can be updated together.
- Rollback is feasible through version control/deployment rollback, data is unchanged or reversible, and acceptance checks are strong.
- The risk of compatibility scaffolding is greater than the risk of a coordinated refactor.

Choose a hybrid when:

- The internal structure should be replaced directly, but external behavior must remain compatible.
- A stable facade can protect callers while internals are rewritten.
- Migration can be staged by data/domain boundary rather than by UI feature.
- Temporary compatibility has an owner, metric, and deletion trigger.

## Minimal Safe Plan Gate

Use a minimal safe plan when the original proposal is unsafe but the goal is still valid.

A minimal safe plan must:

- avoid irreversible data, contract, or user-visible changes until evidence exists;
- preserve existing public contracts unless migration and compatibility are explicit;
- validate the riskiest assumption with the smallest useful slice or spike;
- include rollback, cleanup, or an explicit stop condition;
- avoid creating long-lived compatibility shells, adapter sprawl, duplicated business rules, or unowned feature flags;
- state what evidence would allow the larger plan to continue.

Do not call a plan minimal if it adds broad abstraction, new infrastructure, or multi-component coordination before proving the core risk.

## Implementation Readiness Score

A score may help communication, but it is non-authoritative and cannot override severity gates.

```markdown
Implementation Readiness: <0-100>
Confidence: Low/Medium/High
Reason:
- ...
```

Scoring constraints:

- Any unresolved `Blocker` means implementation is unsafe except for discovery or validation work.
- Any unresolved `Major` means implementation should not start except for targeted risk-reduction work.
- `Pass under assumptions` should not have high confidence unless each assumption has a clear validation owner and trigger.
- A high score cannot override a failed gate.

## Disagreement Resolution Gate

When reviewers disagree, the plan cannot pass until the disagreement is resolved or recorded as an accepted risk.

Resolve by comparing:

- blast radius;
- reversibility;
- validation strength;
- user or caller impact;
- operational burden;
- delivery complexity;
- long-term debt.

Do not average conflicting recommendations. Choose one path and record why the rejected path was not selected.
