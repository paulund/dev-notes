# PHPStan: Advanced Static Analysis for PHP

PHPStan is a powerful static analysis tool for PHP that finds bugs in your code without running it. It performs deep analysis of your codebase to catch errors, enforce type safety, and improve code quality before your code reaches production.

## Table of Contents

1. [What is PHPStan?](#what-is-phpstan)
2. [Why Use PHPStan?](#why-use-phpstan)
3. [Installation](#installation)
4. [Configuration Explained](#configuration-explained)
5. [Analysis Levels](#analysis-levels)
6. [Common Use Cases](#common-use-cases)
7. [Running PHPStan](#running-phpstan)
8. [Best Practices](#best-practices)
9. [Advanced Configuration](#advanced-configuration)

## What is PHPStan?

PHPStan is a static analysis tool that analyzes your PHP code without executing it. It:

-   **Detects bugs** before they reach production
-   **Enforces type safety** through comprehensive type checking
-   **Validates code logic** and identifies unreachable code
-   **Analyzes method calls** and property access
-   **Checks PHPDoc annotations** for accuracy

PHPStan understands your code structure and can identify issues that traditional testing might miss.

## Why Use PHPStan?

### 1. **Early Bug Detection**

Catches errors before they cause runtime failures:

-   Undefined variables and methods
-   Type mismatches and incompatibilities
-   Logic errors and unreachable code
-   Missing return statements

### 2. **Type Safety Enforcement**

Ensures your code respects type contracts:

-   Validates method signatures
-   Checks parameter and return types
-   Identifies nullable type violations
-   Enforces strict type declarations

### 3. **Code Quality Improvement**

Promotes better coding practices:

-   Identifies unused code and variables
-   Validates PHPDoc annotations
-   Ensures consistent API usage
-   Detects potential security issues

### 4. **Refactoring Confidence**

Provides safety net during code changes:

-   Validates that changes don't break existing functionality
-   Identifies impact of API changes
-   Ensures backward compatibility
-   Catches regression bugs

## Installation

### Via Composer (Recommended)

```bash
# Install as dev dependency
composer require --dev phpstan/phpstan

# Install with extensions
composer require --dev phpstan/phpstan
composer require --dev phpstan/extension-installer
```

### Verify Installation

```bash
# Check PHPStan version
./vendor/bin/phpstan --version

# Run basic analysis
./vendor/bin/phpstan analyse src
```

## Configuration Explained

Here's the configuration file (`phpstan.neon.dist`) with detailed explanations:

```neon
parameters:
    level: max
    paths:
        - src

    reportUnmatchedIgnoredErrors: true
```

### Configuration Breakdown

#### 1. **Analysis Level**

```neon
level: max
```

-   **Purpose**: Sets the strictness level of analysis
-   **Range**: 0-9 (0 = basic, 9 = maximum strictness)
-   **`max`**: Always uses the highest available level
-   **Effect**: Enables all available checks and rules

#### 2. **Analysis Paths**

```neon
paths:
    - src
```

-   **Purpose**: Defines which directories to analyze
-   **Common paths**: `src/`, `app/`, `tests/`
-   **Effect**: Focuses analysis on relevant code directories

#### 3. **Error Reporting**

```neon
reportUnmatchedIgnoredErrors: true
```

-   **Purpose**: Reports when ignore patterns don't match any errors
-   **Benefit**: Prevents outdated ignore rules from hiding new issues
-   **Effect**: Ensures ignore patterns remain relevant

## Analysis Levels

PHPStan uses levels 0-9 to gradually increase analysis strictness:

### Level 0 (Basic)

-   Basic syntax checking
-   Undefined functions and classes
-   Wrong number of arguments passed to methods

### Level 1-3 (Intermediate)

-   Unknown methods called on objects
-   Unknown properties accessed on objects
-   Unknown variables in certain contexts

### Level 4-6 (Advanced)

-   Return types declared in PHPDoc
-   Basic dead code detection
-   Unreachable statements after return/throw

### Level 7-9 (Maximum)

-   Report partially wrong union types
-   Report calling methods on nullable types
-   Strict comparisons of incompatible types

### Level Max (Bleeding Edge)

-   Uses the highest available level
-   Includes experimental rules
-   Future-proof configuration

## Common Use Cases

### 1. **Type Safety Validation**

**PHPStan detects:**

```php
// Type mismatch
function processUser(User $user): void
{
    echo $user->name; // Error if User::$name is private
}

// Nullable type violation
function getName(?string $name): string
{
    return $name; // Error: might return null
}

// Correct version
function getName(?string $name): string
{
    return $name ?? 'Unknown';
}
```

### 2. **Method and Property Validation**

**PHPStan catches:**

```php
class User
{
    private string $name;

    public function getName(): string
    {
        return $this->name;
    }
}

$user = new User();
echo $user->email; // Error: Property User::$email does not exist
$user->invalidMethod(); // Error: Method does not exist
```

### 3. **Array and Collection Analysis**

**PHPStan validates:**

```php
// Array key existence
$config = ['database' => ['host' => 'localhost']];
echo $config['cache']['driver']; // Error: Offset 'cache' does not exist

// Collection type checking
/** @var User[] $users */
$users = getUsers();
foreach ($users as $user) {
    echo $user->name; // PHPStan knows $user is User instance
}
```

### 4. **Return Type Validation**

**PHPStan enforces:**

```php
function findUser(int $id): User
{
    $user = User::find($id);

    if (!$user) {
        return null; // Error: Cannot return null, expects User
    }

    return $user;
}

// Correct version
function findUser(int $id): ?User
{
    return User::find($id);
}
```

## Running PHPStan

### 1. **Basic Analysis**

```bash
# Analyze src directory
./vendor/bin/phpstan analyse src

# Analyze multiple directories
./vendor/bin/phpstan analyse src tests

# Analyze specific file
./vendor/bin/phpstan analyse src/User.php
```

### 2. **Configuration Options**

```bash
# Use custom configuration
./vendor/bin/phpstan analyse -c phpstan.neon

# Set analysis level
./vendor/bin/phpstan analyse --level=5 src

# Memory limit for large projects
./vendor/bin/phpstan analyse --memory-limit=1G src
```

### 3. **Output Formats**

```bash
# Default output
./vendor/bin/phpstan analyse src

# JSON output for CI/CD
./vendor/bin/phpstan analyse --error-format=json src

# GitHub Actions format
./vendor/bin/phpstan analyse --error-format=github src
```

### 4. **Advanced Options**

```bash
# Generate baseline (ignore existing errors)
./vendor/bin/phpstan analyse --generate-baseline

# Clear cache
./vendor/bin/phpstan clear-cache

# Debug mode
./vendor/bin/phpstan analyse --debug src
```

## Best Practices

### 1. **Incremental Adoption**

Start with lower levels and gradually increase:

```bash
# Start with level 0
./vendor/bin/phpstan analyse --level=0 src

# Gradually increase
./vendor/bin/phpstan analyse --level=5 src

# Eventually reach max
./vendor/bin/phpstan analyse --level=max src
```

### 2. **Use Baseline for Legacy Code**

Generate baseline to ignore existing errors:

```bash
# Generate baseline
./vendor/bin/phpstan analyse --generate-baseline

# Run analysis ignoring baseline errors
./vendor/bin/phpstan analyse
```

### 3. **CI/CD Integration**

Add PHPStan to your CI pipeline:

```yaml
# .github/workflows/phpstan.yml
name: PHPStan
on: [push, pull_request]

jobs:
    phpstan:
        runs-on: ubuntu-latest
        steps:
            - uses: actions/checkout@v3
            - uses: shivammathur/setup-php@v2
              with:
                  php-version: "8.2"
            - run: composer install --no-dev --optimize-autoloader
            - run: ./vendor/bin/phpstan analyse --error-format=github
```

### 4. **IDE Integration**

**VS Code with PHPStan extension:**

```json
{
    "phpstan.enabled": true,
    "phpstan.path": "./vendor/bin/phpstan",
    "phpstan.configFile": "phpstan.neon"
}
```

## Advanced Configuration

### Complete Configuration Example

```neon
parameters:
    level: max
    paths:
        - src
        - tests

    # Exclude specific directories
    excludePaths:
        - src/Legacy/*
        - tests/fixtures/*

    # Custom error reporting
    reportUnmatchedIgnoredErrors: true
    checkMissingIterableValueType: true
    checkGenericClassInNonGenericObjectType: true

    # Ignore specific errors
    ignoreErrors:
        - '#Call to an undefined method App\\User::invalidMethod\(\)#'
        -
            message: '#Access to an undefined property#'
            path: src/Legacy/OldClass.php

    # Additional rules
    checkAlwaysTrueCheckTypeFunctionCall: true
    checkAlwaysTrueInstanceof: true
    checkAlwaysTrueStrictComparison: true
    checkExplicitMixedMissingReturn: true
    checkFunctionNameCase: true
    checkInternalClassCaseSensitivity: true

    # Bootstrap files
    bootstrapFiles:
        - tests/bootstrap.php

    # Autoload directories
    scanDirectories:
        - src/helpers

    # Stub files for missing extensions
    stubFiles:
        - stubs/custom.stub
```

### Framework-Specific Configuration

**Laravel Configuration:**

```neon
includes:
    - ./vendor/nunomaduro/larastan/extension.neon

parameters:
    level: max
    paths:
        - app
        - database
        - routes
        - tests

    # Laravel-specific ignores
    ignoreErrors:
        - '#Unsafe usage of new static\(\)#'
        - '#Call to an undefined method Illuminate\\Database\\Eloquent\\Builder#'

    # Laravel features
    checkMissingIterableValueType: false
    checkGenericClassInNonGenericObjectType: false
```

**Symfony Configuration:**

```neon
includes:
    - ./vendor/phpstan/phpstan-symfony/extension.neon

parameters:
    level: max
    paths:
        - src
        - tests

    symfony:
        container_xml_path: var/cache/dev/App_KernelDevDebugContainer.xml
```

### Custom Rules and Extensions

```neon
parameters:
    level: max
    paths:
        - src

    # Custom rules
    rules:
        - App\PHPStan\Rules\NoEntityManagerInControllerRule

    # Service extensions
    services:
        -
            class: App\PHPStan\Extension\CustomExtension
            tags:
                - phpstan.broker.dynamicMethodReturnTypeExtension

    # Type extensions
    typeAliases:
        UserId: 'int<1, max>'
        EmailAddress: 'string'
```

### Performance Optimization

```neon
parameters:
    level: max
    paths:
        - src

    # Memory optimization
    memoryLimitFile: .phpstan-memory-limit

    # Parallel processing
    parallel:
        jobSize: 20
        maximumNumberOfProcesses: 4

    # Cache optimization
    tmpDir: var/cache/phpstan

    # Exclude large files
    excludePaths:
        - src/Generated/*
        - '*/large-file.php'
```

## Extensions and Integrations

### Popular Extensions

```bash
# Doctrine extension
composer require --dev phpstan/phpstan-doctrine

# Symfony extension
composer require --dev phpstan/phpstan-symfony

# PHPUnit extension
composer require --dev phpstan/phpstan-phpunit

# Laravel extension (Larastan)
composer require --dev nunomaduro/larastan
```

### Integration with Other Tools

**With Rector:**

```bash
#!/bin/bash
# Quality check script
./vendor/bin/rector process --dry-run
./vendor/bin/phpstan analyse
./vendor/bin/pint --test
```

**With PHPUnit:**

```bash
#!/bin/bash
# Test and analyze
./vendor/bin/phpunit
./vendor/bin/phpstan analyse
```

## Troubleshooting

### Common Issues

1. **Memory Limit Exceeded**

```bash
php -d memory_limit=1G ./vendor/bin/phpstan analyse src
```

2. **Autoloading Issues**

```neon
parameters:
    bootstrapFiles:
        - vendor/autoload.php
        - bootstrap/app.php
```

3. **False Positives**

```neon
parameters:
    ignoreErrors:
        - '#Specific error pattern#'
```

4. **Performance Issues**

```bash
# Clear cache
./vendor/bin/phpstan clear-cache

# Reduce parallel processes
./vendor/bin/phpstan analyse --no-progress src
```

### Debugging Tips

```bash
# Verbose output
./vendor/bin/phpstan analyse --debug src

# Analyse specific error
./vendor/bin/phpstan analyse --debug src/User.php

# Generate baseline for debugging
./vendor/bin/phpstan analyse --generate-baseline phpstan-baseline.neon
```

## Conclusion

PHPStan is an essential tool for maintaining high-quality PHP codebases. It provides comprehensive static analysis that catches bugs early, enforces type safety, and improves overall code quality.

### Key Benefits:

-   **Early bug detection** prevents runtime errors
-   **Type safety enforcement** improves code reliability
-   **Code quality insights** guide better architecture decisions
-   **Refactoring confidence** through comprehensive analysis
-   **CI/CD integration** maintains quality standards

### Getting Started:

1. Install PHPStan via Composer
2. Create basic configuration (`phpstan.neon`)
3. Start with lower analysis levels
4. Gradually increase strictness
5. Integrate with development workflow

### Best Practices:

-   Use baseline for legacy code
-   Integrate with CI/CD pipeline
-   Configure IDE for real-time feedback
-   Combine with other quality tools
-   Regular analysis during development

PHPStan becomes more valuable as your codebase grows and team size increases. It's particularly beneficial for teams working on critical applications where code quality and reliability are paramount.

---

## Related Resources

-   [Rector PHP Refactoring](/software-engineer-notebook/fundamentals/coding-standards/php/rector-php-automated-refactoring)
-   [Laravel Pint Code Formatting](/software-engineer-notebook/fundamentals/coding-standards/php/laravel-pint-code-formatting)
-   [PHP Coding Standards](/software-engineer-notebook/fundamentals/coding-standards/php/php-coding-standards)

_Want to see more PHP development tools? Check out our comprehensive guides on building robust PHP applications._
