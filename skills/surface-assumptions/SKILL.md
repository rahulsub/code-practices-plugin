---
name: surface-assumptions
description: Identify and validate hidden assumptions before they become bugs. Use before implementing features or when reviewing generated code.
---

# Surface Assumptions Skill

## Trigger
Use before implementing a feature, or when reviewing generated code. Critical for catching the #1 LLM coding mistake.

## The Problem
Karpathy: "The most common category [of LLM errors] is that the models make wrong assumptions on your behalf and just run along with them without checking."

LLMs don't ask clarifying questions. They pick an interpretation and commit to it. This skill forces assumptions to be explicit.

## Process

### Step 1: List Every Assumption
Before or during implementation, explicitly list what the code assumes:

**Input Assumptions:**
- What types/shapes are expected?
- What ranges are valid?
- What formats are expected (dates, strings, etc.)?
- Can inputs be null/undefined?

**State Assumptions:**
- What state must exist before this runs?
- What order must operations happen in?
- What other code must have run first?

**Environment Assumptions:**
- What APIs/services must be available?
- What browser/runtime features are needed?
- What permissions are required?

**Business Logic Assumptions:**
- What do domain terms mean?
- What are the business rules?
- What happens in ambiguous cases?

### Step 2: Categorize Each Assumption

| Assumption | Confidence | Impact if Wrong | Action Needed |
|------------|------------|-----------------|---------------|
| User is authenticated | High | Security breach | Add auth check |
| Dates are ISO format | Medium | Parse errors | Validate format |
| Max items is 100 | Low | Performance issues | Clarify with user |

### Step 3: For Low-Confidence Assumptions
**STOP and ask the user.** Don't guess. Present the assumption and alternatives:

```
I'm assuming the date format is ISO 8601 (YYYY-MM-DD).
Should I also support:
1. US format (MM/DD/YYYY)
2. EU format (DD/MM/YYYY)
3. Unix timestamps
```

### Step 4: Document or Validate
For each assumption, either:
1. **Validate at runtime** - Add explicit checks
2. **Document clearly** - Add comments explaining the assumption
3. **Add types** - Use TypeScript to enforce constraints

## Output Format
When surfacing assumptions:

```
## Assumptions Made

### Validated in Code
- [x] User ID is a valid UUID (validated at line 45)
- [x] Amount is non-negative (validated at line 52)

### Documented
- [x] Dates are in UTC timezone (see comment at line 30)

### Needs Clarification
- [ ] Should deleted users still appear in reports?
- [ ] What happens if payment fails mid-transaction?
- [ ] Is the 100-item limit a hard requirement?
```

## Key Questions to Always Ask
1. What happens with empty/null inputs?
2. What happens with very large inputs?
3. What happens if external services fail?
4. Who can access this? Who shouldn't?
5. What happens concurrently?
