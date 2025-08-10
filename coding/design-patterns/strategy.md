# Strategy Design Pattern

The Strategy Design Pattern is a behavioral design pattern that allows you to define a family of algorithms, encapsulate
each algorithm, and make them interchangeable. The pattern enables the client to choose the algorithm to use at runtime
without modifying the client code. This pattern is useful when you have multiple algorithms that perform similar tasks
but differ in their implementation.

## Problem

Consider a scenario where you have a class that represents a payment processor. The payment processor can process
payments using different payment gateways, such as PayPal, Stripe, and Authorize.Net. Each payment gateway has its
implementation for processing payments. Implementing this behavior using conditional statements can lead to a complex
and hard-to-maintain codebase.

## Solution

The Strategy Design Pattern solves this problem by defining a family of algorithms, encapsulating each algorithm into a
separate class, and making them interchangeable. The client can choose the algorithm to use at runtime without modifying
the client code.

The solution will allow the payment processor to use different payment gateways interchangeably without modifying the
payment processor class. Each payment gateway will have its implementation for processing payments that have a shared interface
to be able to be interacted with in the same way.

```php
<?php

namespace Strategy;

interface PaymentGateway
{
    public function processPayment(float $amount): void;
}

class PayPal implements PaymentGateway
{
    public function processPayment(float $amount): void
    {
        echo "Processing payment of \${$amount} using PayPal.\n";
    }
}

class Stripe implements PaymentGateway
{
    public function processPayment(float $amount): void
    {
        echo "Processing payment of \${$amount} using Stripe.\n";
    }
}

class AuthorizeNet implements PaymentGateway
{
    public function processPayment(float $amount): void
    {
        echo "Processing payment of \${$amount} using Authorize.Net.\n";
    }
}

class PaymentProcessor
{
    private PaymentGateway $paymentGateway;

    public function __construct(PaymentGateway $paymentGateway)
    {
        $this->paymentGateway = $paymentGateway;
    }

    public function processPayment(float $amount): void
    {
        $this->paymentGateway->processPayment($amount);
    }
}

// Usage
$paymentProcessor = new PaymentProcessor(new PayPal());
$paymentProcessor->processPayment(100.00);

$paymentProcessor = new PaymentProcessor(new Stripe());
$paymentProcessor->processPayment(200.00);

$paymentProcessor = new PaymentProcessor(new AuthorizeNet());
$paymentProcessor->processPayment(300.00);
```

In the example above, we have defined a `PaymentGateway` interface that defines the contract for processing payments.
We have also defined three concrete payment gateway classes: `PayPal`, `Stripe`, and `AuthorizeNet`. Each payment gateway
class implements the `PaymentGateway` interface and provides its implementation for processing payments.

The `PaymentProcessor` class takes a `PaymentGateway` object in its constructor and uses it to process payments. The
client can choose the payment gateway to use at runtime by passing the desired payment gateway object to the
`PaymentProcessor` constructor.

## When To Use

Use the Strategy Design Pattern when you have a family of algorithms that perform similar tasks but differ in their
implementation. The pattern allows you to encapsulate each algorithm into a separate class and make them interchangeable.
It also simplifies the client code by allowing the client to choose the algorithm to use at runtime without modifying the
client code.

## Laravel Example

Laravel uses the Strategy Design Pattern in various places, such as the `Illuminate\Mail` component. The `Illuminate\Mail`
component allows you to send emails using different mail drivers, such as SMTP, Mailgun, and Sendmail. Each mail driver
implements the `Illuminate\Contracts\Mail\Mailer` interface, which defines the contract for sending emails. The client can
choose the mail driver to use at runtime without modifying the client code.

The decision to select the driver to use in done with a ENV variable in the `.env` file.

```php
MAIL_MAILER=smtp
```

This variable is then used in the `config/mail.php` file to determine which driver to use.

```php
<?php

return [
    'driver' => env('MAIL_MAILER', 'smtp'),
    // Other mail configuration options...
];
```

## Pros and Cons

### Pros

- **Simplifies code**: The Strategy Design Pattern simplifies code by encapsulating the behavior associated with each
  algorithm into separate classes.
- **Improves maintainability**: The pattern improves maintainability by allowing you to add new algorithms or change the
  behavior of existing algorithms without modifying the client code.
- **Promotes code reusability**: The pattern promotes code reusability by allowing you to reuse the algorithm classes in
  different contexts.
- **Promotes flexibility**: The pattern promotes flexibility by allowing objects to change their behavior dynamically
  based on the selected algorithm.
- **Promotes testability**: The pattern promotes testability by allowing you to test each algorithm class independently.

### Cons

- **Increased number of classes**: The Strategy Design Pattern can lead to an increased number of classes in the codebase,
  which can make the code harder to navigate and understand.
- **Complexity**: The pattern can introduce complexity, especially when dealing with a large number of algorithms or
  when the algorithms have complex interactions.
- **Potential performance impact**: The pattern may introduce a performance impact due to the overhead of managing
  multiple algorithm classes.
- **Potential tight coupling**: The pattern may lead to tight coupling between the context class and the algorithm
  classes if not implemented correctly.
- **Potential misuse**: The pattern may be misused if the client code is not designed to handle the selection of
  algorithms correctly.

## Use With Factory Pattern

You can combine the Strategy Design Pattern with the Factory Design Pattern to create a factory that produces
different strategy objects based on the client's requirements. The factory can encapsulate the logic for creating
different strategy objects and provide a simple interface for the client to obtain the desired strategy object.

```php
<?php

namespace Strategy;

interface PaymentGateway
{
    public function processPayment(float $amount): void;
}

class PayPal implements PaymentGateway
{
    public function processPayment(float $amount): void
    {
        echo "Processing payment of \${$amount} using PayPal.\n";
    }
}

class Stripe implements PaymentGateway
{
    public function processPayment(float $amount): void
    {
        echo "Processing payment of \${$amount} using Stripe.\n";
    }
}

class AuthorizeNet implements PaymentGateway
{
    public function processPayment(float $amount): void
    {
        echo "Processing payment of \${$amount} using Authorize.Net.\n";
    }
}

class PaymentGatewayFactory
{
    public static function create(string $gateway): PaymentGateway
    {
        switch ($gateway) {
            case 'paypal':
                return new PayPal();
            case 'stripe':
                return new Stripe();
            case 'authorize_net':
                return new AuthorizeNet();
            default:
                throw new \InvalidArgumentException("Invalid payment gateway: {$gateway}");
        }
    }
}

class PaymentProcessor
{
    private PaymentGateway $paymentGateway;

    public function __construct(PaymentGateway $paymentGateway)
    {
        $this->paymentGateway = $paymentGateway;
    }

    public function processPayment(float $amount): void
    {
        $this->paymentGateway->processPayment($amount);
    }
}

// Usage
$paymentGateway = PaymentGatewayFactory::create('paypal');
$paymentProcessor = new PaymentProcessor($paymentGateway);
$paymentProcessor->processPayment(100.00);
```

The Laravel example above will use the Factory Pattern to create the mail driver based on the `MAIL_MAILER` environment
variable. It does this in the `Illuminate\Mail\MailManager` class. If you're interested in how Laravel uses the Strategy I recommend
reading this class and understand how it works. [MailManager](https://github.com/illuminate/mail/blob/master/MailManager.php)

## Conclusion

The Strategy Design Pattern is a powerful pattern that allows you to define a family of algorithms, encapsulate each algorithm
into a separate class, and make them interchangeable. The pattern simplifies code, improves maintainability, promotes
code reusability, and enhances flexibility. By using the Strategy Design Pattern, you can create a flexible and
maintainable codebase that can adapt to changing requirements and new algorithms.
