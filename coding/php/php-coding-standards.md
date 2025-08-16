# PHP Code Standards

This document serves as a comprehensive guide for writing clean, maintainable PHP code. It follows industry standards and PSR guidelines established by the PHP community and leading developers. These standards are framework-agnostic and apply to any PHP project.

## Table of Contents

1. [Core Principles](#core-principles)
2. [PHP Code Style](#php-code-style)
3. [Type Declarations](#type-declarations)
4. [Classes and Object-Oriented Code](#classes-and-object-oriented-code)
5. [Control Structures](#control-structures)
6. [Documentation and Comments](#documentation-and-comments)
7. [Testing Standards](#testing-standards)
8. [Common Anti-Patterns to Avoid](#common-anti-patterns-to-avoid)

## Core Principles

### Write Expressive Code

Code should be self-documenting. If you need a comment to explain what code does, consider refactoring:

```php
// ✅ Good: Method name explains the intent
public function calculateMonthlyInterest(): float
{
    return $this->principal * $this->rate / 12;
}

// ❌ Bad: Requires comment to understand
public function calculate(): float
{
    // Calculate monthly interest
    return $this->principal * $this->rate / 12;
}
```

## PHP Code Style

### PSR Standards Compliance

Follow PSR-1, PSR-2, and PSR-12 standards. Use camelCase for everything that's not public-facing:

```php
// ✅ Good
class UserRegistrationService
{
    private string $apiKey;

    public function registerUser(array $userData): User
    {
        // Implementation
    }
}

// ❌ Bad
class user_registration_service
{
    private $api_key;

    public function register_user($user_data)
    {
        // Implementation
    }
}
```

### String Handling

Prefer string interpolation over concatenation:

```php
// ✅ Good
$greeting = "Hello, {$user->name}! Welcome to {$application->name}.";

// ❌ Bad
$greeting = 'Hello, ' . $user->name . '! Welcome to ' . $application->name . '.';
```

### Whitespace for Readability

Statements should be allowed to breathe. Add blank lines between logical sections:

```php
// ✅ Good
public function processOrder(Order $order): bool
{
    $paymentResult = $this->paymentService->charge($order);

    if (!$paymentResult->isSuccessful()) {
        $this->logger->error('Payment failed', ['order_id' => $order->id]);
        return false;
    }

    $order->markAsPaid();
    $this->emailService->sendConfirmation($order);

    return true;
}

// ❌ Bad: Everything cramped together
public function processOrder(Order $order): bool
{
    $paymentResult = $this->paymentService->charge($order);
    if (!$paymentResult->isSuccessful()) {
        $this->logger->error('Payment failed', ['order_id' => $order->id]);
        return false;
    }
    $order->markAsPaid();
    $this->emailService->sendConfirmation($order);
    return true;
}
```

## Type Declarations

### Always Use Type Hints

Type everything whenever possible. Don't rely on docblocks for basic types:

```php
// ✅ Good
class UserService
{
    public function findByEmail(string $email): ?User
    {
        return User::where('email', $email)->first();
    }
}

// ❌ Bad
class UserService
{
    /**
     * @param string $email
     * @return User|null
     */
    public function findByEmail($email)
    {
        return User::where('email', $email)->first();
    }
}
```

### Nullable Types

Use short nullable notation:

```php
// ✅ Good
public ?string $description;
public function getUser(): ?User

// ❌ Bad
public string|null $description;
public function getUser(): User|null
```

### Void Return Types

If a method returns nothing, indicate it with `void`:

```php
// ✅ Good
public function sendNotification(User $user): void
{
    Mail::to($user)->send(new WelcomeEmail());
}

// ❌ Bad
public function sendNotification(User $user)
{
    Mail::to($user)->send(new WelcomeEmail());
}
```

### Constructor Property Promotion

Use constructor property promotion when all properties can be promoted:

```php
// ✅ Good
class CreateUserAction
{
    public function __construct(
        private readonly UserRepository $userRepository,
        private readonly EmailService $emailService,
        private readonly Logger $logger,
    ) {}
}

// ❌ Bad
class CreateUserAction
{
    private UserRepository $userRepository;
    private EmailService $emailService;
    private Logger $logger;

    public function __construct(
        UserRepository $userRepository,
        EmailService $emailService,
        Logger $logger
    ) {
        $this->userRepository = $userRepository;
        $this->emailService = $emailService;
        $this->logger = $logger;
    }
}
```

## Classes and Object-Oriented Code

### Single Responsibility Principle

Each class should have one reason to change:

```php
// ✅ Good: Separate concerns
class UserRegistrationService
{
    public function __construct(
        private UserRepository $userRepository,
        private EmailService $emailService,
    ) {}

    public function register(array $userData): User
    {
        $user = $this->userRepository->create($userData);
        $this->emailService->sendWelcomeEmail($user);

        return $user;
    }
}

class EmailService
{
    public function sendWelcomeEmail(User $user): void
    {
        // Use appropriate email service (mail(), PHPMailer, SwiftMailer, etc.)
        $this->mailer->send(new WelcomeEmail($user));
    }
}

// ❌ Bad: Mixed concerns
class UserRegistrationService
{
    public function register(array $userData): User
    {
        // Database logic
        $user = new User($userData);
        $user->save();

        // Email logic
        $emailContent = "Welcome {$user->name}...";
        mail($user->email, 'Welcome', $emailContent);

        // Logging logic
        file_put_contents('log.txt', "User {$user->id} registered\n", FILE_APPEND);

        return $user;
    }
}
```

### Traits Usage

Each trait should go on its own line:

```php
// ✅ Good
class User
{
    use TimestampsTrait;
    use ValidationTrait;
    use SerializableTrait;
}

// ❌ Bad
class User
{
    use TimestampsTrait, ValidationTrait, SerializableTrait;
}
```

### Enums

Use PascalCase for enum values:

```php
// ✅ Good
enum UserStatus: string
{
    case Active = 'active';
    case Inactive = 'inactive';
    case Suspended = 'suspended';
}

// ❌ Bad
enum UserStatus: string
{
    case active = 'active';
    case inactive = 'inactive';
    case suspended = 'suspended';
}
```

## Control Structures

### Happy Path Pattern

Structure conditionals with the unhappy path first:

```php
// ✅ Good: Happy path last and unindented
public function processPayment(Payment $payment): bool
{
    if (!$payment->isValid()) {
        $this->logger->error('Invalid payment data');
        return false;
    }

    if (!$this->hasInsufficientFunds($payment)) {
        $this->logger->error('Insufficient funds');
        return false;
    }

    // Happy path - main logic
    $this->chargeCard($payment);
    $this->updateBalance($payment);
    $this->sendConfirmation($payment);

    return true;
}

// ❌ Bad: Nested conditions
public function processPayment(Payment $payment): bool
{
    if ($payment->isValid()) {
        if ($this->hasInsufficientFunds($payment)) {
            $this->chargeCard($payment);
            $this->updateBalance($payment);
            $this->sendConfirmation($payment);
            return true;
        } else {
            $this->logger->error('Insufficient funds');
            return false;
        }
    } else {
        $this->logger->error('Invalid payment data');
        return false;
    }
}
```

### Avoid Else Statements

Prefer early returns over else statements:

```php
// ✅ Good
public function calculateDiscount(User $user): float
{
    if (!$user->isPremium()) {
        return 0.0;
    }

    if ($user->yearlyPurchases() > 10000) {
        return 0.15;
    }

    return 0.10;
}

// ❌ Bad
public function calculateDiscount(User $user): float
{
    if ($user->isPremium()) {
        if ($user->yearlyPurchases() > 10000) {
            return 0.15;
        } else {
            return 0.10;
        }
    } else {
        return 0.0;
    }
}
```

### Separate Compound Conditions

Break complex conditions into separate if statements:

```php
// ✅ Good
public function canAccessResource(User $user, Resource $resource): bool
{
    if (!$user->isActive()) {
        return false;
    }

    if (!$resource->isPublished()) {
        return false;
    }

    if (!$this->hasPermission($user, $resource)) {
        return false;
    }

    return true;
}

// ❌ Bad
public function canAccessResource(User $user, Resource $resource): bool
{
    return $user->isActive()
        && $resource->isPublished()
        && $this->hasPermission($user, $resource);
}
```

## Documentation and Comments

### When to Use Docblocks

Only use docblocks when they provide additional context:

```php
// ✅ Good: Docblock adds value for complex return types
/**
 * @return array<int, User>
 */
public function getActiveUsers(): array
{
    return array_filter($this->users, fn($user) => $user->isActive());
}

// ✅ Good: No docblock needed - method signature is clear
public function createUser(string $email, string $name): User
{
    return new User($email, $name);
}

// ❌ Bad: Redundant docblock
/**
 * Create a user with email and name
 *
 * @param string $email
 * @param string $name
 * @return User
 */
public function createUser(string $email, string $name): User
{
    return new User($email, $name);
}
```

### Comment Guidelines

Comments should explain **why**, not **what**:

```php
// ✅ Good: Explains the business reason
public function calculateShipping(): float
{
    // Free shipping for orders over $100 to encourage larger purchases
    if ($this->total > 100) {
        return 0;
    }

    return 5.99;
}

// ❌ Bad: Explains what the code does (obvious)
public function calculateShipping(): float
{
    // Check if total is greater than 100
    if ($this->total > 100) {
        // Return zero
        return 0;
    }

    // Return shipping cost
    return 5.99;
}
```

## Testing Standards

### Test Organization

Keep test classes focused and well-organized:

```php
class UserServiceTest extends PHPUnit\Framework\TestCase
{
    private UserService $userService;

    protected function setUp(): void
    {
        parent::setUp();
        $this->userService = new UserService();
    }

    public function testCreatesUserWithValidData(): void
    {
        // Arrange
        $userData = [
            'name' => 'John Doe',
            'email' => 'john@example.com',
            'password' => 'password123',
        ];

        // Act
        $user = $this->userService->createUser($userData);

        // Assert
        $this->assertInstanceOf(User::class, $user);
        $this->assertEquals('John Doe', $user->getName());
        $this->assertEquals('john@example.com', $user->getEmail());
    }

    public function testSendsWelcomeEmailAfterCreatingUser(): void
    {
        // Test implementation using mocks for email service
        $emailService = $this->createMock(EmailService::class);
        $emailService->expects($this->once())
            ->method('sendWelcomeEmail');

        $userService = new UserService($emailService);

        $userData = [
            'name' => 'John Doe',
            'email' => 'john@example.com',
            'password' => 'password123',
        ];

        $userService->createUser($userData);
    }
}
```

### Test Method Naming

Use descriptive test method names that explain the scenario:

```php
// ✅ Good: Descriptive test names
public function testThrowsExceptionWhenEmailAlreadyExists(): void {}
public function testReturnsNullWhenUserNotFound(): void {}
public function testCalculatesDiscountCorrectlyForPremiumUsers(): void {}

// ❌ Bad: Unclear test names
public function testUserCreation(): void {}
public function testEmail(): void {}
public function testDiscount(): void {}
```

## Common Anti-Patterns to Avoid

### God Classes

Avoid classes that try to do everything:

```php
// ❌ Bad: God class
class UserManager
{
    public function createUser($data) {}
    public function deleteUser($id) {}
    public function sendEmail($user) {}
    public function processPayment($user, $amount) {}
    public function generateReport($user) {}
    public function validateData($data) {}
    // ... 50 more methods
}

// ✅ Good: Separate responsibilities
class UserService
{
    public function createUser(array $data): User {}
    public function deleteUser(int $id): void {}
}

class EmailService
{
    public function sendWelcomeEmail(User $user): void {}
}

class PaymentService
{
    public function processPayment(User $user, float $amount): bool {}
}
```

### Magic Numbers and Strings

Use constants or configuration for magic values:

```php
// ✅ Good
class ShippingCalculator
{
    private const FREE_SHIPPING_THRESHOLD = 100.00;
    private const STANDARD_SHIPPING_RATE = 5.99;

    public function calculateShipping(float $orderTotal): float
    {
        if ($orderTotal >= self::FREE_SHIPPING_THRESHOLD) {
            return 0;
        }

        return self::STANDARD_SHIPPING_RATE;
    }
}

// ❌ Bad
class ShippingCalculator
{
    public function calculateShipping(float $orderTotal): float
    {
        if ($orderTotal >= 100.00) {
            return 0;
        }

        return 5.99;
    }
}
```

### Primitive Obsession

Use value objects instead of primitive types for domain concepts:

```php
// ✅ Good: Value objects
class Email
{
    public function __construct(private string $value)
    {
        if (!filter_var($value, FILTER_VALIDATE_EMAIL)) {
            throw new InvalidArgumentException('Invalid email format');
        }
    }

    public function getValue(): string
    {
        return $this->value;
    }

    public function getDomain(): string
    {
        return substr($this->value, strpos($this->value, '@') + 1);
    }
}

class User
{
    public function __construct(
        private string $name,
        private Email $email,
    ) {}
}

// ❌ Bad: Primitive obsession
class User
{
    public function __construct(
        private string $name,
        private string $email, // Just a string, no validation or behavior
    ) {}
}
```

---

## Conclusion

Writing clean PHP code is an ongoing practice that requires discipline and attention to detail. These guidelines provide a foundation for maintainable PHP applications across any framework or project. Remember:

1. **Consistency is key** - Follow the same patterns throughout your codebase
2. **Readability first** - Code is read more often than it's written
3. **Follow PSR standards** - Leverage established PHP community standards
4. **Test your code** - Clean code is testable code
5. **Refactor regularly** - Code quality requires continuous improvement

For more advanced topics and patterns, consider exploring:

-   [PSR Standards](https://www.php-fig.org/psr/)
-   [Clean Code by Robert C. Martin](https://www.amazon.com/Clean-Code-Handbook-Software-Craftsmanship/dp/0132350884)
-   [PHP The Right Way](https://phptherightway.com/)

Remember: "Any fool can write code that a computer can understand. Good programmers write code that humans can understand." — Martin Fowler
