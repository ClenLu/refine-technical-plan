# Repository-Aware Review

Use this reference when the reviewer is running inside a repository-aware coding agent such as Codex, Claude Code, Cursor Agent, Devin, or any environment that can inspect local project files.

Repository-aware review is mandatory before material review, rewrite, pass-like, implementation-ready, or implementation-conformance decisions when repository context is available and material. This applies even when the user supplies a polished plan, because the plan can still conflict with code, schemas, callers, tests, prompts/tools, docs, deployment, or runbooks.

## Core Principle

Do not treat the user-provided plan as the only source of truth. A plan can be internally consistent and still conflict with existing code, schemas, callers, tests, prompts, tools, docs, deployment, or production conventions.

Repository evidence confirms, weakens, or rejects plan claims. If repository evidence conflicts with plan text, prefer repository evidence unless a newer authoritative owner decision or artifact explicitly supersedes it. Repository evidence can show pinned versions and local integration behavior, but it does not by itself prove that external provider behavior, third-party APIs, security advisories, or compliance rules are current; use the freshness/external authority gate when those facts are material.

## Ask-User-Last Policy

Ask the user only for facts outside the repository or outside available tooling:

- business priority, product intent, or user-experience tradeoff;
- compliance, policy, legal, security, privacy, or domain-owner decision;
- production incident context, traffic shape, operational constraints, or historical failure not present in available logs/docs;
- external contract, provider behavior, vendor limit, official documentation ambiguity, or partner behavior not represented in the repo;
- rollout appetite, accepted-risk owner, or domain-owner approval.

Do not ask the user for facts that can reasonably be discovered from the repository, such as file paths, existing API shape, schema fields, caller inventory, route names, config names, test commands, prompt/tool definitions, migration files, feature flags, docs, or current implementation boundaries.

If tools, permissions, checkout state, or context-window limits prevent inspection, say so and mark related conclusions as assumption-backed.

## What to Inspect

Select the smallest inspection set that can validate or falsify the plan's material claims. Inspection must be sufficient for the decision being made; a rewrite that will constrain implementation needs stronger repository evidence than a casual brainstorm.

Inspect when relevant:

- existing implementation paths and ownership boundaries;
- public APIs, routes, events, DTOs, schemas, storage formats, prompts, tools, UI states, CLI behavior, workflow states, or docs contracts;
- callers, consumers, imports, routes, jobs, workflows, feature flags, config paths, generated code, or integration points;
- tests, fixtures, snapshots, evals, migration files, seed data, build scripts, CI commands, and existing failure coverage;
- documentation, ADRs, runbooks, deployment files, environment assumptions, pinned dependency versions, lockfiles, generated clients, SDK configs, and operational procedures;
- error semantics, permission checks, observability, logging, rollback mechanisms, cleanup conventions, and owner conventions.

## Repository Inspection Ledger

When repository context is material, include a compact ledger before pass-like decisions. For low-risk docs-only work, this can be compressed to one paragraph.

```markdown
| Area | Paths / Commands / Search Terms | Why Inspected | Finding | Confidence |
| --- | --- | --- | --- | --- |
| Existing implementation |  |  |  | High/Medium/Low |
| Callers / consumers |  |  |  | High/Medium/Low |
| Contracts / schemas / prompts / tools |  |  |  | High/Medium/Low |
| Tests / fixtures / evals |  |  |  | High/Medium/Low |
| Migrations / data / config |  |  |  | High/Medium/Low |
| Docs / runbooks / deployment |  |  |  | High/Medium/Low |
```

Rules:

- Name concrete paths, commands, or search terms where possible, such as `rg "FooService"`, `src/foo/*`, `db/migrations/*`, or `docs/runbooks/*`.
- Do not write vague claims such as "checked the repo" without naming what was checked.
- If an area is material but uninspected, list it as `Not inspected`, explain why, and treat related conclusions as assumptions.
- If no repository is available, say so explicitly and do not use repository-backed language.
- Confidence is about inspection coverage, not how convincing the plan sounds.

## Inspection Strategy

Use a staged approach to avoid both shallow review and excessive search.

### 1. Identify Claims

Extract plan claims that depend on repository facts:

- APIs, files, schemas, prompts, tools, commands, owners, configs, migrations, routes, jobs, permissions, tests, deployment, rollback, or docs.

### 2. Validate Names and Boundaries

Search for named artifacts and adjacent ownership boundaries.

Examples:

- exact symbol search;
- route, event, or schema search;
- config and feature-flag search;
- generated code and migration search;
- docs and runbook search.

### 3. Inventory Callers and Consumers

Before approving contract, schema, permission, prompt/tool, workflow, or behavior changes, search callers and consumers.

Include:

- imports and references;
- API routes and clients;
- event producers/consumers;
- database readers/writers;
- tests and fixtures;
- external docs or generated clients when present.

### 4. Inspect Tests and False Confidence

Existing tests are evidence only for behavior they actually cover.

Check:

- what behavior is protected;
- what behavior is only snapshot/happy-path covered;
- whether tests cover callers, contracts, permissions, migrations, rollback, or real UI/agent behavior;
- whether a new test could pass while production still breaks.

### 5. Inspect Rollout and Operations

For production-impacting work, inspect:

- deployment files;
- config and feature flags;
- runbooks;
- observability conventions;
- rollback paths;
- cleanup conventions;
- on-call or owner metadata when present.

## Repository-Backed Confidence

A conclusion, rewrite, pass-like decision, or implementation-readiness decision can be called repository-backed only when:

- material repository areas were inspected;
- inspected paths, commands, or search terms are listed;
- material uninspected areas are listed as assumptions;
- plan claims align with inspected repository facts;
- conflicts are resolved, scoped away, or recorded as findings.

Do not call a conclusion repository-backed when:

- the reviewer only read the plan;
- tool limits prevented material inspection;
- names were searched but callers, tests, schemas, or configs were not inspected when material;
- relevant repository areas were not inspected but the conclusion depends on them.

## Evidence Collection From Repository

When material evidence is missing, produce concrete collection tasks.

```markdown
| Missing Evidence | Repository Search / Command / Artifact | Why It Matters | Decision Impact |
| --- | --- | --- | --- |
| Caller inventory | `rg "<symbol>" src tests docs` | Breaking contract risk | Blocks absolute Pass |
| Schema shape | `db/migrations/*`, schema file, sample query | Data compatibility risk | Blocks rollout |
| Existing tests | test directory, CI command | False confidence risk | Blocks implementation readiness |
```

Separate repo-discoverable facts from external owner or production facts. Do not ask the user to provide repo facts before attempting inspection.

## Common Repository-Aware Agent Traps

- The agent states it inspected the repository but provides no ledger.
- The plan invents APIs, files, schemas, prompts, tools, flags, jobs, commands, owners, migrations, permissions, or product behavior.
- The agent searches only the symbol named in the prompt and misses callers, generated code, docs, tests, configs, or migrations.
- The implementation changes public behavior outside the approved plan.
- CI passes because tests cover the changed lines but not the real contract, migration, permission, rollback, UI, or agent-behavior risk.
- A temporary compatibility layer has no owner, metric or signal, deletion trigger, or cleanup task.
- Context-window limits hide a relevant file or caller, but the agent still claims full confidence.

## Output Snippet

Use this snippet when repository context matters:

```markdown
**Repository Inspection**
- Repository-backed decision: Yes / No / Partially
- Material areas inspected: ...
- Material areas not inspected: ...
- External-authority facts still needed: ...
- Repository coverage rating: High / Medium / Low / N/A
- Confidence impact: ...

| Area | Paths / Commands / Search Terms | Why Inspected | Finding | Confidence |
| --- | --- | --- | --- | --- |
|  |  |  |  |  |
```
