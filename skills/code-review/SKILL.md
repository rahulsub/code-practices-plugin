---
name: code-review
description: Review code like a skeptical senior engineer. Use when reviewing PRs, after generating significant code, or before merging changes.
---

# Code Review Skill

## Trigger
Use when reviewing code changes, PRs, or after generating significant code.

## Mindset
Review code as a skeptical senior engineer who has seen LLMs make subtle mistakes. The errors won't be syntax - they'll be conceptual: wrong assumptions, missing edge cases, over-engineering, and silent behavior changes.

## Review Checklist

### 1. Surface Hidden Assumptions
- What assumptions does this code make about inputs, state, or environment?
- Are these assumptions documented or validated?
- Would a different developer reading this code make the same assumptions?
- Flag any assumption that isn't explicitly checked or documented.

### 2. Check for Over-Engineering
- Could this be done in fewer lines without sacrificing clarity?
- Are there abstractions that only have one implementation?
- Are there config options that will never be used?
- Is there a "clever" solution where a simple one would work?
- **Ask yourself: "Could this 1000-line solution be 100 lines?"**

### 3. Verify Consistency
- Does this change conflict with patterns used elsewhere in the codebase?
- Are similar things done similarly?
- Does naming follow existing conventions?
- Are there now two ways to do the same thing?

### 4. Identify Collateral Damage
- Were any comments removed or changed that shouldn't have been?
- Was any code modified that's unrelated to the task?
- Were any imports, exports, or dependencies changed unnecessarily?
- Is there dead code left behind from a refactor?

### 5. Evaluate Tradeoffs
- What are the performance implications?
- What are the maintenance implications?
- What could go wrong? What are the failure modes?
- Is error handling appropriate (not too much, not too little)?

### 6. Test Coverage
- Are the happy paths tested?
- Are edge cases tested?
- Are error conditions tested?
- Do tests actually verify behavior or just check that code runs?

## Output Format
```
## Code Review Summary

### Critical Issues
[Issues that must be fixed before merge]

### Suggestions
[Improvements that would make the code better]

### Assumptions Identified
[Hidden assumptions that should be validated or documented]

### Simplification Opportunities
[Places where code could be reduced without losing functionality]

### Questions
[Clarifications needed to complete review]
```

## Multi-Claude Review Pattern
From Anthropic's Claude Code best practices: Run separate Claude instances in parallel—one writing code, another reviewing.

### Why Separate Instances
The Claude that wrote the code has biases:
- Attached to its own implementation decisions
- May not question its own assumptions
- Context filled with implementation details, not review perspective

A fresh Claude instance:
- Reviews with fresh eyes
- Questions decisions the writer took for granted
- Catches issues the writer is blind to

### How to Set Up

**Terminal 1 - Writer:**
```bash
claude
# Implement the feature
```

**Terminal 2 - Reviewer:**
```bash
claude
# "Review the changes in src/feature/.
#  I didn't write this code. Review critically for:
#  - Hidden assumptions
#  - Missing edge cases
#  - Over-engineering
#  - Security issues
#  - Performance problems"
```

### Review Prompts for Reviewer Instance

```markdown
Review the following code changes as if you're seeing them for the first time.
You did NOT write this code. Be skeptical.

Focus on:
1. What assumptions does this code make that aren't validated?
2. What inputs would break this?
3. Is there a simpler way to achieve the same result?
4. What's missing from the error handling?
5. Are there any security implications?

Be direct about issues. Don't soften criticism.
```

### When to Use Multi-Claude Review
- Security-sensitive code
- Complex business logic
- Code that will be hard to change later
- Before merging significant PRs
- When you want a second opinion
