---
name: refine-technical-plan
description: Review and refine technical plans with simulated expert-panel, mandatory repo-aware inspection, evidence ledgers, severity gates, rewrites, stress tests, implementation-readiness, and plan-to-code conformance decisions. Use when asked to 打磨方案, review architecture/refactor/migration/implementation plans, PRD-to-engineering plans, agent-generated proposals, implementation diffs from an approved plan, “方案已经更新”, or multi-round review until safe to proceed.
---

# Refine Technical Plan

Use this skill as the primary entrypoint for hardening technical plans before and after implementation. Turn a draft or evolving proposal into a defensible implementation-ready plan through repeated expert-style review, targeted rewrites, mandatory repository-aware evidence checks, stress tests, implementation-readiness gates, and optional plan-to-code conformance review.

This is a simulated expert-panel review workflow. It does not claim that real external experts participated.

## When to Use

Use this skill when the user asks to:

- polish, 打磨, review, revise, or approve a technical plan;
- review an architecture, refactor, migration, rollout, integration, platform, data, AI/agent, frontend, backend, or implementation plan;
- convert a PRD or product idea into an engineering plan;
- check an agent-generated proposal for hidden assumptions, technical debt, missing validation, missing rollback, or unsafe design;
- review code, config, migration, prompt/tool, tests, docs, or deployment changes produced from an approved plan;
- run multi-round review until a plan reaches `Pass`, `Pass under assumptions`, `Pass with notes`, `Revise`, or `Blocked`;
- continue from an updated plan, especially when the user says "方案已经更新".

Do not use this skill when the user only asks for direct code implementation, debugging a concrete error, casual explanation, or brainstorming without plan hardening. If the user asks for code after a plan passes, implementation may proceed, but the approved plan's gates become implementation constraints. After code, config, migration, prompt/tool, tests, docs, or deployment changes are produced, run `Implementation Conformance Review` by default unless the user explicitly opts out.

## Defaults

- Reply in the user's language; use Chinese when the user writes Chinese.
- Do not write implementation code unless the user explicitly asks for code.
- Default to automatic multi-round review and rewrite when enough context exists.
- Keep one response bounded: run at most 3 review/rewrite cycles per response, then stop with the best current plan and remaining blockers.
- Continue across conversation turns without a fixed round limit. Do not force `Pass` just to end the loop.
- Treat the expert panel as a simulated review method, not real external expert endorsement.
- The user does not need to provide an implementation plan. A rough proposal, architecture idea, PRD, refactor direction, or agent-generated plan is enough. Derive implementation steps only when needed to make the plan executable, testable, and reviewable.
- In Codex, Claude Code, Cursor Agent, Devin, or any repository-aware coding agent, repository inspection is mandatory before any pass-like or implementation-ready decision.
- Inspect local docs/code/config/tests when they are available and materially affect the plan.
- Output inspected paths, commands, or search areas when repository evidence is used.
- Ask the user last: ask for input only when continuing would require guessing about business constraints, production behavior, external contracts, compliance constraints, operator constraints, or domain-owner facts that cannot reasonably be discovered from the repository.

## References

Use progressive disclosure: load only the reference files needed for the current request.

- Read `references/review-rubric.md` for universal review lenses, change-type lenses, AI/agent lenses, non-expert decision brief guidance, and domain adaptation questions.
- Read `references/quality-gates.md` for severity definitions, pass/fail gates, evidence rules, approval-to-action semantics, high-risk validation gates, implementation conformance gates, technical debt traps, and slice/direct/hybrid refactor selection.
- Read `references/domain-packs.md` when the plan clearly involves frontend, backend/API, data/migration, infrastructure/SRE, security/privacy, AI/agent, mobile/desktop, or other specialized engineering domains.
- Read `references/document-template.md` when the user asks to write or update a plan, implementation-ready plan, ADR-like note, or docs artifact.

## Expected Inputs

Minimum input:

- a plan, proposal, PRD, agent-generated方案, or rough technical direction.

Helpful context when available:

- repository paths, code areas, docs, schemas, APIs, tests, configs, logs, metrics, rollout constraints, owner constraints, production behavior, or specific concerns from the user.

Do not require the user to provide everything up front. In repo-aware environments, inspect available artifacts before asking.

## Review Modes

Select the lightest mode that satisfies the user request:

- Review-only: critique without rewriting.
- Rewrite: critique, revise, and re-review.
- Delta review: compare an updated plan against prior blockers and decisions.
- Document mode: write or update a docs artifact.
- Implementation-readiness review: decide whether engineers or coding agents can start safely.
- Implementation-conformance review: compare actual code/config/migration/test/docs changes against an approved plan.
- Evidence-collection mode: convert missing evidence into concrete repo searches, validation tasks, owner confirmations, or production checks.
- Validation-evidence review: evaluate test, CI, dry-run, eval, smoke, dashboard, rollback rehearsal, or owner-confirmation artifacts.
- Pre-implementation risk review: focus only on hidden debt, rollback, validation, and unknowns.

## Context Intake

Before review, extract these from the artifact, repository, docs, and prior discussion. Ask only when a missing fact materially changes pass/fail status.

- Artifact under review: draft plan, architecture proposal, PRD, code diff, migration plan, docs update, or agent-generated proposal.
- Change type: new capability, refactor/replacement, migration, integration, runtime/platform, state/data, human workflow, AI/model behavior, documentation/process, or mixed change.
- Affected surface: users, callers, APIs, events, DTOs, schemas, persisted state, prompts/tools, permissions, jobs, config, deployment, operations, documentation, or support process.
- Available evidence: plan text, code, docs, schemas, configs, tests, fixtures, logs, metrics, traces, evals, prior discussion, user-provided constraints, or owner confirmations.
- Repository inspection status: inspected paths, commands/search terms, artifacts not inspected, and why.
- Implementation state: no implementation yet, partial diff exists, full diff exists, validation artifacts exist, or rollout artifacts exist.
- Unknowns: missing facts that materially affect design, risk, validation, rollout, or pass/fail status.
- Blast radius: low, medium, or high.
- Reversibility: reversible, partially reversible, or irreversible.


## Mandatory Repo-Aware Agent Mode

When running inside Codex, Claude Code, Cursor Agent, Devin, or any repository-aware coding agent, repository inspection is mandatory before review, rewrite, implementation-readiness, pass-like, or implementation-conformance decisions.

Do not treat the user-provided plan as the only source of truth. Before reviewing or rewriting the plan, inspect relevant repository context when available and material to the decision:

- existing implementation paths and ownership boundaries;
- public APIs, events, DTOs, schemas, storage formats, prompts, tools, UI states, CLI behavior, or workflow states;
- callers, consumers, imports, routes, jobs, workflows, feature flags, config paths, generated code, or integration points;
- tests, fixtures, snapshots, evals, migration files, seed data, build scripts, and CI-relevant commands;
- documentation, ADRs, runbooks, deployment files, environment assumptions, and operational procedures;
- existing error semantics, permission checks, observability, logging, rollback mechanisms, and cleanup conventions.

Use repository evidence to confirm, weaken, or reject the plan's claims. Prefer repository evidence over plan claims when they conflict, and mark the conflict as `Blocker` or `Major` depending on impact.

Do not claim repository-backed confidence, implementation readiness, or safe-to-proceed status unless relevant repository areas were inspected or the uninspected areas are explicitly listed as assumptions.

If the agent cannot inspect the repository because of tool limits, missing checkout, permissions, context-window limits, or unavailable artifacts, mark the decision as assumption-backed rather than repository-backed.

## Repository Inspection Ledger

When repository context is material, include a compact ledger before any pass-like decision. For low-risk docs-only work, this can be compressed to one paragraph.

```markdown
| Area | Paths / Commands / Search Terms | Why Inspected | Finding | Confidence |
| --- | --- | --- | --- | --- |
| Existing implementation |  |  |  | High/Medium/Low |
| Callers / consumers |  |  |  | High/Medium/Low |
| Contracts / schemas / prompts / tools |  |  |  | High/Medium/Low |
| Tests / fixtures / evals |  |  |  | High/Medium/Low |
| Migrations / data / config |  |  |  | High/Medium/Low |
| Docs / runbooks / deployment |  |  |  | High/Medium/Low |
```

Rules:

- Name concrete paths, commands, or search terms where possible, such as `rg "FooService"`, `src/foo/*`, `db/migrations/*`, or `docs/runbooks/*`.
- Do not write vague claims like "checked the repo" without naming what was checked.
- If an area is material but uninspected, list it as `Not inspected`, explain why, and treat related conclusions as assumptions.
- If no repository is available, say so explicitly and do not use repository-backed language.

## Ask-User-Last Policy

In repository-aware coding agents, ask the user only for facts outside the repository or outside available tooling:

- business priority, product intent, or user-experience tradeoff;
- compliance, policy, legal, security, or privacy owner decision;
- production incident context, traffic shape, operational constraints, or historical failure not present in logs/docs;
- external contract or partner behavior not represented in the repo;
- rollout appetite, accepted-risk owner, or domain-owner approval.

Do not ask the user for facts that can be discovered from the repository, such as file paths, existing API shape, schema fields, caller inventory, test commands, route names, config names, docs, migration files, feature flags, prompt/tool definitions, or current implementation boundaries.

## Domain Pack

After context intake, derive a temporary domain checklist from evidence. Do not assume a domain by default.

Examples of derived checks:

- Human-facing experience: state ownership, loading/error/empty states, accessibility, responsiveness, stale data, user-visible degradation.
- Service or interface contracts: compatibility, idempotency, authorization, quotas, error semantics, observability, deprecation.
- State or migration: source of truth, invariants, expand-migrate-contract, backfill, reconciliation, rollback after irreversible changes.
- Runtime or operations: deployment, config, resource limits, concurrency, startup/shutdown, alerts, runbooks.
- AI or agent behavior: evals, hallucination containment, prompt injection, tool misuse, human override, auditability, replayability.

If a domain is material and requires deeper checks, load `references/domain-packs.md`. Skip irrelevant checks to avoid cargo-cult review.

## Expert Panel

Simulate a compact expert panel. Use roles to structure critique; do not invent credentials or real people.

Always include:

- Architecture owner: boundaries, coupling, contracts, evolution path, rollback strategy.
- Domain specialist: domain correctness, invariants, edge cases, hidden assumptions.
- Implementation lead: migration sequence, ownership, complexity, testability.
- Reliability/security reviewer: failure modes, data safety, observability, abuse cases.
- Product/operator representative: user impact, operational burden, rollout clarity.

Add specialized roles only when the plan justifies them: interface/contract, runtime/operations, state/migration, user experience, security/privacy, compliance, performance, cost, platform, AI/model behavior, or developer experience.

## Reviewer Independence Protocol

Use role separation to reduce self-approval risk.

For automatic multi-round review, separate the work into three passes:

1. Proposal pass:
   - Improve the plan only as the plan author.
   - Do not approve the plan in this pass.
2. Red-team pass:
   - Review the plan as if it was written by another agent.
   - Look for hidden assumptions, missing evidence, false confidence, over-broad abstractions, and implementation traps.
   - Do not defend prior decisions.
3. Gatekeeper pass:
   - Decide status only from evidence, assumptions, validation, severity rules, and repository facts.
   - Do not mark `Pass` just because the plan improved.
   - Do not mark absolute `Pass` when important evidence is missing.

For high-risk, irreversible, security-sensitive, data-changing, public-contract, billing, compliance, or production-impacting plans, require repository evidence, validation evidence, or owner confirmation before absolute `Pass`.

## Evidence Discipline

Do not treat plausible reasoning as sufficient evidence.

For every `Blocker` or `Major` finding, identify one of:

- exact plan text that creates the risk;
- repository, documentation, schema, config, test, log, metric, trace, eval, or prior-discussion evidence;
- missing business, production, external contract, compliance, or domain-owner fact;
- concrete failure scenario that makes the risk material.

For every `Pass`, `Pass under assumptions`, or `Pass with notes` decision, state whether the decision is:

- evidence-backed;
- assumption-backed;
- validation-backed;
- repository-backed;
- owner/production-backed when applicable.

Do not mark absolute `Pass` when important contracts, caller inventory, data shape, production behavior, permissions, rollback behavior, business constraints, or compliance constraints are only assumed. Use `Pass under assumptions`, `Revise`, or `Blocked` instead.


## Evidence Collection Plan

When missing evidence blocks or conditions approval, do not keep polishing the plan to hide the gap. Produce an evidence collection plan instead of absolute `Pass`.

Use this when a `Blocker`, `Major`, high-risk validation gate, repository mismatch, or `Pass under assumptions` depends on missing facts.

```markdown
| Missing Evidence | Why It Matters | How To Collect | Owner / Source | Decision Impact | Can Implementation Start? |
| --- | --- | --- | --- | --- | --- |
| Caller inventory | Breaking contract risk | Search imports/routes/logs/API clients | Repo / API owner | Blocks absolute Pass | Discovery only |
| Data shape | Migration risk | Schema query/sample/dry-run | DB / data owner | Blocks rollout | No production change |
| Rollback proof | Recovery risk | Rollback rehearsal/smoke test | Operator / CI | Blocks release | Behind flag only |
```

Rules:

- Convert each material unknown into a concrete search, validation, owner confirmation, or production check.
- Separate repo-discoverable evidence from external owner or production facts.
- If implementation can start only as discovery, spike, or guarded reversible work, say so explicitly.
- Do not convert unresolved ambiguity into `Accepted Risk` unless it has owner, impact, mitigation, monitoring/validation, trigger, expiry, and reason for acceptance.


## Approval-to-Action Semantics

Tie every decision to what a coding agent is allowed to do next.

| Decision | Allowed Next Action | Not Allowed |
| --- | --- | --- |
| `Pass` | Full implementation may start; preserve gate evidence and run implementation conformance after changes. | Ignoring validation, rollback, or cleanup evidence. |
| `Pass with notes` | Implementation may start; notes must become tracked tasks or explicit accepted risks. | Treating minor notes as optional when they protect tests, docs, cleanup, or ownership. |
| `Pass under assumptions` | Discovery, spike, validation, owner confirmation, or reversible guarded implementation may start. | Full production implementation, irreversible migration, public contract change, or rollout before assumptions are verified or accepted. |
| `Revise` | Revise plan or perform targeted risk-reduction work only. | Implementing the full plan as if approved. |
| `Blocked` | Collect evidence, confirm owner/business facts, or reduce scope to a minimal safe plan. | Continuing by guessing or implementing production-impacting work. |

For high-risk plans, `Pass under assumptions` is not implementation approval for irreversible or production-impacting work. It is approval to validate assumptions or do bounded reversible discovery.

## Automatic Review Loop

Use this loop by default unless the user asks for a single round, brainstorming, or discussion without rewriting.

1. Run context intake, inspect relevant repository evidence when available, build the repository inspection ledger, and derive a domain pack.
2. Run expert review: list the strongest objections, hidden assumptions, debt risks, and ambiguous decisions. Prefer concrete failure scenarios over generic concerns.
3. Classify findings with severity: `Blocker`, `Major`, `Minor`, or `Accepted Risk`.
4. Build or update the gate evidence matrix for scope, boundaries, contracts, state/migration, failure behavior, observability, validation, rollout/rollback, security/privacy, and debt control.
5. If evidence gaps block or condition approval, produce an Evidence Collection Plan.
6. Resolve expert disagreements explicitly when roles recommend different paths.
7. Decide status and allowed next action using Approval-to-Action Semantics:
   - `Blocked`: essential input is missing and continuing would require guessing.
   - `Revise`: any `Blocker` or unresolved `Major` remains, or the plan would likely create avoidable debt.
   - `Pass under assumptions`: no blocker or major remains if explicitly listed assumptions hold, but important facts were not independently verified.
   - `Pass with notes`: only `Minor` issues or explicit `Accepted Risk` items remain.
   - `Pass`: the plan is evidence-backed, repository-backed, validation-backed, or owner-confirmed as needed; coherent; scoped; testable; observable; and ready to implement.
8. If status is `Revise` and enough context exists, rewrite the plan. Preserve good decisions, remove obsolete text, and directly address blockers and major findings.
9. Re-review the rewritten plan. Track whether earlier concerns were resolved, weakened, still open, or replaced by new risks.
10. Before final `Pass`, `Pass under assumptions`, or `Pass with notes`, run adversarial review and the pre-pass stress test.
11. Stop when the status is `Pass`, `Pass under assumptions`, `Pass with notes`, `Blocked`, or after 3 cycles in the current response.

Do not ask the user to manually trigger the next round when enough information exists to continue.

## Disagreement Resolution

When expert roles disagree, do not average the opinions.

Resolve disagreement by:

1. naming the conflicting recommendations;
2. identifying which risk or invariant each role is protecting;
3. comparing blast radius, reversibility, validation strength, delivery complexity, and long-term debt;
4. choosing one path explicitly;
5. recording rejected options and accepted risks.

## Adversarial Review

Before final rewrite or any pass-like decision, run a short red-team pass:

- What assumption would invalidate the plan?
- What hidden coupling could make implementation unsafe?
- What temporary compatibility path could become permanent debt?
- What test would falsely pass while production still breaks?
- What would a future maintainer misunderstand?
- What rollback path only works in theory but not after data, state, or external behavior changes?
- What user, caller, or operator impact is easy to miss because the plan is written from the implementer's perspective?
- What would an agent hallucinate because the plan lacks concrete contracts?
- What evidence would most quickly prove the plan wrong?

Use adversarial review to strengthen the final plan, not to create endless critique.

## Pre-Pass Stress Test

Before marking `Pass`, `Pass under assumptions`, or `Pass with notes`, run at least three concrete failure scenarios:

1. Most likely failure.
2. Most expensive or user-visible failure.
3. Hardest-to-detect technical-debt failure.

For each scenario, state:

- trigger;
- affected user, caller, component, or data;
- detection signal;
- mitigation;
- rollback or cleanup path;
- whether the final plan handles it.

## Validation Evidence Mode

When the user provides validation artifacts, use them to upgrade or downgrade the decision.

Validation artifacts may include test results, CI logs, migration dry-run output, contract test output, screenshots or recordings, eval result summaries, load/performance results, smoke check output, dashboard or alert screenshots, rollback rehearsal evidence, production observation, or domain owner confirmation.

For each artifact, state:

- what claim it validates;
- what it does not validate;
- whether remaining risk changes severity;
- whether the decision changes.

Do not treat a validation plan as validation evidence. A plan that says "we will test this" is not validation-backed until results are provided.


## Implementation Conformance Review

Use after code, config, migration, prompt/tool, tests, docs, or deployment changes are produced from an approved plan. This mode is mandatory by default after an agent implements a previously approved plan unless the user explicitly opts out.

Compare the approved plan against the actual diff and validation artifacts. Do not approve implementation merely because the code compiles or tests pass.

Required checks:

- Map changed files to the approved plan sections, gates, or accepted risks they satisfy.
- Identify any unplanned APIs, schemas, files, routes, permissions, configs, prompts, tools, migrations, jobs, side effects, public behavior, or operational procedures.
- Verify the implementation preserves approved boundaries, contracts, validation requirements, rollout/rollback, observability, cleanup triggers, and accepted-risk owners.
- Verify tests target the real production/user/contract/migration/security/AI-agent risk, not merely the lines the agent changed.
- Verify validation evidence exists or is explicitly still pending.
- Verify temporary adapters, feature flags, dual paths, compatibility shells, facades, prompts, eval fixtures, queues, dashboards, or runbooks have owner, metric/signal, and deletion or review trigger.
- Identify implementation shortcuts that create new debt not reviewed in the plan.

Decision outcomes:

- `Implementation Pass`: implementation matches the approved plan; required tests/evidence are present or appropriately deferred by accepted risk.
- `Implementation Pass under assumptions`: implementation matches the plan, but material validation, owner confirmation, or production evidence is still pending.
- `Revise implementation`: implementation deviates from the plan, lacks required tests/evidence, or introduces unreviewed debt.
- `Implementation Blocked`: essential implementation evidence is unavailable or the diff cannot be reviewed safely.

Output shape:

```markdown
**Implementation Conformance Decision**
- 结论：Implementation Pass | Implementation Pass under assumptions | Revise implementation | Implementation Blocked
- 是否可以合并/发布：是 / 否 / 仅在验证完成后
- 一句话原因：...

**Diff-to-Plan Mapping**
| Changed Area / File | Approved Plan Section / Gate | Match? | Evidence | Notes |
| --- | --- | --- | --- | --- |

**Unplanned Changes**
| Change | Risk | Severity | Required Action |
| --- | --- | --- | --- |

**Validation Evidence Check**
| Required Evidence | Actual Artifact | Result | Remaining Gap |
| --- | --- | --- | --- |

**Release / Rollback / Cleanup Check**
- Rollout preserved: ...
- Rollback still credible: ...
- Observability present: ...
- Cleanup trigger present: ...
```

## Multi-Round Continuity

Maintain a compact review ledger across turns:

- Prior conclusion.
- Open blockers and major findings.
- Resolved blockers and how they were resolved.
- Accepted risks and owners/triggers.
- Changed decisions since the previous round.
- Rejected options and why.
- New risks introduced by the latest revision.

## Regression Review

When the user says "方案已经更新" or similar, run regression review before new critique:

| Prior Finding | Previous Severity | Current Status | Evidence | New Severity |
| --- | --- | --- | --- | --- |
| ... | Blocker/Major | Resolved/Partial/Open/Regressed | ... | ... |

Do not introduce new critique before checking prior blockers unless the new plan materially changes scope.

## Approval Standard

Use `references/quality-gates.md` for detailed gates. At minimum, only pass when all of these are true:

- Scope is explicit: what changes, what does not change, and what is deferred.
- Architecture boundaries are clear: ownership, dependencies, public contracts, data flow, and side effects are named.
- Migration is safe: rollout, compatibility, rollback, cleanup, and user/data impact are covered when relevant.
- Validation is credible: unit, integration, contract, migration, manual, observability, eval, smoke, and acceptance checks match the risk.
- Debt is controlled: shortcuts are deliberate, documented, owned, and bounded by follow-up triggers.
- Failure behavior is explicit: timeout, retry, partial failure, idempotency, degradation, and error semantics are covered where relevant.
- The plan can be executed by an engineer or coding agent unfamiliar with the discussion history.
- Important pass/fail conclusions are tied to plan evidence, repository evidence, validation evidence, owner confirmation, or explicitly listed assumptions.
- If implementation has already been produced, actual changes conform to the approved plan or deviations are reviewed and resolved.

## Minimal Safe Plan

If the proposed plan is unsafe but the goal is valid and enough context exists, produce the smallest safe path that:

- avoids irreversible changes;
- preserves existing contracts until migration is proven;
- validates the riskiest assumption early;
- includes rollback or cleanup;
- avoids long-lived compatibility debt.

## Implementation Readiness Score

When useful, provide an optional readiness score after the gate decision.

Format:

- Implementation Readiness: 0-100
- Confidence: Low / Medium / High
- Reason:
  - Evidence strength:
  - Repository alignment:
  - Remaining assumptions:
  - Validation strength:
  - Rollback strength:
  - Debt risk:
  - External confirmation needed:

Suggested interpretation:

- 90-100: implementation can start; remaining work is minor or explicitly accepted.
- 70-89: implementation can start only with listed assumptions, validation tasks, or owner confirmations.
- 40-69: revise before implementation; meaningful design, validation, rollout, or evidence gaps remain.
- 0-39: blocked or unsafe; implementation would likely create failure or debt.

Do not let the score override severity rules. A plan with unresolved `Blocker` or `Major` cannot become `Pass` because the score is high.

## Document Mode

Document mode is an output mode of this skill, not a separate responsibility. Use it when the user asks to write, update, supplement, or place a plan in `docs`.

In document mode:

- Load `references/document-template.md`.
- Update the requested document if a path is provided; otherwise choose an appropriate file under the repository `docs/` directory only when the user asks for a file.
- Keep the final plan primary and review history concise.
- Include background, context intake, repository inspection ledger, assumptions/evidence/unknowns, evidence collection plan when needed, goals/non-goals/success criteria, target design, domain pack, alternatives, chosen plan, implementation steps when relevant, validation plan, validation evidence when available, rollout/rollback, approval-to-action semantics, gate evidence matrix, accepted risks, implementation readiness, implementation conformance record when applicable, and material expert review record when relevant.
- Do not preserve every intermediate critique. Preserve only decisions changed by review, blockers/major findings, accepted risks, and why rejected alternatives were rejected.

## Decision-First Output

For users likely outside the domain, start with a short decision layer before technical details.

Default order:

1. Decision:
   - `Pass`, `Pass under assumptions`, `Pass with notes`, `Revise`, or `Blocked`.
   - Whether implementation can start.
   - One-sentence reason.
2. Non-expert control summary:
   - Top 3 things to watch.
   - Red lines that must not be accepted.
   - Questions that require real owner, code, production, or business confirmation.
   - Evidence a later agent or engineer must preserve.
3. Technical review:
   - Severity findings.
   - Gate evidence matrix.
   - Rewritten plan.
   - Stress test.
   - Remaining risks.

For simple or low-risk plans, compress expert discussion and show only material findings. For high-risk plans, keep the full gate matrix and stress test.

## Output Shapes


For automatic multi-round reviews:

```markdown
**决策摘要**
- 结论：Pass | Pass under assumptions | Pass with notes | Revise | Blocked
- 是否可以开始实现：是 / 否 / 仅允许 discovery/spike/验证/owner 确认 / 仅在假设成立或指定验证完成后
- 允许的下一步：...
- 不允许的下一步：...
- 一句话原因：...
- 当前最大风险：...

**给非该领域用户的判断摘要**
- 最应该盯住的 3 个点：...
- 不能接受的红线：...
- 可以接受的权衡：...
- 需要真实负责人确认的问题：...
- 后续实现必须保留的验收证据：...

**Repository Inspection Ledger**
| Area | Paths / Commands / Search Terms | Why Inspected | Finding | Confidence |
| --- | --- | --- | --- | --- |

**自动审核轮次**
- Round 1: <finding summary> -> <change made>
- Round 2: <finding summary> -> <change made>
- Round 3: <final gate result>

**严重级别分类**
| Severity | Finding | Evidence / Assumption | Required Change | Status |
| --- | --- | --- | --- | --- |
| Blocker/Major/Minor/Accepted Risk |  |  |  | Open/Resolved/Accepted |

**Evidence Collection Plan**
| Missing Evidence | Why It Matters | How To Collect | Owner / Source | Decision Impact | Can Implementation Start? |
| --- | --- | --- | --- | --- | --- |

**证据矩阵**
| Gate | Evidence / Assumption / Validation | Status |
| --- | --- | --- |
| Scope | ... | Pass/Revise/N/A |
| Boundary | ... | Pass/Revise/N/A |
| Contracts | ... | Pass/Revise/N/A |
| State/Migration | ... | Pass/Revise/N/A |
| Failure behavior | ... | Pass/Revise/N/A |
| Observability | ... | Pass/Revise/N/A |
| Validation | ... | Pass/Revise/N/A |
| Rollout/Rollback | ... | Pass/Revise/N/A |
| Security/Privacy | ... | Pass/Revise/N/A |
| Technical debt control | ... | Pass/Revise/N/A |

**最终方案**
<polished plan>

**Adversarial Review / Pre-Pass Stress Test**
- <failure scenario and whether the plan handles it>

**Implementation Readiness**
- Score: <0-100>
- Confidence: Low/Medium/High
- Reason: ...

**Implementation Conformance Requirement**
- After implementation, run conformance review: Yes/No and why
- Evidence the implementation must preserve: ...

**剩余风险**
- <Minor or Accepted Risk items, or "无阻断风险">

**通过条件 / 下一步**
- <condition, evidence collection task, implementation next step, or conformance review step>
```

For single-round reviews:

```markdown
**结论**
Revise | Pass under assumptions | Pass with notes | Pass | Blocked

**专家讨论**
- Architecture: ...
- Domain: ...
- Implementation: ...
- Reliability/Security: ...
- Product/Operations: ...

**必须修正**
- [Blocker/Major] <finding> (Evidence/Assumption: ...)

**建议优化**
- [Minor] ...

**通过条件**
- ...
```

For optimized plans:

```markdown
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

**证据、假设与未知项**
...

**遗留问题**
...
```

For document updates, update the document first, then respond with:

```text
已更新：<path>
结论：Pass/Pass under assumptions/Pass with notes/Revise/Blocked
主要变化：
- <change>
剩余风险：
- <risk or "无阻断风险">
```

## Review Discipline

- Do not accept vague phrases such as "enhance validation", "improve architecture", or "handle edge cases" unless the plan names the concrete mechanism.
- Do not assume a web, backend, frontend, data, infrastructure, AI, or product domain unless the artifact or project evidence supports it.
- Do not add ceremony that does not reduce risk.
- Do not over-index on incremental slicing. Slicing is useful when it reduces risk; direct refactor is reasonable when the old boundary is the debt source and compatibility, rollback, and acceptance checks are credible.
- Do not require local validation beyond what the risk justifies. For docs-only planning, specify validation strategy instead of running tests.
- Do not claim certainty when the conclusion depends on assumptions. Mark those assumptions and use `Pass under assumptions`, `Revise`, or `Blocked`.
- Do not bulk-load every reference file. Load only what the current review needs.
- Do not claim repository-backed confidence without a repository inspection ledger or explicit explanation of why repo inspection was unavailable.
- Do not let `Pass under assumptions` authorize irreversible, public-contract, data-changing, security-sensitive, or production-impacting implementation.
- Do not skip implementation conformance review after agent-generated code unless the user explicitly opts out.
