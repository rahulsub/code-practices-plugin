# Code Practices Plugin for Claude Code

A collection of coding best practices for LLM-assisted development. These skills help you and your team write better code with Claude Code by applying proven patterns from Karpathy's insights and Anthropic's official best practices.

## Installation

### From GitHub
```bash
/plugin marketplace add rahulsub/code-practices-plugin
```

### Local Development
```bash
/plugin marketplace add ~/code-practices-plugin
```

## Skills Included

### Development Workflow
| Skill | Command | Description |
|-------|---------|-------------|
| **Explore-Plan-Code** | `/explore-plan-code` | Read files → plan → approve → implement workflow |
| **Goal-Driven** | `/goal-driven` | Define success criteria instead of step-by-step instructions |
| **Test-First** | `/test-first` | Write failing tests before implementation |
| **Naive-Then-Optimize** | `/naive-then-optimize` | Implement obvious solution first, then optimize |

### Code Quality
| Skill | Command | Description |
|-------|---------|-------------|
| **Code-Review** | `/code-review` | Review code like a skeptical senior engineer |
| **Simplify** | `/simplify` | Remove over-engineering and unnecessary abstractions |
| **Cleanup** | `/cleanup` | Systematically remove dead code and tech debt |
| **Surface-Assumptions** | `/surface-assumptions` | Identify and validate hidden assumptions |
| **Tradeoffs** | `/tradeoffs` | Present multiple approaches with explicit tradeoffs |

### Advanced Workflows
| Skill | Command | Description |
|-------|---------|-------------|
| **Visual-Iteration** | `/visual-iteration` | UI feedback loop with screenshots |
| **Context-Management** | `/context-management` | Manage long sessions with /clear, checklists, subagents |
| **Multi-Claude** | `/multi-claude` | Run parallel Claude instances for writer+reviewer patterns |
| **Headless-Automation** | `/headless-automation` | CI/CD integration, fan-out migrations, pipelines |

## Usage Examples

### Before implementing a feature
```
/explore-plan-code
```
Claude will read relevant files, create a plan, and wait for your approval before coding.

### After writing code
```
/code-review
```
Review the changes for hidden assumptions, over-engineering, and edge cases.

### For complex algorithms
```
/naive-then-optimize
```
Implement the obvious correct solution first, then optimize while preserving behavior.

## Attribution

These practices are based on:
- [Karpathy's insights on LLM-assisted coding](https://karpathy.ai)
- [Anthropic's Claude Code Best Practices](https://www.anthropic.com/engineering/claude-code-best-practices)

## License

MIT
