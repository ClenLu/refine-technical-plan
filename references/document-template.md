# Plan Document Template

Use this template when writing or updating a plan under `docs/`.

The document should make the final decision executable by someone who did not read the prior conversation. It should preserve material review decisions, evidence, assumptions, accepted risks, and validation requirements without turning every intermediate critique into permanent documentation.

## Recommended Structure

```markdown
# <Plan Title>

## 1. Background

- Current state:
- Problem:
- Why now:

## 2. Goals and Non-Goals

Goals:
- 

Non-goals:
- 

Success criteria:
- 

## 3. Context, Evidence, and Assumptions

Artifact under review:
- 

Change type:
- 

Affected surface:
- Users/callers:
- APIs/events/contracts:
- Data/state:
- UI/workflow:
- Permissions/security/privacy:
- Jobs/config/deployment:
- Operations/support/docs:

Available evidence:
- 

Assumptions:
| Assumption | Why it matters | Validation / owner | Status |
| --- | --- | --- | --- |
|  |  |  | Open/Validated/Accepted |

Unknowns:
| Unknown | Impact if wrong | Required evidence | Decision impact |
| --- | --- | --- | --- |
|  |  |  | Blocked/Revise/Accepted |

Blast radius:
- Low/Medium/High:
- Reason:

Reversibility:
- Reversible/Partially reversible/Irreversible:
- Reason:

## 4. Current Risks / Technical Debt

- 

## 5. Target Design

- Ownership boundaries:
- Main flow:
- Public contracts:
- Data/state changes:
- Error semantics:
- Failure behavior:
- Observability:
- Security/privacy/permissions:
- Cleanup/removal path:

## 6. Domain Pack

Applicable lenses:
- 

Critical contracts/invariants:
- 

Highest-risk failure modes:
- 

Evidence needed to pass:
- 

Checks intentionally skipped:
- 

## 7. Alternatives Considered

| Option | Pros | Cons | Decision | Why rejected / accepted |
| --- | --- | --- | --- | --- |
|  |  |  |  |  |

## 8. Chosen Plan

- 

## 9. Implementation Plan

1. 
2. 
3. 

Dependency order and rollback safety:
- 

## 10. Validation Plan

- Unit:
- Integration/contract:
- E2E/manual:
- Migration/dry-run/smoke:
- Observability verification:
- Security/privacy check:
- AI/agent evals, if applicable:
- Acceptance evidence to preserve:

## 11. Rollout and Rollback

- Rollout:
- Rollback:
- Cleanup/removal:
- Feature flag / routing / compatibility owner, if relevant:
- Communication/support/runbook, if relevant:

## 12. Gate Evidence Matrix

| Gate | Evidence / Assumption | Validation | Status |
| --- | --- | --- | --- |
| Scope |  |  | Pass/Revise/Blocked/N/A |
| Boundary |  |  | Pass/Revise/Blocked/N/A |
| Contracts |  |  | Pass/Revise/Blocked/N/A |
| State/Migration |  |  | Pass/Revise/Blocked/N/A |
| Failure behavior |  |  | Pass/Revise/Blocked/N/A |
| Observability |  |  | Pass/Revise/Blocked/N/A |
| Rollout/Rollback |  |  | Pass/Revise/Blocked/N/A |
| Security/Privacy |  |  | Pass/Revise/Blocked/N/A |
| Technical debt control |  |  | Pass/Revise/Blocked/N/A |

## 13. Pre-Pass Stress Test

| Scenario | Trigger | Affected user/caller/component/data | Detection signal | Mitigation | Rollback / cleanup | Handled? |
| --- | --- | --- | --- | --- | --- | --- |
| Most likely failure |  |  |  |  |  | Yes/No |
| Most expensive or user-visible failure |  |  |  |  |  | Yes/No |
| Hardest-to-detect technical-debt failure |  |  |  |  |  | Yes/No |

## 14. Acceptance Criteria

- 

## 15. Accepted Risks

| Risk | Owner | Impact | Mitigation | Trigger / Expiry | Reason accepted |
| --- | --- | --- | --- | --- | --- |
|  |  |  |  |  |  |

## 16. Non-Expert Decision Brief

For users who are relying on an agent outside their domain:

- Top 3 things to watch:
- Red lines that must not be accepted:
- Acceptable tradeoffs:
- Questions requiring real owner/code/production/business confirmation:
- Evidence future agent implementation must preserve:

## 17. Implementation Readiness

Implementation Readiness: <0-100>
Confidence: Low/Medium/High
Reason:
- 

## 18. Expert Review Record

### Round <N>

Conclusion: Pass/Pass under assumptions/Pass with notes/Revise/Blocked

Regression review:
| Prior Finding | Previous Severity | Current Status | Evidence | New Severity |
| --- | --- | --- | --- | --- |
|  |  | Resolved/Partial/Open/Regressed |  |  |

Material findings:
| Role | Severity | Finding | Required Change | Evidence / Assumption | Status |
| --- | --- | --- | --- | --- | --- |
|  |  |  |  |  |  |

Decision changes from review:
- 

Remaining accepted risks:
- 
```

## Writing Rules

- Write decisions in a way that can guide implementation, not as aspirational goals.
- Prefer concrete contracts, states, examples, owners, triggers, and validation artifacts over abstract qualities.
- Keep implementation tasks ordered by dependency and rollback safety.
- Keep review history concise; retain only decisions changed by review, blockers/major findings, accepted risks, and why rejected alternatives were rejected.
- Do not preserve every intermediate critique if it no longer affects the final plan.
- If the user asks for an implementation plan only, still include validation, rollout, rollback, acceptance criteria, and material assumptions.
- If important facts are unverified, label the conclusion `Pass under assumptions`, `Revise`, or `Blocked`; do not mark absolute `Pass`.
- Include a gate evidence matrix when the plan affects users, callers, data, contracts, production behavior, security/privacy, or long-term maintainability.
- Include a non-expert decision brief when the user appears to be working outside their domain or using an agent to implement unfamiliar work.
