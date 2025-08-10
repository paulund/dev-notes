# Data Transfer Objects (DTO)

Data Transfer Objects (DTOs) are a specific type of data object used to transfer data between layers, modules, or
services within an application. They are particularly useful in decoupling different parts of your application, enabling
clean and maintainable code structures.

### Key Characteristics of DTOs

#### Encapsulation of Data

DTOs group related properties into a single object, making it easier to pass and manage data.

#### No Business Logic

DTOs typically contain only data and minimal methods, such as getters, setters, or basic validation.

#### Separation of Concerns

DTOs separate the internal representation of data from how it is transmitted or processed.

#### Flattened Structures

DTOs often simplify complex data structures, consolidating nested data into a more manageable form for transfer.

### Benefits of Using DTOs

### Remove arrays

DTOs provide a structured way to pass data between layers, replacing arrays or raw data structures. This improves
readability and maintainability.

#### Improved Maintainability

DTOs create a clear boundary between layers (e.g., database, services, API), making it easier to manage changes without
affecting other parts of the codebase. Type hint everywhere makes it easier to understand what data is being passed
around.

#### Better Data Validation

By centralizing data in a DTO, you can validate the structure and ensure consistent formatting before processing or
transmitting.

#### Decoupling

DTOs decouple the internal representation of data (e.g., database models) from external communication (e.g., API
responses), protecting the internal structure from external changes.

#### Performance Optimization

When transmitting data (e.g., in APIs), DTOs allow you to control what data is sent, avoiding unnecessary fields.

### Example: User DTO

```php
namespace App\DTO;

class UserDTO
{
    public function __construct(
        public readonly string $name,
        public readonly string $email,
        public readonly string $password,
    ) {}

    public static function fromArray(array $data): self
    {
        return new self(
            name: $data['name'],
            email: $data['email'],
            password: $data['password']
        );
    }

    public function toArray(): array
    {
        return [
            'name' => $this->name,
            'email' => $this->email,
            'password' => $this->password,
        ];
    }
}
```

Having fromArray and toArray methods on the DTO allows you to easily convert the data to and from the DTO object.

This means that you can guarantee that the data is in the correct format when it is passed around the application.

```php
class SubscriverController extends Controller
{
    public function __invoke(Request $request)
    {
        $data = SubscriberData::fromArray($request->all());
        $subscriber = CreateSubscription::execute($data);

        ...
    }
}
```

This gives a level of predictability to the data that is being passed into the CreateSubscription action.
