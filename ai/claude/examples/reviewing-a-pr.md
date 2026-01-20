# Reviewing a Pull Request

What you do: Run /review-pr 123

What happens:
1. The /review-pr command is invoked, which loads project instructions from CLAUDE.md
2. The instructions define your code review standards (testing requirements, commit message format, performance considerations)
3. A general-purpose agent is dispatched to analyze the PR
4. The agent uses skills like code analysis and git operations to:
  - Fetch the PR details
  - Compare changes against the base branch
  - Run static analysis
  - Check for compliance with project standards
5. The agent may invoke the Opus model (high capability) for complex reasoning about code quality
6. Results are returned with specific feedback based on your project's standards
