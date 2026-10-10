# Refine Technical Plan

Codex skill for turning existing technical plans into executable, evidence-backed plans. It investigates uncertainties, runs focused isolated checks when useful, compares alternatives, and incorporates findings into a revised plan with a plain-language readiness decision.

This skill is designed for users who may be relying on an agent outside their own domain and need protection from confident but weak plans, hidden technical debt, unsupported "best practice" claims, and implementation drift from an already approved plan.

## What It Does

- Investigates material uncertainties through repository inspection, authoritative sources, existing tests, and isolated experiments.
- Compares alternatives and produces an improved plan the user can merge into the original.
- Gives a readiness decision for the intended stage, separating prototype work from production rollout.
- Identifies remaining blockers and who can resolve them, with compact risk tracking across reviews.
- Checks implementation against approved plan conditions when requested.

The skill requires intentional invocation. Investigation follows the user's scope and budget; production implementation and external changes require authorization.

## Installation

Install the packaged skill with `npx skills`:

```sh
npx skills add ClenLu/refine-technical-plan --skill refine-technical-plan -g -a codex -y
```

Restart Codex or reload skills if your environment requires it.

The installable skill lives under [`skills/refine-technical-plan/`](./skills/refine-technical-plan/). Keeping the runtime skill in that directory ensures `npx skills add` installs `SKILL.md` together with its bundled `references/` and `agents/` files.

## Usage

This skill is intended to be explicitly invoked by the user after they already have a plan or agent-generated proposal:

```text
Use $refine-technical-plan to investigate and improve this existing technical plan. Resolve what you can through code, documentation, and focused experiments; incorporate the findings and tell me which stage can start.
```

For Chinese workflows:

```text
请用 $refine-technical-plan 帮我打磨这个 agent 方案。能自己查清或实验验证的问题先做，把结果融进优化方案，再告诉我哪个阶段能开工，以及还需要我决定什么。
```

To continue a previous review, provide the prior risk summary or changed plan content:

```text
继续上一轮打磨。上一轮 R2 已补充回滚演练，但 R3 的成本和责任归属还没写进方案。请判断这一版是否可以开工。
```

After implementation, ask it to compare produced code, config, migration, prompt/tool, tests, docs, or deployment changes against the approved plan:

```text
Use $refine-technical-plan to run implementation conformance review for this diff against the approved plan.
```

## Expected Output

The answer leads with a readiness decision and its reason. For a refinement request, it normally includes the improved plan, important investigation results, and remaining actions with an explicit agent/user responsibility. It distinguishes measured results, assumptions, and checks that were not run.

There is no mandatory report template, fixed review-cycle count, or fixed action count. Quick go/no-go requests stay brief. High-risk or repeated reviews include enough evidence and a visible risk handoff to preserve traceability. The revised plan is shareable document content, separate from review mechanics.

## Verification And Evals

Regression prompts live in [`skills/refine-technical-plan/evals/evals.json`](./skills/refine-technical-plan/evals/evals.json). They cover invocation, evidence honesty, active investigation, plan revision, continuation, implementation conformance, isolated experiments, stage-specific readiness, and proportional effort.

This repository does not include a dedicated eval runner. Validate changes by installing the skill locally and running the bundled prompts through your normal Codex or skill-evaluation harness. For tool-use scenarios, supply the stated fixtures and inspect tool traces and artifact locations alongside the answer; a plausible written response alone does not establish that an experiment ran.

## Publishing

This repository is the install source for `npx skills add ClenLu/refine-technical-plan --skill refine-technical-plan`. Publishing a new version means pushing the updated skill files to the repository branch users install from. After publishing, users can reinstall or update the skill with the installation command above.

## Repository Structure

```text
.
├── CONTEXT.md
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
