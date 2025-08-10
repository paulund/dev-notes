# Value Objects

A value object is a design pattern in software development that represents a small, immutable object with no identity,
defined solely by its attributes. It is primarily used to model domain-specific concepts in a way that enhances clarity,
maintainability, and correctness in code.

### Immutability

Once created, a value object cannot be changed. If you need a new value, you create a new object. This makes them
predictable and thread-safe.

### Equality Based on Values

Two value objects are considered equal if their attributes (or values) are the same, regardless of whether they are the
same instance.

### No Identity

Unlike entities, value objects do not have a unique identifier. They are interchangeable if they have the same value.

### Encapsulation of Business Logic

Value objects often include logic relevant to their values. For example, a `Money` value object might include methods to
add or subtract amounts or format its representation.

### Example Use Case: Address

An Address might be a value object because:

It represents a concept (a location) defined solely by its properties (e.g., street, city, postcode).
It does not make sense to assign a unique identifier to each address unless it's part of an entity (like a customer).
It is immutable: If an address changes, you create a new Address object rather than modifying the existing one.

```php
class Address {
private string $street;
private string $city;
private string $postcode;

    public function __construct(string $street, string $city, string $postcode) {
        $this->street = $street;
        $this->city = $city;
        $this->postcode = $postcode;
    }

    public function fromArray(array $data): self {
        return new self(
            street: $data['street'],
            city: $data['city'],
            postcode: $data['postcode']
        );
    }

    public function getStreet(): string {
        return $this->street;
    }

    public function getCity(): string {
        return $this->city;
    }

    public function getPostcode(): string {
        return $this->postcode;
    }

    public function equals(Address $other): bool {
        return $this->street === $other->street
            && $this->city === $other->city
            && $this->postcode === $other->postcode;
    }
}
```

### Benefits of Using Value Objects

- Improved Clarity: Represent domain concepts more naturally, making the code easier to understand.
- Reduced Bugs: Immutability ensures that objects aren't unexpectedly changed, avoiding subtle bugs.
- Encapsulation: Keeps related business logic within the object.

By using value objects effectively, you can create clean, maintainable, and expressive code structures that align with
the principles of domain-driven design (DDD).
