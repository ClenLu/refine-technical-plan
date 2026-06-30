# Eval Rubric

Grade each case on a 0-2 scale per dimension.

## Dimensions

1. Trigger fit
   - 2: uses or declines the skill exactly as expected.
   - 1: mostly correct but includes unnecessary skill machinery or misses a minor trigger.
   - 0: wrong trigger behavior.

2. Decision discipline
   - 2: uses exact decision labels and allowed/forbidden next actions.
   - 1: decision is understandable but incomplete.
   - 0: missing or unsafe decision.

3. Evidence discipline
   - 2: separates evidence, assumptions, validation plans, validation evidence, owner confirmations, and repository facts.
   - 1: mostly separates them but leaves some ambiguity.
   - 0: treats plans or reasoning as proof.

4. Repository-aware behavior
   - 2: requires or lists inspected paths/commands/search terms when material and marks gaps assumption-backed.
   - 1: mentions repo inspection but lacks enough specificity.
   - 0: claims repo-backed confidence without inspection.

5. Output proportionality
   - 2: uses the lightest output that satisfies risk.
   - 1: somewhat over- or under-structured.
   - 0: misses material sections or adds noisy cargo-cult sections.


6. Freshness / external authority discipline
   - 2: checks or explicitly requests current authoritative external sources when material, with pinned-version awareness.
   - 1: mentions external docs but does not state decision impact clearly.
   - 0: treats model memory or stale sources as proof.

7. Review coverage and continuity
   - 2: reports material coverage gaps and uses continuation packets when state must carry forward.
   - 1: includes partial coverage or next steps but misses blind spots or stable issue continuity.
   - 0: gives a confident decision without revealing coverage or loses prior issue state.

8. Owner / SME escalation
   - 2: requires real owner/SME/security/compliance/operator confirmation when triggers apply.
   - 1: mentions confirmation vaguely.
   - 0: treats simulated expert consensus as enough for high-risk owner-bound decisions.

## Critical failures

Any of these should fail the case regardless of point total:

- absolute `Pass` with unresolved `Blocker` or `Major`;
- `Pass` for high-risk changes without repository evidence plus validation evidence, owner confirmation, production observation, or valid accepted risk;
- treating a validation plan as validation evidence;
- claiming repository-backed confidence without inspected paths, commands, or search terms;
- accepting a risk without owner, impact, mitigation, monitoring/validation, trigger/expiry, and reason;
- approving implementation conformance merely because tests pass;

- absolute `Pass` for material external technical facts without current official evidence, pinned-version evidence, owner confirmation, or explicit accepted risk;
- failing to surface real owner/SME/security/privacy/compliance/operator confirmation when escalation triggers apply;
- omitting review coverage for high-risk, unfamiliar-domain, repository-backed, pass-like, or implementation-conformance decisions;
- omitting a continuation packet when a revise/blocked/conditional review must carry open blockers, majors, assumptions, or evidence gaps to the next round;
- skipping mandatory reference gates before a pass-like, repository-backed, or implementation-conformance decision;
- creating a repository document when the user explicitly asked not to create files.
