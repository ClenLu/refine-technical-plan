# Plan Document Template

Use this template when writing or updating a durable plan document, especially under `docs/`.

The document should make the final decision executable by someone who did not read the prior conversation. It should preserve material review decisions, repository evidence, assumptions, accepted risks, validation requirements, and implementation-conformance requirements without turning every intermediate critique into permanent documentation.

The section tables below are copy shapes for a durable document. The authoritative semantics and pass/fail rules for each shared structure (gate matrix, review coverage, freshness gate, repository ledger, diff-to-plan mapping, continuation packet, and so on) live in their owning reference; see the Single Source of Truth map in `SKILL.md`. When a rule or column changes, edit the owner, not these copies.

## Recommended Structure

```markdown
# <Plan Title>

## 1. Background

- Current state:
- Problem:
- Why now:

## 2. Context Intake

- Artifact under review:
- Change type:
- Affected surface:
  - Users/callers:
  - APIs/events/contracts/prompts/tools:
  - Data/state:
  - UI/workflow:
  - Permissions/security/privacy:
  - Jobs/config/deployment:
  - Operations/support/docs:
- Implementation state: No implementation yet / Partial diff / Full diff / Validation artifacts available / Rollout artifacts available
- External technical dependencies: third-party APIs / SDKs / frameworks / cloud services / platform behavior / LLM APIs / provider contracts / security advisories / compliance rules
- Blast radius: Low / Medium / High
- Reversibility: Reversible / Partially reversible / Irreversible

## 3. Repository Inspection Ledger

Use when repository context is available or material. If repository inspection was unavailable, state why.

| Area | Paths / Commands / Search Terms | Why Inspected | Finding | Confidence |
| --- | --- | --- | --- | --- |
| Existing implementation |  |  |  | High/Medium/Low |
| Callers / consumers |  |  |  | High/Medium/Low |
| Contracts / schemas / prompts / tools |  |  |  | High/Medium/Low |
| Tests / fixtures / evals |  |  |  | High/Medium/Low |
| Migrations / data / config |  |  |  | High/Medium/Low |
| Docs / runbooks / deployment |  |  |  | High/Medium/Low |

Material areas not inspected:
-

## 4. Goals, Non-Goals, and Success Criteria

Goals:
-

Non-goals:
-

Success criteria:
-

## 5. Source of Truth, Evidence, Assumptions, and Unknowns

Source-of-truth notes:
-

Available evidence:
-

Assumptions:
| ID | Assumption | Why it matters | Validation / Owner | Status |
| --- | --- | --- | --- | --- |
| A-001 |  |  |  | Open/Validated/Accepted |

Unknowns:
| ID | Unknown | Impact if wrong | Required evidence | Decision impact |
| --- | --- | --- | --- | --- |
| E-001 |  |  |  | Blocked/Revise/Pass under assumptions/Accepted |

Evidence:
| Claim | Evidence | Validation Needed | Status |
| --- | --- | --- | --- |
|  |  |  | Evidence-backed/Repository-backed/Assumption/Validation-backed/Owner-backed |

## 6. Evidence Collection Plan

Use this when missing evidence blocks or conditions approval.

| Missing Evidence | Why It Matters | How To Collect | Owner / Source | Decision Impact | Can Implementation Start? |
| --- | --- | --- | --- | --- | --- |
|  |  |  | Repo / Validation / Owner / Production |  | No / Discovery only / Guarded reversible work |


## 6A. Freshness / External Authority Check

Use when the plan depends on current external technical facts.

| External Claim | Relevant Version / Platform | Source Needed / Used | Status | Decision Impact |
| --- | --- | --- | --- | --- |
|  |  | Official docs / changelog / migration guide / deprecation notice / security advisory / compatibility matrix / provider limits / repo-pinned version / owner confirmation | Official-current / Official-versioned / Repo-pinned / Owner-confirmed / Assumption-backed / Stale-unverified |  |

Material external facts not verified:
-

## 6B. Real Expert / Owner Escalation

| Trigger | Required Owner / SME | Why It Matters | Status | Decision Impact |
| --- | --- | --- | --- | --- |
| Data / security / compliance / billing / public contract / production rollback / external partner / business rule / operator constraint / autonomous action |  |  | Missing / Confirmed / N/A |  |

## 6C. Review Coverage

| Coverage Area | Rating | Evidence / Gap | Decision Impact |
| --- | --- | --- | --- |
| Repository coverage | High / Medium / Low / N/A |  |  |
| Domain coverage | High / Medium / Low |  |  |
| External authority / freshness coverage | High / Medium / Low / N/A |  |  |
| Validation coverage | High / Medium / Low / None |  |  |
| Owner / production confirmation coverage | Present / Partial / Missing / N/A |  |  |
| Implementation conformance coverage | High / Medium / Low / N/A |  |  |

Biggest blind spot:
-

What would most improve confidence:
-

## 7. Current Risks / Technical Debt

-

## 8. Target Design

- Ownership boundaries:
- Main flow:
- Public contracts:
- Data/state changes:
- Error semantics:
- Failure behavior:
- Observability:
- Security/privacy/permissions:
- Cleanup/removal path:

## 9. Domain Pack

Selected domain packs:
-

Why selected:
-

Material checks:
-

Checks intentionally skipped:
-

Critical contracts/invariants:
-

Highest-risk failure modes:
-

Evidence needed to pass:
-

## 10. Alternatives Considered

| Option | Pros | Cons | Decision | Why rejected / accepted |
| --- | --- | --- | --- | --- |
|  |  |  |  |  |

## 11. Chosen Plan

-

## 12. Implementation Plan

1.
2.
3.

Dependency order and rollback safety:
-

## 13. Approval-to-Action Semantics

| Decision | Allowed next action | Not allowed | Required evidence before next gate |
| --- | --- | --- | --- |
| Pass / Pass with notes / Pass under assumptions / Revise / Blocked |  |  |  |

## 14. Validation Plan

- Unit:
- Integration/contract:
- End-to-end/manual:
- Migration/dry-run/smoke:
- Observability verification:
- Security/privacy/compliance:
- AI/agent evals, if applicable:
- Acceptance evidence to preserve:

## 15. Validation Evidence

| Artifact | Claim validated | What it does not validate | Result | Remaining gap |
| --- | --- | --- | --- | --- |
| Test result / CI log / dry-run / eval / smoke check / owner confirmation |  |  |  |  |

## 16. Rollout and Rollback

- Rollout:
- Rollback:
- Cleanup/removal:
- Feature flag / routing / compatibility owner, if relevant:
- Communication/support/runbook, if relevant:
- Trigger for rollback or cleanup:

## 17. Gate Evidence Matrix

| Gate | Evidence / Assumption / Validation | Status |
| --- | --- | --- |
| Scope |  | Pass/Revise/N/A |
| Boundary |  | Pass/Revise/N/A |
| Contracts |  | Pass/Revise/N/A |
| State/Migration |  | Pass/Revise/N/A |
| Failure behavior |  | Pass/Revise/N/A |
| Observability |  | Pass/Revise/N/A |
| Validation |  | Pass/Revise/N/A |
| Rollout/Rollback |  | Pass/Revise/N/A |
| Security/Privacy |  | Pass/Revise/N/A |
| External authority / freshness |  | Pass/Revise/N/A |
| Real owner / SME escalation |  | Pass/Revise/N/A |
| Review coverage |  | Pass/Revise/N/A |
| Technical debt control |  | Pass/Revise/N/A |

## 18. Pre-Pass Stress Test

| Scenario | Trigger | Affected user/caller/component/data | Detection signal | Mitigation | Rollback / cleanup | Handled? |
| --- | --- | --- | --- | --- | --- | --- |
| Most likely failure |  |  |  |  |  | Yes/No |
| Most expensive or user-visible failure |  |  |  |  |  | Yes/No |
| Hardest-to-detect technical-debt failure |  |  |  |  |  | Yes/No |

## 19. Acceptance Criteria

-

## 20. Accepted Risks

| ID | Risk | Owner | Impact | Mitigation | Monitoring / Validation | Trigger / Expiry | Reason accepted |
| --- | --- | --- | --- | --- | --- | --- | --- |
| R-001 |  |  |  |  |  |  |  |

## 21. Implementation Readiness

Implementation Readiness: <0-100>
Confidence: Low / Medium / High

Reason:
- Evidence strength:
- Repository alignment:
- Remaining assumptions:
- Validation strength:
- Rollback strength:
- Debt risk:
- External confirmation needed:
- External authority / freshness strength:
- Review coverage and biggest blind spot:
- Observability / operational readiness:
- Security / privacy / compliance risk:
- Implementation conformance status, if artifacts exist:

## 22. Implementation Evidence Bundle

Use after implementation exists.

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
- Cleanup triggers for temporary paths:
- Remaining accepted risks:

## 23. Implementation Conformance Review

Use after code, config, migration, prompt/tool, tests, docs, or deployment changes exist.

Decision: Implementation Pass / Implementation Pass under assumptions / Revise implementation / Implementation Blocked
Merge or release allowed: Yes / No / Only after validation

Diff-to-plan mapping:
| Changed Area / File | Approved Plan Section / Gate | Match? | Evidence | Notes |
| --- | --- | --- | --- | --- |
|  |  | Yes/No/Partial |  |  |

Unplanned changes:
| Change | Risk | Severity | Required Action |
| --- | --- | --- | --- |
|  |  | Blocker/Major/Minor/Accepted Risk |  |

Validation evidence check:
| Required Evidence | Actual Artifact | Claim Validated | Result | Remaining Gap |
| --- | --- | --- | --- | --- |
|  |  |  |  |  |

Release / rollback / cleanup check:
- Rollout preserved:
- Rollback still credible:
- Observability present:
- Cleanup trigger present:
- Temporary paths owned:

## 24. Expert Review Record

### Round <N>

Conclusion: Pass / Pass under assumptions / Pass with notes / Revise / Blocked

Regression review:
| Prior Finding | Previous Severity | Current Status | Evidence | New Severity |
| --- | --- | --- | --- | --- |
|  |  | Resolved/Partial/Open/Regressed |  |  |

Material findings:
| ID | Role | Severity | Finding | Required Change | Evidence / Assumption | Status |
| --- | --- | --- | --- | --- | --- | --- |
|  |  |  |  |  |  |  |

Decision changes from review:
-

Remaining accepted risks:
-


## 24A. Continuation Packet

Use when the document represents a conditional, blocked, revise-only, or multi-round state.

Use the canonical Continuation Packet fields from `SKILL.md`, including current decision, refined score when applicable, implementation permission, open blockers/majors, open assumptions or evidence gaps, required owner/evidence confirmation, constraints that must not change, next review mode, and a paste-ready handoff.

## 25. Non-Expert Decision Brief

For users who are relying on an agent outside their domain:

- Top 3 things to watch:
- Red lines that must not be accepted:
- Acceptable tradeoffs:
- Questions requiring real owner/code/production/business confirmation:
- Evidence future agent implementation must preserve:
- Simplest next instruction for a later implementation agent:
```

## Writing Rules

- Write decisions in a way that can guide implementation, not as aspirational goals.
- If no target path is provided and the user did not explicitly ask for a file, return document content in chat instead of creating a new repository document.
- Prefer concrete contracts, states, evidence, repository paths, commands, examples, owners, triggers, and validation artifacts over abstract qualities.
- Keep implementation tasks ordered by dependency and rollback safety.
- Keep validation plan separate from validation evidence.
- Keep approval-to-action semantics explicit when the decision is conditional.
- Keep review history concise; retain only decisions changed by review, blockers/major findings, accepted risks, rejected options, and why they matter.
- Do not preserve every intermediate critique if it no longer affects the final plan.
- If the user asks for an implementation plan only, still include assumptions/evidence, validation, rollout, rollback, acceptance criteria, and implementation-conformance requirement.
- If repository evidence was used, name inspected areas, artifacts, commands, or search terms.
- If important facts are unverified, label the conclusion `Pass under assumptions`, `Revise`, or `Blocked`; do not mark absolute `Pass`.
- Include a gate evidence matrix when the plan affects users, callers, data, contracts, production behavior, security/privacy, AI/tool behavior, or long-term maintainability.
- Include implementation conformance review when actual changes already exist or the document is meant to constrain a later coding agent.
- Include a non-expert decision brief when the user appears to be working outside their domain or using an agent to implement unfamiliar work.

- Include freshness / external authority evidence when the plan depends on third-party, framework, SDK, cloud, platform, provider, LLM/API, security, or compliance facts that can change over time.
- Include review coverage for high-risk, unfamiliar-domain, repository-backed, pass-like, conditional, or implementation-conformance documents.
- Include owner / SME escalation when real domain, operator, security/privacy/compliance, product, data, billing, legal/policy, or external partner confirmation is required.
- Include a continuation packet when the document is not absolute `Pass` or material evidence remains open.
