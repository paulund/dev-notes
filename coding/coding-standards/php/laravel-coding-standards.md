# Laravel Coding Standards

This document focuses specifically on Laravel framework conventions and best practices. It assumes you're already familiar with [general PHP coding standards](/software-engineer-notebook/fundamentals/coding-standards/php/php-coding-standards) and builds upon those foundations with Laravel-specific guidelines.

## Table of Contents

1. [Core Laravel Principles](#core-laravel-principles)
2. [Controller Organization](#controller-organization)
3. [Request Validation](#request-validation)
4. [Eloquent and Database](#eloquent-and-database)
5. [Route Organization](#route-organization)
6. [Configuration Management](#configuration-management)
7. [Laravel Class Naming Conventions](#laravel-class-naming-conventions)
8. [Service Container and Dependency Injection](#service-container-and-dependency-injection)
9. [Laravel Testing Standards](#laravel-testing-standards)
10. [Laravel-Specific Anti-Patterns](#laravel-specific-anti-patterns)

## Core Laravel Principles

### Follow Laravel's Way

> "Laravel provides the most value when you write things the way Laravel intended you to write. If there's a documented way to achieve something, follow it." — Spatie

Always prefer Laravel's built-in solutions over custom implementations:

```php
// ✅ Good: Use Laravel's validation
$request->validate([
    'email' => 'required|email|unique:users',
    'password' => 'required|min:8',
]);

// ❌ Bad: Custom validation logic
if (empty($request->email) || !filter_var($request->email, FILTER_VALIDATE_EMAIL)) {
    throw new ValidationException('Invalid email');
}
```

### Convention Over Configuration

Laravel follows the principle of convention over configuration. Embrace Laravel's naming conventions and directory structure:

```php
// ✅ Good: Follows Laravel conventions
class User extends Model
{
    // Laravel automatically infers table name as 'users'
    // Primary key as 'id', timestamps enabled
}

// ❌ Bad: Fighting Laravel conventions
class User extends Model
{
    protected $table = 'user_table';
    protected $primaryKey = 'user_id';
    public $timestamps = false;
}
```

## Controller Organization

### Keep Controllers Slim

Controllers should orchestrate, not implement business logic. Extract complex logic to actions, services, or jobs:

```php
// ✅ Good: Controller delegates to action
class UserController extends Controller
{
    public function store(StoreUserRequest $request, CreateUserAction $action): JsonResponse
    {
        $user = $action->execute($request->validated());

        return response()->json($user, 201);
    }
}

class CreateUserAction
{
    public function __construct(
        private UserRepository $userRepository,
        private EmailService $emailService,
    ) {}

    public function execute(array $userData): User
    {
        $user = $this->userRepository->create($userData);
        $this->emailService->sendWelcomeEmail($user);

        return $user;
    }
}

// ❌ Bad: Controller doing too much
class UserController extends Controller
{
    public function store(Request $request): JsonResponse
    {
        $request->validate([
            'name' => 'required|string|max:255',
            'email' => 'required|email|unique:users',
            'password' => 'required|min:8',
        ]);

        $user = new User();
        $user->name = $request->name;
        $user->email = $request->email;
        $user->password = Hash::make($request->password);
        $user->save();

        // Send welcome email
        Mail::to($user->email)->send(new WelcomeEmail($user));

        // Log the registration
        Log::info('New user registered', ['user_id' => $user->id]);

        return response()->json($user, 201);
    }
}
```

### Resource Controllers

Use resource controllers for standard CRUD operations:

```php
// ✅ Good: Resource controller
class UserController extends Controller
{
    public function index(): View
    {
        return view('users.index', ['users' => User::paginate()]);
    }

    public function create(): View
    {
        return view('users.create');
    }

    public function store(StoreUserRequest $request): RedirectResponse
    {
        $user = User::create($request->validated());

        return redirect()->route('users.show', $user)
            ->with('success', 'User created successfully');
    }

    public function show(User $user): View
    {
        return view('users.show', compact('user'));
    }

    // ... edit, update, destroy methods
}
```

## Request Validation

### Use Form Request Classes

Always use Form Request classes for validation instead of inline validation:

```php
// ✅ Good: Dedicated Form Request
class StoreUserRequest extends FormRequest
{
    public function authorize(): bool
    {
        return $this->user()->can('create', User::class);
    }

    public function rules(): array
    {
        return [
            'name' => ['required', 'string', 'max:255'],
            'email' => ['required', 'email', 'unique:users'],
            'password' => ['required', 'min:8', 'confirmed'],
            'role' => ['required', 'in:admin,user,moderator'],
        ];
    }

    public function messages(): array
    {
        return [
            'email.unique' => 'This email address is already registered.',
            'password.min' => 'Password must be at least 8 characters long.',
        ];
    }
}

// ❌ Bad: Inline validation in controller
public function store(Request $request)
{
    $request->validate([
        'name' => 'required|string|max:255',
        'email' => 'required|email|unique:users',
        'password' => 'required|min:8|confirmed',
    ]);
}
```

### Use Array Notation for Rules

Use array notation for validation rules instead of pipe-separated strings:

```php
// ✅ Good: Array notation
public function rules(): array
{
    return [
        'email' => ['required', 'email', 'unique:users'],
        'password' => ['required', 'min:8', 'confirmed'],
        'tags' => ['array', 'max:5'],
        'tags.*' => ['string', 'max:50'],
    ];
}

// ❌ Bad: Pipe notation
public function rules(): array
{
    return [
        'email' => 'required|email|unique:users',
        'password' => 'required|min:8|confirmed',
        'tags' => 'array|max:5',
        'tags.*' => 'string|max:50',
    ];
}
```

## Eloquent and Database

### Use Eloquent Relationships

Leverage Eloquent relationships instead of manual joins:

```php
// ✅ Good: Using relationships
class User extends Model
{
    public function posts(): HasMany
    {
        return $this->hasMany(Post::class);
    }

    public function roles(): BelongsToMany
    {
        return $this->belongsToMany(Role::class);
    }
}

// Usage
$user = User::with(['posts', 'roles'])->find(1);
$userPosts = $user->posts;
$userRoles = $user->roles;

// ❌ Bad: Manual joins
$user = DB::table('users')
    ->leftJoin('posts', 'users.id', '=', 'posts.user_id')
    ->leftJoin('role_user', 'users.id', '=', 'role_user.user_id')
    ->leftJoin('roles', 'role_user.role_id', '=', 'roles.id')
    ->where('users.id', 1)
    ->first();
```

### Use Custom Query Builder Classes

Create dedicated query builder classes for complex or reusable query logic instead of cluttering models with scopes:

```php
// ✅ Good: Custom Query Builder
class UserQueryBuilder extends Builder
{
    public function active(): self
    {
        return $this->where('status', 'active');
    }

    public function verified(): self
    {
        return $this->whereNotNull('email_verified_at');
    }

    public function createdInLastDays(int $days): self
    {
        return $this->where('created_at', '>=', now()->subDays($days));
    }

    public function withPostsCount(): self
    {
        return $this->withCount('posts');
    }

    public function premiumUsers(): self
    {
        return $this->whereHas('subscriptions', function ($query) {
            $query->where('type', 'premium')
                  ->where('expires_at', '>', now());
        });
    }
}

// Model setup
class User extends Model
{
    public function newEloquentBuilder($query): UserQueryBuilder
    {
        return new UserQueryBuilder($query);
    }
}

// Usage - Clean and chainable
$activeUsers = User::query()->active()->verified()->get();
$recentPremiumUsers = User::query()
    ->active()
    ->premiumUsers()
    ->createdInLastDays(30)
    ->withPostsCount()
    ->get();

// ❌ Bad: Repeated query logic scattered throughout codebase
$activeUsers = User::where('status', 'active')
    ->whereNotNull('email_verified_at')
    ->get();

$recentPremiumUsers = User::where('status', 'active')
    ->whereHas('subscriptions', function ($query) {
        $query->where('type', 'premium')
              ->where('expires_at', '>', now());
    })
    ->where('created_at', '>=', now()->subDays(30))
    ->withCount('posts')
    ->get();
```

Benefits of custom query builders:

-   **Centralized Logic**: All query logic for a model in one place
-   **Better IDE Support**: Full autocomplete and type hints
-   **Easier Testing**: Query builders can be unit tested independently
-   **Reusability**: Complex queries can be easily reused across the application
-   **Maintainability**: Changes to query logic only need to be made in one place

### Use Accessors and Mutators

Handle data transformation at the model level:

```php
// ✅ Good: Accessors and mutators
class User extends Model
{
    protected function firstName(): Attribute
    {
        return Attribute::make(
            get: fn($value) => ucfirst($value),
            set: fn($value) => strtolower($value),
        );
    }

    protected function fullName(): Attribute
    {
        return Attribute::make(
            get: fn() => "{$this->first_name} {$this->last_name}",
        );
    }
}

// Usage
$user = new User();
$user->first_name = 'JOHN'; // Stored as 'john'
echo $user->first_name; // Outputs 'John'
echo $user->full_name; // Outputs 'John Doe'
```

## Route Organization

### Use Route Model Binding

Let Laravel resolve model instances automatically:

```php
// ✅ Good: Route model binding
Route::get('users/{user}', [UserController::class, 'show']);

class UserController extends Controller
{
    public function show(User $user): View
    {
        return view('users.show', compact('user'));
    }
}

// ❌ Bad: Manual model resolution
Route::get('users/{id}', [UserController::class, 'show']);

class UserController extends Controller
{
    public function show(int $id): View
    {
        $user = User::findOrFail($id);
        return view('users.show', compact('user'));
    }
}
```

### Use Named Routes

Always name your routes and use route helpers:

```php
// ✅ Good: Named routes with tuple notation
Route::get('users', [UserController::class, 'index'])->name('users.index');
Route::post('users', [UserController::class, 'store'])->name('users.store');
Route::get('users/{user}', [UserController::class, 'show'])->name('users.show');

// In views
<a href="{{ route('users.show', $user) }}">View User</a>

// In controllers
return redirect()->route('users.index');

// ❌ Bad: Unnamed routes with string notation
Route::get('users', 'UserController@index');
Route::post('users', 'UserController@store');

// In views
<a href="/users/{{ $user->id }}">View User</a>

// In controllers
return redirect('/users');
```

### Group Related Routes

Use route groups for organization and middleware:

```php
// ✅ Good: Organized route groups
Route::middleware(['auth', 'verified'])->group(function () {
    Route::prefix('admin')->name('admin.')->middleware('can:access-admin')->group(function () {
        Route::resource('users', Admin\UserController::class);
        Route::resource('posts', Admin\PostController::class);
    });

    Route::prefix('profile')->name('profile.')->group(function () {
        Route::get('/', [ProfileController::class, 'show'])->name('show');
        Route::put('/', [ProfileController::class, 'update'])->name('update');
    });
});

// API routes
Route::prefix('api/v1')->name('api.v1.')->middleware('api')->group(function () {
    Route::apiResource('users', Api\V1\UserController::class);
    Route::apiResource('posts', Api\V1\PostController::class);
});
```

## Configuration Management

### Environment-Based Configuration

Use environment variables for configuration:

```php
// ✅ Good: config/services.php
return [
    'stripe' => [
        'key' => env('STRIPE_KEY'),
        'secret' => env('STRIPE_SECRET'),
        'webhook_secret' => env('STRIPE_WEBHOOK_SECRET'),
    ],

    'mailgun' => [
        'domain' => env('MAILGUN_DOMAIN'),
        'secret' => env('MAILGUN_SECRET'),
        'endpoint' => env('MAILGUN_ENDPOINT', 'api.mailgun.net'),
    ],
];

// Usage
$stripeKey = config('services.stripe.key');

// ❌ Bad: Hardcoded values or direct env() calls in code
class PaymentService
{
    public function charge()
    {
        $key = env('STRIPE_KEY'); // Don't call env() outside config files
        $key = 'sk_test_123456'; // Don't hardcode sensitive values
    }
}
```

### Use Kebab-Case for Config Files

```php
// ✅ Good: config/payment-processor.php
return [
    'stripe_key' => env('STRIPE_KEY'),
    'paypal_client_id' => env('PAYPAL_CLIENT_ID'),
    'default_currency' => 'USD',
    'webhook_timeout' => 30,
];

// ❌ Bad: config/PaymentProcessor.php or config/payment_processor.php
```

## Laravel Class Naming Conventions

### Controllers

Use plural resource names with Controller suffix:

```php
// ✅ Good
class UsersController extends Controller {}
class OrderItemsController extends Controller {}
class BlogPostsController extends Controller {}

// ❌ Bad
class UserController extends Controller {} // Singular
class User extends Controller {} // Missing suffix
```

### Actions

Use descriptive verb phrases:

```php
// ✅ Good
class CreateUserAction {}
class SendWelcomeEmailAction {}
class ProcessPaymentAction {}
class GenerateInvoiceAction {}

// ❌ Bad
class UserAction {} // Too generic
class CreateAction {} // Missing context
class UserCreator {} // Not following convention
```

### Jobs

Describe the action being performed:

```php
// ✅ Good
class SendEmailJob {}
class ProcessImageJob {}
class GenerateReportJob {}
class BackupDatabaseJob {}

// ❌ Bad
class EmailJob {} // Missing verb
class ProcessJob {} // Too generic
```

### Events and Listeners

Events use past tense, listeners describe the action:

```php
// ✅ Good: Events (past tense)
class UserRegistered {}
class OrderShipped {}
class PaymentProcessed {}
class PostPublished {}

// ✅ Good: Listeners (action + Listener)
class SendWelcomeEmailListener {}
class UpdateUserStatisticsListener {}
class NotifyAdminListener {}

// ❌ Bad
class UserRegister {} // Not past tense
class UserRegisteredListener {} // Event name, not listener action
```

### Requests

Use action + resource + Request pattern:

```php
// ✅ Good
class StoreUserRequest {}
class UpdateUserRequest {}
class CreatePostRequest {}
class DeleteCommentRequest {}

// ❌ Bad
class UserRequest {} // Too generic
class UserFormRequest {} // Unnecessary "Form"
```

### Resources

Use the model name + Resource:

```php
// ✅ Good
class UserResource {}
class PostResource {}
class OrderResource {}

// Collections
class UserCollection {}
class PostCollection {}
```

## Service Container and Dependency Injection

### Use Constructor Injection

Prefer constructor injection over facade usage in classes:

```php
// ✅ Good: Constructor injection
class UserService
{
    public function __construct(
        private UserRepository $userRepository,
        private EmailService $emailService,
        private Logger $logger,
    ) {}

    public function createUser(array $data): User
    {
        $user = $this->userRepository->create($data);
        $this->emailService->sendWelcomeEmail($user);
        $this->logger->info('User created', ['user_id' => $user->id]);

        return $user;
    }
}

// ❌ Bad: Using facades directly
class UserService
{
    public function createUser(array $data): User
    {
        $user = User::create($data);
        Mail::to($user->email)->send(new WelcomeEmail($user));
        Log::info('User created', ['user_id' => $user->id]);

        return $user;
    }
}
```

### Bind Interfaces to Implementations

Use service container bindings for flexibility:

```php
// ✅ Good: Interface binding in AppServiceProvider
class AppServiceProvider extends ServiceProvider
{
    public function register(): void
    {
        $this->app->bind(UserRepositoryInterface::class, EloquentUserRepository::class);
        $this->app->bind(PaymentProcessorInterface::class, StripePaymentProcessor::class);
    }
}

// Usage in classes
class UserService
{
    public function __construct(
        private UserRepositoryInterface $userRepository,
        private PaymentProcessorInterface $paymentProcessor,
    ) {}
}
```

## Laravel Testing Standards

### Use Feature and Unit Tests Appropriately

Feature tests for HTTP endpoints, unit tests for isolated logic:

```php
// ✅ Good: Feature test for HTTP behavior
class UserRegistrationTest extends TestCase
{
    use RefreshDatabase;

    public function test_user_can_register_with_valid_data(): void
    {
        $response = $this->post('/register', [
            'name' => 'John Doe',
            'email' => 'john@example.com',
            'password' => 'password',
            'password_confirmation' => 'password',
        ]);

        $response->assertRedirect('/dashboard');
        $this->assertDatabaseHas('users', ['email' => 'john@example.com']);
    }
}

// ✅ Good: Unit test for isolated logic
class UserServiceTest extends TestCase
{
    public function test_creates_user_with_hashed_password(): void
    {
        $mockRepository = $this->createMock(UserRepositoryInterface::class);
        $mockRepository->expects($this->once())
            ->method('create')
            ->with($this->callback(function ($data) {
                return Hash::check('password', $data['password']);
            }));

        $service = new UserService($mockRepository);
        $service->createUser([
            'name' => 'John Doe',
            'email' => 'john@example.com',
            'password' => 'password',
        ]);
    }
}
```

### Use Database Factories

Create test data with factories:

```php
// ✅ Good: Using factories
class UserFactory extends Factory
{
    protected $model = User::class;

    public function definition(): array
    {
        return [
            'name' => $this->faker->name(),
            'email' => $this->faker->unique()->safeEmail(),
            'email_verified_at' => now(),
            'password' => Hash::make('password'),
        ];
    }

    public function unverified(): static
    {
        return $this->state(['email_verified_at' => null]);
    }
}

// Usage in tests
public function test_verified_users_can_access_dashboard(): void
{
    $user = User::factory()->create();

    $this->actingAs($user)
        ->get('/dashboard')
        ->assertOk();
}

public function test_unverified_users_cannot_access_dashboard(): void
{
    $user = User::factory()->unverified()->create();

    $this->actingAs($user)
        ->get('/dashboard')
        ->assertRedirect('/email/verify');
}
```

### Use Laravel Testing Helpers

Leverage Laravel's testing utilities:

```php
// ✅ Good: Using Laravel testing helpers
public function test_user_can_update_profile(): void
{
    $user = User::factory()->create();

    $this->actingAs($user)
        ->put('/profile', [
            'name' => 'Updated Name',
            'email' => 'updated@example.com',
        ])
        ->assertRedirect('/profile')
        ->assertSessionHas('success');

    $this->assertDatabaseHas('users', [
        'id' => $user->id,
        'name' => 'Updated Name',
        'email' => 'updated@example.com',
    ]);
}
```

## Laravel-Specific Anti-Patterns

### Don't Use Models in Views

Keep business logic out of views:

```php
// ✅ Good: Logic in controller/service
class PostController extends Controller
{
    public function index(): View
    {
        $posts = Post::with('author')
            ->published()
            ->latest()
            ->paginate(10);

        return view('posts.index', compact('posts'));
    }
}

// In view
@foreach($posts as $post)
    <h2>{{ $post->title }}</h2>
    <p>By {{ $post->author->name }}</p>
@endforeach

// ❌ Bad: Logic in view
// In view
@foreach(Post::where('published', true)->with('author')->latest()->get() as $post)
    <h2>{{ $post->title }}</h2>
    <p>By {{ $post->author->name }}</p>
@endforeach
```

### Don't Override Laravel Conventions Unnecessarily

Work with Laravel, not against it:

```php
// ✅ Good: Following Laravel conventions
class User extends Model
{
    protected $fillable = ['name', 'email', 'password'];

    // Laravel automatically handles:
    // - Table name: 'users'
    // - Primary key: 'id'
    // - Timestamps: created_at, updated_at
}

// ❌ Bad: Fighting Laravel conventions
class User extends Model
{
    protected $table = 'user_accounts';
    protected $primaryKey = 'user_id';
    public $timestamps = false;

    // Now you lose Laravel's automatic handling
}
```

### Don't Use Raw Queries When Eloquent Suffices

Use Eloquent unless you have specific performance requirements:

```php
// ✅ Good: Using Eloquent
$users = User::whereHas('posts', function ($query) {
    $query->where('published', true);
})->withCount('posts')->get();

// ✅ Acceptable: Raw queries for complex operations where Eloquent is insufficient
$complexReport = DB::select('
    SELECT
        users.name,
        COUNT(posts.id) as post_count,
        AVG(ratings.score) as avg_rating
    FROM users
    LEFT JOIN posts ON users.id = posts.user_id
    LEFT JOIN ratings ON posts.id = ratings.post_id
    WHERE posts.created_at >= ?
    GROUP BY users.id
    HAVING avg_rating > ?
    ORDER BY post_count DESC
', [now()->subMonths(6), 4.0]);

// ❌ Bad: Raw query for simple operations
$users = DB::select('SELECT * FROM users WHERE email = ?', [$email]);
// Should be: User::where('email', $email)->first();
```

## Conclusion

Laravel's power comes from embracing its conventions and patterns. These guidelines help you write Laravel applications that are:

1. **Predictable** - Following Laravel conventions makes code familiar to other Laravel developers
2. **Maintainable** - Proper separation of concerns and organization
3. **Testable** - Clean architecture enables comprehensive testing
4. **Scalable** - Proper patterns support application growth

Remember to:

-   Leverage Laravel's built-in features before building custom solutions
-   Follow the framework's naming conventions and directory structure
-   Use dependency injection over facades in business logic
-   Keep controllers slim and move complex logic to dedicated classes
-   Write comprehensive tests using Laravel's testing utilities

For more Laravel-specific resources:

-   [Laravel Documentation](https://laravel.com/docs)
-   [Spatie Guidelines](https://spatie.be/guidelines/laravel-php)
-   [Laravel Best Practices](https://github.com/alexeymezenin/laravel-best-practices)
-   [Laravel Beyond CRUD](https://laravel-beyond-crud.com/)
