# Refine Technical Plan

Codex skill for hardening existing technical plans before implementation. It reviews and rewrites a user-provided plan, gives a plain-language start decision, and tells the user exactly what to add next when the plan is not ready.

This skill is designed for users who may be relying on an agent outside their own domain and need protection from confident but weak plans, hidden technical debt, unsupported "best practice" claims, and implementation drift from an already approved plan.

## What It Does

- Reviews architecture, refactor, migration, integration, AI/agent, product, and implementation plans that the user explicitly brings to the workflow.
- Stays passive by design: it should not auto-trigger from every mention of a plan, architecture, migration, refactor, or technical debt.
- Starts with a direct start decision: `可以开工`, `基本可以开工，但有小备注`, `暂缓开工，先修改方案`, or `缺关键输入，暂时不能判断`.
- Always gives concrete next actions when the plan cannot clearly start, including whether the user must provide more information.
- Produces an `优化后方案` that can replace or be merged into the original plan when refinement is useful.
- Chooses only the expert review lenses that matter for the actual risk area instead of simulating a fixed expert panel.
- Checks accessible repository context when it can affect the judgment, and says plainly when local evidence is missing.
- Checks authoritative external sources when the plan depends on version-sensitive APIs, SDKs, frameworks, cloud behavior, provider guarantees, security guidance, compliance rules, or pricing.
- Treats unchecked claims as assumptions or `待补证据`; it should not mark a plan ready to start when critical facts are unverified.
- Uses risk-matched validation instead of a fixed test matrix.
- Keeps ledgers compact by default; detailed ledgers are reserved for high-risk, repeated, accepted-risk, or user-requested traceability cases.
- Does not use numeric readiness scores.
- Supports implementation conformance review when the user provides an approved plan and implementation evidence.

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
Use $refine-technical-plan to harden this existing technical plan. Tell me whether implementation can start, what I need to add next, and rewrite the plan if needed.
```

For Chinese workflows:

```text
请用 $refine-technical-plan 帮我打磨这个 agent 方案。先告诉我能不能开工，再告诉我下一步要往方案里补什么；需要的话直接给优化后方案。
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

Default plan-refinement output starts with:

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
...

**依据与风险摘要**
- 已检查证据：...
- 关键假设：...
- 本轮评审视角：...
- 未关闭风险：...
```

The skill should omit sections that do not help the current decision. Quick go/no-go requests should stay short. High-risk or repeated reviews may include a detailed ledger after the action layer.

## Verification And Evals

Regression prompts live in [`skills/refine-technical-plan/evals/evals.json`](./skills/refine-technical-plan/evals/evals.json). They cover:

- explicit user invocation rather than broad auto-triggering;
- direct start decisions without numeric scores;
- next actions that tell the user what information, decision, evidence, or plan content is needed;
- optimized plans that can be merged into the source plan;
- ledger summaries and detailed ledgers only when traceability is useful;
- agent overconfidence and unsupported "best practice" claims;
- local repository evidence checks and honest missing-evidence handling;
- authoritative external verification for version-sensitive claims;
- risk-matched validation and rejection of "用户反馈不错" as primary acceptance;
- continuation of prior risk items across review rounds;
- implementation conformance review against approved plan conditions;
- proportional output for quick go/no-go prompts.

This repository does not include a dedicated eval runner. Validate changes by installing the skill locally and running the bundled prompts through your normal Codex or skill-evaluation harness.

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
