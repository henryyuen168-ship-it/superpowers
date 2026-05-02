---
name: receiving-code-review
description: Use when receiving code review feedback, before implementing suggestions, especially if feedback seems unclear or technically questionable - requires technical rigor and verification, not performative agreement or blind implementation
---

# Receiving Code Review

Code review requires technical evaluation, not emotional performance.

**Core principle:** verify before implementing. Ask before assuming. Technical correctness over social comfort.

## Response Pattern

When receiving review feedback:

1. Read all feedback without reacting
2. Restate the requirement in your own words, or ask if unclear
3. Verify against the actual codebase and current behavior
4. Evaluate whether the suggestion is correct for this codebase
5. Respond with a technical acknowledgment or reasoned pushback
6. Implement one item at a time and test each change

## Forbidden Responses

**Never:**
- Performative agreement
- Blindly promise implementation before verification
- Partially implement a multi-item review when key items are still unclear

**Instead:**
- Restate the technical requirement
- Ask precise clarification questions
- Push back with evidence when the suggestion is wrong
- Start working once the requirement is understood

## Handling Unclear Feedback

If any item is unclear, stop and clarify before implementing anything.

Why: review items are often related. Partial understanding leads to wrong changes.

Example:

```text
Requester: "Fix items 1-6"
You understand 1,2,3,6. Unclear on 4,5.

Wrong: implement 1,2,3,6 now and ask about 4,5 later
Right: "I understand 1,2,3,6. Need clarification on 4 and 5 before implementing."
```

## Source-Specific Handling

### From the requester / owner
- Treat as authoritative on goals
- Still clarify unclear scope
- Skip praise and get to the substance

### From external reviewers
Before implementing:
1. Check whether the suggestion is technically correct here
2. Check whether it breaks existing behavior
3. Check why the current implementation exists
4. Check whether compatibility or platform constraints matter
5. Check whether the reviewer is missing local context

If the suggestion seems wrong, push back with technical reasoning.

If you cannot verify easily, say so plainly: `I can't verify this without X. Should I investigate, ask, or proceed?`

If it conflicts with a prior owner decision, stop and confirm before changing course.

## YAGNI Check

If a reviewer asks for a more "proper" implementation, grep the codebase for actual usage first.

- If unused: suggest removal or deferral
- If used: implement properly

## Implementation Order

For multi-item feedback:
1. Clarify unclear items first
2. Fix blocking issues first
3. Fix simple issues next
4. Fix complex issues after that
5. Test each fix individually
6. Re-run the relevant verification before moving on

## When to Push Back

Push back when the suggestion:
- Breaks existing functionality
- Ignores real codebase constraints
- Violates YAGNI
- Is technically incorrect for this stack
- Conflicts with prior architectural decisions

Push back with technical reasoning, not defensiveness.

## Acknowledging Correct Feedback

Good acknowledgments:
- `Fixed. [brief description]`
- `Good catch: [specific issue]. Fixed in [location].`
- Or just make the change and show it in the diff

Avoid gratitude theater. The fix is the acknowledgment.

## If Your Pushback Was Wrong

Respond briefly and factually:
- `You were right. I verified X and it does Y. Implementing now.`
- `Verified this and you're correct. My first read was wrong because Z. Fixing.`

No long apology. No defensiveness.

## GitHub Review Thread Rule

When replying to inline GitHub review comments, reply in the thread rather than as a top-level PR comment.

## Bottom Line

External feedback is input to evaluate, not a command to obey blindly.

Verify. Question. Then implement.
