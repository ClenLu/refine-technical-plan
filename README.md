# Refine Technical Plan

Codex skill for hardening technical plans before and after implementation. It runs a simulated expert-panel workflow that reviews, rewrites, stress-tests, documents, and checks implementation conformance with explicit repository evidence, assumptions, rollback, validation, and technical-debt gates.

This skill is designed for users who may be using an agent outside their own domain and need protection from confident but weak plans.

## What It Does

- Reviews architecture, refactor, migration, integration, AI/agent, product, and implementation plans.
- Requires repository-aware evidence checks before pass-like or implementation-ready decisions when running inside a coding agent.
- Finds weak assumptions, hidden coupling, missing validation, rollback gaps, and long-term debt risks.
- Produces compact expert-style findings with `Blocker`, `Major`, `Minor`, and `Accepted Risk` severity.
- Iterates through review and rewrite cycles until the plan passes, passes under assumptions, or reaches clear blockers.
- Checks implementation diffs against approved plans through implementation conformance review.
- Can write implementation-plan style documentation with gate evidence, stress tests, and non-expert decision guidance.

## Installation

Clone this repository into your Codex skills directory:

```sh
git clone https://github.com/ClenLu/refine-technical-plan.git ~/.codex/skills/refine-technical-plan
```

Restart Codex or reload skills if your environment requires it.

## Usage

Ask Codex to use the skill:

```text
Use $refine-technical-plan to automatically review, revise, document, and re-review this technical plan until it passes or reaches clear blockers.
```

Or paste the prompt template from [`prompt-template.md`](./prompt-template.md) and replace the placeholder with your plan.

After implementation, ask it to compare the produced code, config, migration, prompt/tool, tests, docs, or deployment changes against the approved plan:

```text
Use $refine-technical-plan to run implementation conformance review for this diff against the approved plan.
```

## Repository Structure

```text
.
├── SKILL.md
├── agents/
│   └── openai.yaml
├── references/
│   ├── domain-packs.md
│   ├── document-template.md
│   ├── quality-gates.md
│   └── review-rubric.md
├── LICENSE
├── README.md
└── prompt-template.md
```

## License

MIT License. See [`LICENSE`](./LICENSE).
