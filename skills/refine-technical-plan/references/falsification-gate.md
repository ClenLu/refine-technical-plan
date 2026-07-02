# Falsification / Kill-Shot Gate

This gate converts a pass-like review from confirmation (accumulating green checks) into falsification (a genuine, pre-registered attempt to kill the plan). A high-risk plan earns a pass-like decision only when its single most lethal failure was pre-registered and then survived a real disconfirmation attempt, not when it merely accumulated satisfied checklist items.

This gate owns the semantics for the kill-shot protocol; other files reproduce only its output shape.

## Why This Gate Exists

Reviewer independence, evidence rules, and the pre-pass stress test still run inside one model. When the model is confidently wrong, its own red-team is often wrong in the same direction (correlated failure). Green checks reward confirmation bias; this gate rewards a survived attempt to disprove the plan. Absence of a successful kill after a genuine attempt is evidence of robustness. A pile of satisfied checks is not.

## When to Apply

- Mandatory before `Pass`, `Pass with notes`, or `Pass under assumptions` on high-risk plans: irreversible data/state change, public API/event/schema/prompt/tool/permission contract change, security/privacy/compliance/billing/auth impact, production migration or high-blast-radius rollout, autonomous agent with real-world side effects, or cross-team/external-contract coordination.
- Lightweight (single-agent, cheapest probe only) for medium-risk plans.
- Skip for low-risk, reversible, docs-only work; forcing it there is over-engineering and violates the Review Discipline Gate.

## Protocol

Run in order. Pre-registration comes before the decision.

### 1. Pre-register the kill-shot (before deciding pass)

Commit, in writing, to the single most likely lethal failure before running the probe or choosing a decision:

- Lethal failure: the concrete way this plan most likely breaks in production or use.
- Mechanism: why it happens.
- Observable signal: the exact metric, log line, error, query result, test, or user-visible symptom that would show it.
- Falsifiable prediction: "If we run `<cheapest probe>`, we expect `<result that would prove the failure is real>`."

Pre-registration rule: write this before seeing probe results. Do not edit it afterward to make the plan look safer.

### 2. Choose the cheapest disconfirming probe

Prefer executed evidence over argument. Pick the probe with the highest kill-power per cost:

- Repo probe: `rg` caller/consumer search, schema/migration read, config/flag check, generated client or lockfile version.
- Data probe: sample query, count, shape check, null/duplicate/ordering scan.
- Behavior probe: minimal dry-run, unit/contract test, smoke check, assert, or a tiny script.
- Independence probe: spawn a separate red-team subagent that sees only the plan (not this reviewer's reasoning) and is tasked to kill it.

### 3. Attempt the kill

Run the probe or spawn the adversary. Record exactly what was run and the raw result. Do not paraphrase a probe you did not run.

### 4. Grade the attempt

- `Killed`: the probe confirmed the failure, or the adversary found a credible lethal path. The plan does not pass; downgrade to `Revise` or `Blocked` and record the finding with a stable ID.
- `Survived (empirical)`: the probe ran and the failure was not reproduced or was refuted. This is real evidence of robustness on that dimension; the pass-like decision may proceed.
- `Untestable here`: the probe cannot be run in this environment (no repo/production access, owner-only fact). The kill-shot remains unresolved; cap the decision at `Pass under assumptions` and name the exact probe or owner confirmation needed.

### 5. Independence rule

The agent that pre-registers and grades the kill-shot should not be the agent that authored or rewrote the plan whenever a genuinely independent pass is feasible; spawn a subagent for the adversary. If independence is not feasible, state that the falsification was self-run and record correlated-failure risk as a named limitation.

## Decision Impact

- No absolute `Pass` on a high-risk plan without a pre-registered kill-shot that was genuinely attempted and survived with executed or owner-confirmed evidence.
- A kill-shot that could not be tested caps the decision at `Pass under assumptions`; it never yields absolute `Pass`.
- A successful kill cannot be downgraded by argument. Only new evidence, scope reduction that removes the failure path, or a valid `Accepted Risk` can change it.
- Confirmation-style green checks do not substitute for a survived kill-shot on high-risk plans.

## Anti-Gaming Rules

- Pre-register the single most likely lethal failure by blast radius x probability, not the easiest straw failure to refute.
- Do not edit the pre-registered prediction after seeing the probe result.
- Do not mark `Survived` from reasoning alone when a cheap real probe was available and skipped.
- Do not spawn an adversary and then ignore or explain away a credible kill it found.
- One strong kill-shot beats many weak ones; do not bury the lethal prediction in trivia.

## Relationship to the Pre-Pass Stress Test

The three-scenario pre-pass stress test in `quality-gates.md` enumerates failure classes (most likely, most expensive, hardest-to-detect). The kill-shot gate selects the single deadliest candidate from that list, pre-registers it, and empirically tries to make it real. Use the stress test to find candidates, then this gate to execute against the deadliest one.

## Output Block

```markdown
**Falsification / Kill-Shot**
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
```
