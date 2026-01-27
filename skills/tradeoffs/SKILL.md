---
name: tradeoffs
description: Present multiple approaches with explicit tradeoffs before committing to one. Use when there are multiple valid solutions to a problem.
---

# Present Tradeoffs Skill

## Trigger
Use when there are multiple valid approaches to a problem. Force explicit consideration of alternatives.

## The Problem
Karpathy: "They don't present tradeoffs, they don't push back when they should."

LLMs pick an approach and run with it. This skill forces consideration of alternatives and their tradeoffs.

## Process

### Step 1: Identify the Decision Point
Before implementing, ask: "Is there more than one reasonable way to do this?"

If yes, stop and enumerate the options.

### Step 2: List All Viable Options
Don't filter yet. List every approach that could work:

```markdown
## Options for User Session Storage

### Option A: JWT in localStorage
### Option B: JWT in httpOnly cookie
### Option C: Server-side sessions with Redis
### Option D: Server-side sessions with database
```

### Step 3: Evaluate Each Option

For each option, consider:

| Criterion | Option A | Option B | Option C | Option D |
|-----------|----------|----------|----------|----------|
| Security | XSS vulnerable | CSRF vulnerable | Most secure | Secure |
| Scalability | Excellent | Excellent | Needs Redis | DB load |
| Complexity | Simple | Medium | Medium | Simple |
| Stateless | Yes | Yes | No | No |
| Mobile support | Easy | Hard | Easy | Easy |

### Step 4: Make Tradeoffs Explicit

```markdown
## Recommendation: Option B (JWT in httpOnly cookie)

### Why
- Best balance of security and simplicity
- Immune to XSS attacks (main threat vector)
- CSRF mitigated with SameSite=Strict

### What We Give Up
- Slightly more complex mobile integration (need to handle cookies)
- Token can't be easily accessed from JavaScript (intentional)
- Slightly larger request payload (cookie sent with every request)

### What We Gain
- Automatic CSRF protection with SameSite
- No JavaScript access = immune to XSS token theft
- Built-in expiration handling

### Alternative Considered
Option C (Redis sessions) would be more secure but adds infrastructure complexity. Recommended if we later need session revocation.
```

### Step 5: Get Explicit Approval
Present the tradeoffs to stakeholders. Don't assume the "obvious" choice is correct.

## Tradeoff Categories to Consider

### Performance vs Simplicity
- Caching adds speed but complexity
- Denormalization speeds reads but complicates writes
- Premature optimization is the root of all evil

### Security vs Usability
- Shorter sessions are safer but annoy users
- 2FA is more secure but adds friction
- Strict validation catches attacks but may reject valid input

### Flexibility vs Complexity
- Generic solutions handle more cases but are harder to understand
- Configuration options add flexibility but increase surface area
- Abstractions enable change but obscure behavior

### Consistency vs Availability (CAP)
- Strong consistency means potential unavailability
- Eventual consistency means potential stale reads
- There is no free lunch

### Build vs Buy
- Building gives control but costs time
- Buying saves time but adds dependencies
- Open source is "free" but still has maintenance cost

## Template

```markdown
## Decision: [What needs to be decided]

### Context
[Why this decision matters]

### Options Considered

#### Option A: [Name]
**Pros:** [List]
**Cons:** [List]
**Best when:** [Conditions]

#### Option B: [Name]
**Pros:** [List]
**Cons:** [List]
**Best when:** [Conditions]

### Recommendation
[Which option and why]

### Tradeoffs Accepted
[What we're giving up]

### Revisit If
[Conditions that would change this decision]
```
