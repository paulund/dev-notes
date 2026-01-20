# CLAUDE.md File

Project instructions in Claude work similarly to GitHub Copilot's instructions files. You create a *CLAUDE.md* file (or language-specific variants like php.instructions.md, typescript.instructions.md) and check it into your repository's root.

Claude automatically reads and applies these instructions when working within your project. The CLAUDE.md file can include:

- [ ] Clear project overview
- [ ] Complete tech stack list
- [ ] Project structure explanation
- [ ] Coding conventions and standards
- [ ] Language and localisation preferences
- [ ] Development workflow commands
- [ ] Integration points documented
- [ ] Claude-specific instructions
- [ ] Known issues and technical debt
- [ ] Environment setup requirements
- [ ] Concise and actionable content
- [ ] Regular update schedule planned
- [ ] Referenced detailed documentation where needed
- [ ] Examples for complex conventions
- [ ] No sensitive information included

These instructions override default Claude behavior and ensure that Claude works consistently with your project's conventions and requirements. Language-specific instruction files allow you to provide different guidance for different parts of your codebase.

This is an iterative process, regularly update *CLAUDE.md* as your project evolves to keep Claude aligned with your current practices.

## Claude Terms

- *Instructions* define your project's standards
- *Agents* do the autonomous work
- *Skills* are the tools they use
- *Commands* are shortcuts you invoke,
- *Models* provide the reasoning capability.

Together, they create a cohesive development workflow tailored to your specific project needs.

## Why You Need CLAUDE.md

Without `CLAUDE.md`, Claude Code starts each session with no memory of your previous instructions. You'll find yourself repeatedly explaining:
- Your coding style preferences
- How to run tests or build the project
- Which libraries or patterns to use
- Project-specific terminology
- Known issues or gotchas

A well-crafted `CLAUDE.md` solves these problems by providing:

- **Consistency**: Ensures all Claude Code agents follow the same coding standards and practices
- **Efficiency**: Eliminates the need to repeat project context in every session
- **Quality**: Reduces errors by providing clear guidance on project-specific conventions
- **Team Alignment**: Serves as living documentation for development practices
- **Context Preservation**: Claude Code reads this file at the start of every session
- **Onboarding**: Helps new team members understand project conventions
