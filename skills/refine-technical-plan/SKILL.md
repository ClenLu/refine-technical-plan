---
name: refine-technical-plan
description: Use when the user explicitly invokes `$refine-technical-plan` or clearly requests this workflow for an existing technical plan or approved-plan implementation check. Do not auto-trigger from generic planning, architecture, refactoring, or technical-debt discussions.
---

# Refine Technical Plan

Help turn an existing technical plan into an executable, evidence-backed plan. Work as a technical collaborator: investigate uncertainties, test important assumptions, compare alternatives, and improve the plan before asking the user to fill gaps. The goal is useful progress toward implementation, with an honest decision about what can start.

## Scope And Initiative

- Use the plan the user supplied or referenced. If it is unavailable, ask for it while investigating any relevant context already accessible.
- Preserve the user's objective and useful decisions; challenge the approach when evidence suggests a better way to meet that objective.
- Plan refinement includes reading code and documentation, tracing dependencies, running relevant existing checks, and writing temporary probes or prototypes in isolated local locations. Use these capabilities when they can resolve a material uncertainty; do not stop at recommending an investigation you can perform.
- Keep experiments separate from production implementation. Preserve user changes, identify artifacts you create, and report their location and disposition. Modify application code, commit changes, deploy, or affect external state only within the user's authorized scope. Refinement alone does not authorize shipping the proposed solution.
- Follow the user's time, cost, network, and tooling constraints. Prefer inexpensive, reversible checks; ask before an action requiring unavailable permissions, credentials, consequential spending, or an irreversible change. Continue independent work while waiting.

## Working Method

1. **Find the decision that matters.** Identify the goal, constraints, and uncertainties most likely to change the design or prevent implementation. Select relevant architecture, domain, implementation, security, reliability, product, or state perspectives as needed; expert roleplay adds no evidence.
2. **Resolve what you can.** Inspect accessible repository context and authoritative external sources. Run focused checks or isolated experiments when documentation alone cannot settle the question. Compare alternatives when they could improve correctness, complexity, cost, or operability.
3. **Integrate the result.** Rewrite the relevant plan sections, including concrete boundaries, sequencing, acceptance criteria, and tradeoffs. Record what the investigation changed, rather than leaving solved issues as requests to the user.
4. **Reassess remaining uncertainty.** Continue while a useful next action can materially improve the plan within the task's scope and budget. Stop when the plan supports the intended next stage, an external prerequisite blocks all remaining useful work, or further investigation has little decision value. There is no fixed number of review cycles.

Ask only for facts or decisions you cannot reasonably obtain: business priorities, production constraints, risk acceptance, ownership authority, credentials, or access. State what the answer changes. Missing input should block only the work that depends on it.

## Evidence And Experiments

Choose evidence according to the uncertainty:

- **Boundaries and behavior:** trace real callers, contracts, state ownership, and failure paths; run relevant existing tests.
- **Version-sensitive support:** check the installed or proposed version against official documentation, release notes, or provider guarantees. Test a minimal call when accessible and useful.
- **Performance or complexity:** compare representative alternatives with a bounded benchmark or prototype; describe workload and measurement limitations.
- **Migration and rollback:** use synthetic data or a disposable local copy to exercise restartability, compatibility, reconciliation, and recovery. Never treat a local rehearsal as proof of production readiness.

Use the questions in [review-rubric.md](references/review-rubric.md) when the risk warrants deeper investigation. They are prompts for choosing work, not mandatory sections or a universal checklist.

Distinguish inspected facts, measured results, assumptions, and unverified claims. Cite relevant file paths, versions, sources, or commands and outcomes. Report failed, skipped, and unrun checks honestly. Neither confident wording, "best practice", nor an agent's own rewrite verifies a critical assumption. If tools or context are unavailable, explain the precise limitation and still complete useful independent analysis.

## Readiness

Judge readiness for the intended next stage, not for every eventual production activity. A reversible prototype can begin while deployment decisions remain open if its boundary, purpose, and stop conditions are clear. Do not let unrelated checklist items block low-risk work.

Use plain language, normally:

- `可以开工`: the intended stage has clear boundaries, credible validation, and resolved or explicitly accepted material risks.
- `基本可以开工，但有小备注`: only low-risk, non-blocking follow-up remains.
- `暂缓开工，先修改方案`: material design or validation problems remain after useful investigation.
- `缺关键输入，暂时不能判断`: essential facts, access, or decisions remain unavailable.

Name which stage is ready and which is still blocked. Risk acceptance must come from an authorized decision-maker, with consequences, affected scope, ownership, validation, and mitigation or exit conditions proportional to the risk. Do not invent acceptance or infer it from silence. Avoid numeric readiness scores.

## Deliverables

Lead with the decision and its reason. Then provide what helps the user move forward:

- **Work completed:** important evidence obtained, experiments and their outcomes, or alternatives ruled out. Keep this compact and explicit about limitations.
- **Optimized plan:** provide a replacement or mergeable revision when the user asks for refinement or a rewrite is useful. Make it understandable without the conversation. Include only relevant goal, boundary, design, stages, validation, recovery, operations, and open-decision content; keep review mechanics outside the plan body.
- **Remaining actions:** prioritize the immediate blockers, explain why they matter, and identify whether the agent or user can resolve each. A short list is usually enough; do not hide blockers to meet a numerical limit.
- **Evidence and risk summary:** include material assumptions and unresolved risks when they affect the decision.

For `快速把关`, keep the investigation and answer proportional to the requested time and risk; do not weaken evidence standards or expand into unnecessary rewrites. No fixed report template is required.

## Continuation

Reuse prior risk IDs and update their evidence and resolution. Keep a compact visible handoff of material open items when context may be lost; do not rely on an invisible ledger persisting across sessions.

Use a detailed ledger only when traceability helps a high-risk or repeated review, accepted risk, or a user request. Choose fields that carry the necessary information, such as issue, impact, evidence, owner, status, and resolution condition.

## Implementation Conformance Review

When the user provides an approved plan and implementation evidence, compare the two rather than reopening settled design decisions. Inspect diffs and relevant checks where available. Identify skipped rollout stages, removed compatibility windows, missing rollback or validation, and weakened security, observability, or cost controls.

Lead with `可以合并`, `暂缓合并，先修正`, or `缺证据，暂时不能判断`. Show material approved conditions, implementation evidence, deviations, and corrective actions; use a table when it helps. Investigate apparent deviations before concluding, and propose justified plan amendments when new evidence warrants them. Do not silently waive approved conditions or change the implementation without authorization.

Use `agents/openai.yaml` as interface metadata.
