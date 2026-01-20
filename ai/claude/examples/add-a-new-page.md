# Add A New Page

Adding a New Page to Your Application

What you do: "Add a user dashboard page following the existing patterns"

What happens:
1. Claude loads your CLAUDE.md instructions which specify:
 - Your folder structure (pages/, components/, styles/)
 - Component patterns and naming conventions
 - State management approach
 - Testing requirements
2. An Explore agent runs to understand existing page implementations
3. A Plan agent designs the implementation based on discovered patterns
4. You approve the plan, and agents execute the implementation using skills like:
 - File creation and editing
 - Code generation
 - Testing execution
5. Once complete, you run the /commit command to create a properly formatted commit
 - The command respects commit conventions defined in your instructions
 - It may automatically reference relevant issues or tickets

The model choice adapts to complexity: simpler additions use Haiku for speed, while complex architectural decisions use Opus for reasoning.
