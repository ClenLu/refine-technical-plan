---
name: refine-technical-plan
description: Automated simulated expert-panel workflow for reviewing, stress-testing, revising, documenting, and re-reviewing technical plans before implementation. Use when the user asks to polish a technical plan, 方案, architecture design, refactor plan, implementation plan, technical document, PRD-to-engineering plan, or agent-generated proposal; when the user wants expert-style architecture/domain/implementation/reliability/product review; when the user is working outside their domain and needs protection from hidden technical debt; when the user asks for multi-round review, "方案已经更新", docs/implementation-plan output, automated plan refinement, or iteration until the plan passes stated evidence, assumptions, and quality gates.
---

# Refine Technical Plan

Use this skill as the primary entrypoint for hardening technical plans before implementation. Turn a draft or evolving proposal into a defensible implementation plan through repeated expert-style review, targeted rewrites, quality gates, stress tests, evidence discipline, and optional document output.

This workflow is designed for users who may be using an agent outside their own domain. Its purpose is not to make an agent-generated plan sound more convincing; its purpose is to expose weak assumptions, hidden coupling, validation gaps, rollback gaps, and long-term technical debt before implementation begins.

## Defaults

- Reply in the user's language; use Chinese when the user writes Chinese.
- Do not write implementation code unless the user explicitly asks for code.
- Default to automatic multi-round review and rewrite when enough context exists.
- Keep one response bounded: run at most 3 review/rewrite cycles per response, then stop with the best current plan and remaining blockers.
- Continue across conversation turns without a fixed round limit. When the user says the plan was updated, compare against prior blockers before introducing new critique.
- Do not force `Pass` merely to end the loop. If progress stalls, summarize the unresolved decision and mark `Revise`, `Blocked`, or `Pass under assumptions` as appropriate.
- Inspect local docs/code/config when they are available and materially affect the plan.
- Ask for input only when continuing would require guessing about business constraints, production behavior, external contracts, or domain facts that materially change the design.
- If essential facts are missing but the user still needs progress, produce the safest conditional plan and explicitly mark assumptions, validation steps, and non-negotiable confirmation points.

## References

- Read `references/review-rubric.md` for universal review lenses, change-type lenses, domain adaptation questions, and domain-pack examples.
- Read `references/quality-gates.md` for severity definitions, pass/fail gates, evidence rules, technical debt traps, and slice/direct/hybrid refactor selection.
- Read `references/document-template.md` when the user asks to write or update a plan, implementation plan, ADR-like note, or docs artifact.

## Review Modes

Select the lightest mode that satisfies the user request:

- `Review-only`: critique without rewriting.
- `Rewrite`: critique, revise, and re-review.
- `Delta review`: compare an updated plan against prior blockers, major findings, accepted risks, and changed decisions.
- `Document mode`: write or update a docs artifact using `references/document-template.md`.
- `Implementation-readiness review`: decide whether engineers can safely start implementation.
- `Pre-implementation risk review`: focus only on hidden debt, rollback, validation, assumptions, and unknowns.
- `Minimal safe plan`: when the proposed plan is unsafe but the goal is valid, produce the smallest safe path that validates the riskiest assumption early, preserves rollback, and avoids long-lived compatibility debt.

## Context Intake

Before reviewing, extract the following from the user's artifact, prior discussion, repository evidence, or provided documents. Ask only if a missing item materially changes the design or pass/fail result.

- Artifact under review: draft plan, architecture proposal, PRD, code diff, migration plan, implementation plan, ADR, or docs update.
- Change type: new feature, refactor, migration, integration, platform/runtime, state/data, UI/workflow, AI/agent behavior, operational process, or mixed.
- Goal and non-goals: what must change, what must not change, what is explicitly deferred.
- Affected surface: users, callers, APIs, events, schemas, storage, UI states, permissions, jobs, config, deployment, observability, or support process.
- Available evidence: plan text, code, docs, schemas, logs, tests, configs, metrics, prior discussion, production constraints, or external contract.
- Unknowns: business, production, domain, external, data, or ownership facts that materially affect safety.
- Blast radius: low / medium / high, with reason.
- Reversibility: reversible / partially reversible / irreversible, with reason.
- Primary risks: hidden coupling, compatibility, state drift, operational failure, security/privacy, user impact, long-term maintainability.

## Evidence Discipline

Do not treat plausible reasoning as sufficient evidence. A simulated expert opinion is not evidence by itself.

For every `Blocker` or `Major` finding, identify at least one of:

- exact plan text that creates the risk;
- repository, documentation, schema, config, test, log, metric, or prior-discussion evidence;
- missing business, production, external contract, or domain fact;
- concrete failure scenario that makes the risk material.

For every `Pass`, `Pass under assumptions`, or `Pass with notes` decision, state whether the decision is:

- evidence-backed;
- assumption-backed;
- validation-backed.

Do not mark absolute `Pass` when important contracts, caller inventory, data shape, production behavior, permissions, rollback behavior, or business constraints are only assumed. Use `Pass under assumptions`, `Revise`, or `Blocked` instead.

## Expert Panel

Simulate a compact expert panel. Use roles to structure critique; do not invent credentials or real people.

Always include:

- Architecture owner: boundaries, coupling, contracts, evolution path, rollback strategy.
- Domain specialist: domain correctness, invariants, edge cases, hidden assumptions.
- Implementation lead: migration sequence, ownership, complexity, testability.
- Reliability/security reviewer: failure modes, data safety, observability, abuse cases.
- Product/operator representative: user impact, operational burden, rollout clarity.

Add specialized roles only when the plan justifies them: interface/contract, runtime/operations, state/migration, user experience, security/privacy, compliance, performance, cost, platform, AI/model behavior, or developer experience.

For simple or low-risk plans, compress expert discussion into material findings only. Do not create long meeting notes when a short gate result would reduce risk just as well.

## Disagreement Resolution

When expert roles disagree, do not average the opinions.

Resolve disagreement by:

1. naming the conflicting recommendations;
2. identifying which invariant, user impact, delivery constraint, or risk each role is protecting;
3. comparing blast radius, reversibility, validation strength, delivery complexity, operational burden, and long-term debt;
4. choosing one path explicitly;
5. recording rejected options and accepted risks.

## Domain Pack

After context intake, derive a temporary domain checklist from the artifact and evidence. Do not assume a domain just because the user used generic words such as "frontend", "backend", "platform", "agent", or "refactor"; use the plan and repository evidence to decide which checks apply.

Examples of domain-pack checks:

- Frontend/UI: state ownership, loading/error/empty states, permission-denied states, stale data, optimistic updates, accessibility, responsive behavior, text overflow, analytics, and user-visible degradation.
- Backend/API: public contracts, caller inventory, idempotency, auth/authz, rate limits, schema compatibility, error semantics, retries, timeout budgets, observability, and client migration.
- Data/Migration: source of truth, invariants, expand-migrate-contract sequencing, backfill, reconciliation, data quality checks, irreversible transformations, rollback after data changes, and cleanup ownership.
- Infra/Runtime: deployment strategy, config, environment differences, resource limits, concurrency, startup/shutdown, health checks, alerts, runbooks, ownership, and production smoke tests.
- AI/Agent behavior: tool permissions, prompt-injection exposure, hallucination containment, evals, golden tests, human override, auditability, fallback behavior, privacy boundaries, and unsafe autonomous action limits.
- Developer experience: local setup, migration ergonomics, debugging path, generated code boundaries, test speed, documentation, and future maintainer clarity.

Skip irrelevant checks to avoid cargo-cult review.

## Automatic Review Loop

Use this loop by default unless the user asks for a single round, brainstorming, or discussion without rewriting.

1. Establish context using `Context Intake`.
2. Build a `Domain Pack` for the specific artifact.
3. Run expert review: list the strongest objections, hidden assumptions, debt risks, and ambiguous decisions. Prefer concrete failure scenarios over generic concerns.
4. Classify findings with severity: `Blocker`, `Major`, `Minor`, or `Accepted Risk`.
5. Attach evidence or missing-evidence status to every `Blocker` and `Major` finding.
6. Resolve expert disagreements explicitly when roles recommend different paths.
7. Decide status:
   - `Blocked`: essential input is missing and continuing would require unsafe guessing.
   - `Revise`: any `Blocker` or unresolved `Major` remains, or the plan would likely create avoidable debt.
   - `Pass under assumptions`: no blocker or major issue remains if the explicitly listed assumptions are true, but one or more important facts were not independently verified.
   - `Pass with notes`: only `Minor` issues or explicit `Accepted Risk` items remain.
   - `Pass`: the plan is coherent, scoped, testable, observable, evidence-backed where material, and ready to implement.
8. If status is `Revise` and enough context exists, rewrite the plan. Preserve good decisions, remove obsolete text, and directly address blockers and major findings.
9. Re-review the rewritten plan using `Regression Review` before introducing new critique.
10. Before any passing status, run the `Pre-Pass Stress Test`, run `Adversarial Review`, and produce a compact `Gate Evidence Matrix`.
11. Stop when the status is `Pass`, `Pass under assumptions`, `Pass with notes`, `Blocked`, or after 3 cycles in the current response.

Do not ask the user to manually trigger the next round when enough information exists to continue.

## Regression Review

When reviewing an updated plan, first check prior findings before introducing new critique.

Use this compact table when useful:

| Prior Finding | Previous Severity | Current Status | Evidence | New Severity |
| --- | --- | --- | --- | --- |
|  | Blocker/Major/Minor | Resolved/Partial/Open/Regressed |  |  |

Rules:

- Do not reopen resolved issues unless the new plan regresses them or introduces stronger evidence.
- Do not ignore unresolved prior blockers because the new draft looks more polished.
- Mark issues as `Partial` when the text addresses the symptom but not the underlying risk.
- Mark issues as `Regressed` when a previous fix was removed, weakened, or contradicted.

## Pre-Pass Stress Test

Before marking `Pass`, `Pass under assumptions`, or `Pass with notes`, run at least three concrete failure scenarios:

1. Most likely failure.
2. Most expensive or user-visible failure.
3. Hardest-to-detect technical-debt failure.

For each scenario, state:

- trigger;
- affected user/caller/component/data;
- detection signal;
- mitigation;
- rollback or cleanup path;
- whether the final plan handles it.

A plan cannot receive an absolute `Pass` if the stress test exposes an unresolved `Blocker` or `Major` risk.

## Adversarial Review

Before the final rewrite or any pass-like decision, run a short red-team pass:

- What assumption would invalidate the plan?
- What hidden coupling could make implementation unsafe?
- What temporary compatibility path could become permanent debt?
- What test would falsely pass while production still breaks?
- What would a future maintainer misunderstand?
- What rollback path only works in theory but not after data, caller, or external behavior changes?
- What might a future implementation agent hallucinate because contracts, owners, or validation evidence are not concrete?

Escalate the issue if the red-team answer exposes a `Blocker` or `Major` risk.

## Gate Evidence Matrix

Before a passing status, summarize the material gates:

| Gate | Evidence / Assumption | Validation | Status |
| --- | --- | --- | --- |
| Scope |  |  | Pass/Revise/Blocked |
| Boundary |  |  | Pass/Revise/Blocked |
| Contracts |  |  | Pass/Revise/Blocked/N/A |
| State/Migration |  |  | Pass/Revise/Blocked/N/A |
| Failure behavior |  |  | Pass/Revise/Blocked |
| Observability |  |  | Pass/Revise/Blocked/N/A |
| Validation |  |  | Pass/Revise/Blocked |
| Rollout/Rollback |  |  | Pass/Revise/Blocked/N/A |
| Security/Privacy |  |  | Pass/Revise/Blocked/N/A |
| Technical debt control |  |  | Pass/Revise/Blocked |

Use `N/A` only when the gate truly does not apply to the artifact. Do not use `N/A` to avoid hard questions.

## Multi-Round Continuity

Maintain a compact review ledger across turns:

- Prior conclusion.
- Open blockers and major findings.
- Resolved blockers and how they were resolved.
- Accepted risks and owners/triggers.
- Changed decisions since the previous round.
- New risks introduced by the latest revision.
- Evidence that changed the conclusion.
- Assumptions that still make any pass conditional.

When the user says "方案已经更新" or similar:

- Review the new artifact as the source of truth.
- First state whether prior blockers are resolved, partially resolved, still open, or regressed.
- Do not reopen resolved issues unless the new plan regresses them or introduces stronger evidence.
- Do not pass a plan merely because it improved; pass only when the gate is satisfied.

## Approval Standard

Use `references/quality-gates.md` for detailed gates. At minimum, only pass when all of these are true:

- Scope is explicit: what changes, what does not change, and what is deferred.
- Architecture boundaries are clear: ownership, dependencies, public contracts, data flow, and side effects are named.
- Migration is safe: rollout, compatibility, rollback, cleanup, and user/data impact are covered when relevant.
- Validation is credible: unit, integration, contract, migration, manual, observability, and acceptance checks match the risk.
- Debt is controlled: shortcuts are deliberate, documented, owned, and bounded by follow-up triggers.
- Failure behavior is explicit: timeout, retry, partial failure, idempotency, degradation, and error semantics are covered where relevant.
- Evidence is sufficient for the conclusion, or the conclusion is explicitly conditional.
- The plan can be executed by an engineer unfamiliar with the discussion history.

## Minimal Safe Plan

When the proposed plan is unsafe but the goal is valid, produce the smallest safe implementation path that:

- avoids irreversible changes until they are validated;
- preserves existing contracts unless the migration path is explicit;
- validates the riskiest assumption early;
- has credible rollback or cleanup;
- avoids long-lived compatibility shells, adapter sprawl, or duplicated business rules;
- defines the trigger for deleting temporary paths.

Use this mode when a full rewrite would require too much missing context but a safe next step is still possible.

## Implementation Readiness Score

Optionally include a score when it helps communication, but never use it to override severity gates.

```markdown
Implementation Readiness: <0-100>
Confidence: Low/Medium/High
Reason:
- ...
```

A plan with any unresolved `Blocker` or `Major` is not implementation-ready even if the score appears high. Keep confidence low when conclusions are mostly assumption-backed.

## Non-Expert User Summary

When the user is likely working outside their domain, include a compact summary they can use to stay in control:

- The 3 things you should personally watch.
- Non-negotiable red lines.
- Acceptable tradeoffs.
- Questions a real owner/domain expert must confirm.
- Evidence the implementing agent must preserve: tests, screenshots, logs, contract checks, migration dry-runs, rollout proof, or cleanup triggers.

This section should translate expert concerns into practical review handles, not introduce new technical detail.

## Document Mode

Document mode is an output mode of this skill, not a separate responsibility. Use it when the user asks to write, update, supplement, or place a plan in `docs`.

In document mode:

- Load `references/document-template.md`.
- Update the requested document if a path is provided; otherwise choose an appropriate file under the repository `docs/` directory only when the user asks for a file.
- Keep the final plan primary and review history concise.
- Include background, goals/non-goals, context/evidence, assumptions, target design, alternatives, chosen plan, implementation plan, validation, rollout/rollback, acceptance criteria, gate evidence matrix, stress-test summary, accepted risks, and expert review record when relevant.
- Preserve only material review history: decisions changed by review, blockers/major findings, accepted risks, and rejected alternatives. Do not preserve every intermediate critique.

## Output Shapes

For automatic multi-round reviews:

```markdown
**结论**
Pass | Pass under assumptions | Pass with notes | Revise | Blocked

**上下文与证据**
- 变更类型：...
- 影响面：...
- 证据：...
- 关键假设/未知：...
- Blast radius：Low/Medium/High
- Reversibility：Reversible/Partial/Irreversible

**自动审核轮次**
- Round 1: <finding summary> -> <change made>
- Round 2: <finding summary> -> <change made>
- Round 3: <final gate result>

**回归检查**
| Prior Finding | Previous Severity | Current Status | Evidence | New Severity |
| --- | --- | --- | --- | --- |

**Gate Evidence Matrix**
| Gate | Evidence / Assumption | Validation | Status |
| --- | --- | --- | --- |

**Pre-Pass Stress Test**
- Most likely failure: ...
- Most expensive/user-visible failure: ...
- Hardest-to-detect debt failure: ...

**Adversarial Review**
- Invalidating assumption / hidden coupling / false-positive test risk: ...

**最终方案**
<polished plan>

**剩余风险**
- <Minor or Accepted Risk items, or "无阻断风险">

**给非该领域用户的判断摘要**
- 你最应该盯住的 3 个点：...
- 不能接受的红线：...
- 需要真实负责人确认的问题：...
- 后续实现必须保留的验收证据：...

**Implementation Readiness**
- Score: <0-100>
- Confidence: Low/Medium/High

**通过条件 / 下一步**
- <condition or implementation next step>
```

For single-round reviews:

```markdown
**结论**
Revise | Pass under assumptions | Pass with notes | Pass | Blocked

**上下文与证据**
- Artifact：...
- Evidence：...
- Assumptions / Unknowns：...

**专家讨论**
- Architecture: ...
- Domain: ...
- Implementation: ...
- Reliability/Security: ...
- Product/Operations: ...

**分歧处理**
- <only if roles disagree>

**必须修正**
- [Blocker/Major] ... Evidence: ...

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

**证据与假设**
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

**遗留问题 / Accepted Risks**
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
- Do not claim certainty when the conclusion depends on assumptions. Mark those assumptions and make the pass conditional or reject the plan.
- Do not treat a polished rewrite as proof that the underlying design is safe.
- Do not let the same issue disappear across rounds unless the new plan materially resolves it.
- Do not preserve every intermediate critique in final docs; preserve only decisions, blockers/major findings, accepted risks, rejected options, and evidence that changed the conclusion.
- Do not let compatibility bridges, feature flags, adapters, or facades remain without owner, metric, deletion trigger, and cleanup plan.
