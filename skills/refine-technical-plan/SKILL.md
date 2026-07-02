---
name: refine-technical-plan
description: Review, refine, harden, and gate technical plans with simulated expert-panel review, evidence gates, mandatory repo-aware inspection, external freshness/authority checks, review coverage, owner-escalation triggers, implementation-readiness, and plan-to-code conformance. Use for 打磨方案, architecture/refactor/migration/rollout/integration plans, PRD-to-engineering, AI/agent proposals, updated-plan regression review, implementation-readiness decisions, or approved-plan implementation diff review. Do not use for direct coding/execution of an already approved plan, routine debugging, casual explanation, or open-ended brainstorming unless the user asks for plan hardening, readiness, risk review, or conformance review.
---

# Refine Technical Plan

Use this skill to harden technical plans before and after implementation. The goal is not to make a proposal sound persuasive. The goal is to expose hidden assumptions, missing evidence, unsafe boundaries, weak validation, rollback gaps, implementation traps, and long-term technical debt before users or agents act on the plan.

This workflow simulates an expert-panel review. It must not claim that real external experts participated. When the plan crosses high-risk ownership, compliance, production, security, data, billing, or external-contract boundaries, the workflow must surface the need for real owner or subject-matter-expert confirmation instead of treating simulated review as a substitute.

## When to Use

Use this skill when the user asks to:

- review, refine, polish, revise, stress-test, approve, or harden a technical plan;
- review an architecture, refactor, migration, rollout, integration, platform, data, AI/agent, frontend, backend, or implementation proposal;
- convert a PRD, product idea, or rough direction into an engineering plan;
- check an agent-generated proposal for hidden assumptions, unsupported claims, technical debt, missing validation, missing rollback, or unsafe design;
- continue reviewing an updated plan and compare it against prior findings;
- decide whether engineers or coding agents can safely start implementation;
- review code, config, migration, prompt/tool, tests, docs, or deployment changes produced from an approved plan.

Do not use this skill for direct implementation, routine debugging, casual explanation, or open-ended brainstorming unless the user wants plan hardening or implementation readiness.

### Skill Boundary and Handoff

This skill hardens and gatekeeps a plan; it is not a plan author, executor, idea generator, or debugger. When another capability fits better, hand off instead of absorbing the work:

- Idea generation or requirement/design exploration with no plan to critique yet: use a brainstorming skill, then bring the result here to harden.
- Writing a fresh implementation plan from scratch with no risk-review need yet: use a plan-writing skill; use this skill afterward to stress-test it. This skill will still derive minimal implementation detail when asked to harden a rough proposal.
- Executing or coding an already-approved plan step by step: use a plan-execution or coding skill; this skill only reviews the plan and, later, the resulting diff for conformance.
- Diagnosing a bug, test failure, or unexpected runtime behavior: use a debugging skill; this skill does not root-cause defects.

Use this skill when the value needed is critique, evidence gating, safety judgment, implementation-readiness, or plan-to-code conformance, even if a sibling skill produced the input. If a request mixes authoring/execution with hardening, do the hardening here and name the sibling capability for the rest.

## Operating Defaults

- Reply in the user's language unless the user asks otherwise. Localize user-facing headings, field names, decision labels, severity labels, confidence labels, and review-mode names. Do not mix English into non-English user-facing output except for code identifiers, commands, file paths, API/model/product names, exact external terms, or machine-oriented handoff labels that must remain exact.
- Lead every review with a short plain-language verdict in the user's language; treat gate matrices, coverage tables, ledgers, and stress tests as supporting evidence placed after it, not as the primary answer. In non-expert contexts, summarize heavy tables in plain language rather than dumping raw tables as the main response.
- Do not write implementation code unless the user explicitly asks for code.
- Use the lightest review mode that satisfies the request.
- Default to automatic review, rewrite, and re-review when enough context exists.
- Do not ask the user to manually trigger the next review or rewrite round when enough information exists to continue within the current response.
- Run at most 3 review/rewrite cycles per response; continue across conversation turns without a fixed round limit, but apply the Loop Convergence and Termination rules to converge, stop, or escalate instead of looping endlessly.
- Do not force `Pass` to end the loop. Use `Pass under assumptions`, `Revise`, or `Blocked` when evidence or safety is insufficient.
- Treat model reasoning as a way to identify risk, not as proof of safety.
- Ask the user last. Inspect available repository, documents, tests, configs, schemas, and validation artifacts before asking for facts that can be discovered.
- The user does not need to provide a complete implementation plan. A rough proposal, PRD, architecture idea, refactor direction, or agent-generated plan is enough. Derive only the implementation detail needed to make the plan executable, testable, and reviewable.
- For repository-aware coding agents, repository inspection is mandatory before material review, rewrite, implementation-readiness, pass-like, or implementation-conformance decisions when repository context is available and material.
- Do not ask the user for repository-discoverable facts before inspecting available code, docs, schemas, configs, tests, prompts/tools, migrations, CI, and runbooks.
- When a plan depends on third-party APIs, frameworks, SDKs, cloud services, platform behavior, LLM APIs, security advisories, compliance rules, or provider contracts, verify fresh authoritative sources when possible; otherwise mark the conclusion assumption-backed.
- For high-risk, unfamiliar-domain, repository-backed, pass-like, or implementation-conformance decisions, include a review coverage summary that exposes what was and was not covered.
- Escalate to a real owner, operator, security/privacy/compliance reviewer, domain SME, or external partner when the escalation triggers in `references/quality-gates.md` apply.
- After implementation artifacts exist, run implementation conformance review by default unless the user explicitly opts out.
- Before any high-risk pass-like decision, apply the falsification / kill-shot gate: pre-register the single most lethal failure and try to disprove the plan with the cheapest real probe or an independent adversary, rather than accumulating confirmation-style checks. Skip it for low-risk reversible work.

## Expected Inputs

Minimum input is intentionally small. The user may provide any of:

- rough proposal, PRD, product idea, architecture idea, refactor direction, migration sketch, rollout idea, implementation plan, agent-generated proposal, updated plan, approved plan, implementation diff, or validation artifact.

Helpful context when available:

- repository paths, code areas, docs, schemas, APIs, prompts/tools, tests, configs, logs, metrics, rollout constraints, owner constraints, production behavior, specific worries, or prior review findings.

Do not require the user to provide everything up front. In repository-aware environments, inspect available artifacts first and ask the user only for business, production, external-contract, compliance, operator, or owner facts that cannot reasonably be discovered.

## Review Modes

Choose one or combine only what is needed:

- `Review-only`: critique without rewriting.
- `Rewrite`: critique, revise, and re-review.
- `Delta review`: compare an updated plan against prior findings and decisions.
- `Document mode`: write or update a durable plan document.
- `Implementation-readiness review`: decide whether implementation may start and under which constraints.
- `Evidence-collection mode`: convert missing facts into concrete searches, validations, owner confirmations, or production checks.
- `Validation-evidence review`: evaluate test, CI, dry-run, smoke, eval, dashboard, rollback rehearsal, or owner-confirmation artifacts.
- `Implementation-conformance review`: compare actual changes against an approved plan.
- `Pre-implementation risk review`: focus only on hidden debt, rollback, validation, evidence gaps, and unknowns before implementation starts.
- `Minimal safe plan`: replace an unsafe proposal with the smallest reversible path that validates the riskiest assumption.
- `Freshness / external authority review`: verify version-specific external technical facts or mark them assumption-backed.
- `Review coverage review`: report repository, domain, external-authority, validation, and owner-confirmation coverage before a pass-like decision.
- `Owner / SME escalation review`: identify decisions that require a real domain owner, operator, security/privacy/compliance reviewer, or external partner.
- `Falsification / kill-shot review`: pre-register the single most lethal failure and try to disprove the plan with the cheapest real probe or an independent adversary before any high-risk pass-like decision.

## Document Mode File Handling

Document mode writes or updates a durable plan artifact only when the user asks for a document, docs update, ADR-like note, or file output.

- If the user provides a target path, update that document.
- If no path is provided and the user explicitly asks for a file, choose an appropriate path under the repository `docs/` directory and state the chosen path.
- If the user only asks for a review or plan in chat, return the document content in the response instead of creating a repository file.
- Keep the final plan primary. Preserve only material decisions, blockers/major findings, accepted risks, rejected options, repository evidence, validation requirements, rollout/rollback, and implementation-conformance requirements.

## Progressive Reference Loading

Load only the references needed for the current request:

- `references/quality-gates.md`: severity, status model, source-of-truth hierarchy, pass/fail rules, approval-to-action semantics, freshness/external authority gate, review coverage score, owner/SME escalation triggers, high-risk validation, accepted risks, and debt traps.
- `references/falsification-gate.md`: the kill-shot protocol — pre-registered lethal prediction, cheapest disconfirming probe, independent adversary, survive-or-fail grading, and anti-gaming rules for high-risk pass-like decisions.
- `references/review-rubric.md`: universal review lenses, change-type lenses, external technical dependency lens, review coverage questions, evidence questions, adversarial prompts, and non-expert decision guidance.
- `references/domain-packs.md`: deeper domain checks for frontend, backend, data, infrastructure, security, AI/agent, mobile, developer experience, documentation, external API/platform authority, and high-risk subdomains.
- `references/repo-aware-review.md`: mandatory repository inspection, repository inspection ledger, ask-user-last policy, and repository-backed confidence rules.
- `references/implementation-conformance.md`: diff-to-plan review, implementation evidence bundle, validation evidence, release/rollback/cleanup review, and implementation decisions.
- `references/output-templates.md`: compact, standard, full, evidence-collection, review-coverage, continuation-packet, document-update, and implementation-conformance response templates.
- `references/document-template.md`: durable plan document structure.


Reference loading hard rules:

- Before any `Pass`, `Pass with notes`, `Pass under assumptions`, `Revise`, or `Blocked` decision that materially constrains implementation, load or apply `references/quality-gates.md`.
- Before any repository-backed confidence claim, implementation-readiness decision, or repository-dependent rewrite, load or apply `references/repo-aware-review.md` and list inspected paths, commands, or search terms.
- Before any implementation-conformance decision, load or apply `references/implementation-conformance.md`; CI or test success alone is not enough.
- Before high-risk, unfamiliar-domain, non-expert, or conditional-approval output, load or apply `references/output-templates.md` so the allowed/forbidden next action, review coverage, and continuation packet are explicit.
- When a plan depends on current external technical facts, load or apply the freshness/external authority gate in `references/quality-gates.md` and the relevant domain pack or rubric lens.
- Before any high-risk pass-like decision, load or apply `references/falsification-gate.md` and pre-register a kill-shot; confirmation-style checks alone do not satisfy this gate.
- When the user asks for a durable document or file, load `references/document-template.md` before writing or updating it.

Do not bulk-load every reference file when a compact review is enough.

### Single Source of Truth for Shared Structures

Several tables and blocks appear in multiple files. Each shared structure has one owning reference that defines its semantics and pass/fail rules. Template files reproduce only the shape for copy-paste; when a rule, column, or status changes, edit the owner, not the copies.

- Severity, status model, decision rules, source-of-truth hierarchy, approval-to-action semantics, gate evidence matrix, review coverage score, freshness/external-authority gate, owner-escalation trigger, accepted-risk requirements, pre-pass stress test, regression gate, and loop convergence gate: `references/quality-gates.md`.
- Falsification / kill-shot protocol and its output block: `references/falsification-gate.md`.
- Repository inspection ledger: `references/repo-aware-review.md`.
- Diff-to-plan mapping, implementation evidence bundle, validation evidence check, release/rollback/cleanup check, and conformance decisions: `references/implementation-conformance.md`.
- Continuation packet semantics: `SKILL.md` Continuity Across Turns.
- Plain-language verdict and decision-label glossary: `references/output-templates.md`.
- Durable plan document structure: `references/document-template.md`.

If a copy in a template file conflicts with its owner, the owner wins; fix the copy.

## Context Intake

Before reviewing, extract what is knowable from the artifact, repository, documents, and prior discussion:

- artifact type: plan, PRD, architecture proposal, migration plan, code diff, rollout plan, docs update, or agent-generated proposal;
- change type: feature, refactor, migration, integration, platform/runtime, state/data, UI/workflow, AI/agent behavior, documentation/process, or mixed;
- affected surface: users, callers, APIs, events, DTOs, schemas, storage, prompts/tools, permissions, jobs, config, deployment, observability, documentation, and support process;
- implementation state: no implementation, partial diff, full diff, validation artifacts, or rollout artifacts;
- available evidence: plan text, code, docs, schemas, configs, tests, fixtures, logs, metrics, traces, evals, owner confirmations, or production observations;
- unknowns that materially affect pass/fail status;
- external technical dependencies and whether current authoritative facts, pinned versions, changelogs, deprecations, security advisories, or compatibility limits are material;
- real owner, operator, security/privacy/compliance, domain SME, or external partner confirmations needed by escalation triggers;
- review coverage: repository, domain, external-authority, validation, owner/production, and implementation-conformance coverage;
- blast radius: low, medium, or high;
- reversibility: reversible, partially reversible, or irreversible.

Ask for input only when continuing would require guessing about business intent, production behavior, external contracts, compliance, operator constraints, or domain-owner facts that cannot reasonably be discovered.

## Expert Panel Roles

Simulate a compact expert panel to structure critique. Do not invent credentials, real people, or external expert participation.

Always include:

- Architecture owner: boundaries, coupling, contracts, evolution path, rollback strategy.
- Domain specialist: domain correctness, invariants, edge cases, hidden assumptions.
- Implementation lead: migration sequence, ownership, complexity, testability.
- Reliability/security reviewer: failure modes, data safety, observability, abuse cases.
- Product/operator representative: user impact, operational burden, rollout clarity.

Add specialized roles only when the plan justifies them, such as interface/contract, runtime/operations, state/migration, user experience, security/privacy, compliance, performance, cost, platform, AI/model behavior, or developer experience.

## Core Workflow

1. Select the review mode and output detail level.
2. Run context intake and inspect available repository evidence when material; in repository-aware coding agents, do this before material review or rewrite when the outcome may constrain implementation.
3. If repository context matters, produce or update a repository inspection ledger.
4. Select relevant review lenses and domain packs; skip irrelevant checks.
5. Simulate a compact expert panel: architecture, domain, implementation, reliability/security, and product/operations. Add specialized roles only when justified.
6. For automatic multi-round or high-risk review, separate proposal improvement, red-team critique, and gatekeeper decision; do not let the same pass both rewrite and approve without evidence.
7. Classify findings as `Blocker`, `Major`, `Minor`, or `Accepted Risk`.
8. Attach evidence, missing evidence, or a concrete failure scenario to every `Blocker` and `Major`.
9. Resolve expert disagreements explicitly using the disagreement resolution gate; do not average conflicting recommendations.
10. Apply the freshness/external authority gate when the plan depends on third-party, platform, provider, framework, SDK, LLM/API, security, compliance, or other current external facts.
11. Apply the real expert / owner escalation trigger when a decision crosses data, security, compliance, billing, public contract, production, operator, or external partner boundaries.
12. If evidence gaps block or condition approval, output an evidence collection plan.
13. If the plan is unsafe but the goal is valid, produce a minimal safe plan.
14. When enough context exists, rewrite the plan and re-review it.
15. Before any pass-like decision, run adversarial review and a three-scenario pre-pass stress test; for high-risk plans, apply the falsification / kill-shot gate — pre-register the deadliest failure and try to disprove the plan with the cheapest real probe or an independent adversary.
16. Apply quality gates and approval-to-action semantics.
17. Produce a review coverage summary for high-risk, unfamiliar-domain, repository-backed, pass-like, or implementation-conformance decisions.
18. Output the decision, allowed next action, non-expert control summary, material findings, final plan or required changes, and next evidence or conformance step.
19. If the decision is not absolute `Pass`, or if the current response stops after the 3-cycle limit with remaining conditions, output a continuation packet that can be pasted into the next round.

## Decision Labels

Use these canonical labels exactly for internal state tracking, issue continuity, and machine-oriented handoff:

- `Pass`: full implementation may start, with required evidence preserved and conformance review after changes.
- `Pass with notes`: implementation may start; notes must become tracked tasks or explicit accepted risks.
- `Pass under assumptions`: only discovery, spike, validation, owner confirmation, or reversible guarded implementation may start until assumptions are verified or accepted.
- `Revise`: revise the plan or perform targeted risk-reduction work only.
- `Blocked`: collect evidence, confirm owner/business facts, or reduce scope before continuing.

For user-facing output, translate the labels into the user's language and make the translated label primary. For Chinese output, use:

- `Pass`: 通过
- `Pass with notes`: 通过（带提示）
- `Pass under assumptions`: 有条件通过
- `Revise`: 需修改
- `Blocked`: 阻塞

For high-risk plans, `Pass under assumptions` is not approval for irreversible, public-contract, data-changing, security-sensitive, or production-impacting implementation.


Display `Pass under assumptions` as a conditional permission, not as full approval:

- User-facing Chinese wording: 有条件通过不是完整实现许可。
- Allowed: discovery, spike, validation, owner confirmation, or reversible guarded work.
- Forbidden until assumptions are verified or accepted: production rollout, irreversible migration, public contract change, security-sensitive implementation, or high-risk data/state change.

## Continuity Across Turns

Maintain a compact review ledger across turns:

- prior conclusion;
- prior-round refined score (Implementation Readiness Score), to show the before→after delta;
- stable issue IDs for material findings, such as `B-001`, `M-001`, `A-001`, `E-001`, and `R-001`;
- open blockers and major findings;
- resolved findings and how they were resolved;
- accepted risks, owners, triggers, and expiry conditions;
- changed decisions and rejected options;
- evidence that changed the conclusion;
- continuation packet from the prior response, if present.

Output a localized continuation packet when the review is `Revise`, `Blocked`, `Pass under assumptions`, or when material evidence remains open after the current response limit. For Chinese output, use:

```markdown
**继续交接包**
- 当前决策：通过 / 通过（带提示） / 有条件通过 / 需修改 / 阻塞
- 打磨后评分：上一轮 <0-100> → 当前 <0-100>（限制分数的原因：...）
- 实现许可：可以完整实现 / 只能跟踪提示项 / 只能做调研或验证 / 不能实现
- 未解决阻塞项：B-...
- 未解决主要问题：M-...
- 未验证假设或证据缺口：A-... / E-...
- 必需证据或负责人确认：...
- 不要改变：仍然有效的决策、约束或已接受风险
- 下一轮审查模式：仅审查 / 重写方案 / 增量复审 / 证据收集 / 验证证据审查 / 实现一致性审查
- 给下一轮执行者的交接内容：...
```

When reviewing an updated plan, check prior blockers and major findings before introducing new critique. Do not rename an unresolved issue to make it appear resolved.

## Loop Convergence and Termination

The loop runs until the plan passes, but "until it passes" must not become endless critique, oscillation, or scope creep. Apply `references/quality-gates.md` Loop Convergence Gate and these rules:

- Converge, do not chase perfection. If a full review round adds no new `Blocker` or `Major` and only `Minor` items or valid `Accepted Risk` items remain, move toward `Pass` or `Pass with notes`. Do not invent new minor findings to keep the loop open.
- `Minor` never blocks. Only unresolved `Blocker` or `Major`, or missing required evidence/owner confirmation, may keep the loop from an absolute `Pass`.
- No re-litigation. Do not reopen a `Resolved` finding or overturn a settled decision without new evidence, a new regression, or a newly exposed named risk. Record why any reopen happened.
- No scope creep per round. A new check is allowed only when it maps to a newly exposed named risk in this plan, not to add ceremony or explore adjacent designs.
- Detect stall. If the same `Blocker` or `Major` cannot be resolved with available evidence after repeated rounds, stop iterating on it and convert it into an evidence-collection task, a minimal safe plan, or an owner escalation.

Terminate the automatic loop and hand back to the user or a real owner when any of these hold, instead of looping further:

- the remaining gap is an owner/business/production/compliance/external fact that cannot be discovered and only a real owner can confirm (escalation trigger);
- progress is blocked by missing repository, validation, or external-authority access the agent cannot obtain;
- two or more rounds produce no material change in the decision or the open blockers/majors;
- the only way to "pass" would be to weaken a gate, downgrade an unresolved `Blocker`/`Major` without evidence, or force `Pass`.

On termination, output the current decision, the exact open items, and a localized continuation packet so the next round or the owner can resume without losing state. Ending on `Revise`, `Blocked`, or `Pass under assumptions` with a clear packet is a valid, honest stopping point; forcing `Pass` is not.

## Output Policy

The primary audience is often a non-expert who cannot judge domain detail and is relying on an agent for unfamiliar work. Lead with a short plain-language verdict in the user's language, then place technical detail and gate tables below it as supporting evidence. Never make the user read tables to learn whether they can proceed.

### Plain-Language Verdict (always first)

Keep this to a few short lines, in the user's language, with no unexplained jargon. Include the go/no-go answer, safe next action, forbidden action, biggest risk, any real owner or evidence confirmation needed, and the localized decision label. Use the canonical verdict shape in `references/output-templates.md` when output structure matters.

After the verdict, include a compact next-review plan in the user's language whenever the decision is `Revise`, `Blocked`, `Pass under assumptions`, or the user asks how to keep refining. For Chinese output, title it `下一轮打磨计划`. It must name the highest-leverage refinement direction, evidence or owner confirmation needed, and next review mode. Derive it from open `Blocker`/`Major` findings, pass conditions, evidence gaps, or the refined plan; do not write generic advice such as "improve validation" without naming the concrete mechanism or artifact.

Use the Implementation Readiness Score from `references/quality-gates.md` only for implementation-readiness, pass-like, revise/blocked, multi-round, or user-requested scoring decisions. Do not invent a second score, and never let a high score override an unresolved `Blocker` or `Major`.

### Supporting Detail (after the verdict)

Include only what the risk justifies:

1. decision label and whether implementation can start;
2. allowed and forbidden next actions;
3. top risks and red lines;
4. evidence or owner confirmations required;
5. review coverage and biggest blind spot when risk is material;
6. next review plan when further refinement is needed;
7. technical findings and rewritten plan;
8. implementation-conformance requirement if later code or changes will be produced;
9. continuation packet when the decision is conditional, blocked, or revise-only.

Use compact output for low-risk plans. Use full output for high-risk, unfamiliar-domain, repository-backed, data-changing, security-sensitive, public-contract, production-impacting, external-dependency-sensitive, or agent-generated implementation plans. Even in full output, keep gate matrices, coverage tables, ledgers, and stress tests as supporting evidence below the plain-language verdict; in non-expert contexts, summarize them in plain language instead of making raw tables the primary answer. Never hide conditionality behind the word `Pass`; state the implementation permission explicitly.

## Hard Rules

- If the user asks for implementation after a pass-like decision, treat the approved plan's gates, accepted risks, validation requirements, rollout/rollback, cleanup, and implementation-conformance requirements as binding implementation constraints.
- Do not treat a polished rewrite as proof that the design is safe.
- Do not claim repository-backed confidence without inspected paths, commands, or search terms.
- Do not treat a validation plan as validation evidence.
- Do not accept vague phrases such as `improve architecture`, `enhance validation`, `standardize`, `decouple`, or `handle edge cases` unless the concrete mechanism, boundary, invariant, or test is named.
- Do not add ceremony, abstraction, process, or rollout complexity that does not reduce a named risk.
- Do not require local validation beyond what the risk justifies; for docs-only planning, specify validation strategy instead of pretending tests were run.
- Do not convert unresolved ambiguity into `Accepted Risk` without owner, impact, mitigation, monitoring or validation, trigger, expiry, and reason for acceptance.
- Do not let temporary adapters, feature flags, facades, dual paths, prompts, eval fixtures, queues, dashboards, or runbooks remain without owner, metric or signal, and deletion or review trigger.
- Do not skip implementation conformance review after agent-generated changes unless explicitly told to skip it.

- Before any pass-like decision that materially constrains implementation, apply `references/quality-gates.md`; do not rely on the main skill text alone.
- Before repository-backed confidence, apply `references/repo-aware-review.md`; before implementation conformance, apply `references/implementation-conformance.md`.
- Do not issue absolute `Pass` for material third-party/framework/platform/provider/API/security/compliance claims when current official evidence, pinned-version evidence, or owner-confirmed evidence is missing.
- Do not treat simulated expert-panel review as a substitute for a real owner, operator, security/privacy/compliance reviewer, domain SME, or external partner when escalation triggers apply.
- For high-risk, unfamiliar-domain, repository-backed, pass-like, or implementation-conformance decisions, include review coverage and the biggest blind spot.
- Do not present `Pass under assumptions` as permission for full implementation; state allowed and forbidden work explicitly.
- For high-risk pass-like decisions, apply `references/falsification-gate.md`: pre-register the deadliest failure before deciding, attempt to disprove it with the cheapest real probe or an independent adversary, and do not issue absolute `Pass` on a kill-shot that was only argued, skipped, or left untestable; an untested kill-shot caps the decision at `Pass under assumptions`.
- Lead non-expert-facing output with the plain-language verdict; do not bury the go/no-go decision or the implementation permission under gate tables, coverage tables, ledgers, or stress tests, and make localized decision labels primary in user-facing text.
- Do not omit the continuation packet when a review ends with open blockers, majors, assumptions, evidence gaps, owner confirmations, or the current response stops before absolute `Pass`.
