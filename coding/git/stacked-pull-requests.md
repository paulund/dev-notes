# Stacked Pull Requests

Stacked pull requests are a workflow where a larger piece of work is split
into a linear series of small, reviewable branches. Each branch builds on (has its Git parent set to) the previous branch
instead of all branches pointing directly at `main`.

```
main ← feature/part-1 ← feature/part-2 ← feature/part-3
```

---

## Why Stack PRs Instead of One Big PR?

| Benefit                          | Description                                                                           |
| -------------------------------- | ------------------------------------------------------------------------------------- |
| Smaller cognitive load           | Reviewers focus on one cohesive change, increasing review speed & quality.            |
| Faster feedback loop             | You can incorporate feedback from early layers while still progressing on later ones. |
| Reduced merge conflicts          | Continuous merging & rebasing prevents a giant conflict resolution at the end.        |
| Clear dependency chain           | Each PR documents _why_ the next change is needed (narrative development).            |
| Safer deploys & easier rollbacks | A bad layer can be reverted without losing unrelated progress.                        |
| Parallel collaboration           | Teammates can review/QA early layers while you code later ones.                       |
| Higher merge velocity            | Small PRs usually meet team size guidelines and pass CI faster.                       |
| Better blame granularity         | Git history explains intent step by step.                                             |

---

## When To Use Stacked PRs

Use them when a task is:

- Large refactor touching many files
- Multi-step feature (schema change → backend support → API → UI)
- Risky migration where you want progressive rollout
- Exploratory work where earlier structural changes unlock later polish

Avoid stacking when:

- The change is already small
- Branch order is unclear / interdependent in _both_ directions (consider isolating responsibilities first)
- The team / tooling can’t easily visualise the chain (educate first)

---

## Core Principles

1. Each PR must deliver _one_ reviewable, internally coherent improvement (narrow scope).
2. The diff should only show what is new relative to the previous layer (keep branches clean; rebase as needed).
3. Keep the narrative flowing: description of a PR should reference the prior one and preview the next.
4. CI must stay green at every layer (no broken intermediate states).
5. Do not “sneak” unrelated fixes into a later stack layer—create a side PR off `main` if independent.

---

## Branch Naming Tips

Use a common prefix so IDE / Git hosting sorts them together:

- `feature/payments/01-models`
- `feature/payments/02-service`
- `feature/payments/03-endpoints`
- `feature/payments/04-ui`

Optionally number them to make ordering obvious.
