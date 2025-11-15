# GitHub Copilot Instructions Template

This is a reusable template for creating project-specific GitHub Copilot instructions. Copy this file to your project's `.github/copilot-instructions.md` and customize it for your specific needs.

---

## Project Overview

[Provide a brief description of what this project does, its purpose, and main functionality]

**Project Type:** [e.g., Laravel API, React SPA, Node.js Microservice, Python CLI Tool]

**Tech Stack:**

- [Primary Language/Framework]
- [Database]
- [Key Libraries/Dependencies]
- [Other relevant technologies]

## Architecture & Structure

[Describe the high-level architecture and how the project is organized]

### Directory Structure

```
[Describe key directories and their purposes]
/src
  /app
  /config
  /tests
```

### Design Patterns

[List any specific design patterns used in this project]

- [e.g., Repository Pattern, Service Layer, CQRS, etc.]

## Code Style & Standards

### Project-Specific Conventions

#### Naming Conventions

[Any project-specific naming conventions beyond language standards]

- [e.g., API endpoints should be plural: `/users`, not `/user`]
- [e.g., Feature branches: `feature/TICKET-123-description`]

#### Code Organization

[How code should be organized in this specific project]

- [e.g., All business logic goes in Actions under `app/Actions`]
- [e.g., API resources go in `app/Http/Resources`]

## Testing Requirements

### Test Coverage

[Define expected test coverage and types]

- Unit tests required for: [specify what needs unit tests]
- Feature/Integration tests for: [specify what needs integration tests]
- E2E tests for: [critical user flows]

### Test Naming

[Project-specific test naming conventions]

```php
// Example format
test_[action]_[scenario]_[expected_result]
```

### Mocking Strategy

[How to handle mocks and test doubles]

- [When to mock external services]
- [Test data generation approach]

## API Design

### REST API Conventions

[If applicable, define API standards]

- **Versioning**: [e.g., `/api/v1/`]
- **Authentication**: [e.g., Bearer token in Authorization header]
- **Response Format**: [Standard response structure]
- **Error Handling**: [Error response format]

## Database

### Migration Guidelines

[How to handle database changes]

- [When to create new migrations vs. modifying existing]
- [Naming conventions for migrations]
- [How to handle data migrations]

### Query Optimization

[Project-specific database considerations]

- [Indexing strategy]
- [Query performance requirements]
- [Caching approach]

## Security Requirements

### Authentication & Authorization

[How auth is handled in this project]

- [Authentication method]
- [Authorization/permission system]
- [Session management]

### Data Validation

[Validation requirements]

- [Input sanitization rules]
- [Output encoding]
- [File upload restrictions]

### Sensitive Data

[How to handle sensitive information]

- [Never commit: API keys, passwords, tokens]
- [Use environment variables for: ...]
- [Secrets management approach]

## Error Handling & Logging

### Error Handling Strategy

[How errors should be handled]

- [Exception hierarchy]
- [Error recovery approach]
- [User-facing error messages]

### Logging Standards

[What and how to log]

- **Info Level**: [What to log at info level]
- **Warning Level**: [What to log at warning level]
- **Error Level**: [What to log at error level]
- **Structured Logging**: [Format and required fields]

## Performance Considerations

### Optimization Requirements

[Performance standards and optimization strategies]

- [Response time targets]
- [Memory usage constraints]
- [Scalability considerations]

### Caching Strategy

[How caching is implemented]

- [What to cache]
- [Cache invalidation approach]
- [TTL standards]

## Documentation

### Code Documentation

[Documentation requirements]

- [When to write docblocks/comments]
- [Documentation format]
- [What needs inline explanation]

### API Documentation

[How API is documented]

- [Tool used: OpenAPI/Swagger, Postman, etc.]
- [Documentation location]
- [Update requirements]

## Git Workflow

### Branch Strategy

[Branching model used]

- `main`: [Purpose and protection rules]
- `develop`: [If using Gitflow]
- `feature/*`: [Feature branch conventions]
- `hotfix/*`: [Hotfix branch conventions]

### Commit Messages

[Commit message format]

```
[type]([scope]): [subject]

[body]

[footer]
```

Types: feat, fix, docs, style, refactor, test, chore

### Pull Request Requirements

[What's needed for PR approval]

- [ ] Tests added/updated
- [ ] Documentation updated
- [ ] Code review completed
- [ ] CI/CD passes
- [ ] [Other requirements]

## CI/CD Pipeline

### Continuous Integration

[What runs on PR/push]

- [Linting checks]
- [Test execution]
- [Code quality checks]
- [Security scans]

### Deployment Process

[How deployments work]

- [Deployment environments]
- [Deployment triggers]
- [Rollback procedure]

## Development Environment

### Setup Requirements

[How to set up the project locally]

- [Prerequisites]
- [Installation steps]
- [Environment configuration]

### Local Development

[Development workflow]

- [How to run locally]
- [How to run tests]
- [How to debug]
- [Common issues and solutions]

## Project-Specific Rules

### DO's

- [Specific things to always do in this project]
- [Required patterns or approaches]
- [Mandatory checks or validations]

### DON'Ts

- [Things to avoid in this project]
- [Anti-patterns specific to this codebase]
- [Legacy code patterns being phased out]

## Business Logic & Domain

### Domain Concepts

[Key business concepts and terminology]

- **[Term]**: [Definition and usage]
- **[Term]**: [Definition and usage]

### Business Rules

[Important business rules that code must enforce]

- [Rule 1]
- [Rule 2]

## Third-Party Integrations

### [Service Name]

- **Purpose**: [Why we use it]
- **Documentation**: [Link to docs]
- **Implementation Notes**: [Key considerations]
- **Error Handling**: [How to handle service failures]

## Monitoring & Observability

### Metrics

[What metrics are tracked]

- [Key performance indicators]
- [Business metrics]
- [Technical metrics]

### Alerting

[What triggers alerts]

- [Critical alerts]
- [Warning conditions]
- [On-call procedures]

### Health Checks

[How health is monitored]

- [Health check endpoints]
- [What to check]
- [Response format]

## Common Tasks

### Adding a New Feature

[Step-by-step guide for common feature addition]

1. [Step 1]
2. [Step 2]
3. [Step 3]

### Troubleshooting

[Common issues and solutions]

- **Issue**: [Description]
  **Solution**: [How to resolve]

## Resources

### Documentation

- [Link to main documentation]
- [Link to API docs]
- [Link to architecture docs]

### Tools

- [Development tools used]
- [Required IDE extensions]
- [Helpful CLI commands]

### Team Contacts

- [Technical Lead]: [Contact method]
- [DevOps]: [Contact method]
- [Other key contacts]
