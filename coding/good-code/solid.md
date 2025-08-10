# SOLID Principles

SOLID is a set of principles that can help you design and write better object-oriented code. These principles were
first introduced by Robert C. Martin, also known as "Uncle Bob".

The SOLID principles are:

- Single Responsibility Principle (SRP)
- Open-Closed Principle (OCP)
- Liskov Substitution Principle (LSP)
- Interface Segregation Principle (ISP)
- Dependency Inversion Principle (DIP)

Let's take a closer look at each of these principles.

## Single Responsibility Principle (SRP)

The Single Responsibility Principle states that a class should have only one reason to change.

In other words, a class should have only one responsibility or job. This helps to keep your code modular and easy to maintain.

For example, consider a class that handles both the creation and validation of user accounts.

This violates the Single Responsibility Principle, as these are two separate responsibilities.

A better approach would be to create separate classes for account creation and validation.

Here's an example of a class that violates the Single Responsibility Principle:

```php
class User
{
    public function create($data)
    {
        // Create user account
    }

    public function validate($data)
    {
        // Validate user account
    }
}
```

In this example, the UserAccount class handles both the creation and validation of user accounts. A better approach
would be to create separate classes for these responsibilities:

```php
class UserAccountCreator
{
    public function create($data)
    {
        // code to create user account
    }
}

class UserAccountValidator
{
    public function validate($data)
    {
        // code to validate user account
    }
}
```

## Open-Closed Principle (OCP)

The Open-Closed Principle states that a class should be open for extension but closed for modification.

In other words, you should be able to add new functionality to a class without changing its existing code.

To follow this principle in PHP, you can use inheritance and polymorphism.

For example, you can create a base class that defines a set of common methods, and then create subclasses that override
or extend these methods as needed. For example, consider the following base class:

```php
abstract class Shape
{
    abstract public function area();
}
```

You can create subclasses for specific shapes, such as a rectangle and a circle, that override the `area()` method to provide the correct implementation:

```php
class Rectangle extends Shape
{
    private $width;
    private $height;

    public function __construct($width, $height)
    {
        $this->width = $width;
        $this->height = $height;
    }

    public function area()
    {
        return $this->width * $this->height;
    }
}

class Circle extends Shape
{
    private $radius;

    public function __construct($radius)
    {
        $this->radius = $radius;
    }

    public function area()
    {
        return pi() * pow($this->radius, 2);
    }
}
```

Now you can create objects of these classes and call the area() method without modifying the base Shape class.

## Liskov Substitution Principle (LSP)

The Liskov Substitution Principle states that subtypes must be substitutable for their base types.

In other words, if a class is a subtype of another class, it should be able to be used in the same way as the base class without any issues.

To follow this principle in PHP, you should make sure that your subclasses do not introduce any new behavior that is not present in the base class.

This helps to ensure that your code is predictable and easy to use.

```php
class Rectangle
{
    protected $width;
    protected $height;

    public function setWidth($width)
    {
        $this->width = $width;
    }

    public function setHeight($height)
    {
        $this->height = $height;
    }

    public function getArea()
    {
        return $this->width * $this->height;
    }
}

class Square extends Rectangle
{
    public function setWidth($width)
    {
        $this->width = $width;
        $this->height = $width;
        return $width;
    }

    public function setHeight($height)
    {
        $this->width = $height;
        $this->height = $height;
        return $height;
    }
}
```

In this example, the Square class is a subtype of the Rectangle class. However, the Square class violates the
Liskov Substitution Principle because it changes the behavior of the `setWidth()` and `setHeight()` methods.

A better approach would be to define the Square class as follows:

```php
class Square extends Rectangle
{
    public function setWidth($width)
    {
        $this->width = $width;
        $this->height = $width;
    }

    public function setHeight($height)
    {
        $this->width = $height;
        $this->height = $height;
    }
}
```

This way, the Square class is substitutable for the Rectangle class because it does not change the behavior of the `setWidth()` and `setHeight()` methods.

## Interface Segregation Principle (ISP)

The Interface Segregation Principle states that clients should not be forced to depend on interfaces they do not use.

In other words, a class should have a specific interface that exposes only the methods that are relevant to the class's clients.

To follow this principle in PHP, you can use interfaces to define a set of methods that a class must implement.

This helps to keep your code flexible and easy to maintain.

Here is an example of how the Interface Segregation Principle can be violated in PHP:

```php
interface Shape
{
    public function draw();
    public function resize();
    public function rotate();
}

class Circle implements Shape
{
    public function draw()
    {
        // code to draw a circle
    }

    public function resize()
    {
        // code to resize a circle
    }

    public function rotate()
    {
        // code to rotate a circle
    }
}

class Square implements Shape
{
    public function draw()
    {
        // code to draw a square
    }

    public function resize()
    {
        // code to resize a square
    }

    public function rotate()
    {
        // code to rotate a square
    }
}

class Line implements Shape
{
    public function draw()
    {
        // code to draw a line
    }

    public function resize()
    {
        // code to resize a line
    }

    public function rotate()
    {
        // code to rotate a line
    }
}
```

In this example, the `Shape` interface defines three methods: `draw()`, `resize()`, and `rotate()`.

However, not all shapes need to support all of these methods. For example, a `line` only needs to support
the `draw()` method and does not need to support resizing or rotating.

A better approach would be to define separate interfaces for different sets of methods:

```php
interface Drawable
{
    public function draw();
}

interface Resizable
{
    public function resize();
}

interface Rotatable
{
    public function rotate();
}

class Circle implements Drawable, Resizable, Rotatable
{
    public function draw()
    {
        // code to draw a circle
    }

    public function resize()
    {
        // code to resize a circle
    }

    public function rotate()
    {
        // code to rotate a circle
    }
}

class Square implements Drawable, Resizable, Rotatable
{
    public function draw()
    {
        // code to draw a square
    }

    public function resize()
    {
        // code to resize a square
    }

    public function rotate()
    {
        // code to rotate a square
    }
}

class LineSegment implements Drawable
{
    public function draw()
    {
        // code to draw a line segment
    }
}
```

This way, each class only implements the interfaces that are relevant to its clients, which follows the Interface Segregation Principle.

## Dependency Inversion Principle (DIP)

The Dependency Inversion Principle states that high-level modules should not depend on low-level modules. Both should depend on abstractions.

In other words, your code should depend on abstractions rather than concrete implementations.

To follow this principle in PHP, you can use dependency injection to decouple your code from specific implementations.

This helps to make your code more flexible and easier to test.

Here is an example of how dependency injection can be used in PHP:

```php
interface Mailer
{
    public function send($to, $subject, $body);
}

class SmtpMailer implements Mailer
{
    private $host;
    private $port;
    private $username;
    private $password;

    public function __construct($host, $port, $username, $password)
    {
        $this->host = $host;
        $this->port = $port;
        $this->username = $username;
        $this->password = $password;
    }

    public function send($to, $subject, $body)
    {
        // code to send email using SMTP
    }
}

class UserMailer
{
    private $mailer;

    public function __construct(Mailer $mailer)
    {
        $this->mailer = $mailer;
    }

    public function sendWelcomeEmail($to, $name)
    {
        $subject = 'Welcome to our site';
        $body = 'Hi ' . $name . ', welcome to our site!';
        $this->mailer->send($to, $subject, $body);
    }
}
```

In this example, the `SmtpMailer` class is responsible for sending emails using SMTP. The `UserMailer` class uses the `SmtpMailer`
class to send a welcome email to new users.

The `UserMailer` class depends on the `SmtpMailer` class, but it is not tightly coupled to it. This is because the
`UserMailer` class depends on the `Mailer` interface rather than the `SmtpMailer` implementation. This allows us to
use different implementations of the `Mailer` interface, such as a mock mailer for testing purposes.

To use dependency injection in this example, we can create an instance of the `SmtpMailer` class and pass it to the
constructor of the `UserMailer` class:

```php
$mailer = new SmtpMailer('smtp.example.com', 587, 'user@example.com', 'password');
$userMailer = new UserMailer($mailer);
```

In conclusion, the SOLID principles are a set of guidelines that can help you design and write better object-oriented code.

By following these principles, you can create code that is more modular, flexible, and maintainable.
