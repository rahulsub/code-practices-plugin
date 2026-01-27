---
name: goal-driven
description: Define success criteria instead of step-by-step instructions. Enables autonomous LLM iteration toward testable goals.
---

# Goal-Driven Development Skill

## Trigger
Use when starting a new task. Maximizes LLM leverage by enabling autonomous iteration.

## The Insight
Karpathy: "Don't tell it what to do, give it success criteria and watch it go. Change your approach from imperative to declarative to get the agents looping longer and gain leverage."

## Imperative vs Declarative

**Imperative (less effective):**
> "Add a function that takes a user ID, queries the database, filters out inactive users, and returns the result."

**Declarative (more effective):**
> "I need to get active users. Success criteria:
> - Returns only users where `status = 'active'`
> - Returns user ID, name, and email
> - Handles case where user doesn't exist
> - Has tests proving all criteria are met"

## Process

### Step 1: Define Success Criteria
What does "done" look like? Be specific and testable.

Bad: "Make the page faster"
Good: "Page loads in under 2 seconds on 3G. Largest Contentful Paint < 2.5s. No layout shift."

Bad: "Add user authentication"
Good: "Users can sign up with email/password. Users can log in. Sessions expire after 24h. Passwords are hashed with bcrypt. Tests verify all auth flows."

### Step 2: Define Constraints
What must NOT happen?

- "Don't modify the public API"
- "Don't add new dependencies"
- "Keep backward compatibility with v2 clients"
- "Don't exceed 100ms response time"

### Step 3: Define Verification
How will we know it works?

- "All existing tests pass"
- "New tests cover the success criteria"
- "Manual test: can complete checkout flow"
- "Lighthouse score > 90"

### Step 4: Let It Loop
Provide the criteria and let the agent iterate until success:

```
## Task: Implement rate limiting

## Success Criteria
- [ ] Requests limited to 100/minute per IP
- [ ] Returns 429 status when limit exceeded
- [ ] Limit resets after 1 minute
- [ ] Bypass for authenticated admin users
- [ ] Tests verify all criteria

## Constraints
- Use Redis for tracking (already in stack)
- Don't add new dependencies
- Keep response latency under 10ms

## Verification
Run: `npm test -- rate-limit`
All tests must pass.
```

## Template for Goal-Driven Tasks

```markdown
## Task: [What needs to be accomplished]

## Success Criteria
- [ ] [Specific, testable criterion 1]
- [ ] [Specific, testable criterion 2]
- [ ] [Specific, testable criterion 3]

## Constraints
- [What must NOT change]
- [What must NOT be added]
- [Performance/security requirements]

## Verification
- [How to prove it works]
- [Commands to run]
- [Expected output]

## Out of Scope
- [What this task does NOT include]
```

## Why This Works
LLMs excel at iteration and don't get tired. By providing clear success criteria, you enable them to:
1. Try an approach
2. Verify against criteria
3. If failing, try a different approach
4. Repeat until all criteria pass

This is their superpower. Use it.

## Specificity Dramatically Reduces Course Corrections
From Anthropic's Claude Code best practices: Vague requests perform poorly. Specific instructions dramatically reduce the corrections needed.

### Bad vs Good Prompts

| Vague (many corrections needed) | Specific (minimal corrections) |
|--------------------------------|-------------------------------|
| "Add tests" | "Write tests for edge cases: empty input, null values, logged-out users. Avoid mocks for the database layer." |
| "Fix the bug" | "Fix the race condition in user session handling. The bug occurs when two requests update the same session simultaneously." |
| "Make it faster" | "Reduce API response time from 800ms to under 200ms. Profile first to identify the bottleneck." |
| "Clean up the code" | "Remove unused imports, extract the validation logic into a separate function, rename `x` to `userId`." |
| "Add error handling" | "Add try-catch around the API call. On network error, retry 3 times with exponential backoff. On 4xx, throw user-facing error. On 5xx, log and show generic error." |

### Why Specificity Works
- Eliminates ambiguity that leads to wrong assumptions
- Provides clear completion criteria
- Reduces back-and-forth clarification
- Enables autonomous iteration toward defined goal

### Template for Specific Requests

```markdown
## Task: [Specific action verb] [specific target]

## Context
[Why this needs to be done]
[What's currently wrong or missing]

## Requirements
- [Specific requirement 1]
- [Specific requirement 2]
- [Specific requirement 3]

## Constraints
- [What NOT to do]
- [What NOT to change]

## Acceptance Criteria
- [How to verify requirement 1]
- [How to verify requirement 2]
```
