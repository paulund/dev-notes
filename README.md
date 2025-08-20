# Dev Notes

A curated, structured collection of personal developer knowledge: architecture decisions, design patterns, coding standards, deployment strategies, career notes, tools, and productivity tips. Think of it as a fast, searchable second brain you can evolve over time.

## Repository Structure

Top-level domains map to broad areas of practice:

| Path                      | Focus                                                                                                                     |
| ------------------------- | ------------------------------------------------------------------------------------------------------------------------- |
| `career/`                 | Career development, job search, contracting, certification notes                                                          |
| `coding/`                 | Core engineering topics (APIs, architecture, deployments, patterns, git, code quality, observability, language specifics) |
| `coding/apis/rest-api/`   | REST design conventions: methods, naming, versioning, etc.                                                                |
| `coding/architecture/`    | Diagrams, decisions, assets, architecture practices                                                                       |
| `coding/deployments/`     | Release strategies (blue/green, canary, feature toggles, IaC, CI/CD)                                                      |
| `coding/design-patterns/` | Classic patterns distilled (adapter, strategy, state, etc.)                                                               |
| `coding/git/`             | Workflow guidance: branching, hooks, PRs, commit messages                                                                 |
| `coding/good-code/`       | Principles: SOLID, DDD, 12-factor, YAGNI, value objects, cognitive load                                                   |
| `coding/laravel/`         | Laravel standards, Pint config, best practices                                                                            |
| `coding/observability/`   | Logging, monitoring, alerting, health checks                                                                              |
| `coding/php/`             | PHP standards, static analysis (PHPStan), Rector automation                                                               |
| `composer/`               | Composer scripts and related docs                                                                                         |
| `docker/`                 | Docker maintenance & cleanup                                                                                              |
| `gcp/`                    | Cloud provider specifics (e.g. GCP VM port forwarding)                                                                    |
| `github/`                 | GitHub automation, workflows, Dependabot, releases                                                                        |
| `tools/`                  | Productivity & business tool references                                                                                   |
| `vscode/`                 | Editor settings and recommended tweaks                                                                                    |

## Writing & Organising Notes

Principles:

1. Make each note answer ONE intent quickly.
2. Prefer concise bullet points over prose when clarity isn't lost.
3. Link related notes (relative markdown links) to build a knowledge graph.
4. Use examples (code / commands) minimally but concretely.
5. Keep vendor-neutral unless specifics add real value.

Conventions:

- File naming: `kebab-case.md` for readability in terminals.
- Group by concept before technology (e.g. `observability/logging.md`).
- Avoid duplication—link instead of copy/paste repeating concepts.
- If a concept evolves, add a small CHANGELOG section or reference an architecture decision.
- Use present tense, active voice.

## Adding a New Note

1. Pick or create the right folder (avoid deep nesting unless necessary).
2. Create `topic-name.md` with: purpose (1–2 lines), core principles, actionable steps, pitfalls, references.
3. Cross-link at least one related existing note.
4. (If code standard) reference supporting tools or configs in repo.
5. Commit with a conventional style (see `coding/git/commit-messages.md`).

Example skeleton:

```
# Feature Toggles
Purpose: Safely deploy incomplete features by decoupling deploy from release.

## When to Use
... concise bullets ...

## Implementation Tips
... code or config examples ...

## Risks / Mitigations
...

## Related
../deployments/blue-green.md
```
