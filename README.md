# Refine Technical Plan

Codex skill for hardening technical plans before implementation. It runs a simulated expert-panel workflow that reviews, rewrites, scores, and gates plans with a plain-language go/no-go conclusion, repository and external evidence checks, an approval state machine, and a refinement ledger that tracks risks until closure.

This skill is designed for users who may be relying on an agent outside their own domain and need protection from confident but weak plans, hidden technical debt, and unsupported "best practice" claims.

## What It Does

- Reviews architecture, refactor, migration, integration, AI/agent, product, and implementation plans.
- Chooses only the expert review lenses that matter for the actual risk area instead of applying a fixed checklist or placeholder panel.
- Requires repository-aware evidence checks before pass-like or implementation-ready decisions when running inside a coding agent.
- Checks current external technical facts against authoritative sources when plans depend on third-party APIs, SDKs, frameworks, cloud services, platform behavior, LLM APIs, security advisories, compliance rules, or provider contracts.
- Starts with a plain-language conclusion so non-specialists can tell whether to start, pause, or add missing plan content.
- Finds weak assumptions, hidden coupling, missing validation, rollback gaps, unclear ownership, and long-term debt risks.
- Uses an approval state machine and refinement ledger to track risk status, evidence status, missing plan content, score caps, and pass conditions.
- Scores implementation readiness from 0 to 100 and caps the score when high-risk assumptions, missing owner decisions, or blockers remain.
- Runs a default first batch of up to three real refinement cycles. Each real cycle diagnoses the plan, rewrites or reshapes it, and gates the refined version.
- Stops early on clear pass or blocker instead of narrating empty extra cycles.
- Keeps the visible output proportional: the conclusion, score, next actions, state machine, and ledger stay required, while supporting sections shrink when they add little decision value.
- Continues across later user updates until the plan reaches `Pass` / `通过` or becomes blocked on missing facts or decisions.
- Checks implementation diffs against approved plans through implementation conformance review.
- Reports review coverage and owner/SME escalation needs for high-risk, unfamiliar-domain, production, security, data, or compliance decisions.
- Uses localized Chinese status labels such as `信息收集中`, `打磨中`, `等补充信息`, `基本通过，带小备注`, `通过`, and `卡住` when the user writes in Chinese.

## Installation

Install the packaged skill with `npx skills`:

```sh
npx skills add ClenLu/refine-technical-plan --skill refine-technical-plan -g -a codex -y
```

Restart Codex or reload skills if your environment requires it.

The installable skill lives under [`skills/refine-technical-plan/`](./skills/refine-technical-plan/). Keeping the runtime skill in that directory ensures `npx skills add` installs `SKILL.md` together with its bundled `references/` and `agents/` files.

## Usage

Ask Codex to use the skill when you have a plan, agent proposal, architecture idea, or PRD-to-engineering direction that needs hardening:

```text
Use $refine-technical-plan to automatically review, revise, document, and re-review this technical plan until it passes or reaches clear blockers.
```

You can provide a rough proposal, PRD, migration sketch, architecture idea, updated plan, approved plan, implementation diff, validation artifact, or an agent-generated plan that looks plausible but needs an independent risk review. The skill should inspect available repository context first and ask only for business, production, compliance, external-contract, operator, or owner facts that cannot reasonably be discovered.

If the plan depends on unstable or versioned external facts, the skill should check current authoritative documentation before treating those claims as pass-worthy evidence.

For Chinese workflows, prompts like this should trigger the full refinement loop:

```text
请用 $refine-technical-plan 帮我打磨这个 agent 方案，持续到专家团队说能通过为止。先用普通话告诉我能不能开工，再给出要补进方案里的内容。
```

To continue a previous review, provide the prior ledger or the changed plan content:

```text
继续上一轮打磨。上一轮 R2 已补充回滚演练，但 R3 的成本和责任归属还没写进方案。请判断这一版是否通过。
```

After implementation, ask it to compare the produced code, config, migration, prompt/tool, tests, docs, or deployment changes against the approved plan:

```text
Use $refine-technical-plan to run implementation conformance review for this diff against the approved plan.
```

## Verification And Evals

Regression prompts live in [`skills/refine-technical-plan/evals/evals.json`](./skills/refine-technical-plan/evals/evals.json). They cover:

- agent overconfidence and unsupported "best practice" claims;
- three-cycle refinement batches and continuation into later batches;
- approval state machine and refinement ledger updates;
- plain-language conclusions for non-specialists;
- score caps, missing-plan-content handling, and pass/block decisions;
- Chinese output labels and shareable plan language;
- repository-aware evidence inspection when local context exists;
- authoritative external verification for version-sensitive claims;
- plan-to-code conformance review against an approved plan;
- proportional output when the user wants a quick go/no-go answer.

This repository does not include a dedicated eval runner. Validate changes by installing the skill locally and running the bundled prompts through your normal Codex or skill-evaluation harness.

## Publishing

This repository is the install source for `npx skills add ClenLu/refine-technical-plan --skill refine-technical-plan`. Publishing a new version means pushing the updated skill files to the repository branch users install from. After publishing, users can reinstall or update the skill with the installation command above.

## Repository Structure

```text
.
├── skills/
│   └── refine-technical-plan/
│       ├── SKILL.md
│       ├── agents/
│       │   └── openai.yaml
│       ├── evals/
│       │   └── evals.json
│       └── references/
│           └── review-rubric.md
├── LICENSE
└── README.md
```

## License

MIT License. See [`LICENSE`](./LICENSE).
