---
name: explore-plan-code
description: Structured workflow for non-trivial tasks. Read files first, create detailed plan, get approval, then implement. Prevents premature coding.
---

# Explore-Plan-Code Skill

## Trigger
Use when starting any non-trivial implementation task. This workflow prevents premature coding and significantly improves outcomes for complex problems.

## The Insight
From Anthropic's Claude Code best practices: "Research and planning before implementation dramatically outperforms jumping to code."

## The Anti-Pattern
Jumping straight to writing code without understanding:
- What files are involved
- How existing code works
- What patterns are already established
- What the full scope of changes will be

## The Workflow

### Phase 1: Explore
Read relevant files first **without writing any code**.

```
Read the authentication module and understand how sessions are currently handled.
Don't write any code yet.
```

Key activities:
- Read existing implementations of similar features
- Understand the data flow
- Identify all files that will need changes
- Note existing patterns and conventions

### Phase 2: Plan
Request a detailed plan. Use "think" to trigger extended reasoning.

```
Think through how we should implement the new caching layer.
Consider:
- What components need to change
- What order to make changes
- What could go wrong
- How to test each part

Write out a detailed plan before any implementation.
```

The plan should include:
- [ ] Files to modify (in order)
- [ ] New files to create
- [ ] Key implementation decisions
- [ ] Testing strategy
- [ ] Rollback approach if something breaks

### Phase 3: Approve
Review the plan. Ask questions. Identify risks.

**Don't proceed until you're confident the plan is sound.**

Questions to ask:
- Does this cover all edge cases?
- Is this the simplest approach?
- What could go wrong?
- How will we verify it works?

### Phase 4: Code
Now implement, following the approved plan.

```
Implement the plan we discussed. Start with [first file] and work through the list.
```

Benefits of coding after planning:
- Fewer false starts
- Less context wasted on wrong approaches
- Clear stopping points
- Easier to course-correct

### Phase 5: Commit
Review changes, write clear commit message, verify tests pass.

## Template Prompt

```markdown
## Task: [Description]

### Phase 1: Explore
Please read and understand these files before doing anything else:
- [file1]
- [file2]
- [relevant directory]

Summarize how [relevant system] currently works.

### Phase 2: Plan
Think through the implementation approach. Write a detailed plan including:
- Files to modify
- Order of changes
- Testing approach
- Potential risks

### Phase 3: Wait for Approval
Stop after the plan. I'll review before you implement.
```

## When to Use Each Phase

| Task Complexity | Explore | Plan | Approve | Code |
|-----------------|---------|------|---------|------|
| Trivial fix | Skip | Skip | Skip | Do |
| Simple feature | Brief | Brief | Quick | Do |
| Complex feature | Thorough | Detailed | Careful | Do |
| Architecture change | Deep | Comprehensive | Multiple reviews | Do |

## Why This Works
- Exploration builds accurate mental model
- Planning catches issues before they're coded
- Approval creates checkpoint for course correction
- Coding becomes execution of known-good plan
