---
name: requesting-code-review
description: Use when completing tasks, implementing major features, or before merging to verify work meets requirements
---

# Requesting Code Review

Use OpenClaw review passes to catch requirement gaps and code-quality issues before they compound.

**Core principle:** review early, review often.

## When to Request Review

**Mandatory:**
- After each meaningful task in subagent-driven-development
- After completing a major feature or risky refactor
- Before merging or declaring work complete

**Optional but valuable:**
- When stuck and you need a fresh technical pass
- Before a large refactor
- After fixing a subtle bug

## Review Order

1. **Spec compliance review first** when a spec, task, or plan exists
   - Use `spec-reviewer` to answer: did we build what was asked, nothing more, nothing less?
2. **Code quality review second**
   - Use an isolated reviewer session with only the task-local context, diff range, and requirements
3. **Verification pass after fixes**
   - Use `verification-before-completion` before claiming success

This ordering matters. A beautiful implementation that misses the spec is still wrong.

## What Context to Give the Reviewer

Give the reviewer only what they need:
- what was implemented
- the requirements, spec, or plan reference
- the git range or changed files
- the verification commands and results so far
- any known risks or open questions

Do **not** dump your full session history into the review. Keep the reviewer focused on the work product.

## Minimal Review Workflow

### 1. Get the diff range

```bash
BASE_SHA=$(git rev-parse HEAD~1)   # or another appropriate base
HEAD_SHA=$(git rev-parse HEAD)
git diff --stat "$BASE_SHA".."$HEAD_SHA"
```

### 2. Run spec compliance review if applicable

If the work came from a spec or plan, invoke `spec-reviewer` first.

### 3. Run code quality review in an isolated session

Use `sessions_spawn` for a focused reviewer with:
- the goal of reviewing the diff for code quality and risk
- the requirement reference
- the base and head SHAs
- instructions to cite file/line issues and categorize severity

Template: `requesting-code-review/code-reviewer.md`

### 4. Act on the feedback

- Fix critical issues immediately
- Fix important issues before proceeding
- Defer minor issues only deliberately
- Push back when the reviewer is wrong, with evidence

### 5. Re-verify

Before closing the work, run the smallest meaningful validation and use `verification-before-completion` discipline.

## Suggested OpenClaw Reviewer Prompt Shape

A good isolated reviewer prompt includes:
- what changed
- what the code was supposed to do
- exact files or git range to inspect
- constraints like `do not rewrite unrelated files`
- required output sections: strengths, critical, important, minor, assessment

See `code-reviewer.md` for a reusable template.

## Integration with Other Skills

**Subagent-Driven Development**
- review after each task
- spec review first, then code quality review

**Writing Plans / approved specs**
- treat the plan as the source of truth for spec review

**Verification Before Completion**
- do not claim done until the fixes are verified

## Red Flags

Never:
- skip review because the change feels simple
- proceed with known unfixed important issues unless the owner explicitly accepts the risk
- let the reviewer inherit noisy transcript context when a clean prompt would do
- confuse spec review with code quality review

## Bottom Line

Request review as a structured gate, not a social ritual.

Spec first. Quality second. Verification before completion.
