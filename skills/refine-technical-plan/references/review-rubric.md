# Technical Plan Review Rubric

Use this rubric to sharpen expert-panel reviews and decide whether a plan should pass. Apply the checks that match the plan's actual failure modes.

Start from the universal criteria, then select or synthesize the risk checks needed for the specific plan.

## Review Coverage

- Evidence: local repo/docs and authoritative external sources were checked when relevant.
- Domain fit: the selected expert roles match the actual risk area and stay specific to the plan.
- Prior-cycle tracking: repeated reviews identify which earlier concerns are `已解决`, `部分解决`, or `仍未解决`.
- Approval state machine: each review shows the current approval state in user-facing language and updates a refinement ledger for material risks.
- Missing plan content: unresolved product, security, compliance, data, rollout, or operational decisions are requested as concrete plan additions.
- Implementation readiness: the plan is specific enough that a different engineer can execute it from self-contained plan context.
- Scoring: every review includes a 0-100 readiness score, a continuation decision, and whether the next cycle needs additional plan content.
- Plain-language conclusion: the first conclusion says, in general-reader language, whether implementation can start, why, and what the user should do next.
- Action layer: before the detailed ledger, the output names at most three concrete plan additions and why each matters, without prescribing completion thresholds or post-addition workflow.
- Shareable language: formal plan sections, scoring tables, and ledgers use neutral document language such as `待补方案内容`, `待定决策`, `待补证据`, `责任归属`, and `通过条件`.
- Cycle batch: default polishing starts with a three-cycle batch, each cycle includes diagnosis + plan rewrite + final gate, stops early on pass/blocker, and treats later batches as continuation of the same unbounded approval loop.
- Evidence and judgment: the review distinguishes facts from assumptions, uses evidence-backed claims, and keeps expert-panel commentary tied to the decision or plan.

## Universal Architecture Checks

- Problem fit: the plan solves the stated problem and stays focused on the target outcome.
- Boundary clarity: modules, ownership, APIs, persistence, and side effects are explicitly separated.
- Contract safety: public interfaces, compatibility requirements, versioning, and consumers are named.
- Data correctness: source of truth, invariants, consistency, idempotency, and error semantics are covered.
- Failure behavior: retries, timeouts, partial failure, fallback, rollback, and user-visible degradation are explicit.
- Observability: logs, metrics, traces, audit records, and success/failure signals are sufficient for rollout.
- Security/privacy: authentication, authorization, secrets, tenant isolation, data exposure, and abuse cases are considered.
- Operability: deployment, configuration, ownership, alerts, runbooks, and cleanup are realistic.
- Testability: unit, integration, contract, migration, and manual validation match the blast radius.
- Change economics: the plan reduces meaningful complexity or risk with proportionate ceremony.
- Plan-to-code traceability: implementation steps can later be checked clearly against the approved design.

## Refactoring Checks

- Define the target architecture before sequencing implementation.
- Prefer coherent vertical milestones over arbitrary file or route slices when behavior crosses boundaries.
- Preserve compatibility until callers and data are migrated.
- Stranglers and adapters include deletion plans.
- Temporary dual paths include explicit ownership, metrics, and cleanup criteria.
- Old and new concepts stay separated through clear state ownership and naming.
- State how to verify behavioral equivalence before and after the refactor.
- Proposed abstractions reduce net complexity or risk.

## Service And API Interaction Checks

- Route matching, auth, policy enforcement, request shaping, and upstream selection have clear order and ownership.
- Cross-cutting concerns are centralized when they are truly cross-cutting; domain-specific behavior stays near the domain.
- Timeout, retry, circuit-breaking, and rate-limit behavior is explicit per upstream or traffic class.
- Error mapping preserves upstream failure meaning and keeps distinct failure modes visible.
- Header, identity, tenant, and trace propagation are deliberate and tested.
- Backward compatibility for clients is covered, including deprecation and versioning strategy.
- Local validation is complemented by contract and integration checks that cover real system behavior.

## User-Facing Interaction Checks

- State ownership, server synchronization, loading, empty, error, and permission states are explicit.
- User workflows are complete, including loading, empty, error, permission, and recovery states beyond happy-path screens.
- Component boundaries follow product behavior and reuse patterns.
- Accessibility, responsiveness, and text overflow are considered for real data.
- API contracts and error semantics are verified or called out as assumptions.
- Performance budgets, hydration/client-state cost, and bundle impact are considered when the plan changes large surfaces.
- Design-system alignment and expected interaction states are explicit enough to prevent one-off UI debt.

## State Change And Migration Checks

- Schema changes are backward-compatible or sequenced with expand-migrate-contract.
- Migration scripts are idempotent or safely restartable.
- Rollback behavior is realistic, especially after irreversible data transformation.
- Data quality checks and reconciliation are included when moving or rewriting persisted data.
- Ownership of cleanup and old data paths is explicit.

## Agent-Generated Plan Checks

- The plan distinguishes facts found in the repo/docs from model inference.
- Proposed libraries, APIs, commands, and framework features are verified against current documentation when they are unstable or version-sensitive.
- The plan includes enough decision context: out-of-scope boundaries, rejected alternatives, and why simpler approaches are insufficient.
- The plan uses evidence-backed confidence: verified benchmarks, verifiable claims, and specific test or monitoring targets.

## Evidence And Judgment Checks

- Evidence claims match inspected files, code, docs, metrics, and production behavior.
- Expert comments are useful: each role contributes a concrete risk, decision, rewrite, or pass condition.
- The review stops at pass, or at missing input that blocks responsible review.
- Missing information stays explicit and traceable as plan content to add.
- Recommendations are proportional: added abstractions, migrations, tools, or governance steps directly reduce named risk.
- The plain conclusion remains short and actionable even when the later expert analysis is detailed.

## Rollout and Operations Checks

- Feature flags, staged rollout, canarying, backfill timing, and rollback are proportionate to the blast radius.
- Metrics and alerts identify both technical failure and user-visible degradation.
- Runbooks or ownership notes exist for new operational responsibilities.
- Cleanup has a trigger, owner, and observable completion condition.

## Scoring and Continuation Checks

- Score matches status: `通过` normally scores 90+, `基本通过，带小备注` 80-89, `需要修改` 60-79, and `卡住` below 60.
- Score caps are applied from the ledger: unresolved high-impact risks cap at 79; required missing plan content caps at 69; blocked reviews cap at 59.
- Continuation is explicit: the review says whether another complete refinement cycle is needed, optional, or unnecessary.
- Missing information is precise: if information is needed, the review asks for concrete plan additions, decisions, artifacts, metrics, constraints, or source-of-truth docs.
- Next-cycle focus is actionable: the review identifies exactly what will be checked after the user updates the plan.

## Approval State Machine Checks

- Current state is explicit and localized for the reader. In Chinese output, use `信息收集中`, `打磨中`, `等补充信息`, `基本通过，带小备注`, `通过`, or `卡住`.
- Ledger entries use readable labels: `编号`, `问题`, `影响`, `证据状态`, `严重程度`, `处理状态`, `待补方案内容`, `分数上限`, and `通过条件`.
- Evidence and risk states use plain vocabulary: `已验证`, `方案已写明`, `暂按假设`, `缺证据`, `未检查`; `卡住`, `高`, `中`, `低`; `未解决`, `部分解决`, `已解决`, `待补充`, `已接受风险`, `已被替代`.
- Continuing reviews reuse the existing ledger and carry prior context forward.
- State transitions are explained in plain language: `新发现`, `已解决`, `部分解决`, `仍未解决`, `待补充`, `已接受风险`, or `已被替代`.
- Open blockers or high-severity items keep the plan below `通过`.
- `基本通过，带小备注` is used when remaining ledger items are low-risk, owned, and have clear follow-up conditions.
- Accepted risks state the consequence and the exact text that should appear in the plan.

## Plain-Language Output Checks

- The first section is useful to a user outside the domain and starts with plain-language judgment.
- The conclusion includes a simple start-or-pause judgment, one-sentence reason, and next action.
- The immediate reader can identify the next 1-3 plan additions from the action brief.
- Responsibility and confirmation work are phrased as plan content to add, such as `责任归属`, `决策依据`, `验收口径`, and `清理触发条件`.
- Formal plan sections use `待补方案内容`, `待定决策`, `待补证据`, `责任归属`, and `通过条件`.
- The shareable plan body is separated from review mechanics; scores, state machine, ledger, and next actions sit outside the formal plan section.
- Technical terms appear only after the plain conclusion or are tied to concrete impact.
- The output still preserves expert rigor in later sections; plain language simplifies presentation while preserving review strength.

## Three-Cycle Refinement Checks

- Each cycle is a complete pass: it diagnoses the current plan, rewrites or reshapes it, and gates the refined version with score, start-or-pause judgment, remaining blockers, and missing plan-content needs.
- Cycle 1 produces a complete refined plan from the user's draft.
- Cycle 2 reviews and improves the result of cycle 1.
- Cycle 3 reviews and improves the result of cycle 2, then makes the final default start-or-pause call.
- The review stops early when the plan passes or when essential information is missing.
- If the first three-cycle batch requires follow-up, the output says so, names the exact next-cycle focus, and states whether another batch needs additional plan content.

## Passing Criteria

Mark `通过` when the plan:

- can be implemented with self-contained context;
- has resolved high-impact ambiguity;
- has a credible validation path for the riskiest behavior;
- includes rollout and rollback where user or production impact exists;
- makes tradeoffs explicit enough that a future maintainer can understand why the design exists.

Mark `基本通过，带小备注` when only minor improvements remain and implementation safety is intact.

Mark `需要修改` when the plan has any unclear boundary, unvalidated assumption, unsafe migration path, or likely long-lived debt.

Mark `卡住` when essential input is unavailable and further review would require guessing.
