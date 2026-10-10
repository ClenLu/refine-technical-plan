# Repository Context

The skill turns existing technical plans into executable plans through investigation, isolated experiments, and revision. Prototype readiness and production readiness are separate judgments.

## File Responsibilities

- `skills/refine-technical-plan/SKILL.md`: authoritative workflow, scope, evidence, readiness, and deliverable instructions.
- `skills/refine-technical-plan/references/review-rubric.md`: optional domain-specific investigation questions.
- `skills/refine-technical-plan/agents/openai.yaml`: interface metadata and invocation prompt.
- `skills/refine-technical-plan/evals/evals.json`: regression prompts and expected behavior; scenarios requiring tools need corresponding fixtures and execution traces.
- `README.md`: installation, usage, verification, and publishing guidance.

Keep general rules in SKILL.md and domain questions in the reference to avoid duplicated or conflicting instructions. Regression expectations are not evidence that evaluations passed.
