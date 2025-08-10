# Actions Pattern

## Services / Actions

A service layer in your application centralizes business logic in a single place, separate from controllers and models.
This improves code organization, maintainability, and testability by enforcing a single responsibility for each layer of
your application.

- Shared Logic: When multiple controllers or parts of the application need to perform the same complex operation.
- Decoupling: To keep controllers lightweight by moving business logic elsewhere.
- Testability: Services are easier to mock and test independently.

### Example: A User Service

Imagine you need a service to handle user-related operations, such as creating a new user and sending a welcome email.

```php
namespace App\Services;

use App\Models\User;
use Illuminate\Support\Facades\Mail;

class UserService
{
    public function createUser(array $data): User
    {
        // Create a user
        $user = User::create($data);

        // Send a welcome email
        Mail::to($user->email)->send(new WelcomeEmail($user));

        return $user;
    }
}
```

Another popular approach to using service classes are action classes. Action Classes are single-purpose classes designed
to encapsulate a specific piece of functionality or action. They provide a lightweight way to organize logic into
reusable, testable units.

### When to Use Actions

- **Single Responsibility**: To encapsulate a single, well-defined task.
- **Reusability**: When an action might be called from multiple parts of the application.
- **Simplifying Controllers**: Like services, they keep controllers clean but at a more granular level.

```php
namespace App\Actions;

use App\Models\User;
use Illuminate\Support\Facades\Mail;

class CreateUserAction
{
    public function execute(array $data): User
    {
        // Create a user
        $user = User::create($data);

        // Send a welcome email
        Mail::to($user->email)->send(new WelcomeEmail($user));

        return $user;
    }
}
```

Then in your controller you can use the action class like this.

```php
namespace App\Http\Controllers;

use App\Actions\CreateUserAction;
use Illuminate\Http\Request;

class UserController extends Controller
{
    public function __construct(private CreateUserAction $createUserAction) {}

    public function store(Request $request)
    {
        $data = $request->validate([
            'name' => 'required|string',
            'email' => 'required|email|unique:users',
            'password' => 'required|string|min:8',
        ]);

        $user = $this->createUserAction->execute($data);

        return response()->json($user, 201);
    }
}
```
