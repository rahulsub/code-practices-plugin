---
name: simplify
description: Remove over-engineering and unnecessary abstractions. Use after implementing features or when code feels bloated.
---

# Simplify Code Skill

## Trigger
Use after implementing a feature, or when code feels bloated or over-engineered.

## Philosophy
LLMs tend to over-engineer. The best code is code that doesn't exist. Every line is a liability. Simpler code is easier to understand, test, debug, and maintain.

## Process

### Step 1: Question Every Abstraction
- Does this abstraction have more than one user?
- If not, inline it.
- Is this interface used by external code? If not, simplify it.
- Are there generic solutions for specific problems? Make them specific.

### Step 2: Count the Lines
Before and after. Set a target. If you have 500 lines, ask: "Can this be 200?"

### Step 3: Remove These Patterns

**Over-abstracted factories:**
```typescript
// Before: Factory that only creates one thing
class UserServiceFactory {
  create(): UserService { return new UserService(); }
}

// After: Just use the class directly
const userService = new UserService();
```

**Unnecessary interfaces:**
```typescript
// Before: Interface with single implementation
interface IUserRepository { ... }
class UserRepository implements IUserRepository { ... }

// After: Just the class (add interface when you need it)
class UserRepository { ... }
```

**Config objects for fixed values:**
```typescript
// Before
const config = { maxRetries: 3, timeout: 5000 };
function fetch(url: string, config: Config) { ... }

// After (if values never change)
function fetch(url: string) {
  const maxRetries = 3;
  const timeout = 5000;
  ...
}
```

**Premature error handling:**
```typescript
// Before: Handling impossible errors
try {
  const x = 1 + 1;
} catch (e) {
  // This can never throw
}

// After: Trust code that can't fail
const x = 1 + 1;
```

### Step 4: Flatten Hierarchies
- Deep nesting is a smell
- Early returns over nested ifs
- Composition over inheritance
- Inline small helper functions called once

### Step 5: Remove Dead Code
- Unused imports
- Unused variables
- Commented-out code
- Functions that are never called
- Branches that can never execute

## Output
After simplification, report:
- Lines before: X
- Lines after: Y
- Reduction: Z%
- Key simplifications made
