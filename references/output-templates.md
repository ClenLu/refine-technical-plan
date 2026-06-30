# Output Templates

Use these templates as response shapes. Keep the content in the user's language unless the user requests another language. Keep decision labels in English.

Select the lightest template that satisfies the request.

## Detail Levels

### Tiny

Use for low-risk, docs-only, local, reversible work.

Include only:

- decision;
- material findings;
- required changes or final plan;
- next action.

### Standard

Use for normal technical plans with moderate risk.

Include:

- decision;
- non-expert control summary when relevant;
- context and evidence summary;
- material findings;
- gate summary;
- final plan or required changes;
- next action.

### Full

Use for high-risk, unfamiliar-domain, repository-backed, data-changing, security-sensitive, public-contract, production-impacting, AI/agent, or implementation-conformance reviews.

Include:

- decision and allowed next action;
- non-expert control summary;
- repository inspection ledger when material;
- review rounds or regression review;
- severity table with stable issue IDs;
- evidence collection plan when needed;
- gate evidence matrix;
- final plan or required changes;
- adversarial review and pre-pass stress test;
- implementation readiness;
- implementation conformance requirement;
- freshness / external authority check when material;
- review coverage and biggest blind spot;
- owner / SME escalation status when triggers apply;
- remaining risks and next step;
- continuation packet when the decision is `Revise`, `Blocked`, `Pass under assumptions`, or stops with open evidence.

## Automatic Multi-Round Review

```markdown
**Decision Summary**
- Decision: Pass | Pass under assumptions | Pass with notes | Revise | Blocked
- Can implementation start: Yes / No / Discovery only / Only after listed validation or owner confirmation
- Allowed next action: ...
- Forbidden next action: ...
- Reason: ...
- Highest current risk: ...
- If decision is `Pass under assumptions`: Implementation permission is **not full approval**; allowed work is discovery/spike/validation/owner confirmation/reversible guarded work only.

**Non-Expert Control Summary**
- Top 3 things to watch: ...
- Red lines that must not be accepted: ...
- Acceptable tradeoffs: ...
- Questions requiring real owner, code, production, business, security/privacy/compliance, or external-partner confirmation: ...
- Owner / SME escalation triggers to watch: ...
- Evidence future implementation must preserve: ...

**Context and Evidence**
- Artifact: ...
- Change type: ...
- Affected surface: ...
- Implementation state: ...
- Evidence used: ...
- Assumptions / unknowns: ...
- External technical dependencies: ...
- Blast radius: Low / Medium / High
- Reversibility: Reversible / Partially reversible / Irreversible

**Freshness / External Authority Check**
Use when material.
| External Claim | Source Needed / Used | Status | Decision Impact |
| --- | --- | --- | --- |
|  | Official-current / Official-versioned / Repo-pinned / Owner-confirmed / Assumption-backed / Stale-unverified |  |  |

**Real Expert / Owner Escalation**
Use when escalation triggers apply.
- Required real confirmation: None / Domain owner / Operator / Security / Privacy / Compliance / Product / Data / Billing / External partner / Other
- Why required: ...
- Current status: Confirmed / Missing / Not applicable
- Decision impact: ...

**Review Coverage**
| Coverage Area | Rating | Evidence / Gap | Decision Impact |
| --- | --- | --- | --- |
| Repository coverage | High / Medium / Low / N/A |  |  |
| Domain coverage | High / Medium / Low |  |  |
| External authority / freshness coverage | High / Medium / Low / N/A |  |  |
| Validation coverage | High / Medium / Low / None |  |  |
| Owner / production confirmation coverage | Present / Partial / Missing / N/A |  |  |
| Implementation conformance coverage | High / Medium / Low / N/A |  |  |

- Biggest blind spot: ...
- What would most improve confidence: ...
- Backing type: repository-backed / validation-backed / owner-backed / production-backed / external-authority-backed / assumption-backed

**Repository Inspection Ledger**
| Area | Paths / Commands / Search Terms | Why Inspected | Finding | Confidence |
| --- | --- | --- | --- | --- |

**Review Rounds**
- Round 1: <finding summary> -> <change made>
- Round 2: <finding summary> -> <change made>
- Round 3: <final gate result>

**Severity Findings**
| ID | Severity | Finding | Evidence / Assumption | Required Change | Status |
| --- | --- | --- | --- | --- | --- |
| B-001 / M-001 / A-001 / E-001 / R-001 | Blocker/Major/Minor/Accepted Risk |  |  |  | Open/Resolved/Accepted |

**Evidence Collection Plan**
| Missing Evidence | Why It Matters | How To Collect | Owner / Source | Decision Impact | Can Implementation Start? |
| --- | --- | --- | --- | --- | --- |

**Disagreement Resolution**
Use only when material expert roles disagree.
- Conflicting recommendations: ...
- Risk or invariant each protects: ...
- Blast radius / reversibility / validation strength: ...
- Chosen path: ...
- Rejected options / accepted risks: ...

**Gate Evidence Matrix**
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

**Final Plan**
<polished plan or required revision direction>

**Adversarial Review and Pre-Pass Stress Test**
- Most likely failure: ...
- Most expensive or user-visible failure: ...
- Hardest-to-detect technical-debt failure: ...
- Invalidating assumption: ...
- False-positive test risk: ...

**Implementation Readiness**
- Score: <0-100>
- Confidence: Low / Medium / High
- Reason: ...

**Implementation Conformance Requirement**
- Conformance review required after implementation: Yes / No
- Evidence implementation must preserve: ...

**Remaining Risks**
- ...

**Pass Conditions / Next Step**
- ...

**Continuation Packet**
Use when the decision is conditional, blocked, revise-only, or stops with open evidence.
- Current decision: ...
- Implementation permission: Full implementation / Notes tracked / Discovery only / No implementation
- Open blockers: ...
- Open majors: ...
- Open assumptions or evidence gaps: ...
- Required external authority or owner confirmation: ...
- Do not change: ...
- Next review mode: ...
- Paste this into the next agent or next round: ...
```

Remove empty sections when they are not relevant. Do not include `Repository Inspection Ledger` if no repository context exists; instead state that the decision is not repository-backed.

## Single-Round Review

```markdown
**Decision**
Revise | Pass under assumptions | Pass with notes | Pass | Blocked

**Reason**
...

**Expert Review**
- Architecture: ...
- Domain: ...
- Implementation: ...
- Reliability/Security: ...
- Product/Operations: ...

**Required Fixes**
- [Blocker/Major] <finding> (Evidence/Assumption: ...)

**Suggested Improvements**
- [Minor] ...

**Pass Conditions**
- ...
```

## Optimized Plan

```markdown
**Goal**
...

**Non-Goals**
...

**Evidence, Assumptions, and Unknowns**
...

**Target Design**
...

**Implementation Stages**
1. ...
2. ...

**Validation and Acceptance**
...

**Rollout, Rollback, and Cleanup**
...

**Risks and Accepted Tradeoffs**
...

**Implementation Conformance Requirement**
...
```

## Evidence Collection Mode

```markdown
**Decision**
Blocked | Revise | Pass under assumptions

**Why Evidence Is Needed**
...

**Evidence Collection Plan**
| Missing Evidence | Why It Matters | How To Collect | Owner / Source | Decision Impact | Can Implementation Start? |
| --- | --- | --- | --- | --- | --- |

**Allowed Work Before Evidence Is Collected**
- ...

**Forbidden Work Before Evidence Is Collected**
- ...

**What Would Upgrade the Decision**
- ...
- Current official external authority, pinned-version evidence, owner confirmation, validation evidence, or production observation needed to move from assumption-backed to pass-like status.

**Continuation Packet**
- Current decision: ...
- Open evidence gaps: ...
- Exact evidence to collect next: ...
- Forbidden work until collected: ...
```

## Validation Evidence Review

```markdown
**Validation Decision**
Validation-backed Pass | Still assumption-backed | Revise | Blocked

**Artifact Review**
| Artifact | Claim Validated | What It Does Not Validate | Result | Decision Impact |
| --- | --- | --- | --- | --- |

**Remaining Gaps**
- ...

**Updated Gate Decision**
- ...
```

## Implementation Conformance Review

Use `references/implementation-conformance.md` for the full template.

```markdown
**Implementation Conformance Decision**
- Decision: Implementation Pass | Implementation Pass under assumptions | Revise implementation | Implementation Blocked
- Merge/release allowed: Yes / No / Only after validation
- Reason: ...

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
```


## Review Coverage and Continuation Packet

Use this compact add-on for conditional or multi-round reviews.

```markdown
**Review Coverage**
- Repository coverage: High / Medium / Low / N/A — ...
- Domain coverage: High / Medium / Low — ...
- External authority / freshness coverage: High / Medium / Low / N/A — ...
- Validation coverage: High / Medium / Low / None — ...
- Owner / production confirmation coverage: Present / Partial / Missing / N/A — ...
- Implementation conformance coverage: High / Medium / Low / N/A — ...
- Biggest blind spot: ...
- What would most improve confidence: ...

**Continuation Packet**
- Current decision: Pass / Pass with notes / Pass under assumptions / Revise / Blocked
- Implementation permission: Full implementation / Notes tracked / Discovery only / No implementation
- Open blockers: ...
- Open majors: ...
- Open assumptions or evidence gaps: ...
- Required external authority or owner confirmation: ...
- Do not change: ...
- Next review mode: Review-only / Rewrite / Delta review / Evidence-collection / Validation-evidence / Implementation-conformance
- Paste this into the next agent or next round: ...
```

## Document Update Response

After updating a document, respond with:

```text
Updated: <path>
Decision: Pass / Pass under assumptions / Pass with notes / Revise / Blocked
Main changes:
- <change>
Remaining risks:
- <risk or "No blocking risk">
Next step:
- <next step>
```
