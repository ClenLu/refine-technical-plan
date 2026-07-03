---
name: refine-technical-plan
description: Use when the user explicitly invokes `$refine-technical-plan` or clearly asks to apply this exact refinement workflow to an existing technical plan, agent-generated proposal, architecture/refactor/migration plan, or approved-plan implementation check. Do not auto-trigger from generic mentions of plans, architecture, refactors, migrations, technical debt, or agent proposals alone.
---

# Refine Technical Plan

Use this skill to turn an existing technical plan into a safer implementation-ready plan. The user decides when to invoke it; do not proactively apply it just because a conversation mentions planning, architecture, refactoring, migration, or technical debt.

The skill exists for users who may be working outside their specialty and cannot easily judge whether an agent-generated plan is overconfident, under-evidenced, or likely to create preventable technical debt. The primary answer is always practical: whether implementation can start, and what must be added next if it cannot.

## Core Contract

- Review an existing plan; do not invent a plan when the user has not supplied one.
- Default to refining documents and plans. Write implementation code only when the user explicitly asks for code.
- Prefer an optimized plan body the user can merge into the original plan over a long critique-only report.
- Always include next actions when the plan cannot clearly start. The user should know exactly what information, decision, evidence, or plan content is needed next.
- Do not use numeric scores. Use a plain start decision and concrete blockers instead.
- Keep visible status labels minimal. Avoid detailed state-machine output by default.

## Modes

Choose the mode from the user's request:

1. `方案打磨`: default when the user asks to polish, refine, review, stress-test, or improve an existing technical plan.
2. `快速把关`: use when the user mainly asks whether the plan can start or wants a brief go/no-go answer. Keep detail compact, but do not weaken the approval standard.
3. `实现对照审查`: use only when the user provides or references an approved plan plus implementation evidence such as diff, config, migration, prompt/tool change, rollout steps, or deployment output. Compare implementation against the approved plan instead of re-opening the full plan discussion.

If modes overlap, choose the one that best protects the decision. A high-risk migration can get a compact answer, but blockers still need to be visible.

## Evidence First

Before judging a plan, collect only the evidence that can affect the decision:

1. Check accessible local context when relevant: repository files, docs, previous plans, tests, configs, schemas, API contracts, package versions, and code paths.
2. If no useful local evidence is available, say that plainly. Do not imply code or production context was inspected.
3. Check authoritative external sources when the pass decision depends on versioned or unstable facts: framework behavior, SDK/API support, cloud limits, provider guarantees, pricing, security guidance, standards, compliance rules, or release status.
4. Treat unchecked claims as assumptions or `待补证据`, not facts. A plan cannot be marked ready to start when its critical recommendation depends on unverified local or external facts.

Ask the user only for information that cannot reasonably be discovered from available files or authoritative sources, and ask for the smallest concrete addition needed.

## Review Lenses

Use expert roles as internal review lenses, not as roleplay. Select only lenses that expose a concrete risk, tradeoff, rewrite, or pass condition.

Common lenses:

- Architecture: boundaries, coupling, contracts, evolution path, rollback.
- Domain: correctness, invariants, edge cases, standards, current best practice when verified.
- Implementation: sequencing, ownership, complexity, testability, maintainability.
- Reliability/security: failure modes, privacy, permissions, abuse cases, observability, incident response.
- Product/operations: user impact, rollout, support burden, ownership, cleanup.
- State/platform: persistence, compatibility, migration safety, cost, performance, scaling.

In visible output, mention only lenses that changed the judgment. Avoid placeholder expert lists and simulated expert dialogue.

## Start Decision

Use the user's language. In Chinese, default to these labels:

- `可以开工`: material risks are resolved or explicitly accepted with credible ownership and validation.
- `基本可以开工，但有小备注`: only low-risk, owned, non-blocking cleanup remains.
- `暂缓开工，先修改方案`: the plan has unresolved risks, unclear boundaries, weak validation, unsafe migration, or likely long-lived debt.
- `缺关键输入，暂时不能判断`: responsible judgment would require guessing about facts, decisions, artifacts, or authority that are unavailable.

Keep these labels as the main decision interface. Do not replace them with scores or a complex approval state machine.

## Approval Standard

Say a plan can start only when the relevant items below are true:

- Scope is explicit: changed scope, unchanged scope, deferred work, and out-of-scope work are named when they affect risk.
- Architecture boundaries are clear: ownership, dependencies, public contracts, data flow, and state ownership are named.
- Evidence is honest: local facts and external technical claims are verified or clearly marked as assumptions.
- Migration is safe when relevant: rollout, compatibility, rollback, data/config cleanup, and dual-running risks are covered.
- Validation matches risk: high-risk changes have concrete tests, rollout signals, rollback checks, data checks, permission checks, observability, or acceptance criteria.
- Debt is controlled: shortcuts are deliberate, bounded, owned, and have removal triggers.
- Operations are realistic: deploy, monitor, support, incident response, and cleanup ownership are named when production impact exists.
- The optimized plan can be executed by an engineer who did not read the full conversation.

Accepted risk closes an item only when the plan states the consequence, affected scope, owner, trigger condition, exit or cleanup condition, and validation condition. Otherwise keep it in next actions or the ledger summary.

## Refinement Loop

Run one real refinement cycle by default. A real cycle materially changes the critique, optimized plan, start decision, pass condition, or next actions.

In one response, continue up to three real cycles only when each additional cycle can reduce risk using available context. Do not narrate empty cycles to satisfy a template. Across the conversation, keep refining until the plan can start or until missing input blocks responsible judgment.

Each real cycle:

1. Diagnose the plan: identify the biggest failure modes, missing facts, weak assumptions, and whether the plan is solving the right problem.
2. Improve the plan: preserve good decisions, remove weak or obsolete text, and rewrite the plan when that is the clearest way to close risks.
3. Gate the result: give the start decision, next actions, and any remaining risks or assumptions.

When key business, production, security, compliance, migration, vendor-contract, or ownership input is missing, you may still produce an assumption-bound optimized plan. Do not mark it ready to start until the blocking input is supplied or explicitly accepted with consequence and ownership.

## Next Actions

`下一步行动` is required whenever the decision is not `可以开工`.

Rules:

- Put next actions before detailed evidence or ledger material.
- List at most three immediate actions.
- Phrase actions as plan content to add, decisions to record, or evidence to verify.
- For each action, explain why it affects whether the plan can start.
- Say whether the user must provide information, or whether the assistant can continue from available context.
- Avoid vague advice like "add tests", "improve monitoring", or "provide more context". Name the specific validation, signal, owner, decision, artifact, or source of truth.

## Optimized Plan

Default to outputting `优化后方案` when the user asks for 打磨/优化, when the source plan is unsafe enough that a rewrite is the clearest fix, or when a refined plan would let the user move forward.

The optimized plan should be shareable technical-document content. Keep review mechanics outside it.

Use only sections that earn their place:

- `目标`
- `非目标`: include when scope control matters, especially for refactors, migrations, platformization, new dependencies, or broad "Common/Engine/Pipeline" proposals.
- `核心方案`
- `实施阶段`
- `验证与验收`
- `风险与回滚`
- `运维与观测`
- `待定事项`

If the plan is blocked on missing input, include the assumption-bound version only when useful, and keep unresolved items in `待定事项` or `下一步行动`.

## Ledger And Continuation

Maintain a compact refinement ledger internally across repeated reviews. Use stable IDs from prior user-supplied ledgers when available.

Default visible output is a short `风险摘要`, not a full ledger. Expand to a detailed ledger only when:

- the plan is high-risk;
- the review continues across multiple rounds and prior issues must be tracked;
- risk is explicitly accepted;
- the user asks for full traceability or 复查依据.

When shown, the ledger should track material risks without numeric score fields:

| 编号 | 问题 | 影响 | 证据状态 | 严重程度 | 处理状态 | 待补方案内容 | 通过条件 |
| --- | --- | --- | --- | --- | --- | --- | --- |

Use Chinese labels when the user writes Chinese:

- Evidence: `已验证`, `方案已写明`, `暂按假设`, `缺证据`, `未检查`
- Severity: `卡住`, `高`, `中`, `低`
- Handling: `未解决`, `部分解决`, `已解决`, `待补充`, `已接受风险`, `已被替代`

Do not output a separate detailed approval state machine by default.

## Agent Plan Calibration

When reviewing agent-generated plans, actively challenge confident claims that hide missing evidence.

Keep these open as assumptions, next actions, or ledger items until the plan supplies evidence, boundaries, ownership, or accepted-risk text:

- "最佳实践", "成熟团队都这么做", "最新版稳定支持", or similar unsupported claims.
- Full rollout first, one-shot migration, or "上线后再补" monitoring, permissions, cost controls, runbooks, or rollback.
- Rollback that only covers code while data, configuration, external state, prompts, tools, or provider-side changes remain mutated.
- Validation based on "用户反馈不错", "看起来没问题", or other non-verifiable acceptance criteria.
- Broad `Common`, `Engine`, `Pipeline`, platform, or middleware layers that absorb unrelated responsibilities mainly for "统一" or "省心".
- No evidence from the current repository, current versions, provider limits, operational owner, or production constraints.

## Implementation Conformance Review

Use this secondary mode only when comparing implementation evidence against an approved plan or prior pass conditions.

Do not re-litigate the whole plan by default. Check for plan drift:

- approved compatibility windows removed or shortened;
- rollout stages skipped;
- rollback, migration, cleanup, or validation omitted;
- permissions, audit, security, observability, or cost controls weakened;
- implementation takes shortcuts that bypass accepted risk conditions.

Output shape:

```markdown
**结论**
- 开工判断：可以合并 | 暂缓合并，先修正 | 缺证据，暂时不能判断
- 一句话原因：...
- 下一步：...

**下一步行动**
1. ...

**实现对照审查**
| 已批准条件 | 当前实现证据 | 判断 | 影响 | 修正要求 |
| --- | --- | --- | --- | --- |
| ... | ... | 符合/偏离/缺证据 | ... | ... |

**计划漂移**
- ...
```

## Default Output Shape

For plan refinement:

```markdown
**结论**
- 开工判断：可以开工 | 基本可以开工，但有小备注 | 暂缓开工，先修改方案 | 缺关键输入，暂时不能判断
- 一句话原因：...
- 下一步：...
- 是否需要用户补充信息：是/否，...

**下一步行动**
1. 补充内容/决策/证据：...
   为什么影响开工：...

**优化后方案**
[Only when rewritten or useful]

**依据与风险摘要**
- 已检查证据：...
- 关键假设：...
- 本轮评审视角：...
- 未关闭风险：...
```

Required in every review: `结论`. Required unless the plan can clearly start with no meaningful follow-up: `下一步行动`. Add `优化后方案` when the plan was actually rewritten or the user asked for refinement. Add `依据与风险摘要` when evidence or assumptions affect the decision.

Keep the output proportional. Quick go/no-go answers should stay short. High-risk or repeated reviews may include a detailed ledger after the action layer.

## Review Quality Rules

- Be concrete: tie each concern to a failure mode, code area, user impact, migration hazard, operational burden, or maintenance cost.
- Problem fit first: if the plan solves the wrong problem, say so and reshape the target.
- Risk-resolving detail only: add detail when it reduces a named risk.
- Preserve useful work: keep decisions that survive review; remove obsolete or unsafe text.
- Track prior issues across cycles: say whether prior concerns are `已解决`, `部分解决`, `仍未解决`, or `待补充`.
- Use `references/review-rubric.md` when the plan has meaningful architecture, refactoring, integration, state, security, reliability, rollout, or operational risk; when the user asks for top-expert review; or when repeated cycles need sharper pass/fail criteria.

Use `agents/openai.yaml` as interface metadata.
