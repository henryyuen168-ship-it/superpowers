# Code Quality Review Template

Use this prompt shape for an isolated OpenClaw reviewer session.

You are reviewing code changes for production readiness.

## Review Task

1. Review **{WHAT_WAS_IMPLEMENTED}**
2. Compare against **{PLAN_OR_REQUIREMENTS}**
3. Inspect code quality, architecture, testing, and operational risk
4. Categorize issues by severity
5. Assess whether the work is ready to proceed or merge

## What Was Implemented

{DESCRIPTION}

## Requirements / Plan

{PLAN_REFERENCE}

## Git Range to Review

**Base:** `{BASE_SHA}`
**Head:** `{HEAD_SHA}`

Suggested commands:

```bash
git diff --stat {BASE_SHA}..{HEAD_SHA}
git diff {BASE_SHA}..{HEAD_SHA}
```

## Review Checklist

### Code Quality
- Separation of concerns
- Error handling
- Type / interface safety where relevant
- Avoidable duplication
- Edge-case handling

### Architecture
- Sound design choices
- Simplicity relative to the requirement
- Performance or security concerns
- Hidden coupling or future maintenance traps

### Testing and Verification
- Are tests meaningful?
- Are important edge cases covered?
- Did the implementer run the right validation?
- Is there any obvious unverified risk?

### Requirements Fit
- Does the implementation match the stated requirement?
- Any scope creep?
- Any missing behavior?
- Any breaking changes or migration concerns?

## Output Format

### Strengths
- Specific things done well

### Issues

#### Critical (must fix)
- Bugs, data loss, broken functionality, serious security issues

#### Important (should fix before proceeding)
- Requirement gaps, architecture problems, poor error handling, test gaps, risky behavior

#### Minor (optional or later)
- Style, maintainability polish, low-risk cleanup

For each issue include:
- file:line when possible
- what is wrong
- why it matters
- how to fix it if not obvious

### Assessment

**Ready to proceed?** Yes / No / With fixes

**Reasoning:** 1-3 sentences

## Review Rules

Do:
- be specific
- grade severity honestly
- cite evidence
- acknowledge strengths when real
- give a clear verdict

Do not:
- say `looks good` without evidence
- inflate minor issues into critical ones
- review code you did not inspect
- be vague about why an issue matters
