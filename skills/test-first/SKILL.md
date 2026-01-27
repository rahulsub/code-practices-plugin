---
name: test-first
description: Write failing tests before implementation. Enables LLM self-correction through feedback loops. Use for new features and bug fixes.
---

# Test-First Development Skill

## Trigger
Use when implementing new features or fixing bugs. Especially valuable for complex logic.

## Why Test-First with LLMs
Karpathy insight: "Get it to write tests first and then pass them." Tests create a feedback loop that lets LLMs self-correct. Tests are success criteria that enable autonomous iteration.

## Process

### Step 1: Understand the Requirement
Before writing any code:
- What is the input?
- What is the expected output?
- What are the edge cases?
- What should happen on errors?

### Step 2: Write Failing Tests First
```typescript
describe('calculateDiscount', () => {
  it('returns 0 for orders under $50', () => {
    expect(calculateDiscount(49)).toBe(0);
  });

  it('returns 10% for orders $50-$100', () => {
    expect(calculateDiscount(50)).toBe(5);
    expect(calculateDiscount(100)).toBe(10);
  });

  it('returns 20% for orders over $100', () => {
    expect(calculateDiscount(101)).toBe(20.2);
  });

  it('handles zero', () => {
    expect(calculateDiscount(0)).toBe(0);
  });

  it('throws on negative values', () => {
    expect(() => calculateDiscount(-1)).toThrow();
  });
});
```

### Step 3: Run Tests (They Should Fail)
Verify tests fail for the right reasons. This confirms tests are actually testing something.

### Step 4: Implement Minimal Code to Pass
Don't over-engineer. Write the simplest code that makes tests green.

### Step 5: Refactor if Needed
Now that tests pass, clean up the code while keeping tests green.

## Test Quality Checklist
- [ ] Tests verify behavior, not implementation
- [ ] Each test tests one thing
- [ ] Tests are independent (can run in any order)
- [ ] Test names describe expected behavior
- [ ] Edge cases are covered
- [ ] Error cases are covered
- [ ] Tests run fast

## Anti-Patterns to Avoid
- Tests that just check code runs without asserting behavior
- Tests that duplicate implementation logic
- Tests with many assertions (split them)
- Tests that depend on other tests
- Mocking everything (test real behavior when possible)

## The Loop
```
Write test → Run (should fail) → Implement → Run (should pass) → Refactor → Run (should still pass)
```

## Verify with Independent Subagents
From Anthropic's Claude Code best practices: Use independent subagents to verify implementations aren't overfitting to tests.

After implementation passes tests, spawn a separate verification:

```
Use a subagent to review the implementation of calculateDiscount.
Verify it handles the actual business requirements, not just the test cases.
Check for:
- Edge cases not covered by tests
- Hardcoded values that only work for test inputs
- Logic that's correct by accident
```

### Why This Matters
LLMs can "overfit" to test cases - writing code that passes the specific tests but fails on real-world inputs. An independent reviewer catches:

- Magic numbers that match test values
- Conditional logic that only handles test scenarios
- Missing validation that tests don't exercise

### Example of Overfitting
```typescript
// Tests use ids 1, 2, 3
// BAD: Overfitting to test data
function getUser(id: number) {
  if (id === 1) return { name: "Alice" };
  if (id === 2) return { name: "Bob" };
  return null;
}

// GOOD: Actual implementation
function getUser(id: number) {
  return database.users.find(u => u.id === id);
}
```

The subagent review catches these patterns before they reach production.
