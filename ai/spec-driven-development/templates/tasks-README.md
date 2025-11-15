# Tasks Directory

Task specifications for spec-driven development. All tasks live in this directory with a **Status** property to track progress.

## Quick Start

```bash
# 1. Create new task
cp path/to/task-template.md tasks/feature-name.md

# 2. Fill in: Description, Requirements, Acceptance Criteria
# 3. Ask AI: "Read tasks/feature-name.md and create an implementation plan"
# 4. Review plan, approve or iterate
# 5. Update Status as you progress: Pending → In Progress → Review → Completed
```

## Task Statuses

- **Pending** - Spec written, awaiting planning/approval
- **In Progress** - Actively implementing
- **Review** - Awaiting code review
- **Completed** - Done (add completion date)

## Naming Convention

Use kebab-case: `add-email-verification.md`, `fix-payment-timeout.md`

## Best Practices

**Writing Specs:**

- Be specific with examples
- List edge cases
- Clear acceptance criteria
- Explain the "why"

**Working with AI:**

- Review plans thoroughly
- Give explicit approval
- Update instruction files when issues arise
- Iterate as needed

**Task Management:**

- Update status regularly
- One feature per spec
- Link PRs/commits in task
- Archive completed tasks (optional)

## Troubleshooting

| Problem                      | Solution                                           |
| ---------------------------- | -------------------------------------------------- |
| AI didn't follow conventions | Check instruction files, add examples              |
| Plan incomplete              | Provide feedback, don't approve until complete     |
| Too much back-and-forth      | Make specs more detailed, break into smaller tasks |
| Specs take too long          | Use template, fill only relevant sections          |

---

See [Spec-Driven Development Guide](../spec-driven-development.md) for full details.
