# Researching a Feature Implementation

What you do: Ask Claude "How should we implement real-time notifications?"

What happens:
1. Claude reads your CLAUDE.md instructions and learns about your tech stack, existing patterns, and architectural constraints
2. An Explore agent is dispatched to search your codebase for:
 - Existing notification systems
 - WebSocket or server-sent event implementations
 - Message queue integrations
3. A Plan agent uses the skills it discovered to design an implementation approach that fits your existing patterns
4. The agent may use the Sonnet model for balanced capability and speed
5. It presents a plan showing which files need changes, what patterns to follow (based on instructions), and design decisions
6. You approve the plan, and the agent proceeds with implementation
