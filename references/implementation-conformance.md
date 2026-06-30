# Implementation Conformance Review

Use this reference after code, config, migration, prompt/tool, tests, docs, or deployment changes are produced from an approved plan.

Implementation conformance review is mandatory by default after an agent implements a previously approved plan unless the user explicitly opts out.

## Purpose

A plan can pass and still be implemented incorrectly. A coding agent may silently add files, APIs, schemas, prompts, tools, flags, jobs, permissions, migrations, public behavior, shortcuts, or tests that were never reviewed. Conformance review checks whether the actual changes still satisfy the approved plan and quality gates.

Do not approve implementation merely because code compiles, CI passes, or the diff looks reasonable.

## Required Inputs

Use all available inputs:

- approved plan or latest reviewed plan version;
- prior review ledger, findings, assumptions, accepted risks, and pass conditions;
- actual diff or changed files;
- tests and validation artifacts;
- rollout, rollback, observability, and cleanup artifacts;
- repository inspection ledger when repository context matters.

If the approved plan is unavailable or too ambiguous to constrain the diff, mark the decision `Implementation Blocked` or `Revise implementation` depending on whether safe review is possible.

## Implementation Evidence Bundle

After implementation, require a compact evidence bundle. Missing items become findings when material.

```markdown
**Implementation Evidence Bundle**
- Approved plan version or summary:
- Changed files summary:
- Diff-to-plan mapping:
- Unplanned changes:
- Commands run:
- Test results:
- What each test validates:
- Validation artifacts:
- Rollback proof or rollback procedure:
- Observability evidence:
- External authority / pinned-version evidence, if material:
- Owner / SME confirmation, if required:
- Cleanup triggers for temporary paths:
- Remaining accepted risks:
```

Rules:

- Test names alone are not enough; state the claim each test validates.
- A validation plan is not validation evidence.
- Deferred evidence must be tied to an owner, trigger, and decision impact.
- Unplanned changes must be classified instead of silently accepted.

## Review Workflow

1. Identify the approved plan sections, gates, assumptions, accepted risks, and required evidence.
2. Inspect the actual diff and changed files.
3. Map every material change to an approved plan section, gate, or accepted risk.
4. Identify unplanned APIs, schemas, files, routes, permissions, configs, prompts, tools, migrations, jobs, side effects, public behavior, or operational procedures.
5. Verify the implementation preserves approved boundaries, contracts, validation requirements, rollout/rollback, observability, cleanup triggers, and accepted-risk owners.
6. Verify tests target the real production, user, contract, migration, security, permission, data, UI, or AI-agent risk.
7. Verify validation evidence exists or is explicitly pending with owner and trigger.
8. Verify temporary adapters, feature flags, dual paths, compatibility shells, facades, prompts, eval fixtures, queues, dashboards, or runbooks have owner, metric or signal, and deletion or review trigger.
9. Identify implementation shortcuts that create new debt not reviewed in the plan.
10. If the implementation adds, updates, or depends on third-party APIs, SDKs, frameworks, cloud services, model/provider APIs, security-sensitive packages, or external contracts, verify pinned versions and required external authority evidence or mark the conformance decision assumption-backed.
11. Produce review coverage for implementation conformance: diff-to-plan coverage, validation coverage, rollback/cleanup coverage, external-authority coverage, and owner/production confirmation coverage.
12. Decide implementation status using the conformance decisions below.

## Diff-to-Plan Mapping

Use this mapping:

```markdown
| Changed Area / File | Approved Plan Section / Gate | Match? | Evidence | Notes |
| --- | --- | --- | --- | --- |
|  |  | Yes/No/Partial |  |  |
```

A changed file can map to multiple gates. A file with no approved mapping is not automatically wrong, but it must be reviewed and classified.

## Unplanned Changes

Use this table when actual changes exceed the approved plan:

```markdown
| Change | Risk | Severity | Required Action |
| --- | --- | --- | --- |
|  |  | Blocker/Major/Minor/Accepted Risk |  |
```

Mark unplanned changes as `Blocker` or `Major` when they affect:

- public contract;
- data or state;
- permissions, privacy, or security;
- production rollout or rollback;
- user-visible behavior;
- AI/agent tool behavior;
- external dependencies;
- long-term architecture boundaries.

## Validation Evidence Check

Use this table:

```markdown
| Required Evidence | Actual Artifact | Claim Validated | Result | Remaining Gap |
| --- | --- | --- | --- | --- |
|  |  |  |  |  |
```

Check whether validation covers the approved gates, not only changed code.

Examples of validation artifacts:

- unit, integration, contract, end-to-end, migration, permission, security, accessibility, load, or eval test results;
- CI logs;
- dry-run output;
- smoke check output;
- screenshots or recordings;
- dashboard or alert screenshots;
- rollback rehearsal evidence;
- production observation;
- domain owner confirmation.

## Release / Rollback / Cleanup Check

Use this checklist:

```markdown
**Release / Rollback / Cleanup Check**
- Rollout preserved: Yes/No/Partial, evidence: ...
- Rollback still credible: Yes/No/Partial, evidence: ...
- Observability present: Yes/No/Partial, evidence: ...
- Cleanup trigger present: Yes/No/Partial, evidence: ...
- Temporary paths owned: Yes/No/Partial, evidence: ...
```

Rollback must be evaluated after the actual diff, not only from the original plan. Data changes, external behavior, migrations, or public contract changes may make earlier rollback assumptions invalid.

## Conformance Decisions

Use these labels exactly:

- `Implementation Pass`: implementation matches the approved plan; required tests and evidence are present or appropriately deferred by valid accepted risk.
- `Implementation Pass under assumptions`: implementation matches the plan, but material validation, owner confirmation, or production evidence remains pending.
- `Revise implementation`: implementation deviates from the plan, lacks required tests/evidence, or introduces unreviewed debt.
- `Implementation Blocked`: essential implementation evidence is unavailable, the diff cannot be inspected safely, or the approved plan is missing/ambiguous.

A normal test pass is not sufficient for `Implementation Pass` if tests do not cover approved gates or the riskiest failure modes.

## Output Template

```markdown
**Implementation Conformance Decision**
- Decision: Implementation Pass | Implementation Pass under assumptions | Revise implementation | Implementation Blocked
- Merge/release allowed: Yes / No / Only after validation
- Reason: ...

**Implementation Evidence Bundle**
- Approved plan version or summary: ...
- Changed files summary: ...
- Commands run: ...
- Test results: ...
- Validation artifacts: ...
- Rollback proof or procedure: ...
- Cleanup triggers: ...

**Diff-to-Plan Mapping**
| Changed Area / File | Approved Plan Section / Gate | Match? | Evidence | Notes |
| --- | --- | --- | --- | --- |

**Unplanned Changes**
| Change | Risk | Severity | Required Action |
| --- | --- | --- | --- |

**Validation Evidence Check**
| Required Evidence | Actual Artifact | Claim Validated | Result | Remaining Gap |
| --- | --- | --- | --- | --- |

**Release / Rollback / Cleanup Check**
- Rollout preserved: ...
- Rollback still credible: ...
- Observability present: ...
- Cleanup trigger present: ...
- Temporary paths owned: ...

**Freshness / External Authority Check**
| External Claim or Dependency | Evidence | Status | Decision Impact |
| --- | --- | --- | --- |
|  | Official-current / Official-versioned / Repo-pinned / Owner-confirmed / Assumption-backed / Stale-unverified |  |  |

**Review Coverage**
- Diff-to-plan coverage: High / Medium / Low — ...
- Validation coverage: High / Medium / Low / None — ...
- Rollback / cleanup coverage: High / Medium / Low / N/A — ...
- External authority coverage: High / Medium / Low / N/A — ...
- Owner / production confirmation coverage: Present / Partial / Missing / N/A — ...
- Biggest blind spot: ...

**Required Fixes Before Merge/Release**
- ...

**Remaining Accepted Risks**
- ...
```
