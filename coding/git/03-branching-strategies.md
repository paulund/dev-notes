# Branching Strategies (Index)

This section groups notes and examples about different Git branching strategies. See the `branching-strategies/` folder for detailed breakdowns or diagrams.

Common strategies you might document:

- Git Flow (long‑lived `develop` + release/hotfix branches)
- Trunk Based Development (short‑lived branches, fast merges, feature flags)
- Github Flow (single main branch + small PRs)
- Release Branching (cut a branch per release train)
- Environment Branches (rarely recommended)

When choosing a strategy consider:

- Deployment frequency
- Team size / parallel work
- Need for emergency fixes
- Release stability / long term support
- Tooling & CI maturity

Recommendation: start simple (Github Flow or light Trunk Based) and evolve only when a concrete pain appears.
