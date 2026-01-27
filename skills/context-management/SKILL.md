---
name: context-management
description: Manage Claude Code sessions effectively. Use during long sessions, multi-step tasks, or when responses start degrading. Covers /clear, checklists, and subagents.
---

# Context Management Skill

## Trigger
Use during long sessions, multi-step tasks, or when Claude's responses start degrading in quality.

## The Insight
From Anthropic's Claude Code best practices: "Regularly reset context between tasks to prevent irrelevant conversation and file contents from degrading performance."

## Why Context Matters
- Claude has limited context window
- Old, irrelevant content crowds out useful information
- Accumulated context from failed approaches confuses new attempts
- Fresh context = better responses

## Techniques

### 1. Use `/clear` Between Tasks
After completing a task, before starting a new one:

```
/clear
```

This resets the conversation while keeping:
- CLAUDE.md instructions
- Project context
- Tool permissions

**When to clear:**
- Switching to unrelated task
- After a complex debugging session
- When responses feel "confused"
- Before important implementations

### 2. Checklists for Large Tasks
For migrations, bulk fixes, or multi-file changes:

```markdown
## Migration Checklist

### Completed
- [x] Update User model
- [x] Update User service
- [x] Update User controller

### In Progress
- [ ] Update User tests ← CURRENT

### Remaining
- [ ] Update API documentation
- [ ] Update client SDK
- [ ] Run full test suite
```

Have Claude maintain this checklist:
- Update after each step
- Verify completion before proceeding
- Track what's done vs remaining

### 3. Course Correction Tools

**Escape to interrupt:**
- Press `Escape` once to stop current generation
- Useful when Claude is going down wrong path

**Double-tap Escape to edit:**
- Press `Escape` twice to edit your previous prompt
- Explore alternate directions without losing context

**Request undo:**
```
Undo the last change to user-service.ts
```

### 4. Subagents for Investigation
Early in complex tasks, use subagents to investigate details:

```
Use a subagent to research how error handling is done in this codebase.
Report back the patterns found.
```

Benefits:
- Preserves main conversation context
- Subagent does deep investigation
- Main context gets summarized findings

### 5. Explicit Context Boundaries
Tell Claude what's relevant and what isn't:

```
Forget about the authentication changes we discussed earlier.
Focus only on the new payment integration.
The relevant files are:
- src/payments/
- src/api/checkout.ts
```

## Signs You Need to Manage Context

| Symptom | Solution |
|---------|----------|
| Claude references old, abandoned approaches | `/clear` |
| Responses getting slower | `/clear` or reduce file reads |
| Claude confused about current state | Summarize current state explicitly |
| Repeating the same mistakes | Clear and re-explain requirements |
| Losing track of multi-step task | Use checklist |

## Checklist Template for Large Tasks

```markdown
## Task: [Migration/Refactor/Bulk Fix Name]

### Overview
[Brief description of the full task]

### Progress Tracking

#### Phase 1: [Name]
- [ ] Step 1.1
- [ ] Step 1.2
- [ ] Step 1.3

#### Phase 2: [Name]
- [ ] Step 2.1
- [ ] Step 2.2

#### Phase 3: [Name]
- [ ] Step 3.1
- [ ] Step 3.2

### Current Focus
[Which item is being worked on NOW]

### Blocked Items
[Items waiting on something]

### Verification
- [ ] All tests pass
- [ ] Manual verification complete
- [ ] Ready for review
```

## Anti-Patterns

**Don't:**
- Keep adding to context indefinitely
- Assume Claude remembers everything from 50 messages ago
- Debug the same issue repeatedly without clearing
- Mix multiple unrelated tasks in one session

**Do:**
- Clear between distinct tasks
- Summarize context when resuming
- Use checklists for tracking
- Interrupt early when going off track
