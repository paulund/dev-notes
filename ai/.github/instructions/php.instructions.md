---
applyTo: '**/*.php'
---

# PHP Development Guidelines

## Core Principles

- Write clean, readable, and maintainable code that follows SOLID principles
- Favor composition over inheritance where appropriate
- Keep methods small and focused on a single responsibility
- Use type declarations for all method parameters and return types
- Leverage PHP 8+ features including named arguments, constructor property promotion, and enums
- Write self-documenting code with clear variable and method names
- Avoid premature optimization; prioritize clarity first
- Follow PSR-12 coding standards for consistent formatting
- Use strict types (`declare(strict_types=1)`) at the top of every PHP file
- Write testable code by avoiding static methods and global state
- Apply dependency injection rather than instantiating dependencies directly
- Keep cyclomatic complexity low; refactor complex conditionals into separate methods
- Use early returns to reduce nesting and improve readability
- Prefer immutability use `final` classes where practical and `readonly`
  properties to prevent unintended side effects
- Use native PHP functions over custom implementations - they are optimized, well-tested, and widely understood

## Language Rules

- Native functions: prefer built-in PHP functions over custom implementations (e.g., `array_map()`, `array_filter()`, `array_reduce()` instead of manual loops when appropriate)
- Strings: use `mb_*` functions for string manipulation.
- Comparisons: use strict comparisons (`===`, `!==`); never rely on loose truthiness.
- Dates: prefer `DateTimeImmutable` over mutable `DateTime` objects.
- Imports: import classes/constants/functions into the global namespace; remove unused imports.

## Style Rules

### File Structure

- Always start files with `<?php` on line 1, followed by a blank line
- Include `declare(strict_types=1);` after the opening tag
- One class per file, matching the filename
- Use 4 spaces for indentation (no tabs)
- Maximum line length of 120 characters
- End files with a single blank line

### Type Declarations

- Always declare parameter types and return types
- Use nullable types (`?string`)
- Avoid `mixed` types; be specific whenever possible
- Use array shapes in docblocks for complex array structures

### Method and Function Declarations

- Opening brace on the same line as the method signature
- Use constructor property promotion for simple property assignments
- Keep method signatures on a single line when possible
- For long signatures, place each parameter on its own line
- Use named arguments when calling methods with many parameters

### Control Structures

- Always use braces, even for single-line conditionals
- Opening brace on the same line as the control structure
- Use early returns to avoid deep nesting
- Prefer positive conditionals over negative ones when it improves readability
- Use match expressions instead of switch when returning values

### Arrays and Collections

- Use short array syntax `[]` instead of `array()`
- Use trailing commas in multi-line arrays
- One array element per line for readability when arrays are complex
- Use spread operator for array merging when appropriate
- Prefer specific collection classes over plain arrays for domain objects

## Exception Management

### Exception Hierarchy

- Create custom exception classes that extend appropriate base exceptions
- Group related exceptions in domain-specific namespaces
- Use specific exception types rather than generic Exception class
- Include context in exception messages to aid debugging

### Throwing Exceptions

- Throw exceptions for exceptional conditions, not for control flow
- Include relevant context in exception messages (avoid sensitive data)
- Use named constructor methods on exception classes for clarity
- Fail fast: validate inputs early and throw exceptions immediately

### Catching Exceptions

- Catch specific exceptions rather than catching all exceptions
- Only catch exceptions you can meaningfully handle
- Log exceptions before re-throwing or transforming them
- Never use empty catch blocks; at minimum, log the exception
- Use finally blocks for cleanup operations
- Consider using try-catch at boundaries (controllers, commands) not deep in business logic

### Example Pattern

```php
// Custom exception
final class UserNotFoundException extends \RuntimeException
{
    public static function withId(int $id): self
    {
        return new self("User with ID {$id} not found");
    }
}

// Usage
public function findUser(int $id): User
{
    $user = $this->repository->find($id);

    if ($user === null) {
        throw UserNotFoundException::withId($id);
    }

    return $user;
}
```

## Naming Conventions

### Classes

- Use PascalCase for class names
- Use descriptive, noun-based names that reflect purpose
- Suffix interfaces with `Interface` (e.g., `PaymentGatewayInterface`)
- Suffix traits with `Trait` (e.g., `TimestampableTrait`)
- Suffix abstract classes with `Abstract` when ambiguity exists
- Use singular nouns for entity classes (e.g., `User`, not `Users`)

### Methods

- Use camelCase for method names
- Start with verbs that describe the action (`get`, `set`, `calculate`, `find`, `create`, `update`, `delete`)
- Boolean methods should ask questions (`isActive()`, `hasPermission()`, `canEdit()`)
- Avoid generic names like `process()` or `handle()`; be specific
- Keep names descriptive but concise

### Variables

- Use camelCase for variable names
- Use descriptive names that reveal intent
- Avoid abbreviations unless universally understood
- Use plural names for arrays/collections (`$users`, not `$user_array`)
- Boolean variables should be prefixed with `is`, `has`, `can`, `should`
- Avoid single-letter variables except for loop counters

### Constants

- Use UPPER_SNAKE_CASE for constants
- Group related constants in enums when using PHP 8.1+
- Use class constants instead of global constants

### Namespaces

- Use PascalCase for namespace segments
- Follow PSR-4 autoloading standards
- Organize by feature/domain rather than by type (avoid `Controllers/`, `Models/` at top level)
- Use `\` as namespace separator, with no leading slash in declarations

### Examples

```php
<?php

declare(strict_types=1);

namespace App\Domain\User;

final class UserRegistrationService
{
    private const MAX_LOGIN_ATTEMPTS = 5;

    public function __construct(
        private readonly UserRepository $userRepository,
        private readonly PasswordHasher $passwordHasher,
    ) {}

    public function registerUser(string $email, string $password): User
    {
        if ($this->userRepository->existsByEmail($email)) {
            throw UserAlreadyExistsException::withEmail($email);
        }

        $hashedPassword = $this->passwordHasher->hash($password);

        return $this->userRepository->create(
            email: $email,
            password: $hashedPassword,
        );
    }

    public function canUserLogin(User $user): bool
    {
        return $user->isActive()
            && !$user->isLocked()
            && $user->getFailedLoginAttempts() < self::MAX_LOGIN_ATTEMPTS;
    }
}
```

## Data Transfer Objects (DTOs)

### Use DTOs Instead of Arrays

- Use DTOs to transfer data between layers of your application
- DTOs provide type safety, IDE autocomplete, and self-documenting code
- Make DTOs immutable using `readonly` properties
- Use constructor property promotion for concise syntax
- DTOs should contain no business logic, only data

### DTO Best Practices

```php
// ✅ Good: Type-safe DTO
final readonly class CreateUserData
{
    public function __construct(
        public string $name,
        public string $email,
        public string $password,
        public ?string $phone = null,
        public array $roles = [],
    ) {}

    public static function fromArray(array $data): self
    {
        return new self(
            name: $data['name'],
            email: $data['email'],
            password: $data['password'],
            phone: $data['phone'] ?? null,
            roles: $data['roles'] ?? [],
        );
    }

    public function toArray(): array
    {
        return [
            'name' => $this->name,
            'email' => $this->email,
            'password' => $this->password,
            'phone' => $this->phone,
            'roles' => $this->roles,
        ];
    }
}

// Usage
public function createUser(CreateUserData $data): User
{
    return $this->repository->create($data);
}

// ❌ Bad: Passing arrays
public function createUser(array $userData): User
{
    // No type safety, prone to errors
    return $this->repository->create($userData);
}
```

### When to Use DTOs

- Transferring data between application layers (Controller → Service → Repository)
- API request/response payloads
- Command/Query objects in CQRS patterns
- Data transformation between external systems and your domain
- Form data validation results

### DTO Naming Conventions

- Use descriptive names that indicate purpose: `CreateUserData`, `UpdateProductData`
- Suffix with `Data` or `DTO` for clarity
- Group related DTOs in dedicated namespaces: `App\DataTransferObjects\User\`

## Value Objects

### Use Value Objects for Domain Concepts

- Value objects encapsulate domain concepts that are defined by their values, not identity
- Make value objects immutable and final
- Implement validation in the constructor
- Provide factory methods for common creation patterns
- Override `equals()` method for value comparison

### Value Object Best Practices

```php
// ✅ Good: Immutable value object with validation
final readonly class Email
{
    private function __construct(
        public string $value
    ) {}

    public static function from(string $email): self
    {
        $email = trim(strtolower($email));

        if (!filter_var($email, FILTER_VALIDATE_EMAIL)) {
            throw new InvalidArgumentException("Invalid email address: {$email}");
        }

        return new self($email);
    }

    public function domain(): string
    {
        return substr($this->value, strpos($this->value, '@') + 1);
    }

    public function equals(Email $other): bool
    {
        return $this->value === $other->value;
    }

    public function __toString(): string
    {
        return $this->value;
    }
}

// More complex value object example
final readonly class Money
{
    private function __construct(
        public int $amount,      // Store in smallest unit (cents)
        public string $currency
    ) {}

    public static function fromAmount(float $amount, string $currency): self
    {
        if ($amount < 0) {
            throw new InvalidArgumentException('Amount cannot be negative');
        }

        return new self(
            amount: (int) round($amount * 100),
            currency: strtoupper($currency)
        );
    }

    public static function zero(string $currency): self
    {
        return new self(0, strtoupper($currency));
    }

    public function add(Money $other): self
    {
        $this->assertSameCurrency($other);

        return new self(
            $this->amount + $other->amount,
            $this->currency
        );
    }

    public function format(): string
    {
        $amount = $this->amount / 100;
        return number_format($amount, 2) . ' ' . $this->currency;
    }

    public function equals(Money $other): bool
    {
        return $this->amount === $other->amount
            && $this->currency === $other->currency;
    }

    private function assertSameCurrency(Money $other): void
    {
        if ($this->currency !== $other->currency) {
            throw new InvalidArgumentException(
                "Cannot operate on different currencies: {$this->currency} and {$other->currency}"
            );
        }
    }
}

// Usage in domain models
final class Product
{
    public function __construct(
        private readonly ProductId $id,
        private string $name,
        private Money $price,
    ) {}

    public function changePrice(Money $newPrice): void
    {
        $this->price = $newPrice;
    }

    public function calculateTax(float $taxRate): Money
    {
        $taxAmount = ($this->price->amount * $taxRate) / 100;

        return Money::fromAmount(
            $taxAmount / 100,
            $this->price->currency
        );
    }
}

// ❌ Bad: Primitive obsession
final class Product
{
    public function __construct(
        private readonly int $id,
        private string $name,
        private float $price,  // What currency? Validated?
        private string $email, // Validated? Normalized?
    ) {}
}
```

### Common Value Objects

- `Email` - Email addresses with validation
- `Money` - Monetary amounts with currency
- `Url` - URLs with validation
- `PhoneNumber` - Phone numbers with formatting
- `Address` - Physical addresses
- `DateRange` - Start and end dates with validation
- `Percentage` - Percentage values with bounds checking
- `Uuid` - UUIDs with validation
- `Slug` - URL-friendly slugs

### When to Use Value Objects

- Domain concepts that are defined by their value (not identity)
- Values that require validation or formatting
- Values with behavior or business rules
- Replacing primitive types (avoid primitive obsession)
- Ensuring type safety for specific concepts (Email vs. generic string)

### Value Object vs DTO

- **Value Objects**: Domain concepts with behavior and validation, immutable
- **DTOs**: Data containers for transfer between layers, no business logic
- Value Objects live in your domain layer
- DTOs live in your application/interface layer

### Value Object Naming

- Use domain language: `Email`, `Money`, `Address`
- Don't suffix with `ValueObject` - the name should be the domain concept
- Group in domain-specific namespaces: `App\Domain\Shared\ValueObjects\`
