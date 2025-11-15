# Spec-Driven Development with AI

A systematic approach to building software by starting with detailed specifications and leveraging AI for implementation. The specification becomes the source of truth, and AI assists with code generation following your project's standards.

## The Workflow

### 1. Write the Specification

Create a task spec in `tasks/[feature-name].md` including:

- Requirements and acceptance criteria
- Technical constraints
- Edge cases

### 2. AI Planning

Request a plan from AI:

```
Read tasks/[task-name].md and create an implementation plan
following .github/copilot-instructions.md
```

AI will analyze the spec, apply your coding standards, and create a step-by-step plan.

### 3. Review and Approve

Review the plan:

- ✅ **Approve**: "The plan looks good. Please proceed."
- ❌ **Iterate**: Provide feedback, update instruction files, request revision

### 4. Implementation

AI implements the feature following the approved plan and your project standards.

### 5. Continuous Improvement

When issues arise:

1. Identify the pattern that was missed
2. Update the relevant instruction file
3. Prevent future occurrences

## Project Structure

```
your-project/
├── .github/
│   ├── copilot-instructions.md          # Main project guidelines
│   └── instructions/
│       ├── php.instructions.md          # Language-specific rules
│       └── laravel.instructions.md      # Framework rules
├── tasks/
│   └── [task-name].md                   # Task specs (all in one folder)
└── [application code]
```

Tasks use a **Status** property instead of separate folders.

## Task Template

Your spec only needs the essentials - AI will plan the rest:

```markdown
# Task: [Feature Name]

**Date:** YYYY-MM-DD
**Status:** Pending

## Description

[Clear description of what needs to be built and why]

## Requirements

1. [Specific requirement]
2. [Another requirement]

## Acceptance Criteria

- [ ] [Testable success criterion]
- [ ] [Another criterion]

## Additional Context (optional)

- Technical constraints: [Any limitations]
- Dependencies: [Related features/libraries]
- Edge cases: [Known edge cases to handle]

---

## AI Planning Section

_AI will fill in the implementation plan below after reviewing the spec above_

### Proposed Approach

[AI fills this]

### Files to Create/Modify

[AI fills this]

### Implementation Steps

[AI fills this]

### Testing Strategy

[AI fills this]
```

## Best Practices

**Specifications:**

- Be specific with examples
- List edge cases explicitly
- Define clear acceptance criteria

**Working with AI:**

- Always review plans before implementation
- Update instruction files when issues arise
- Give explicit approval to proceed

**Instruction Files:**

- Keep them current and specific
- Show good/bad code examples
- Organize by scope (language → framework → project)

**Task Management:**

- Update status in task file as work progresses
- Keep all tasks in `tasks/` folder
- Optionally archive completed tasks to `tasks/archive/`

## Common Pitfalls

| Problem                        | Solution                          |
| ------------------------------ | --------------------------------- |
| Over-specifying implementation | Focus on requirements, not "how"  |
| Under-specifying requirements  | Include examples and edge cases   |
| Skipping plan review           | Always review before implementing |
| Stale instructions             | Update when patterns are missed   |

## Why It Works

- **Clarity**: Specs serve as living documentation
- **Consistency**: AI follows your project patterns
- **Learning**: Instruction files evolve with your project
- **Efficiency**: Review code instead of writing boilerplate
- **Quality**: Planning catches issues early

## The Key Loop

```
Spec → Plan → Review → Implement → Learn → Update Instructions → Repeat
```

Each cycle makes AI more effective for your specific project.

---

## Resources

- Full task template: `templates/task-template.md`
- Setup guide: `templates/project-setup-guide.md`
- Tasks README: `templates/tasks-README.md`
- [GitHub Copilot Docs](https://docs.github.com/en/copilot)
