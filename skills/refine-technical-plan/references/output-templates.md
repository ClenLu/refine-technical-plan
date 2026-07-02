# Output Templates

Use these templates as response shapes. Keep the content in the user's language unless the user requests another language. Keep decision labels in English.

Select the lightest template that satisfies the request.

The tables and blocks here are copy shapes only. The authoritative semantics and pass/fail rules for each shared structure (gate matrix, review coverage, freshness gate, repository ledger, diff-to-plan mapping, continuation packet, and so on) live in their owning reference; see the Single Source of Truth map in `SKILL.md`. When a rule or column changes, edit the owner, not these copies. This file owns only the plain-language verdict and decision-label glossary below.

## Plain-Language Verdict (Non-Expert Lead)

Every formal review template must begin with this block. It is written for a user who cannot judge domain detail. Keep it to a few short lines, in the user's language, with no unexplained jargon. Place gate tables, coverage tables, ledgers, and stress tests after it as supporting evidence, never before it.

```markdown
**结论**
- 能不能开始做：能 / 能，但要把提示项记下来 / 现在只能做可回退的验证 / 还不行
- 现在可以安全做：<一句话>
- 现在先别做：<一句话>
- 最大的风险：<一句平实的话>
- 需要先确认/核对的一件事：<谁来确认，或去查什么；没有就写"无">
- 决策标签：<English label>（<平文注释>）
- 打磨后评分：<0-100 或 N/A>（信心 Low/Medium/High；有上一轮时写"打磨前 <0-100> → 打磨后 <0-100>"）；一句话说明分数被什么拉低，或说明为什么本轮不评分
```

The score is the Implementation Readiness Score defined in `references/quality-gates.md`; do not invent a second scale. It is required for implementation-readiness, pass-like, revise/blocked, multi-round, or user-requested scoring decisions. For tiny discovery or evidence-collection replies where scoring would add false precision, use `N/A` and say why. A plan with an unresolved `Blocker` or `Major` cannot be scored implementation-ready no matter how high the number looks. Show the before→after delta only when a prior-round score exists; otherwise show the current score alone.

## Next Review Plan

Use this block when the decision is `Revise`, `Blocked`, `Pass under assumptions`, or the user asks how to continue refining. Keep it compact and concrete. It must come from the current findings, pass conditions, evidence gaps, or refined plan; do not give generic process advice.

```markdown
**Next Review Plan**
- 优先修改方向：<1-3 个最高杠杆改动，例如补 caller inventory、收窄 rollout、改 rollback、拆迁移步骤、找 owner 确认>
- 需要补的证据：<具体 artifact、命令、owner 确认、dry-run、测试、截图、日志；没有就写"无">
- 下一轮审查模式：Review-only / Rewrite / Delta review / Evidence-collection / Validation-evidence / Implementation-conformance
```

### Decision Label Glossary

Gloss the English label once per response for non-expert readers:

- `Pass` = 通过：可以按方案开始实现，保留好要求的验证证据。
- `Pass with notes` = 通过（带提示）：可以开始，但提示项必须记录跟踪或转成明确接受的风险。
- `Pass under assumptions` = 有条件通过：现在只能做调研/试点/可回退的验证或找 owner 确认，不能上线、不能做不可逆改动。
- `Revise` = 需修改：先按要求改方案或做降风险的工作，不要照原方案全量实现。
- `Blocked` = 阻塞：缺关键信息，需要先收集证据或找 owner 确认，不能靠猜继续。

## Detail Levels

### Tiny

Use for low-risk, docs-only, local, reversible work.

Include only:

- plain-language verdict (always first);
- decision;
- material findings;
- required changes or final plan;
- next action.

### Standard

Use for normal technical plans with moderate risk.

Include:

- plain-language verdict (always first);
- decision;
- non-expert control summary when relevant;
- context and evidence summary;
- material findings;
- gate summary;
- final plan or required changes;
- next action.

### Full

Use for high-risk, unfamiliar-domain, repository-backed, data-changing, security-sensitive, public-contract, production-impacting, AI/agent, or implementation-conformance reviews.

Include (plain-language verdict first, heavy tables below as supporting evidence):

- plain-language verdict (always first);
- decision and allowed next action;
- non-expert control summary;
- repository inspection ledger when material;
- review rounds or regression review;
- severity table with stable issue IDs;
- evidence collection plan when needed;
- gate evidence matrix;
- final plan or required changes;
- adversarial review and pre-pass stress test;
- falsification / kill-shot for high-risk plans (see `references/falsification-gate.md`);
- implementation readiness;
- implementation conformance requirement;
- freshness / external authority check when material;
- review coverage and biggest blind spot;
- owner / SME escalation status when triggers apply;
- remaining risks and next step;
- next review plan when further refinement is needed;
- continuation packet when the decision is `Revise`, `Blocked`, `Pass under assumptions`, or stops with open evidence.

## Automatic Multi-Round Review

```markdown
<Start with the Plain-Language Verdict block above.>

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
| Falsification / kill-shot (high-risk) |  | Killed/Survived/Untestable/N/A |
| Technical debt control |  | Pass/Revise/N/A |

**Final Plan**
<polished plan or required revision direction>

**Adversarial Review and Pre-Pass Stress Test**
- Most likely failure: ...
- Most expensive or user-visible failure: ...
- Hardest-to-detect technical-debt failure: ...
- Invalidating assumption: ...
- False-positive test risk: ...

**Falsification / Kill-Shot**
Use for high-risk pass-like decisions. Canonical rules: `references/falsification-gate.md`.
- Pre-registered lethal failure: ...
- Mechanism: ...
- Observable signal: ...
- Falsifiable prediction: if <probe>, then <result proving the failure is real>
- Cheapest disconfirming probe: repo / data / behavior / independent subagent
- Attempt run: <exact command, query, test, or subagent task>
- Raw result: ...
- Verdict: Killed / Survived (empirical) / Untestable here
- Independence: Independent subagent / Self-run (correlated-failure limitation)
- Decision impact: ...

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

**Next Review Plan**
Use when the decision is conditional, blocked, revise-only, or the user wants to keep refining.
- 优先修改方向：...
- 需要补的证据：...
- 下一轮审查模式：...

**Continuation Packet**
Use the canonical packet from `SKILL.md` when the decision is conditional, blocked, revise-only, or stops with open evidence. Include the refined score line when scoring was applicable.
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
Use the canonical packet from `SKILL.md`, focused on open evidence gaps and forbidden work until collected.
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
Use the canonical packet from `SKILL.md`, preserving open blockers, majors, assumptions, evidence gaps, owner confirmations, next review mode, and refined score when applicable.
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
