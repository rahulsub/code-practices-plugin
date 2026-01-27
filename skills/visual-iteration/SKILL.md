---
name: visual-iteration
description: UI feedback loop using screenshots. Provide design mock, implement, screenshot result, iterate until matching. Use for visual work.
---

# Visual Iteration Skill

## Trigger
Use when implementing UI components, layouts, or any visual work where "looks right" matters.

## The Insight
From Anthropic's Claude Code best practices: "Provide design mocks or screenshots. Have Claude implement, screenshot results, and iterate until matching the target. This gives Claude concrete feedback for refinement."

## Why This Works
- Visual feedback is unambiguous - it either matches or it doesn't
- Faster iteration than describing visual problems in words
- Claude can see exactly what needs adjustment
- Creates tight feedback loop for rapid convergence

## The Workflow

### Step 1: Provide the Target
Give Claude a clear visual reference:

```
Here's the design mock for the new dashboard header:
[drag and drop image or provide path]

Implement this in React with Tailwind CSS.
```

Ways to provide visual targets:
- Drag and drop images into the chat
- Provide file paths to images
- Share Figma export screenshots
- Use URLs to design references

### Step 2: Initial Implementation
Claude implements based on the visual reference.

### Step 3: Screenshot the Result
Capture what Claude built:

```bash
# Take screenshot of the running app
# On macOS: Cmd+Shift+4 to select area
# Or use browser DevTools device mode
```

### Step 4: Compare and Iterate
Share the screenshot back with Claude:

```
Here's what it looks like now:
[screenshot of current implementation]

Compare to the original design. The spacing between cards is too tight
and the shadow is too harsh. Adjust to match the design more closely.
```

### Step 5: Repeat Until Matching
Continue the loop:
1. Claude adjusts
2. You screenshot
3. You compare
4. You provide feedback
5. Repeat until satisfied

## Effective Feedback Examples

**Vague (less effective):**
> "It doesn't look right"

**Specific (more effective):**
> "The button is 20px too far from the edge. The font weight should be semibold not bold. The border radius should match the card above it."

**With visual reference:**
> "See how in the design the cards have subtle shadows? Current implementation has no shadow. Add `shadow-sm` to match."

## Template for Visual Tasks

```markdown
## Visual Implementation Task

### Target Design
[Image: design mock or reference screenshot]

### Technical Requirements
- Framework: [React/Vue/etc]
- Styling: [Tailwind/CSS modules/etc]
- Responsive: [breakpoints to support]

### Key Visual Elements
- [ ] [Element 1 - specific requirements]
- [ ] [Element 2 - specific requirements]
- [ ] [Color/spacing tokens to use]

### Iteration Process
1. Implement initial version
2. I'll screenshot and provide feedback
3. Iterate until matching design
```

## Tips for Better Visual Iteration

### Provide Context
- Include surrounding UI elements in screenshots
- Show both light and dark mode if applicable
- Include different viewport sizes

### Be Specific About Differences
- "Too much padding" → "Reduce padding from 24px to 16px"
- "Wrong color" → "Use gray-600 instead of gray-500"
- "Alignment off" → "Left edge should align with the header above"

### Use Browser DevTools
- Inspect computed styles to give exact values
- Use device mode for responsive testing
- Screenshot specific breakpoints

## When to Use This Skill
- Implementing designs from Figma/Sketch
- Matching existing UI patterns
- Fixing visual bugs
- Building responsive layouts
- Polishing UI details before release
