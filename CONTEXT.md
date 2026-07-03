# Refine Technical Plan

This context defines the core language for a skill that helps users harden existing technical plans before implementation.

## Language

**Existing Plan**:
A technical proposal, architecture sketch, implementation plan, or agent-generated plan that the user has already chosen to bring into the refinement workflow.
_Avoid_: Implicitly discovered plan, any plan-like discussion

**User-Invoked Review**:
A refinement session that starts only after the user intentionally asks to use this skill on an existing plan.
_Avoid_: Active auto-trigger, broad plan detection

**Optimized Plan**:
A refined plan body that can replace or be merged into the user's existing plan, separate from review mechanics such as scores, ledgers, and expert commentary.
_Avoid_: Review-only feedback, critique summary

**Next Actions**:
The smallest concrete additions, decisions, or evidence the user should add so refinement can continue or the plan can pass.
_Avoid_: Generic suggestions, post-review workflow

**Refinement Ledger**:
A compact audit trail for material risks, open decisions, evidence status, next required plan content, and pass conditions across refinement rounds.
_Avoid_: Default full report, decorative table

**Ledger Summary**:
The default user-facing form of the refinement ledger, showing only the current material blockers or low-risk notes unless full traceability is needed.
_Avoid_: Hidden state, full ledger by default

**Real Refinement Cycle**:
A review pass that materially changes the critique, optimized plan, start decision, pass condition, or next actions.
_Avoid_: Ritual pass, repeated commentary

**Cycle Cap**:
The maximum number of real refinement cycles to run in one response when each cycle can reduce risk, not a promise to always produce that many cycles.
_Avoid_: Default three-cycle batch, fixed loop count

**Authoritative Evidence Check**:
A targeted verification against official documentation, standards, release notes, security advisories, or provider commitments when the pass decision depends on external technical facts.
_Avoid_: Generic web search, unchecked best-practice claim

**Pending Evidence**:
A fact or claim that may be true but cannot support plan approval until it is verified by local context or authoritative external evidence.
_Avoid_: Assumption treated as fact

**Blocking Missing Input**:
Business, production, security, compliance, migration, vendor-contract, or ownership information whose absence can change the plan shape or approval decision.
_Avoid_: Nice-to-have context, broad request for more details

**Assumption-Bound Optimization**:
An optimized plan produced under explicit assumptions when useful refinement can continue, while the approval status remains blocked or needs revision until blocking input is supplied.
_Avoid_: Assumption-backed approval

**Review Lens**:
A functional expert perspective selected because it exposes a concrete risk, tradeoff, rewrite, or pass condition for the current plan.
_Avoid_: Expert persona, roleplay panel

**Visible Lens Finding**:
A concise user-facing statement of which review lens mattered and what it changed in the judgment.
_Avoid_: Simulated expert dialogue, placeholder role list

**Start Decision**:
The primary pass gate that tells the user whether implementation can start, should pause for revisions, or is blocked on missing input.
_Avoid_: Readiness score, numeric confidence

**Next-Action Gate**:
The secondary pass gate that lists the concrete missing plan content, decisions, or evidence needed before refinement or implementation can continue.
_Avoid_: Score-based progress, vague improvement advice

**Implementation Conformance Review**:
A secondary mode that compares implementation evidence against an already approved plan, without reopening the whole plan refinement discussion by default.
_Avoid_: Fresh plan review, default refinement mode

**Plan Drift**:
A mismatch where implementation, configuration, migration, rollout, prompt, tool, or documentation changes bypass approved plan conditions.
_Avoid_: Minor implementation detail, implicit approval

**Minimal Status Labels**:
The smallest set of user-facing start decisions needed to communicate whether work can begin, should proceed with small notes, must pause for revisions, or is blocked on missing input.
_Avoid_: Detailed approval state machine, score substitute

**Non-Goal Boundary**:
An explicit statement of what the optimized plan will not change, solve, migrate, standardize, or optimize when scope control affects implementation safety.
_Avoid_: Mandatory empty section, accidental scope expansion

**Risk-Matched Validation**:
Validation whose depth matches the plan's blast radius, with each high-risk change tied to a concrete test, rollout signal, rollback check, data check, permission check, or acceptance criterion.
_Avoid_: Fixed test matrix, user-feedback-only acceptance

**Local Evidence Check**:
A review of accessible repository files, documents, prior plans, configs, tests, schemas, or code paths when they can affect the judgment.
_Avoid_: Claimed inspection without evidence, asking the user for discoverable facts
