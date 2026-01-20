# What Are Sub Agents?

Sub-agents (or simply agents) are specialized autonomous workers within Claude Code that handle specific types of tasks independently. Each agent type has particular capabilities and access to specific tools tailored for its purpose.

Common agent types include:

- Bash Agent - Executes shell commands and performs terminal operations
- Explore Agent - Searches and analyzes codebases to understand structure and find information
- Research Agent - Gathers information from external sources, APIs, or documentation
- Plan Agent - Designs implementation strategies and architectural approaches
- Test Runner - Executes tests and validates code
- Build Validator - Compiles and validates build processes

Agents run autonomously, meaning they execute tasks without requiring user interaction at each step. They're particularly useful for complex, multi-step tasks that would be tedious to do manually through sequential tool calls.
