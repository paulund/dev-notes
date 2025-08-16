# Rector: Automated PHP Code Refactoring and Modernization

Rector is a powerful automated refactoring tool for PHP that helps modernize legacy code, improve code quality, and maintain consistency across your codebase. It can automatically upgrade PHP versions, apply coding standards, and perform complex refactoring operations that would be time-consuming to do manually.

## Table of Contents

1. [What is Rector?](#what-is-rector)
2. [Why Use Rector?](#why-use-rector)
3. [Installation](#installation)
4. [Configuration Explained](#configuration-explained)
5. [Common Use Cases](#common-use-cases)
6. [Running Rector](#running-rector)
7. [Best Practices](#best-practices)
8. [Advanced Configuration](#advanced-configuration)

## What is Rector?

Rector is an instant upgrade and automated refactoring tool for PHP. It analyzes your codebase and applies transformations to:

-   **Upgrade PHP versions** - Automatically modernize code for newer PHP versions
-   **Improve code quality** - Apply best practices and remove code smells
-   **Refactor legacy code** - Transform old patterns into modern equivalents
-   **Maintain consistency** - Ensure uniform coding patterns across the project

Unlike traditional linters that only identify issues, Rector actually fixes them automatically.

## Why Use Rector?

### 1. **Automated PHP Version Upgrades**

Upgrading PHP versions manually is time-consuming and error-prone. Rector can automatically:

-   Replace deprecated functions with modern alternatives
-   Update syntax for new PHP features
-   Fix compatibility issues between PHP versions

### 2. **Code Quality Improvements**

Rector applies established best practices:

-   Remove dead code and unused variables
-   Improve type declarations
-   Optimize performance patterns
-   Enforce strict typing

### 3. **Consistency Across Large Codebases**

For teams and large projects:

-   Ensure uniform coding patterns
-   Apply architectural decisions automatically
-   Maintain code standards without manual review

### 4. **Time and Cost Savings**

-   Reduces manual refactoring time by 80-90%
-   Minimizes human error in code transformations
-   Allows focus on business logic rather than boilerplate improvements

## Installation

### Via Composer (Recommended)

```bash
# Install as dev dependency
composer require rector/rector --dev

# Or install globally
composer global require rector/rector
```

### Verify Installation

```bash
# Check Rector version
vendor/bin/rector --version

# View available commands
vendor/bin/rector list
```

## Configuration Explained

Here's the configuration file (`rector.php`) with detailed explanations:

```php
<?php

declare(strict_types=1);

use Rector\Config\RectorConfig;
use Rector\Php83\Rector\ClassMethod\AddOverrideAttributeToOverriddenMethodsRector;

return RectorConfig::configure()
    ->withPaths([
        __DIR__ . '/src',
        __DIR__ . '/tests',
    ])
    ->withSkip([
        AddOverrideAttributeToOverriddenMethodsRector::class,
    ])
    ->withPreparedSets(
        deadCode: true,
        codeQuality: true,
        typeDeclarations: true,
        privatization: true,
        earlyReturn: true,
        strictBooleans: true,
    )
    ->withPhpSets();
```

### Configuration Breakdown

#### 1. **Paths Configuration**

```php
->withPaths([
    __DIR__ . '/src',
    __DIR__ . '/tests',
])
```

-   **Purpose**: Defines which directories Rector should analyze
-   **Common paths**: `src/`, `tests/`, `app/` (for Laravel)
-   **Benefits**: Focuses analysis on relevant code, ignoring vendor dependencies

#### 2. **Skip Configuration**

```php
->withSkip([
    AddOverrideAttributeToOverriddenMethodsRector::class,
])
```

-   **Purpose**: Excludes specific rules from being applied
-   **When to use**: When a rule doesn't fit your project's needs
-   **Example**: Skipping PHP 8.3's `#[Override]` attribute if not ready for it

#### 3. **Prepared Sets**

```php
->withPreparedSets(
    deadCode: true,
    codeQuality: true,
    typeDeclarations: true,
    privatization: true,
    earlyReturn: true,
    strictBooleans: true,
)
```

**Dead Code Removal** (`deadCode: true`):

-   Removes unused variables, methods, and classes
-   Eliminates unreachable code
-   Cleans up commented-out code

**Code Quality** (`codeQuality: true`):

-   Simplifies complex expressions
-   Removes unnecessary code constructs
-   Applies performance optimizations

**Type Declarations** (`typeDeclarations: true`):

-   Adds missing type hints to method parameters
-   Adds return type declarations
-   Improves type safety

**Privatization** (`privatization: true`):

-   Changes public/protected methods to private when possible
-   Improves encapsulation
-   Reduces API surface area

**Early Return** (`earlyReturn: true`):

-   Converts nested if-else to early returns
-   Improves code readability
-   Reduces nesting levels

**Strict Booleans** (`strictBooleans: true`):

-   Converts loose boolean comparisons to strict ones
-   Changes `if ($var)` to `if ($var !== null)` where appropriate
-   Improves type safety

#### 4. **PHP Sets**

```php
->withPhpSets()
```

-   **Purpose**: Applies all PHP version upgrade rules
-   **Effect**: Automatically detects and applies rules for your PHP version
-   **Alternative**: Use `->withPhpSets(php83: true)` for specific PHP version

## Common Use Cases

### 1. **PHP Version Upgrade**

```php
// Before (PHP 7.4)
public function process($data)
{
    if ($data === null) {
        return null;
    }
    return $data->getValue();
}

// After (PHP 8.0) - Rector automatically adds nullsafe operator
public function process($data)
{
    return $data?->getValue();
}
```

### 2. **Type Declaration Improvements**

```php
// Before
class UserService
{
    public function findUser($id)
    {
        return User::find($id);
    }
}

// After
class UserService
{
    public function findUser(int $id): ?User
    {
        return User::find($id);
    }
}
```

### 3. **Dead Code Removal**

```php
// Before
class Calculator
{
    private $unusedProperty;

    public function add($a, $b)
    {
        $unusedVariable = 'test';
        return $a + $b;
    }

    private function neverUsedMethod()
    {
        return 'unused';
    }
}

// After
class Calculator
{
    public function add($a, $b)
    {
        return $a + $b;
    }
}
```

### 4. **Early Return Pattern**

```php
// Before
public function processOrder($order)
{
    if ($order->isValid()) {
        if ($order->isPaid()) {
            return $this->fulfillOrder($order);
        } else {
            return $this->requestPayment($order);
        }
    } else {
        return $this->rejectOrder($order);
    }
}

// After
public function processOrder($order)
{
    if (!$order->isValid()) {
        return $this->rejectOrder($order);
    }

    if (!$order->isPaid()) {
        return $this->requestPayment($order);
    }

    return $this->fulfillOrder($order);
}
```

## Running Rector

### 1. **Dry Run (Preview Changes)**

```bash
# See what would be changed without applying
vendor/bin/rector process --dry-run

# Process specific directory
vendor/bin/rector process src --dry-run
```

### 2. **Apply Changes**

```bash
# Apply all changes
vendor/bin/rector process

# Process specific file
vendor/bin/rector process src/UserService.php
```

### 3. **Generate Configuration**

```bash
# Generate basic rector.php configuration
vendor/bin/rector init
```

### 4. **List Available Rules**

```bash
# Show all available rules
vendor/bin/rector list-rules

# Show rules for specific set
vendor/bin/rector list-rules --set=php83
```

## Best Practices

### 1. **Start with Dry Runs**

Always preview changes before applying:

```bash
vendor/bin/rector process --dry-run
```

### 2. **Use Version Control**

Commit your code before running Rector:

```bash
git add .
git commit -m "Before Rector refactoring"
vendor/bin/rector process
```

### 3. **Run Tests After Rector**

Ensure your test suite passes after refactoring:

```bash
vendor/bin/rector process
./vendor/bin/phpunit
```

### 4. **Incremental Adoption**

Start with safe rule sets:

```php
// Start conservative
->withPreparedSets(
    deadCode: true,
    codeQuality: true,
)

// Gradually add more
->withPreparedSets(
    deadCode: true,
    codeQuality: true,
    typeDeclarations: true,
    privatization: true,
)
```

### 5. **CI/CD Integration**

Add Rector to your CI pipeline:

```yaml
# .github/workflows/rector.yml
name: Rector
on: [push, pull_request]
jobs:
    rector:
        runs-on: ubuntu-latest
        steps:
            - uses: actions/checkout@v2
            - uses: shivammathur/setup-php@v2
              with:
                  php-version: "8.3"
            - run: composer install
            - run: vendor/bin/rector process --dry-run
```

## Advanced Configuration

### Custom Rules and Skips

```php
<?php

use Rector\Config\RectorConfig;
use Rector\Php80\Rector\Class_\ClassPropertyAssignToConstructorPromotionRector;
use Rector\Php81\Rector\ClassConst\FinalizePublicClassConstantRector;

return RectorConfig::configure()
    ->withPaths([
        __DIR__ . '/src',
        __DIR__ . '/tests',
    ])
    ->withSkip([
        // Skip specific rules
        ClassPropertyAssignToConstructorPromotionRector::class,

        // Skip rule for specific files
        FinalizePublicClassConstantRector::class => [
            __DIR__ . '/src/Legacy/OldClass.php',
        ],

        // Skip entire directories
        __DIR__ . '/src/External',
    ])
    ->withPreparedSets(
        deadCode: true,
        codeQuality: true,
        typeDeclarations: true,
    )
    ->withPhpSets(php83: true);
```

### File-based Configuration

```php
<?php

return RectorConfig::configure()
    ->withPaths([
        __DIR__ . '/src',
    ])
    ->withSkip([
        // Skip files matching pattern
        '*/migrations/*',
        '*/vendor/*',
    ])
    ->withImportNames()
    ->withPreparedSets(
        deadCode: true,
        codeQuality: true,
    );
```

### Laravel-specific Configuration

```php
<?php

use Rector\Config\RectorConfig;
use RectorLaravel\Set\LaravelSetList;

return RectorConfig::configure()
    ->withPaths([
        __DIR__ . '/app',
        __DIR__ . '/database',
        __DIR__ . '/resources',
        __DIR__ . '/routes',
        __DIR__ . '/tests',
    ])
    ->withSets([
        LaravelSetList::LARAVEL_100,
        LaravelSetList::LARAVEL_CODE_QUALITY,
        LaravelSetList::LARAVEL_FACADE_ALIASES_TO_FULL_NAMES,
    ])
    ->withPreparedSets(
        deadCode: true,
        codeQuality: true,
        typeDeclarations: true,
    );
```

## Troubleshooting

### Common Issues

1. **Memory Limit Exceeded**

```bash
php -d memory_limit=2G vendor/bin/rector process
```

2. **Skip Problematic Files**

```php
->withSkip([
    __DIR__ . '/src/ProblematicFile.php',
])
```

3. **Debugging Rule Application**

```bash
vendor/bin/rector process --debug
```

## Integration with Other Tools

### With Laravel Pint

```bash
# First run Rector for refactoring
vendor/bin/rector process

# Then run Pint for code style
./vendor/bin/pint
```

### With PHPStan

```bash
# Run Rector first
vendor/bin/rector process

# Then check with PHPStan
./vendor/bin/phpstan analyse
```

## Conclusion

Rector is an invaluable tool for maintaining modern, high-quality PHP codebases. It automates tedious refactoring tasks, helps with PHP version upgrades, and ensures consistent code quality across projects.

### Key Benefits:

-   **Automated refactoring** saves hours of manual work
-   **PHP version upgrades** become manageable
-   **Code quality** improvements are applied consistently
-   **Team productivity** increases with automated maintenance

### Getting Started:

1. Install Rector via Composer
2. Create a basic configuration file
3. Start with conservative rule sets
4. Run dry runs to preview changes
5. Gradually adopt more advanced features

Remember to always test your code after running Rector and use version control to track changes. With proper configuration and workflow integration, Rector becomes an essential part of your PHP development toolkit.
