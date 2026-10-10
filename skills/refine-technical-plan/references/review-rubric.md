# Technical Investigation Prompts

Use these questions to select evidence-gathering work for the actual risk. Workflow, readiness, output, and continuation rules live in SKILL.md; this reference adds domain-specific questions rather than repeating them. A question matters only if its answer could change the plan or intended next stage.

## Architecture And Refactoring

- Which real callers and behavior boundaries justify the proposed modules? Trace representative paths before endorsing a Common/Engine/Pipeline abstraction.
- Can a simpler alternative preserve contracts with less coupling or operational burden? Compare a representative slice when the tradeoff is uncertain.
- Where does state live during transition, and can old and new paths disagree?
- What evidence establishes behavioral equivalence? Can existing tests exercise both paths?
- What removes adapters or dual paths, and who owns that removal?

## Services And APIs

- What is the actual order of routing, authentication, authorization, policy, request transformation, and upstream selection? Can a focused request expose an unsafe order?
- Which API/SDK version is used, and do official guarantees and a minimal call support the proposed behavior?
- Do timeouts, retries, rate limits, and circuit breakers compose safely? Could retries duplicate side effects or amplify failures?
- Are upstream errors distinguishable, and do identity, tenant, headers, and tracing survive the path correctly?
- Which consumers require compatibility, and what contract or integration check exercises that requirement?

## User-Facing Interaction

- Who owns state, and how are server updates, concurrent edits, loading, errors, permissions, and recovery reconciled?
- Do real data and interaction states expose accessibility, responsiveness, or text-overflow problems?
- Do component boundaries reflect product behavior and reuse? Would an isolated interaction prototype clarify the design?
- If hydration, client state, or bundle size matters, which representative measurement would change the choice?

## State And Migration

- What are the source of truth and invariants? What happens under duplicate, concurrent, interrupted, or out-of-order execution?
- Can the migration safely restart, and can old and new consumers coexist during the proposed sequence?
- Does recovery cover data, configuration, and external state, beyond reverting code?
- Can synthetic data or a disposable copy demonstrate reconciliation and recovery? What production conditions remain untested?
- Which irreversible steps require explicit decisions, backups, or forward recovery?

## Security And Operations

- Where are authentication, authorization, tenant isolation, audit, and secret boundaries enforced? Which negative check could expose a bypass?
- What will users see during partial failure or fallback, and can operators detect it?
- Which rollout signals justify expansion or trigger a stop? Who can act on them?
- Do deployment, incident response, cleanup, and ownership match the actual production impact?

## Performance And Cost

- What workload, scale, latency, memory, or cost constraint drives the design?
- Can a bounded benchmark compare the critical alternatives under representative conditions?
- Which bottleneck is observed rather than inferred? What workload or environment differences limit the result?
- Could a new abstraction, cache, dependency, or operational procedure cost more than the problem it solves?
