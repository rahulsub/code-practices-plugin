---
name: cleanup
description: Systematic code cleanup after completing features. Use after refactoring or periodically to remove dead code, console statements, TODOs, and tech debt.
---

# Code Cleanup Skill

## Trigger
Use after completing a feature, after refactoring, or periodically to maintain code health.

## The Problem
Karpathy: "They don't clean up dead code after themselves... They will implement an inefficient, bloated, brittle construction."

LLMs leave messes. This skill systematically cleans them up.

## Cleanup Checklist

### 1. Dead Code
Remove code that is never executed:

```bash
# Find unused exports in TypeScript
npx ts-prune

# Find unused dependencies
npx depcheck

# Find unused variables (ESLint)
npx eslint --rule 'no-unused-vars: error' src/
```

**Manual checks:**
- [ ] Functions that are never called
- [ ] Variables that are assigned but never read
- [ ] Imports that are never used
- [ ] Components that are never rendered
- [ ] CSS classes that are never applied
- [ ] Feature flags that are always on/off

### 2. Commented-Out Code
Delete it. Git remembers.

```typescript
// BAD: Leaving commented code
// function oldImplementation() {
//   // 50 lines of dead code...
// }
function newImplementation() { ... }

// GOOD: Just delete it
function newImplementation() { ... }
```

### 3. Console Statements
Remove debugging artifacts:

```bash
# Find console.log statements
grep -r "console\." src/ --include="*.ts" --include="*.tsx"
```

### 4. TODO Comments
Address or delete:
- If it's important, do it now or create an issue
- If it's not important, delete the comment

```bash
# Find TODOs
grep -r "TODO\|FIXME\|HACK\|XXX" src/
```

### 5. Duplicate Code
Look for copy-paste patterns:

```bash
# Find duplicate code blocks
npx jscpd src/
```

If you see the same logic in multiple places, extract it.

### 6. Orphaned Files
Files that aren't imported anywhere:

```bash
# Find files not imported by anything
npx unimported
```

### 7. Obsolete Dependencies
Dependencies that are no longer used:

```bash
npx depcheck
```

### 8. Type Assertions
Unnecessary `as` casts that hide type errors:

```typescript
// BAD: Hiding type errors
const user = data as User;

// GOOD: Proper type guards
function isUser(data: unknown): data is User {
  return typeof data === 'object' && data !== null && 'id' in data;
}
if (isUser(data)) { ... }
```

## After Cleanup
1. Run all tests
2. Run the linter
3. Run type checking
4. Do a visual review of the changes

## Output Report
```
## Cleanup Summary

### Removed
- 3 unused functions (150 lines)
- 12 unused imports
- 5 commented-out code blocks
- 2 unused dependencies

### Remaining Issues
- 8 TODO comments (see issues #12, #13)
- 3 console.log statements (intentional for debugging feature X)

### Lines Before: 5,432
### Lines After: 5,150
### Reduction: 282 lines (5.2%)
```
