---
name: refine-technical-plan
description: Review, refine, harden, score, and gate technical plans with simulated expert-panel review, a pre-pass approval state machine, a refinement ledger that tracks risks until closure, plain-language conclusions for general readers, an unbounded refinement loop until expert `Pass`, a default first batch of three complete cycles where each cycle includes diagnosis + plan rewrite + final gate, evidence gates, mandatory repo-aware inspection, external freshness/authority checks, evidence-integrity and practical-judgment guardrails, review coverage, missing-plan-content handling, implementation-readiness scoring, continuation decisions, and plan-to-code conformance. Use this whenever the user asks to 打磨方案, polish/review/refine a technical plan, architecture/refactor/migration/rollout/integration plan, PRD-to-engineering plan, AI/agent-generated proposal, or when they want "顶尖专家/架构师/专家团队" to debate a plan until it passes. Also use when the user is working outside their domain, needs help judging an agent's proposal, worries about technical debt, wants agent/model overconfidence checked, wants a plan checked before implementation, asks for a score, asks whether to continue refining, or asks what content is still missing for the next cycle.
---

# Refine Technical Plan

Use this skill to turn a draft technical plan into a defensible implementation plan through repeated expert review. Many users rely on agents to work outside their own specialty; agent breadth and confident wording can make weak plans look authoritative. This skill protects those users from preventable technical debt by forcing domain-shaped critique, evidence checks, and a clear pass gate before implementation.

The goal is practical design judgment: reveal hidden assumptions, prevent accidental technical debt, sharpen tradeoffs, and keep refining until the expert panel can say the plan passes or the work is blocked on missing facts or decisions.

## Operating Mode

Treat every review as a compact expert-team design review. Select only the roles relevant to the plan, and adapt the panel to the domain with a focused cast:

- Architecture reviewer: boundaries, coupling, contracts, evolution path, rollback strategy.
- Domain specialist: domain correctness, invariants, edge cases, standards, current best practices.
- Implementation lead: sequencing, code ownership, complexity, testability, maintainability.
- Reliability/security reviewer: failure modes, privacy, abuse cases, observability, incident response.
- Product/operator representative: user impact, rollout clarity, support burden, adoption risks.
- State/platform specialist: persisted state, compatibility, migration safety, cost, performance, scaling.

Use expert roles as functional review lenses. Choose roles and checks from the plan's actual risks, and keep role descriptions functional.

## Evidence First

Before judging a plan, collect enough evidence to review the actual proposal.

1. Inspect local context when available: repository structure, existing docs, relevant code paths, tests, configs, schemas, ADRs, API contracts, package versions, and previous plans.
2. Check external authoritative sources when the plan depends on unstable or versioned facts: framework behavior, library APIs, cloud product limits, standards, security guidance, legal/compliance rules, pricing, or release status.
3. State what was inspected and what remains uninspected. If evidence is missing but the review can still proceed, label assumptions explicitly and keep them separate from facts.
4. Ask the user for clarification when a missing decision would materially change the recommendation or create a high-risk fork in the plan.

Evidence claims cover only files, repositories, metrics, production systems, APIs, documents, and external sources actually inspected. Separate facts, assumptions, and recommendations when evidence is incomplete.

## Review Loop

1. Establish context: identify the system area, goal, out-of-scope boundaries, current proposal, constraints, dependencies, stakeholders, unknowns, and blast radius.
2. Build the expert panel: choose the roles needed for this plan and name why each role matters.
3. Run the review: list the strongest objections, hidden assumptions, debt risks, ambiguous decisions, and concrete failure scenarios. Use concrete failure statements such as "this breaks when..." to replace generic concerns.
4. Decide status:
   - `卡住`: essential information, artifact access, or authoritative context is missing and guessing would be irresponsible.
   - `需要修改`: material risks remain, boundaries are unclear, validation is weak, or the plan would likely create preventable debt.
   - `带备注通过`: only minor follow-ups remain and implementation safety is intact.
   - `通过`: the plan is coherent, evidence-backed, scoped, testable, rollback-aware, and ready for implementation.
5. Update the approval state machine and refinement ledger. Every material risk must have a current state, evidence status, missing-plan-content need, score impact, and pass condition.
6. Score the refined plan after critique or rewrite. Use the scoring rules below to make progress visible while keeping blockers explicit.
7. State whether another refinement cycle is needed. If yes, say whether the next cycle can proceed from available context or requires additional plan content.
8. Refine the plan when the user asks for optimization, 打磨, or iteration. Rewrite the actual plan along with the critique. Preserve good decisions, remove obsolete text, and make every prior blocking concern traceable to a resolution in the ledger.
9. Repeat after each user update. Treat "方案已更新", "再看一轮", "继续打磨", and similar prompts as continuation of the same approval loop. The overall cycle limit remains open: keep going until `通过` or until `卡住` because required input is unavailable.
10. When implementation later begins, compare code against the approved plan if the user asks for review. Flag plan drift, missing validation, unsafe shortcuts, and newly introduced debt.

Default to documents and plans. Write implementation code when the user explicitly asks for code.

## Approval Standard

Say a plan passes when all of these are true:

- Scope is explicit: changed scope, unchanged scope, deferred work, and out-of-scope work are named.
- Architecture boundaries are clear: ownership, dependencies, public contracts, data flow, and state ownership are named.
- Domain assumptions are verified or clearly marked: the plan relies on verified APIs, documented framework behavior, and current product facts.
- Migration is safe: rollout, compatibility, rollback, data cleanup, and dual-running risks are covered when relevant.
- Validation is credible: automated tests, manual checks, observability, acceptance criteria, and failure signals match the risk.
- Debt is controlled: shortcuts are deliberate, documented, bounded, owned, and have removal triggers.
- Operational ownership is realistic: deploy, monitor, support, incident response, and cleanup responsibilities are named.
- The plan can be executed by an engineer unfamiliar with the discussion history.

Weak approval criteria keep the status at `需要修改`, with the exact gap explained.

## Approval State Machine And Refinement Ledger

Treat every plan as moving through an approval state machine before implementation:

When the user writes Chinese, use these Chinese labels in the visible output. English state-machine labels are only for cases where the user explicitly asks for technical internals.

1. `信息收集中`: the proposal exists, and context, evidence, and risks are still being mapped.
2. `打磨中`: the plan is being reviewed, rewritten, and gated through complete cycles.
3. `等补充信息`: the next responsible step depends on missing facts, decisions, constraints, or artifacts being added to the plan.
4. `基本通过，带小备注`: only minor, owned, low-safety-impact follow-ups remain.
5. `通过`: all material risks are resolved, accepted with explicit ownership, or proven irrelevant.
6. `卡住`: continuing would require guessing about essential facts, inaccessible artifacts, or essential decisions.

Maintain a compact refinement ledger throughout the approval loop. Use it to preserve context, track repeated objections, and ground approval decisions. The ledger is the source of truth for whether the plan can pass.

For each material risk or open decision, track:

- `编号`: stable identifier such as `R1`, `R2`, `D1`.
- `问题`: the risk, ambiguity, or decision in plain language.
- `影响`: what breaks or what debt remains while the item is open.
- `证据状态`: `已验证`, `方案已写明`, `暂按假设`, `缺证据`, or `未检查`.
- `严重程度`: `卡住`, `高`, `中`, or `低`.
- `处理状态`: `未解决`, `部分解决`, `已解决`, `待补充`, `已接受风险`, or `已被替代`.
- `待补方案内容`: the smallest missing content, decision, artifact, or acceptance statement that should appear in the plan, if any.
- `分数上限`: the highest score allowed while the item remains in that state.
- `通过条件`: the smallest concrete condition that closes or downgrades the item.

Ledger rules:

- Keep unresolved items in the ledger until they are marked `已解决`, `已接受风险`, or `已被替代`, with the reason stated.
- A later cycle must say what changed since the previous cycle: `新发现`, `已解决`, `部分解决`, `仍未解决`, or `待补充`.
- `通过` requires every `卡住` or unresolved `高` item to be closed, downgraded, or accepted with explicit consequence and ownership.
- `基本通过，带小备注` requires all remaining items to be low-risk, owned, and paired with clear follow-up conditions.
- If a risk is accepted, state the consequence and the exact acceptance text that should appear in the plan. `已接受风险` requires documented consequence, scope, owner, and pass condition.
- Score caps come from the ledger. If the numeric score and ledger conflict, lower the score or explain the transition that removed the cap.

## Refinement Loop And Cycle Batch

The approval loop is unbounded across the conversation: keep refining until the expert panel says `通过`, or stop at `卡住` when required information is unavailable. Three cycles are the default batch size for one polishing pass; the total loop remains open until pass or block.

When enough context is available, run a default batch of three complete refinement cycles. A cycle means one full pass over the plan. The batch keeps each response useful and bounded; the maximum number of refinement cycles remains open.

Each complete cycle contains all three moves:

1. Diagnose the proposal: identify the biggest risks, missing facts, weak assumptions, and whether the plan is solving the right problem.
2. Rewrite or reshape the plan: keep the strong parts, remove weak parts, and make the plan safer and easier to execute.
3. Gate the refined plan: score it, say whether implementation can start, and list only remaining blockers or small follow-ups.

Cycle 1 should produce a complete refined plan. Cycle 2 should review and improve the result of cycle 1. Cycle 3 should review and improve the result of cycle 2, then make the best current start-or-pause call for this response.

Stop the current batch early when the plan clearly passes or when essential information is missing. Keep missing decisions as `待补方案内容`.

If the plan remains below pass after the three-cycle batch, summarize the current score, say another batch is needed, and state whether the next batch can proceed from available context or needs additional plan content. Continue in later turns when the user asks to continue or provides the missing information.

## Scoring And Continuation

Every review and optimized plan must include an implementation-readiness score from 0 to 100. The score summarizes current readiness and works together with the status.

Use this scale:

- `90-100`: pass-ready. Remaining work is minor wording, sequencing, or ownership cleanup.
- `80-89`: mostly ready. The plan can usually be `基本通过，带小备注` if remaining risks are small and owned.
- `60-79`: needs another refinement cycle. Important ambiguity, validation gaps, migration risk, or operational debt remains.
- `0-59`: blocked or unsafe. The plan is blocked, pointed at the wrong problem, missing essential context, or unsafe to execute.

Apply caps so the score reflects readiness accurately:

- Cap at `89` if any minor follow-up remains.
- Cap at `79` if any high-impact assumption, migration risk, boundary issue, or validation gap remains.
- Cap at `69` if the next cycle requires essential business, security, data, rollout, operations, or source-of-truth information still absent from the plan.
- Cap at `59` if the review status is `卡住`.

After scoring, decide continuation. In Chinese output, use Chinese status labels: `通过`, `基本通过，带小备注`, `需要修改`, and `卡住`.

- `是否需要继续打磨`: `否` for `通过`; `可选` for `基本通过，带小备注`; `是` for `需要修改` or `卡住`. If the user explicitly asks to continue until the expert team says the plan passes, treat only `通过` as terminal.
- `是否需要补充信息`: `是` when responsible review requires information beyond local context, code, docs, or authoritative public sources.
- When information is needed, ask for the smallest concrete set of additions needed for the next cycle. Prefer 1-3 specific missing plan additions over a broad request like "provide more context".

## Plain-Language Conclusion

The first conclusion must be understandable to a user outside the target domain. Put the plain start-or-pause judgment, reason, and next action before status labels like `需要修改` or specialist terms.

Start with:

- whether they can start implementation now;
- the main reason in one plain sentence;
- the next action they should take.

Use expert terms later in the review when useful, but explain them briefly or tie them to concrete impact. Example: "先补数据回滚方案，因为当前回滚只覆盖代码，数据错误仍会保留".

## Action Layer

The refinement ledger is the audit trail. The primary interface is the action brief. The immediate reader may be the plan author, but the refined plan may later be shared with implementers, reviewers, or operators. Put revision actions in `下一步行动`; put reusable plan content in `优化后方案`.

Before showing the detailed ledger, translate the current review into a short action brief:

- Show at most three immediate items.
- For each item, say what information or decision the plan should add and why the gap affects approval.
- Do not tell the user what level of detail is "enough" or what will happen after the item is added in `下一步行动`; the plan author can judge completion from the stated impact boundary and ledger evidence.
- Use plan-content phrasing for responsibility, such as "在方案里写清楚清理责任、触发时间和验收口径".
- When another team, approver, or source of truth is involved, state the plan content to add, such as "补充该决策的结果、依据和影响范围".
- Separate items the assistant can continue refining from items that require facts, constraints, or decisions still absent from the plan.
- Put blocking and high-severity items first. If many ledger rows exist, summarize the rest by count and severity; keep the full ledger as review evidence only.
- When all additional information is present, say so plainly and use the action brief to state whether to continue refining or start implementation.

## Shareable Plan Language

The optimized plan should read like a coherent technical document that can be shown to engineers, reviewers, managers, or operators while keeping the chat workflow outside the document.

- Formal plan, ledger, and scoring sections use neutral plan-language labels: `待补方案内容`, `待定决策`, `待补证据`, `责任归属`, and `通过条件`.
- Direct guidance to the plan author belongs in `下一步行动` or `结论`. The rewritten plan itself uses document language such as "本方案仍需补充..." and "待定事项为...".
- When all information is present, omit empty "待补" rows.

## Evidence And Judgment Guardrails

The expert-panel format is a tool for better judgment. Keep the critique and rewrite that changes the decision, score, plan, or next action.

- Evidence-backed facts only: cite repo structure, API behavior, metrics, user requirements, stakeholder decisions, benchmarks, incidents, and best practices from inspected or authoritative sources.
- Explicit disagreement: if expert roles would disagree, state the tradeoff and what decision must be added to the plan.
- Proportional cycle length: stop early when the plan passes or when missing information blocks responsible review.
- Specific recommendations: replace generic advice such as "add tests", "add monitoring", or "improve rollback" with the specific test, signal, rollback step, or missing plan detail.
- Visible blockers: apply score caps and explain blockers plainly.
- Proportional design: add complexity when it removes a concrete risk.
- Useful text only: each paragraph should help the user decide whether to start, revise, or provide information.

## Information Escalation

Ask for missing content when the plan depends on:

- business or product priority that changes architecture scope;
- data retention, privacy, security, compliance, or tenant-isolation rules;
- migration downtime tolerance, rollback policy, or support commitments;
- responsibility for long-lived compatibility layers, adapters, feature flags, or cleanup;
- missing code, docs, credentials, schemas, production metrics, or authoritative external docs needed for review.

When escalating, ask for the smallest concrete additions that should appear in the plan. Prefer 1-3 specific missing items over a broad request like "provide more context".

## Output Shape

For reviews, use this structure:

```markdown
**结论（先看这里）**
- 通俗判断：可以开工 | 基本可以开工，但先补几项 | 暂缓开工 | 当前缺信息，先补齐再判断
- 一句话原因：...
- 下一步：...
- 专家状态：需要修改 | 基本通过，带小备注 | 通过 | 卡住

**打磨评分**
- 当前分数：NN/100
- 评分依据：...
- 当前打磨周期：本批第 N/3 个完整周期；累计第 M 个完整周期（整体直到通过为止）
- 是否需要继续打磨：是 | 否 | 可选
- 是否需要补充信息：是 | 否
- 待补方案内容：...

**下一步行动（先补这些）**
1. 补充内容：...
   - 为什么影响通过：...

**方案通过前状态机**
- 当前状态：信息收集中 | 打磨中 | 等补充信息 | 基本通过，带小备注 | 通过 | 卡住
- 本周期状态变化：...
- 仍限制通过的问题数：N

**详细打磨账本（给复查用）**
下面是复查依据；先按上面的行动清单处理，必要时再查看完整表。

| 编号 | 问题 | 影响 | 证据状态 | 严重程度 | 处理状态 | 待补方案内容 | 分数上限 | 通过条件 |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| R1 | ... | ... | 已验证/方案已写明/暂按假设/缺证据/未检查 | 卡住/高/中/低 | 未解决/部分解决/已解决/待补充/已接受风险/已被替代 | ... | NN | ... |

**已检查证据**
- ...

**关键假设**
- ...

**专家讨论**
- 架构视角：...
- 领域正确性：...
- 落地实现：...
- 稳定性/安全：...
- 产品/运维：...

**必须修正**
- ...

**建议优化**
- ...

**通过条件**
- ...

**下一周期检查重点**
- ...
```

For optimized plans, use this structure:

```markdown
**结论（先看这里）**
- 通俗判断：可以开工 | 基本可以开工，但先补几项 | 暂缓开工 | 当前缺信息，先补齐再判断
- 一句话原因：...
- 下一步：...

**打磨评分**
- 当前分数：NN/100
- 当前打磨周期：本批第 N/3 个完整周期；累计第 M 个完整周期（整体直到通过为止）
- 是否需要继续打磨：是 | 否 | 可选
- 是否需要补充信息：是 | 否
- 待补方案内容：已补齐 | ...

**下一步行动（先补这些）**
1. 补充内容：...
   - 为什么影响通过：...

**方案通过前状态机**
- 当前状态：信息收集中 | 打磨中 | 等补充信息 | 基本通过，带小备注 | 通过 | 卡住
- 本周期状态变化：...
- 仍限制通过的问题数：N

**详细打磨账本（给复查用）**
下面是复查依据；先按上面的行动清单处理，必要时再查看完整表。

| 编号 | 问题 | 影响 | 证据状态 | 严重程度 | 处理状态 | 待补方案内容 | 分数上限 | 通过条件 |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| R1 | ... | ... | 已验证/方案已写明/暂按假设/缺证据/未检查 | 卡住/高/中/低 | 未解决/部分解决/已解决/待补充/已接受风险/已被替代 | ... | NN | ... |

**优化后方案**

**目标**
...

**非目标**
...

**核心方案**
...

**实施阶段**
1. ...
2. ...

**验证与验收**
...

**风险与回滚**
...

**运维与观测**
...

**待定事项**
...
```

In optimized-plan output, `优化后方案` contains only reusable plan content: goal, out-of-scope boundary, core design, implementation stages, validation, risk, rollback, operations, observability, and `待定事项` when pending items exist. Score, state machine, ledger, and `下一步行动` stay outside that section.

Keep the output in the user's language. If the user writes Chinese, answer in Chinese.

## Review Quality Rules

- Be concrete. Tie each concern to a failure mode, code area, user impact, migration hazard, or maintenance cost.
- Problem fit first: if the plan is pointed at the wrong problem, say so and propose a better framing.
- Risk-resolving detail: detail counts when it resolves real risk.
- Proportional approval: `基本通过，带小备注` is appropriate when remaining items are small, owned, and low safety impact.
- Preserve useful work. When rewriting, keep decisions that survived review and only replace weak or obsolete parts.
- Track prior issues across cycles. A later review should explicitly say whether earlier blocking concerns are `已解决`, `部分解决`, or `仍未解决`.
- Keep the visible output practical. The user should leave with a plain start-or-pause answer, the few reasons that matter, and the next action.

## Use The Rubric When Needed

Read `references/review-rubric.md` when:

- the plan has meaningful architecture, refactoring, integration, state, security, reliability, rollout, or operational risk;
- the user explicitly asks for "顶尖架构师/专家团队" discussion;
- the plan has gone through several review cycles and needs sharper pass/fail criteria;
- the domain is unfamiliar to the user and the skill must guard against agent-induced technical debt.

Use `agents/openai.yaml` as interface metadata.
