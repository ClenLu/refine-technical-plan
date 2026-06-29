# Placement Guide for the Refined Technical Plan Skill

This guide maps every major addition to the exact file and recommended position. The updated package already contains all changes in these positions.

## Package Structure

```text
refine-technical-plan-updated/
├── SKILL.md
├── PLACEMENT_GUIDE.md
└── references/
    ├── quality-gates.md
    ├── review-rubric.md
    └── document-template.md
```

## 1. `SKILL.md` — Main Orchestration and Runtime Behavior

Use `SKILL.md` for anything that changes how the skill runs: workflow, review loop, output format, status handling, and when to rewrite or stop.

### 1.1 Frontmatter `description`

Position: Replace the existing frontmatter `description`.

Added:
- Replaced real-sounding `top architects/experts` framing with `Automated simulated expert-panel workflow` and `expert-style` review.
- Added "iteration until the plan passes stated evidence, assumptions, and quality gates".

Why here:
- The trigger description determines when the skill is selected and must honestly describe that this is simulated expert review, not a real external expert team.

### 1.2 Intro under `# Refine Technical Plan`

Position: Immediately after the first purpose paragraph.

Added:
- The background that users may be using agents outside their own domain.
- The purpose: expose weak assumptions, hidden coupling, validation gaps, rollback gaps, and long-term technical debt.

Why here:
- This locks the skill onto the core use case: preventing confident agent-generated plans from misleading non-domain users.

### 1.3 `## Defaults`

Position: Extend the existing defaults block near the top.

Added:
- Do not force `Pass` just to end the loop.
- If progress stalls, mark `Revise`, `Blocked`, or `Pass under assumptions`.
- If essential facts are missing but progress is still needed, produce the safest conditional plan.
- Keep per-response cycles bounded but allow unlimited cross-turn continuation.

Why here:
- These are global behavior defaults that affect every run.

### 1.4 `## References`

Position: Keep after defaults, update the bullets.

Added:
- `review-rubric.md` now includes domain-pack examples and adaptation questions.
- `quality-gates.md` now includes evidence rules, pass/fail semantics, technical debt traps, and refactor approach selection.
- `document-template.md` now includes evidence, assumptions, gate matrix, stress tests, accepted risks, and non-expert summary fields.

Why here:
- The main skill must know which reference file owns which logic.

### 1.5 New `## Review Modes`

Position: After `## References`, before `## Context Intake`.

Added:
- `Review-only`.
- `Rewrite`.
- `Delta review`.
- `Document mode`.
- `Implementation-readiness review`.
- `Pre-implementation risk review`.
- `Minimal safe plan`.

Why here:
- This prevents the skill from always running a heavy full-cycle process when a lighter review mode is enough.

### 1.6 New `## Context Intake`

Position: After `## Review Modes`, before `## Evidence Discipline`.

Added:
- Artifact under review.
- Change type.
- Goals and non-goals.
- Affected surface.
- Available evidence.
- Unknowns.
- Blast radius.
- Reversibility.
- Primary risks.

Why here:
- The skill must map the problem before judging or rewriting the plan.

### 1.7 New `## Evidence Discipline`

Position: After `## Context Intake`, before `## Expert Panel`.

Added:
- Simulated expert opinion is not evidence.
- Every `Blocker` or `Major` must be tied to plan text, repo/doc evidence, missing fact, or concrete failure scenario.
- Every passing decision must be labeled evidence-backed, assumption-backed, or validation-backed.
- Absolute `Pass` is forbidden when material contracts, data, production behavior, permissions, rollback, or business constraints are only assumed.

Why here:
- This is the central anti-self-convincing / anti-false-pass mechanism.

### 1.8 `## Expert Panel`

Position: Keep original section and extend its ending.

Added:
- Compress expert discussion for simple or low-risk plans.
- Do not produce long meeting-note style output when it does not reduce risk.

Why here:
- The expert panel should reduce risk, not create empty ceremony.

### 1.9 New `## Disagreement Resolution`

Position: After `## Expert Panel`, before `## Domain Pack`.

Added:
- Name conflicting recommendations.
- Identify the invariant, user impact, delivery constraint, or risk each role protects.
- Compare blast radius, reversibility, validation strength, delivery complexity, operational burden, and long-term debt.
- Choose one path explicitly and record rejected options/accepted risks.

Why here:
- Real review often contains disagreement; averaging opinions hides risk.

### 1.10 New `## Domain Pack`

Position: After `## Disagreement Resolution`, before `## Automatic Review Loop`.

Added:
- Dynamic checklist generation based on artifact evidence.
- Example packs for Frontend/UI, Backend/API, Data/Migration, Infra/Runtime, AI/Agent behavior, and Developer Experience.
- Instruction to skip irrelevant checks.

Why here:
- The skill must adapt to frontend/backend/data/infra/AI/etc. without assuming a domain prematurely.

### 1.11 `## Automatic Review Loop`

Position: Replace the old loop.

Added:
- Context Intake.
- Domain Pack.
- Evidence binding for `Blocker` and `Major`.
- Disagreement resolution.
- New status `Pass under assumptions`.
- Regression Review before new critique.
- Pre-Pass Stress Test and Gate Evidence Matrix before any passing status.

Why here:
- The loop becomes a real refinement process rather than a one-time review plus self-approval.

### 1.12 New `## Regression Review`

Position: After `## Automatic Review Loop`, before `## Pre-Pass Stress Test`.

Added:
- Prior finding table.
- Statuses: `Resolved`, `Partial`, `Open`, `Regressed`.
- Rules to avoid ignoring old blockers, reopening solved issues without evidence, or accepting wording-only fixes.

Why here:
- This prevents multi-round drift, where every round finds new issues but old issues are not actually closed.

### 1.13 New `## Pre-Pass Stress Test`

Position: After `## Regression Review`, before `## Adversarial Review`.

Added:
- Most likely failure.
- Most expensive or user-visible failure.
- Hardest-to-detect technical-debt failure.
- Trigger, affected surface, detection signal, mitigation, rollback/cleanup, and handled status.

Why here:
- A plan should not pass until concrete failure modes have been tested against it.

### 1.14 New `## Adversarial Review`

Position: After `## Pre-Pass Stress Test`, before `## Gate Evidence Matrix`.

Added:
- Invalidating assumption check.
- Hidden coupling check.
- Temporary compatibility becoming permanent debt.
- False-passing test scenario.
- Future maintainer misunderstanding.
- Theory-only rollback path.
- What a later implementation agent might hallucinate.

Why here:
- This is the red-team layer that prevents the skill from approving a polished but fragile plan.

### 1.15 New `## Gate Evidence Matrix`

Position: After `## Adversarial Review`, before `## Multi-Round Continuity`.

Added:
- Gates: Scope, Boundary, Contracts, State/Migration, Failure behavior, Observability, Validation, Rollout/Rollback, Security/Privacy, Technical debt control.
- Columns: Evidence / Assumption, Validation, Status.

Why here:
- The pass/fail decision becomes visible and auditable.

### 1.16 `## Multi-Round Continuity`

Position: Extend existing section.

Added:
- Evidence that changed the conclusion.
- Assumptions that still make a pass conditional.

Why here:
- Cross-turn review needs memory of why the status changed.

### 1.17 `## Approval Standard`

Position: Extend existing pass criteria.

Added:
- Evidence is sufficient for the conclusion, or the conclusion is explicitly conditional.

Why here:
- This aligns approval with evidence rather than confidence.

### 1.18 New `## Minimal Safe Plan`

Position: After `## Approval Standard`, before `## Implementation Readiness Score`.

Added:
- Smallest safe path when the proposed plan is unsafe but the goal is valid.
- Avoid irreversible changes until validated.
- Preserve contracts unless migration path is explicit.
- Validate riskiest assumption early.
- Avoid long-lived compatibility shells/adapters/duplicated rules.
- Define deletion trigger for temporary paths.

Why here:
- The skill should offer a safe next step, not only criticism.

### 1.19 New `## Implementation Readiness Score`

Position: After `## Minimal Safe Plan`, before `## Non-Expert User Summary`.

Added:
- Optional `Implementation Readiness: 0-100`.
- Optional `Confidence: Low/Medium/High`.
- Rule: score never overrides severity gates.
- Unresolved blocker/major means not implementation-ready.

Why here:
- Useful for communication, but safely subordinate to quality gates.

### 1.20 New `## Non-Expert User Summary`

Position: After `## Implementation Readiness Score`, before `## Document Mode`.

Added:
- The 3 things the user should watch.
- Non-negotiable red lines.
- Acceptable tradeoffs.
- Questions requiring real owner/domain expert confirmation.
- Evidence the implementing agent must preserve.

Why here:
- The skill is meant to help users who are not experts in the target domain stay in control.

### 1.21 `## Document Mode`

Position: Extend existing section.

Added:
- Include context/evidence, assumptions, gate matrix, stress-test summary, accepted risks, and material review history.
- Preserve only material review history.

Why here:
- Final docs should be complete, executable, and not bloated with every intermediate critique.

### 1.22 `## Output Shapes`

Position: Replace old output templates.

Added:
- `Pass under assumptions` in all relevant statuses.
- Context and evidence section.
- Regression check table.
- Gate Evidence Matrix.
- Pre-Pass Stress Test.
- Non-expert summary.
- Evidence field for blocker/major findings.

Why here:
- The user-visible output must expose evidence, assumptions, unresolved risks, and pass conditions.

### 1.23 `## Review Discipline`

Position: Extend final discipline list.

Added:
- Do not treat polished rewrite as proof.
- Do not let issues disappear across rounds unless materially resolved.
- Do not preserve every intermediate critique in final docs.
- Do not leave compatibility bridges, feature flags, adapters, or facades without owner, metric, deletion trigger, and cleanup plan.

Why here:
- These are global guardrails against technical debt and self-review failure.

## 2. `references/quality-gates.md` — Severity, Pass/Fail Gates, and Debt Traps

Use this file for all status semantics, pass/fail criteria, accepted risk structure, stress-test requirements, debt traps, and approach selection rules.

### 2.1 `## Severity`

Position: Near top, replace/extend original definitions.

Added:
- Broader blocker/major definitions covering severe debt, user breakage, data exposure, compatibility, security/privacy, and maintainability debt.
- Stronger `Accepted Risk` requiring owner, impact, mitigation, reason, and review/removal trigger.

### 2.2 New `## Decision Outcomes`

Position: After `## Severity`, before `## Decision Rules`.

Added:
- `Pass`.
- `Pass under assumptions`.
- `Pass with notes`.
- `Revise`.
- `Blocked`.

### 2.3 `## Decision Rules`

Position: Replace/extend existing rules.

Added:
- Absolute `Pass` only when material assumptions are verified or bounded by validation.
- Use `Pass under assumptions` when safety depends on named assumptions.
- Do not pass merely because the plan improved.
- Do not downgrade severity unless the risk is actually resolved, proven irrelevant, or accepted with owner/mitigation/trigger.

### 2.4 New `## Evidence Rules`

Position: After `## Decision Rules`, before `## Pass Criteria`.

Added:
- Simulated consensus is not evidence.
- Required evidence categories for blocker/major findings.
- Passing decisions must be evidence-backed, assumption-backed, or validation-backed.
- Absolute `Pass` forbidden when core facts are assumed.

### 2.5 `## Pass Criteria`

Position: Replace/extend original pass criteria.

Added:
- More complete contract surface.
- Cancellation, audit events, production smoke signals.
- AI/tool-use, external dependency, security/privacy considerations.
- Remaining risks must be owned, mitigated, and bounded.
- Plan must state evidence vs assumptions.

### 2.6 New `## Gate Evidence Matrix`

Position: After `## Pass Criteria`.

Added:
- Common matrix for gate, evidence/assumption, validation, and status.

### 2.7 New `## Pre-Pass Stress Test`

Position: After `## Gate Evidence Matrix`.

Added:
- Three required stress scenarios with trigger, affected surface, detection, mitigation, rollback/cleanup, and handled status.

### 2.8 New `## Regression Gate`

Position: After `## Pre-Pass Stress Test`.

Added:
- Prior findings must be marked resolved/partial/open/regressed.
- Partial wording fixes do not count as resolution.
- Regressed blockers remain blockers.

### 2.9 New `## Accepted Risk Requirements`

Position: After `## Regression Gate`.

Added:
- Risk description, owner, affected surface, reason, mitigation, monitoring/validation, review date/metric/deletion trigger.

### 2.10 `## Common Technical Debt Traps`

Position: Extend original trap list.

Added:
- Local cleanup preserving boundary debt.
- Feature flags/adapters/dual paths without cleanup.
- Premature abstraction.
- Treating generated plan as evidence.
- Generic validation/logging that cannot prove risk.
- Irreversible changes before rollback/compatibility proof.

### 2.11 New `## Agent-Generated Plan Traps`

Position: After `## Common Technical Debt Traps`.

Added:
- Invented APIs/files/schemas/owners.
- Internal consistency unsupported by facts.
- Confident language hiding unknowns.
- Wrong-level validation.
- False-passing tests.
- Prompt overfitting.
- Smooth compromise hiding disagreement.
- TODO completion mistaken for gate success.

### 2.12 New `## Overengineering Traps`

Position: After `## Agent-Generated Plan Traps`.

Added:
- Premature platform/framework/abstraction.
- Generic extension points without concrete need.
- Increased operational surface.
- Long-lived parallel systems.
- Diffused ownership.
- Ceremony without evidence or risk reduction.

### 2.13 `## Approach Selection`

Position: Extend original slice/direct/hybrid guidance.

Added:
- Slice-based delivery needs deletion/convergence path.
- Direct refactor is preferred when compatibility scaffolding is riskier than coordinated change.
- Hybrid temporary compatibility requires owner, metric, deletion trigger.

### 2.14 New `## Minimal Safe Plan Gate`

Position: After `## Approach Selection`, before `## Implementation Readiness Score`.

Added:
- Minimum safe next step must avoid irreversible change, preserve contracts, validate riskiest assumption, keep rollback credible, and prevent permanent compatibility debt.

### 2.15 New `## Implementation Readiness Score`

Position: After `## Minimal Safe Plan Gate`, before `## Disagreement Resolution Gate`.

Added:
- Score is optional and communication-only.
- Confidence must reflect evidence quality.
- Unresolved blockers/majors prevent implementation-readiness.

### 2.16 New `## Disagreement Resolution Gate`

Position: End of file.

Added:
- No pass until reviewer disagreement is resolved or recorded as accepted risk.
- Compare blast radius, reversibility, validation, user/caller impact, operational burden, delivery complexity, and long-term debt.

## 3. `references/review-rubric.md` — Review Lenses and Domain Adaptation

Use this file for what experts should inspect: universal lenses, change-type lenses, domain-pack generation, adversarial questions, and non-expert control handles.

### 3.1 `## Universal Review Lenses`

Position: Extend original list.

Added:
- Evidence quality.
- Reversibility.
- Tool permissions and evals where relevant.

### 3.2 New `## Context Intake Questions`

Position: After universal lenses, before `## Change-Type Lenses`.

Added:
- Artifact type.
- Change type.
- Affected surface.
- Available evidence.
- Material missing facts.
- Blast radius.
- Reversibility.

### 3.3 Existing Change-Type Lenses

Position: Extend existing subsections.

Added:
- Architecture/boundary: avoid facade debt and clarify intentional coupling.
- Refactor/migration: target shape before sequencing, temporary bridges need owner/metric/deletion trigger.
- New capability: full workflow, user/consumer states, value-based acceptance.
- Integration/external dependency: external assumptions, provider failure, rate/timeout/degradation behavior.
- State/data/migration: data-shape evidence, code rollback vs data rollback.
- Human-facing workflow: accessibility, stale/partial/error states, user-visible degradation.

### 3.4 New `### Backend / API / Service Behavior`

Position: Under `## Change-Type Lenses`.

Added:
- Caller inventory.
- Request/response/event/schema/status/error behavior.
- Idempotency, retries, ordering, concurrency.
- Auth/authz and tenant isolation.
- Caller/downstream observability.

### 3.5 New `### Frontend / UI / Client Workflow`

Position: Under `## Change-Type Lenses`.

Added:
- Client/server/cache/form/URL state ownership.
- Loading/empty/error/permission/partial/stale/retry/cancellation states.
- Accessibility, responsiveness, localization, text overflow, focus behavior.
- Client validation drift.
- Analytics/support/manual QA.

### 3.6 Existing `### Platform / Runtime / Operations`

Position: Extend existing subsection.

Added:
- Runbook/incident actions.
- Capacity, cost, rate limits, and failure isolation.

### 3.7 New `### AI / Agent / Model Behavior`

Position: Under `## Change-Type Lenses`.

Added:
- Deterministic contracts vs probabilistic model behavior.
- Evals, success metrics, failure examples, regression checks.
- Prompt injection, tool misuse, unauthorized actions, exfiltration, hallucinations.
- Concrete tool boundaries.
- Human override/fallback/refusal.
- Model/prompt rollout, monitoring, rollback, baseline comparison.
- Privacy, retention, logging, user-data exposure.

### 3.8 New `### Documentation-Only / Planning Artifact`

Position: Under `## Change-Type Lenses`.

Added:
- Plan must be actionable without hidden chat context.
- Validation strategy even if tests are not run.
- Decisions/tradeoffs/accepted risks/unresolved assumptions recorded concisely.
- Avoid preserving every intermediate critique.

### 3.9 New `## Domain Pack Generation`

Position: After change-type lenses.

Added:
- Domain Pack template.
- Evidence examples for Frontend/UI, Backend/API, Data/Migration, Infra/Runtime, and AI/Agent.

### 3.10 New `## Evidence Matrix Questions`

Position: After `## Domain Pack Generation`.

Added:
- What evidence supports the conclusion?
- Which facts remain assumptions?
- What validation would convert assumption into evidence?
- Which assumption invalidates design if false?
- Which missing fact should block review?

### 3.11 New `## Adversarial Review Prompts`

Position: After `## Evidence Matrix Questions`.

Added:
- Invalidating assumption.
- Hidden coupling.
- Permanent temporary compatibility.
- False-passing test.
- Future maintainer misunderstanding.
- Theory-only rollback.
- Missed user/caller/operator impact.
- Agent hallucination due to vague contracts.

### 3.12 `## Domain Adaptation Questions`

Position: Extend existing questions.

Added:
- What must a non-domain user monitor so they are not misled by a confident agent-generated implementation?

### 3.13 `## Passing Criteria`

Position: Replace/extend existing criteria.

Added:
- Evidence/validation support.
- Pre-pass stress test survival.
- New `Pass under assumptions` status.
- `Revise` when stress test/gate fails.

### 3.14 New `## Non-Expert Decision Brief`

Position: End of file.

Added:
- Top 3 things to watch.
- Red lines.
- Owned tradeoffs.
- Confirmation questions.
- Evidence later implementation must preserve.

## 4. `references/document-template.md` — Final Plan Document Structure

Use this file for the shape of docs produced by the skill.

### 4.1 Intro

Position: After title.

Added:
- The final document must be executable by someone who did not read the prior conversation.
- Preserve material decisions, evidence, assumptions, accepted risks, and validation requirements without bloating docs.

### 4.2 Section 2 `Goals and Non-Goals`

Position: Extend existing section.

Added:
- Success criteria.

### 4.3 New Section 3 `Context, Evidence, and Assumptions`

Position: After goals/non-goals, before risks.

Added:
- Artifact under review.
- Change type.
- Affected surface.
- Available evidence.
- Assumptions table.
- Unknowns table.
- Blast radius.
- Reversibility.

### 4.4 Section 5 `Target Design`

Position: Extend original target design fields.

Added:
- Public contracts.
- Failure behavior.
- Security/privacy/permissions.
- Cleanup/removal path.

### 4.5 New Section 6 `Domain Pack`

Position: After target design.

Added:
- Applicable lenses.
- Critical contracts/invariants.
- Highest-risk failure modes.
- Evidence needed to pass.
- Checks intentionally skipped.

### 4.6 Section 7 `Alternatives Considered`

Position: Extend table.

Added:
- `Why rejected / accepted` column.

### 4.7 Section 9 `Implementation Plan`

Position: Extend section.

Added:
- Dependency order and rollback safety.

### 4.8 Section 10 `Validation Plan`

Position: Extend validation categories.

Added:
- Observability verification.
- Security/privacy check.
- AI/agent evals if applicable.
- Acceptance evidence to preserve.

### 4.9 Section 11 `Rollout and Rollback`

Position: Extend rollout/rollback fields.

Added:
- Feature flag / routing / compatibility owner.
- Communication/support/runbook.

### 4.10 New Section 12 `Gate Evidence Matrix`

Position: After rollout/rollback, before stress test.

Added:
- Gate, Evidence/Assumption, Validation, Status matrix.

### 4.11 New Section 13 `Pre-Pass Stress Test`

Position: After gate matrix.

Added:
- Three failure scenarios table.

### 4.12 Section 14 `Acceptance Criteria`

Position: Kept after stress test.

Reason:
- Acceptance criteria should reflect the result of evidence and stress-test work.

### 4.13 New Section 15 `Accepted Risks`

Position: After acceptance criteria.

Added:
- Risk, owner, impact, mitigation, trigger/expiry, reason accepted.

### 4.14 New Section 16 `Non-Expert Decision Brief`

Position: After accepted risks.

Added:
- Top 3 things to watch.
- Red lines.
- Acceptable tradeoffs.
- Confirmation questions.
- Evidence future agent implementation must preserve.

### 4.15 New Section 17 `Implementation Readiness`

Position: After non-expert brief.

Added:
- Implementation readiness score.
- Confidence.
- Reason.

### 4.16 Section 18 `Expert Review Record`

Position: Replace original review record with expanded version.

Added:
- `Pass under assumptions` status.
- Regression review table.
- Material findings table with evidence/assumption column.
- Decision changes from review.
- Remaining accepted risks.

### 4.17 `## Writing Rules`

Position: Extend final writing rules.

Added:
- Prefer concrete contracts, states, owners, triggers, and validation artifacts.
- Preserve only material review history.
- Use `Pass under assumptions`, `Revise`, or `Blocked` when facts are unverified.
- Include gate evidence matrix for user/caller/data/contract/production/security/maintainability impact.
- Include non-expert brief when user is outside domain or using an agent for unfamiliar work.

## Summary of What Was Added Where

| Capability | File | Position |
| --- | --- | --- |
| Honest simulated-expert framing | `SKILL.md` | Frontmatter description + intro |
| Review modes | `SKILL.md` | After References |
| Context intake | `SKILL.md`, `review-rubric.md`, `document-template.md` | New top-level section / questions / doc section |
| Evidence discipline | `SKILL.md`, `quality-gates.md`, `review-rubric.md` | Before expert panel / evidence rules / evidence questions |
| `Pass under assumptions` | All 4 files | Decision/status sections and templates |
| Disagreement resolution | `SKILL.md`, `quality-gates.md` | New section / final gate |
| Domain pack | `SKILL.md`, `review-rubric.md`, `document-template.md` | New section and doc section |
| Regression review | `SKILL.md`, `quality-gates.md`, `document-template.md` | New section / regression gate / review record |
| Pre-pass stress test | `SKILL.md`, `quality-gates.md`, `review-rubric.md`, `document-template.md` | New section/prompt/table |
| Adversarial red-team review | `SKILL.md`, `review-rubric.md` | New section/prompts |
| Gate evidence matrix | `SKILL.md`, `quality-gates.md`, `document-template.md` | New matrix section |
| Agent-generated plan traps | `quality-gates.md`, `review-rubric.md` | Debt trap section / AI-agent lens |
| Overengineering traps | `quality-gates.md` | New section |
| Minimal safe plan | `SKILL.md`, `quality-gates.md` | New mode/gate |
| Implementation readiness score | `SKILL.md`, `quality-gates.md`, `document-template.md` | Optional scoring section/template |
| Non-expert control summary | `SKILL.md`, `review-rubric.md`, `document-template.md` | Summary/brief sections |
| Document template expansion | `document-template.md` | Recommended structure |
