# Adapter Design Pattern

The Adapter Design Pattern is a structural design pattern that allows objects with incompatible interfaces to work
together. The pattern acts as a bridge between two incompatible interfaces by providing a wrapper that translates the
interface of one object into an interface that the client expects.

## Problem

Consider a scenario where you have a client that expects a specific interface to interact with an object. However, the
object you want to use does not implement the required interface. Modifying the object's interface to match the client's
expectations is not feasible due to various reasons, such as the object being part of a third-party library or having a
complex interface that cannot be easily changed.

## Solution

The Adapter Design Pattern solves this problem by providing a wrapper class that implements the client's interface and
delegates the calls to the adapted object. The adapter class translates the calls from the client's interface to the
interface of the adapted object, allowing them to work together seamlessly.

The Adapter Design Pattern consists of the following components:

- **Target**: Defines the interface that the client expects.
- **Adapter**: Implements the Target interface and wraps the adapted object. It translates the calls from the client's
  interface to the interface of the adapted object.
- **Adaptee**: Represents the object that needs to be adapted. It has an incompatible interface that cannot be directly
  used by the client.
- **Client**: Interacts with the Target interface to use the adapted object through the Adapter.

## Example

Let's implement the Adapter Design Pattern using an example of a legacy payment gateway that has an incompatible
interface
with a new payment processing system. We will create an `IPaymentGateway` interface that defines the methods expected by
the new payment processing system. The legacy payment gateway has a different interface that needs to be adapted to work
with the new system.

```php
<?php

interface IPaymentGateway
{
    public function processPayment(float $amount): void;
}

class LegacyPaymentGateway
{
    public function pay(float $amount): void
    {
        echo "Processing payment of \${$amount} using the legacy payment gateway.\n";
    }
}

class PaymentGatewayAdapter implements IPaymentGateway
{
    private $legacyPaymentGateway;

    public function __construct(LegacyPaymentGateway $legacyPaymentGateway)
    {
        $this->legacyPaymentGateway = $legacyPaymentGateway;
    }

    public function processPayment(float $amount): void
    {
        $this->legacyPaymentGateway->pay($amount);
    }
}

// Client code
$legacyPaymentGateway = new LegacyPaymentGateway();
$paymentGatewayAdapter = new PaymentGatewayAdapter($legacyPaymentGateway);
$paymentGatewayAdapter->processPayment(100.0);
```

In this example, the `IPaymentGateway` interface defines the methods expected by the new payment processing system. The
`LegacyPaymentGateway` class represents the legacy payment gateway with an incompatible interface. We create a
`PaymentGatewayAdapter` class that implements the `IPaymentGateway` interface and wraps the `LegacyPaymentGateway`
object.
The adapter class translates the calls from the new system's interface to the legacy payment gateway's interface.

The client code creates an instance of the `LegacyPaymentGateway` and adapts it using the `PaymentGatewayAdapter`. The
client interacts with the adapted object through the `IPaymentGateway` interface, allowing the legacy payment gateway to
work with the new payment processing system.

## When To Use

The above example is a good example of when to use this pattern, it should be used when you want to standardize the
interface of an existing class, or when you want to create a reusable class that can work with multiple classes that
have different interfaces. The Adapter pattern is also useful when you want to create a class that can work with classes
that are not known at compile time.

## Advantages of the Adapter Pattern

The Adapter Design Pattern provides several advantages:

- **Seamless integration**: The Adapter allows objects with incompatible interfaces to work together seamlessly without
  modifying their existing code.
- **Reusability**: The Adapter can be reused to adapt multiple objects with different interfaces to work with the same
  client code.
- **Flexibility**: The Adapter provides flexibility by allowing the client to interact with the adapted object through a
  common interface.
- **Encapsulation**: The Adapter encapsulates the translation of calls between the client's interface and the adapted
  object's interface, keeping the client code clean and simple.
- **Maintainability**: The Adapter pattern improves maintainability by isolating the changes required to adapt an object
  to a separate class.

## Related Patterns

- **Bridge Pattern**: The Adapter and Bridge patterns are similar in that they both involve wrapping an object to
  provide a different interface. However, the Adapter pattern is used to make two incompatible interfaces work together,
  while the Bridge pattern is used to separate an abstraction from its implementation.
- **Decorator Pattern**: The Adapter and Decorator patterns are similar in that they both involve wrapping an object to
  provide additional functionality. However, the Adapter pattern is used to provide a different interface to an object,
  while the Decorator pattern is used to add new behavior to an object without changing its interface.
- **Facade Pattern**: The Adapter and Facade patterns are similar in that they both involve providing a simplified
  interface to a complex system. However, the Adapter pattern is used to make two incompatible interfaces work together,
  while the Facade pattern is used to provide a unified interface to a set of interfaces in a subsystem.
- **Proxy Pattern**: The Adapter and Proxy patterns are similar in that they both involve wrapping an object to provide
  additional functionality. However, the Adapter pattern is used to provide a different interface to an object, while the
  Proxy pattern is used to control access to an object.
- **Composite Pattern**: The Adapter and Composite patterns are similar in that they both involve wrapping objects to
  provide a unified interface. However, the Adapter pattern is used to make two incompatible interfaces work together,
  while the Composite pattern is used to treat individual objects and compositions of objects uniformly.

## Conclusion

The Adapter Design Pattern is a powerful pattern that allows objects with incompatible interfaces to work together. By
providing a wrapper that translates the interface of one object into an interface that the client expects, the Adapter
pattern enables seamless integration between different systems. The pattern is widely used in software development to
standardize interfaces, improve reusability, and simplify code maintenance.
