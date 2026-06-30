# Evals

These evals are lightweight regression cases for maintaining the `refine-technical-plan` skill. They are designed for manual or script-assisted grading.

Use them when changing the skill instructions, references, examples, or output templates. They should verify:

- correct trigger and non-trigger behavior;
- decision-label discipline;
- repository-aware inspection requirements;
- evidence vs validation-plan distinction;
- accepted-risk completeness;
- document-mode file handling;
- implementation conformance after code/config/migration/prompt/tool/tests/docs/deployment changes.

The cases are intentionally small. A passing answer should follow the skill behavior, not copy exact wording.

## Files

- `cases.jsonl`: evaluation cases.
- `rubric.md`: grading criteria and critical failures.
- `trigger-tests.md`: prompt-level trigger/non-trigger checks.
- `manual-eval-spec.yaml`: compact metadata for local eval harnesses.
