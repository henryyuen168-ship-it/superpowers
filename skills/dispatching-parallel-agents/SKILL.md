---
name: dispatching-parallel-agents
description: Use when facing 2+ independent tasks that can be worked on without shared state or sequential dependencies
---

# Dispatching Parallel Agents

Use parallel subagents when multiple independent problems can be investigated or implemented without shared state.

**Core principle:** one agent per independent problem domain.

## When to Use

Use this when:
- 2+ failures appear unrelated
- multiple subsystems can be investigated independently
- each task can succeed without reading the others' outputs first
- agents will not fight over the same files or mutable environment

Do **not** use this when:
- failures are likely coupled
- one fix may collapse the others
- the tasks need the same files at the same time
- shared state or sequencing matters

## Decision Rule

Before spawning parallel work, answer:
1. Are the tasks actually independent?
2. Can each agent work from a clean task-local prompt?
3. Will they avoid editing the same files or stepping on the same runtime state?

If any answer is no, do not parallelize yet.

## OpenClaw Pattern

### 1. Split by domain

Bad split:
- `Fix everything failing`

Good split:
- `Investigate failing auth tests in auth.test.ts`
- `Fix broken billing formatter in billing/`
- `Review flaky job runner tests in jobs.test.ts`

### 2. Craft focused prompts

Each agent should get:
- one clear scope
- relevant files, errors, or commands
- constraints on what not to touch
- a required output summary

### 3. Spawn isolated subagents

Use `sessions_spawn` with isolated context by default. Only use `context:"fork"` if the child truly needs the live transcript.

Prefer one spawn per independent domain.

### 4. Coordinate, review, and integrate

After results return:
- read each summary
- check for overlapping edits or conflicting conclusions
- integrate carefully
- run final verification across the combined result

## Prompt Guidelines

Good prompts are:
1. **Focused** - one problem domain
2. **Self-contained** - enough context to start immediately
3. **Constrained** - explicit boundaries
4. **Specific about output** - summary, root cause, files changed, verification run

Example:

```text
Investigate and fix the failing tests in src/jobs/job-runner.test.ts.

Scope:
- Only this test file and directly related production code
- Do not modify unrelated tests

Tasks:
1. Reproduce the failures
2. Identify the root cause
3. Fix the issue without masking it by just increasing timeouts
4. Run the targeted test again

Return:
- root cause
- files changed
- verification result
- any risk or follow-up
```

## Common Mistakes

**Too broad**
- `Fix all tests`

**Too vague**
- `Handle the race condition`

**Too little context**
- no failing test names, no file paths, no constraints

**Unsafe parallelism**
- two agents editing the same subsystem or branch area blindly

## Integration Rules

After agents return:
1. Review each summary before trusting it
2. Resolve edit conflicts deliberately
3. Re-run the relevant full verification, not just each local check
4. If one result changes assumptions for another task, stop and reconcile before proceeding

## Best Fits in OpenClaw

This skill pairs well with:
- `subagent-driven-development` for plan-based execution
- `systematic-debugging` when multiple failures need separation by domain
- `verification-before-completion` after integration

## Bottom Line

Parallel agents are a force multiplier only when the work is truly independent.

Split cleanly. Prompt precisely. Verify after integration.
