# Laravel Pint: Opinionated PHP Code Style Fixer

Laravel Pint is an opinionated PHP code style fixer built on top of PHP-CS-Fixer. It provides zero-configuration code formatting for Laravel projects while offering extensive customization options for teams that need specific coding standards.

## Table of Contents

1. [What is Laravel Pint?](#what-is-laravel-pint)
2. [Why Use Laravel Pint?](#why-use-laravel-pint)
3. [Installation](#installation)
4. [Configuration Explained](#configuration-explained)
5. [Rule Categories](#rule-categories)
6. [Running Pint](#running-pint)
7. [Best Practices](#best-practices)
8. [Advanced Configuration](#advanced-configuration)

## What is Laravel Pint?

Laravel Pint is a zero-configuration PHP code style fixer for minimalists. It's built on top of PHP-CS-Fixer and provides:

-   **Opinionated defaults** - Works out of the box with Laravel conventions
-   **Customizable rules** - Extensive configuration options for team preferences
-   **Fast execution** - Optimized for performance with parallel processing
-   **Laravel integration** - Seamless integration with Laravel projects

Pint ensures your code follows consistent formatting standards without requiring complex configuration.

## Why Use Laravel Pint?

### 1. **Zero Configuration**

Works immediately with sensible Laravel defaults:

-   No setup required for basic Laravel projects
-   Follows Laravel's official coding standards
-   Handles PSR-12 compliance automatically

### 2. **Consistent Code Style**

Eliminates style debates and inconsistencies:

-   Uniform formatting across the entire codebase
-   Automatic fixing of common style issues
-   Consistent indentation, spacing, and structure

### 3. **Developer Productivity**

Reduces mental overhead and code review time:

-   Developers focus on logic, not formatting
-   Automated formatting in CI/CD pipelines
-   Fewer style-related code review comments

### 4. **Team Collaboration**

Ensures consistent code across team members:

-   Eliminates "style wars" between developers
-   Consistent code regardless of IDE or editor
-   Onboarding new team members with established standards

## Installation

### Via Composer (Laravel 9+)

Laravel Pint comes pre-installed with Laravel 9+:

```bash
# Check if Pint is installed
./vendor/bin/pint --version

# If not installed, add it manually
composer require laravel/pint --dev
```

### For Non-Laravel Projects

```bash
composer require laravel/pint --dev
```

## Configuration Explained

Here's the configuration file (`pint.json`) with detailed explanations:

```json
{
    "preset": "laravel",
    "notPath": ["tests/TestCase.php"],
    "rules": {
        "array_push": true,
        "backtick_to_shell_exec": true,
        "date_time_immutable": true,
        "declare_strict_types": true,
        "lowercase_keywords": true,
        "lowercase_static_reference": true,
        "final_class": true,
        "final_internal_class": true,
        "final_public_method_for_abstract_class": true,
        "fully_qualified_strict_types": true,
        "global_namespace_import": {
            "import_classes": true,
            "import_constants": true,
            "import_functions": true
        },
        "mb_str_functions": true,
        "modernize_types_casting": true,
        "new_with_parentheses": false,
        "no_superfluous_elseif": true,
        "no_useless_else": true,
        "no_multiple_statements_per_line": true,
        "ordered_class_elements": {
            "order": [
                "use_trait",
                "case",
                "constant",
                "constant_public",
                "constant_protected",
                "constant_private",
                "property_public",
                "property_protected",
                "property_private",
                "construct",
                "destruct",
                "magic",
                "phpunit",
                "method_abstract",
                "method_public_static",
                "method_public",
                "method_protected_static",
                "method_protected",
                "method_private_static",
                "method_private"
            ],
            "sort_algorithm": "none"
        },
        "ordered_interfaces": true,
        "ordered_traits": true,
        "protected_to_private": true,
        "self_accessor": true,
        "self_static_accessor": true,
        "strict_comparison": true,
        "visibility_required": true
    }
}
```

### Configuration Breakdown

#### 1. **Preset Configuration**

```json
"preset": "laravel"
```

-   **Purpose**: Uses Laravel's official coding standards as base
-   **Alternatives**: `"psr12"`, `"symfony"`, `"per"`
-   **Effect**: Applies Laravel-specific formatting rules

#### 2. **Path Exclusions**

```json
"notPath": ["tests/TestCase.php"]
```

-   **Purpose**: Excludes specific files from formatting
-   **Common exclusions**: Generated files, third-party code, legacy files
-   **Example**: `["bootstrap/cache/*", "storage/*", "vendor/*"]`

#### 3. **Rule Categories**

**Modern PHP Features**:

```json
"declare_strict_types": true,
"modernize_types_casting": true,
"date_time_immutable": true
```

-   Enforces strict typing declarations
-   Modernizes type casting syntax
-   Prefers `DateTimeImmutable` over `DateTime`

**Code Quality Rules**:

```json
"final_class": true,
"final_internal_class": true,
"protected_to_private": true
```

-   Makes classes final when possible
-   Reduces visibility scope when appropriate
-   Improves encapsulation

**Import Organization**:

```json
"global_namespace_import": {
    "import_classes": true,
    "import_constants": true,
    "import_functions": true
}
```

-   Organizes use statements
-   Imports global functions and constants
-   Reduces fully qualified name usage

**Class Structure**:

```json
"ordered_class_elements": {
    "order": [
        "use_trait",
        "case",
        "constant",
        "property_public",
        "property_protected",
        "property_private",
        "construct",
        "destruct",
        "magic",
        "phpunit",
        "method_public",
        "method_protected",
        "method_private"
    ]
}
```

-   Enforces consistent class member ordering
-   Improves code readability
-   Standardizes class structure

## Rule Categories

### 1. **Safety and Type Safety**

```json
{
    "declare_strict_types": true,
    "strict_comparison": true,
    "fully_qualified_strict_types": true
}
```

**Before:**

```php
<?php

class Calculator
{
    public function add($a, $b)
    {
        if ($a == null) {
            return $b;
        }
        return $a + $b;
    }
}
```

**After:**

```php
<?php

declare(strict_types=1);

class Calculator
{
    public function add(int $a, int $b): int
    {
        if ($a === null) {
            return $b;
        }
        return $a + $b;
    }
}
```

### 2. **Code Organization**

```json
{
    "ordered_class_elements": {
        "order": [
            "use_trait",
            "constant",
            "property_public",
            "property_protected",
            "property_private",
            "construct",
            "method_public",
            "method_protected",
            "method_private"
        ]
    }
}
```

**Before:**

```php
class User
{
    public function getName(): string
    {
        return $this->name;
    }

    private string $name;

    public const STATUS_ACTIVE = 'active';

    public function __construct(string $name)
    {
        $this->name = $name;
    }
}
```

**After:**

```php
class User
{
    public const STATUS_ACTIVE = 'active';

    private string $name;

    public function __construct(string $name)
    {
        $this->name = $name;
    }

    public function getName(): string
    {
        return $this->name;
    }
}
```

### 3. **Modernization Rules**

```json
{
    "modernize_types_casting": true,
    "mb_str_functions": true,
    "array_push": true
}
```

**Before:**

```php
$string = (string) $value;
$length = strlen($text);
array_push($items, $newItem);
```

**After:**

```php
$string = (string) $value;
$length = mb_strlen($text);
$items[] = $newItem;
```

### 4. **Control Flow Improvements**

```json
{
    "no_superfluous_elseif": true,
    "no_useless_else": true,
    "no_multiple_statements_per_line": true
}
```

**Before:**

```php
if ($condition1) {
    return 'A';
} elseif ($condition2) {
    return 'B';
} else {
    return 'C';
}

$x = 1; $y = 2;
```

**After:**

```php
if ($condition1) {
    return 'A';
}

if ($condition2) {
    return 'B';
}

return 'C';

$x = 1;
$y = 2;
```

## Running Pint

### 1. **Basic Usage**

```bash
# Format all files
./vendor/bin/pint

# Format specific directory
./vendor/bin/pint app/

# Format specific file
./vendor/bin/pint app/Models/User.php
```

### 2. **Preview Mode**

```bash
# See what would be changed without applying
./vendor/bin/pint --test

# Verbose output showing all changes
./vendor/bin/pint --test -v
```

### 3. **Configuration Options**

```bash
# Use custom config file
./vendor/bin/pint --config=custom-pint.json

# Use specific preset
./vendor/bin/pint --preset=psr12

# Format only dirty files (git)
./vendor/bin/pint --dirty
```

### 4. **CI/CD Integration**

```bash
# Test mode for CI (exits with error if formatting needed)
./vendor/bin/pint --test

# Combine with other tools
./vendor/bin/pint && ./vendor/bin/phpstan analyse
```

## Best Practices

### 1. **Pre-commit Hooks**

Install pre-commit hook to ensure code is formatted before commits:

```bash
# Install pre-commit hook
echo '#!/bin/sh
./vendor/bin/pint --test
' > .git/hooks/pre-commit

chmod +x .git/hooks/pre-commit
```

### 2. **IDE Integration**

Configure your IDE to run Pint on save:

**VS Code** (`.vscode/settings.json`):

```json
{
    "emeraldwalk.runonsave": {
        "commands": [
            {
                "match": "\\.php$",
                "cmd": "./vendor/bin/pint ${file}"
            }
        ]
    }
}
```

### 3. **CI/CD Pipeline**

```yaml
# .github/workflows/pint.yml
name: Laravel Pint
on: [push, pull_request]

jobs:
    pint:
        runs-on: ubuntu-latest
        steps:
            - uses: actions/checkout@v3
            - uses: shivammathur/setup-php@v2
              with:
                  php-version: "8.2"
            - run: composer install --no-dev --optimize-autoloader
            - run: ./vendor/bin/pint --test
```

### 4. **Team Workflow**

```bash
# Developer workflow
git add .
./vendor/bin/pint
git add .
git commit -m "Your commit message"

# Or create alias
alias pint-commit="./vendor/bin/pint && git add . && git commit"
```

## Advanced Configuration

### Custom Preset

Create a custom preset for your organization:

```json
{
    "preset": "psr12",
    "rules": {
        "array_syntax": { "syntax": "short" },
        "binary_operator_spaces": {
            "default": "single_space"
        },
        "blank_line_after_namespace": true,
        "blank_line_after_opening_tag": true,
        "blank_line_before_statement": {
            "statements": ["return", "throw", "try"]
        },
        "class_definition": {
            "single_line": true,
            "single_item_single_line": true
        },
        "concat_space": { "spacing": "one" },
        "method_argument_space": {
            "on_multiline": "ensure_fully_multiline"
        },
        "no_unused_imports": true,
        "ordered_imports": {
            "sort_algorithm": "alpha"
        },
        "trailing_comma_in_multiline": {
            "elements": ["arrays", "arguments", "parameters"]
        }
    }
}
```

### Environment-specific Configuration

```json
{
    "preset": "laravel",
    "exclude": ["bootstrap/cache", "storage"],
    "notPath": ["tests/TestCase.php", "database/migrations"],
    "rules": {
        "declare_strict_types": true,
        "final_class": false,
        "php_unit_test_class_requires_covers": false
    }
}
```

### Integration with Other Tools

**Combined with Rector**:

```bash
#!/bin/bash
# Format and refactor script
./vendor/bin/rector process
./vendor/bin/pint
./vendor/bin/phpstan analyse
```

**Package.json Scripts**:

```json
{
    "scripts": {
        "format": "./vendor/bin/pint",
        "format:test": "./vendor/bin/pint --test",
        "format:dirty": "./vendor/bin/pint --dirty"
    }
}
```

## Common Configuration Examples

### Laravel API Project

```json
{
    "preset": "laravel",
    "rules": {
        "declare_strict_types": true,
        "final_class": true,
        "ordered_class_elements": {
            "order": [
                "use_trait",
                "constant",
                "property_public",
                "property_protected",
                "property_private",
                "construct",
                "method_public",
                "method_protected",
                "method_private"
            ]
        },
        "php_unit_test_class_requires_covers": false,
        "visibility_required": true
    }
}
```

### Legacy Project Migration

```json
{
    "preset": "psr12",
    "rules": {
        "declare_strict_types": false,
        "final_class": false,
        "modernize_types_casting": true,
        "no_superfluous_elseif": true,
        "no_useless_else": true,
        "protected_to_private": false,
        "strict_comparison": false
    }
}
```

## Troubleshooting

### Common Issues

1. **Memory Issues with Large Files**

```bash
php -d memory_limit=512M ./vendor/bin/pint
```

2. **Exclude Problematic Files**

```json
{
    "notPath": ["app/Legacy/*", "database/migrations/*"]
}
```

3. **Rule Conflicts**

```json
{
    "rules": {
        "final_class": false,
        "final_internal_class": false
    }
}
```

## Conclusion

Laravel Pint provides an excellent balance between zero-configuration simplicity and powerful customization options. It ensures consistent code formatting across your Laravel projects while being flexible enough to adapt to team preferences.

### Key Benefits:

-   **Zero configuration** for Laravel projects
-   **Consistent formatting** across entire codebase
-   **Fast execution** with parallel processing
-   **Extensive customization** options
-   **Seamless CI/CD integration**

### Getting Started:

1. Install via Composer (comes with Laravel 9+)
2. Run `./vendor/bin/pint` to format your code
3. Customize rules in `pint.json` as needed
4. Integrate with your development workflow
5. Add to CI/CD pipeline for automated checking

Remember to run Pint regularly and consider integrating it with your pre-commit hooks and CI/CD pipeline for maximum effectiveness.

---

## Related Resources

-   [Rector PHP Refactoring](/software-engineer-notebook/fundamentals/coding-standards/php/rector-php-automated-refactoring)
-   [PHPStan Static Analysis](/software-engineer-notebook/fundamentals/coding-standards/php/phpstan-static-analysis)
-   [Laravel Coding Standards](/software-engineer-notebook/fundamentals/coding-standards/php/laravel-coding-standards)

_Want to see more PHP development tools? Check out our comprehensive guides on building robust PHP applications._
