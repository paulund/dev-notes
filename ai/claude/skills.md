# What Are Claude Skills?

Claude skills are capabilities and tools that agents can access to accomplish their work. They're part of the agent framework and enable agents to perform specialized operations within their domain.

Skills available to agents include:
- File and code manipulation
- Terminal execution
- Testing and validation
- Web fetching and searching
- Code analysis and exploration
- Debugging and diagnostics

Rather than being user-invoked directly, skills are automatically used by agents as needed to complete assigned tasks. When you dispatch an agent to accomplish something, it determines which skills are appropriate and uses them autonomously.

## Claude Instructions In Skills

One of the main differences between Claude and Github Copilot is how they handle project instructions. In Claude, project instructions are stored in a *CLAUDE.md* file at the root of your repository. Claude automatically reads and applies these instructions when working within your project. The problem with this is if you have a large monolithic repository with multiple services or components, a single CLAUDE.md file may be populated with different language standards.

Unlike Github Copilot where you can have language specific instructions in files like *php.instructions.md* or *typescript.instructions.md*, Claude does not currently support this feature.

To work around this limitation, you can create Claude skills that encapsulate the instructions for specific parts of your codebase.

For example, you could create a "PHP Expert Skill" that contains all the PHP-specific instructions and coding standards. When an agent is working on PHP code, it can leverage the PHP Expert Skill to ensure it follows the correct conventions. Then create a separate "TypeScript Expert Skill" for TypeScript code. This skills can store all the relevant instructions and standards for that language. This will keep your *CLAUDE.md* file cleaner and more focused on high-level project instructions, while the skills handle the language-specific details.

## Learn How to Create Claude Skills

Here is a helpful guide on creating Claude skills from scratch: [Guide To Agent Skills](https://www.youtube.com/watch?v=fabAI1OKKww)

## Resources
- [Claude Skills Documentation](https://claude.ai/docs/skills)
- [Using Skills with Agents](https://claude.ai/docs/agents/using-skills)
- [Awesome Claude Skills](https://github.com/ComposioHQ/awesome-claude-skills)
- [More Awesome Claude Skills](https://github.com/travisvn/awesome-claude-skills)
- [Even More Awesome Claude Skills](https://awesomeclaude.ai/awesome-claude-skills)
