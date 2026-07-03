# Technical Plan Review Rubric

Use this rubric to sharpen reviews when a plan has meaningful architecture, refactoring, integration, migration, state, security, reliability, rollout, operations, or production risk.

Apply only checks that match the plan's actual failure modes. The goal is a better start decision and a better optimized plan, not a fixed checklist.

## Review Coverage

- Existing plan: the user supplied or referenced a plan to refine.
- Start decision: the review clearly says whether implementation can start, can mostly start with small notes, should pause for revision, or is blocked on missing input.
- Next actions: when the plan cannot clearly start, the review lists at most three concrete additions, decisions, or evidence items and explains why each affects start readiness.
- Optimized plan: when useful, the review provides a shareable revised plan body rather than only critique.
- Evidence honesty: local repo/docs and authoritative external sources are checked when they can affect the decision; unchecked claims are labeled as assumptions or `待补证据`.
- Review lenses: visible expert perspectives are limited to lenses that changed the judgment.
- Proportional output: quick go/no-go answers stay compact; high-risk or repeated reviews include enough evidence and ledger detail to preserve traceability.
- Continuation: repeated reviews track whether prior concerns are `已解决`, `部分解决`, `仍未解决`, or `待补充`.
- No numeric score: readiness is communicated through the start decision, blockers, next actions, and pass conditions.

## Universal Checks

- Problem fit: the plan solves the stated problem and does not widen scope without a reason.
- Boundary clarity: ownership, modules, APIs, persistence, and side effects are explicitly separated where relevant.
- Contract safety: public interfaces, compatibility requirements, versioning, and consumers are named.
- Data correctness: source of truth, invariants, consistency, idempotency, and error semantics are covered.
- Failure behavior: retries, timeouts, partial failure, fallback, rollback, and user-visible degradation are explicit.
- Observability: logs, metrics, traces, audit records, and success/failure signals are sufficient for rollout.
- Security/privacy: authentication, authorization, secrets, tenant isolation, data exposure, and abuse cases are considered.
- Operability: deployment, configuration, ownership, alerts, runbooks, and cleanup are realistic.
- Validation: tests, manual checks, rollout signals, rollback checks, data checks, permission checks, and acceptance criteria match the blast radius.
- Change economics: added abstractions, migrations, tools, or governance reduce a named risk.
- Plan-to-code traceability: implementation can later be checked against the approved design.

## Refactoring Checks

- The target architecture is defined before implementation sequencing.
- Milestones follow coherent behavior boundaries instead of arbitrary file slices when behavior crosses modules.
- Compatibility is preserved until callers and data are migrated.
- Stranglers, adapters, compatibility layers, and dual paths include owner, deletion trigger, and cleanup condition.
- Old and new concepts have clear state ownership and naming.
- Behavioral equivalence is verifiable before and after the refactor.
- Proposed abstractions reduce net complexity or risk; broad `Common`, `Engine`, or `Pipeline` layers are justified by concrete boundaries.

## Service And API Checks

- Route matching, auth, policy enforcement, request shaping, and upstream selection have clear order and ownership.
- Cross-cutting concerns are centralized only when they are truly cross-cutting; domain-specific behavior stays near the domain.
- Timeout, retry, circuit-breaking, and rate-limit behavior is explicit per upstream or traffic class.
- Error mapping preserves upstream failure meaning and keeps distinct failure modes visible.
- Header, identity, tenant, and trace propagation are deliberate and tested.
- Backward compatibility for clients is covered, including deprecation and versioning.
- Contract and integration checks cover real system behavior, not only local unit paths.

## User-Facing Interaction Checks

- State ownership, server synchronization, loading, empty, error, permission, and recovery states are explicit.
- Component boundaries follow product behavior and reuse patterns.
- Accessibility, responsiveness, and text overflow are considered for real data.
- API contracts and error semantics are verified or called out as assumptions.
- Performance, hydration/client-state cost, and bundle impact are considered when the plan changes large surfaces.
- Design-system alignment and expected interaction states are explicit enough to prevent one-off UI debt.

## State Change And Migration Checks

- Schema changes are backward-compatible or sequenced with expand-migrate-contract.
- Migration scripts are idempotent or safely restartable.
- Rollback is realistic after data, config, external state, prompt, or provider-side changes.
- Data quality checks and reconciliation are included when moving or rewriting persisted data.
- Cleanup ownership and old-path deletion criteria are explicit.

## Agent-Generated Plan Checks

- The plan distinguishes repo/docs facts from model inference.
- "最佳实践", "成熟团队都这么做", and "最新版稳定支持" claims are verified or kept as `待补证据`.
- Proposed libraries, APIs, commands, and framework features are checked against current authoritative docs when version-sensitive.
- The plan includes out-of-scope boundaries, rejected alternatives when useful, and why simpler approaches are insufficient.
- Confidence comes from verified facts, benchmarks, acceptance criteria, or operational signals rather than agent tone.
- "上线后再补" monitoring, permissions, cost controls, runbooks, or rollback remains blocking unless explicitly accepted with consequence and owner.

## Evidence Checks

- Evidence claims match inspected files, code, docs, metrics, production behavior, or authoritative sources.
- Missing information is traceable as concrete plan content to add.
- Business priority, privacy, compliance, production scale, downtime tolerance, external contracts, and operational ownership are treated as blocking missing input when they can change the plan shape.
- When local context is unavailable, the review says so plainly and avoids repo-specific conclusions.
- When external facts are unchecked, the review does not mark the plan ready to start.

## Output Checks

- The first section gives a plain start decision, one-sentence reason, next step, and whether user-supplied information is needed.
- `下一步行动` appears before detailed evidence or ledger material when the plan cannot clearly start.
- The action brief uses plan-content phrasing such as `责任归属`, `决策依据`, `验收口径`, `回滚演练`, `清理触发条件`, and `待补证据`.
- The optimized plan body excludes scores, state-machine mechanics, expert discussion, and ledger tables.
- `非目标` appears when scope control affects safety; it is omitted when it would be empty ceremony.
- Full ledger output appears only for high-risk, repeated, accepted-risk, or user-requested traceability cases.

## Ledger Checks

When a detailed ledger is shown, it uses these fields:

| 编号 | 问题 | 影响 | 证据状态 | 严重程度 | 处理状态 | 待补方案内容 | 通过条件 |
| --- | --- | --- | --- | --- | --- | --- | --- |

- Continuing reviews reuse stable IDs when available.
- Open blockers or high-severity items prevent `可以开工`.
- `基本可以开工，但有小备注` is used only when remaining items are low-risk, owned, and non-blocking.
- Accepted risks include consequence, affected scope, owner, trigger condition, exit or cleanup condition, and validation condition.

## Implementation Conformance Checks

- The review compares implementation evidence against approved plan conditions instead of re-reviewing the plan from scratch.
- It flags plan drift, missing validation, unsafe shortcuts, skipped rollout stages, removed compatibility windows, missing rollback, and weakened security/observability/cost controls.
- If the approved plan or implementation evidence is incomplete, the review says what is missing and still reports visible inconsistencies.

## Passing Criteria

Mark `可以开工` when the plan:

- can be implemented from self-contained context;
- has resolved high-impact ambiguity;
- has credible validation for the riskiest behavior;
- includes rollout and rollback where user or production impact exists;
- makes tradeoffs explicit enough for future maintainers.

Mark `基本可以开工，但有小备注` when only minor, owned, low-risk cleanup remains.

Mark `暂缓开工，先修改方案` when the plan has unclear boundaries, unvalidated assumptions, unsafe migration, weak validation, or likely long-lived debt.

Mark `缺关键输入，暂时不能判断` when further review would require guessing about essential facts, decisions, artifacts, or authority.
