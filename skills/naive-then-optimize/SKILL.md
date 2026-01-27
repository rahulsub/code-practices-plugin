---
name: naive-then-optimize
description: Implement the obvious correct solution first, then optimize while preserving correctness. Use for algorithms, data transformations, and critical code paths.
---

# Naive-Then-Optimize Skill

## Trigger
Use when implementing algorithms, data transformations, or any code where correctness is critical.

## The Insight
Karpathy: "Write the naive algorithm that is very likely correct first, then ask it to optimize it while preserving correctness."

## Why This Works
1. Naive implementations are easier to verify as correct
2. They serve as a reference/oracle for testing optimized versions
3. Optimization bugs are caught by comparing to naive version
4. You might find the naive version is fast enough

## Process

### Step 1: Implement the Obvious Solution
Don't think about performance. Think about correctness. Write the dumbest, most straightforward code that works.

```typescript
// Naive: O(n²) but obviously correct
function findDuplicates(arr: number[]): number[] {
  const duplicates: number[] = [];
  for (let i = 0; i < arr.length; i++) {
    for (let j = i + 1; j < arr.length; j++) {
      if (arr[i] === arr[j] && !duplicates.includes(arr[i])) {
        duplicates.push(arr[i]);
      }
    }
  }
  return duplicates;
}
```

### Step 2: Write Tests Against Naive Implementation
```typescript
describe('findDuplicates', () => {
  it('finds duplicates', () => {
    expect(findDuplicates([1, 2, 2, 3])).toEqual([2]);
  });
  it('handles multiple duplicates', () => {
    expect(findDuplicates([1, 1, 2, 2, 3])).toEqual([1, 2]);
  });
  it('handles no duplicates', () => {
    expect(findDuplicates([1, 2, 3])).toEqual([]);
  });
  it('handles empty array', () => {
    expect(findDuplicates([])).toEqual([]);
  });
});
```

### Step 3: Verify Naive Implementation Passes
Run the tests. They should all pass. If they don't, fix the naive implementation first.

### Step 4: Measure Performance
Is the naive version fast enough? Profile it with realistic data.

```typescript
console.time('naive');
findDuplicates(largeArray);
console.timeEnd('naive');
```

If it's fast enough, **stop here**. Don't optimize code that doesn't need it.

### Step 5: Optimize While Preserving Behavior
Now optimize, but keep the same tests passing:

```typescript
// Optimized: O(n) using a Set
function findDuplicates(arr: number[]): number[] {
  const seen = new Set<number>();
  const duplicates = new Set<number>();
  for (const num of arr) {
    if (seen.has(num)) {
      duplicates.add(num);
    }
    seen.add(num);
  }
  return Array.from(duplicates);
}
```

### Step 6: Property-Based Testing (Optional)
For critical code, test that naive and optimized produce same results:

```typescript
it('optimized matches naive for random inputs', () => {
  for (let i = 0; i < 1000; i++) {
    const input = generateRandomArray();
    const naiveResult = findDuplicatesNaive(input);
    const optimizedResult = findDuplicates(input);
    expect(optimizedResult.sort()).toEqual(naiveResult.sort());
  }
});
```

## When to Use This
- Algorithm implementations
- Data transformations
- Parsers
- Anything where correctness matters more than cleverness

## When to Skip This
- Simple CRUD operations
- Code that's obviously correct
- When the naive version would be identical to the optimized version
