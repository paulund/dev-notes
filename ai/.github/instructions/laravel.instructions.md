---
applyTo: '**/*.php'
---

# Laravel Development Guidelines

## Core Laravel Principles

### Follow Laravel's Way

- Always prefer Laravel's built-in solutions over custom implementations
- If there's a documented Laravel way to achieve something, use it
- Leverage the framework's conventions to reduce boilerplate code
- Trust Laravel's battle-tested solutions for common problems

### Convention Over Configuration

- Follow Laravel's directory structure and file organization
- Use conventional names for models, controllers, and other classes
- Let Laravel automatically infer table names, primary keys, and timestamps
- Only override conventions when there's a compelling reason

### Thin Controllers, Fat Models, Specific Actions

- Controllers should orchestrate, not implement business logic
- Extract complex logic into Actions, Services, or dedicated classes
- Models should contain domain logic specific to that entity
- For complex operations, use single-responsibility Action classes

### Dependency Injection

- Use constructor injection for dependencies in controllers and classes
- Bind interfaces to implementations in service providers
- Avoid using facades in business logic (use dependency injection instead)
- Leverage Laravel's automatic dependency resolution

### Query Optimization

- Always eager load relationships to avoid N+1 queries
- Use query scopes for reusable query logic
- Index database columns used in WHERE, JOIN, and ORDER BY clauses
- Use `select()` to load only needed columns from the database
- Consider using database views for complex queries
- Use `chunk()` or `cursor()` for processing large datasets

## Controllers

### Keep Controllers Slim

```php
// ✅ Good: Slim controller delegating to Action
class UserController extends Controller
{
    public function __construct(
        private readonly CreateUserAction $createUser
    ) {}

    public function store(StoreUserRequest $request): RedirectResponse
    {
        $user = $this->createUser->execute($request->validated());

        return redirect()->route('users.show', $user)
            ->with('success', 'User created successfully');
    }
}

// ❌ Bad: Fat controller with business logic
class UserController extends Controller
{
    public function store(Request $request)
    {
        $validated = $request->validate([...]);
        $user = new User();
        $user->name = $validated['name'];
        $user->email = $validated['email'];
        $user->password = Hash::make($validated['password']);
        $user->save();

        Mail::to($user->email)->send(new WelcomeEmail($user));
        Log::info('User registered', ['user_id' => $user->id]);

        return redirect()->route('users.show', $user);
    }
}
```

### Use Resource Controllers

- Use resource controllers for standard CRUD operations
- Stick to the seven RESTful actions: index, create, store, show, edit, update, destroy
- Use invokable controllers for single-action controllers
- Use `php artisan make:controller UserController --resource` to scaffold
- For non-CRUD actions, create separate single-action controllers

### Use Route Model Binding

```php
// ✅ Good: Route model binding
Route::get('users/{user}', [UserController::class, 'show']);

public function show(User $user): View
{
    return view('users.show', compact('user'));
}

// ❌ Bad: Manual model retrieval
public function show(int $id): View
{
    $user = User::findOrFail($id);
    return view('users.show', compact('user'));
}
```

### Return Type Declarations

- Always declare return types for controller methods
- Use `View`, `RedirectResponse`, `JsonResponse`, `Response` types
- For API controllers, use API Resources instead of returning models directly

## Validation

### Always Use Form Request Classes

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
            'role' => ['required', Rule::in(['admin', 'user', 'moderator'])],
        ];
    }

    public function messages(): array
    {
        return [
            'email.unique' => 'This email address is already registered.',
        ];
    }
}

// ❌ Bad: Inline validation in controller
public function store(Request $request)
{
    $request->validate([
        'email' => 'required|email|unique:users',
    ]);
}
```

### Validation Rules

- Use array notation for validation rules, not pipe-separated strings
- Use `Rule` class for complex validations
- Create custom validation rules for business logic validation
- Keep validation logic in Form Requests, not in models or controllers

## Eloquent & Database

### Use Eloquent Relationships

```php
// ✅ Good: Proper relationship definition
class User extends Model
{
    public function posts(): HasMany
    {
        return $this->hasMany(Post::class);
    }

    public function roles(): BelongsToMany
    {
        return $this->belongsToMany(Role::class)
            ->withTimestamps()
            ->withPivot('expires_at');
    }
}

// Usage with eager loading
$users = User::with(['posts', 'roles'])->get();
```

### Always Eager Load to Avoid N+1

```php
// ✅ Good: Eager loading
$users = User::with('posts')->get();
foreach ($users as $user) {
    echo $user->posts->count();
}

// ❌ Bad: N+1 query problem
$users = User::all(); // 1 query
foreach ($users as $user) {
    echo $user->posts->count(); // N queries
}
```

### Use Query Scopes for Reusable Queries

```php
// ✅ Good: Query scopes
class User extends Model
{
    public function scopeActive(Builder $query): void
    {
        $query->where('active', true);
    }

    public function scopeWithRole(Builder $query, string $role): void
    {
        $query->whereHas('roles', fn($q) => $q->where('name', $role));
    }
}

// Usage
$activeAdmins = User::active()->withRole('admin')->get();

// ❌ Bad: Repeating query logic
$activeAdmins = User::where('active', true)
    ->whereHas('roles', fn($q) => $q->where('name', 'admin'))
    ->get();
```

### Use Accessors and Mutators (Casts)

```php
// ✅ Good: Using casts and accessors
class User extends Model
{
    protected $casts = [
        'email_verified_at' => 'datetime',
        'settings' => 'array',
        'is_admin' => 'boolean',
    ];

    protected function password(): Attribute
    {
        return Attribute::make(
            set: fn(string $value) => Hash::make($value),
        );
    }
}

// ❌ Bad: Manual transformation
$user->password = Hash::make($password);
$user->save();
```

### Mass Assignment Protection

```php
// ✅ Good: Define fillable attributes
class User extends Model
{
    protected $fillable = [
        'name',
        'email',
        'password',
    ];

    // Or use guarded for protection
    protected $guarded = [
        'id',
        'is_admin',
        'created_at',
    ];
}
```

## Routes

### Use Named Routes

```php
// ✅ Good: Named routes with class-based controllers
Route::get('users', [UserController::class, 'index'])->name('users.index');
Route::get('users/{user}', [UserController::class, 'show'])->name('users.show');

// In views
<a href="{{ route('users.show', $user) }}">View User</a>

// In controllers
return redirect()->route('users.index');

// ❌ Bad: Unnamed routes with string controllers
Route::get('users', 'UserController@index');
<a href="/users/{{ $user->id }}">View User</a>
```

### Group Related Routes

```php
// ✅ Good: Organized route groups
Route::middleware(['auth'])->group(function () {
    Route::prefix('admin')->name('admin.')->group(function () {
        Route::resource('users', UserController::class);
        Route::resource('posts', PostController::class);
    });
});

// API routes
Route::prefix('api/v1')->group(function () {
    Route::apiResource('users', Api\UserController::class);
});
```

### Use Resource Routes

```php
// ✅ Good: Resource routes for standard CRUD
Route::resource('posts', PostController::class);
// Generates: index, create, store, show, edit, update, destroy

// For API-only (no create/edit)
Route::apiResource('posts', Api\PostController::class);

// Limit to specific actions
Route::resource('posts', PostController::class)->only(['index', 'show']);
Route::resource('posts', PostController::class)->except(['destroy']);
```

## Service Layer & Actions

### Use Action Classes for Complex Operations

```php
// ✅ Good: Single-responsibility Action
class CreateUserAction
{
    public function __construct(
        private readonly UserRepository $users,
        private readonly HashService $hasher,
        private readonly EventDispatcher $events,
    ) {}

    public function execute(array $data): User
    {
        $user = $this->users->create([
            'name' => $data['name'],
            'email' => $data['email'],
            'password' => $this->hasher->make($data['password']),
        ]);

        $this->events->dispatch(new UserRegistered($user));

        return $user;
    }
}
```

### Use Services for Domain Logic

```php
// ✅ Good: Service for complex business logic
class PaymentService
{
    public function __construct(
        private readonly PaymentGateway $gateway,
        private readonly InvoiceGenerator $invoiceGenerator,
    ) {}

    public function processPayment(Order $order, array $paymentData): Payment
    {
        $payment = $this->gateway->charge(
            $order->total,
            $paymentData['token']
        );

        if ($payment->successful()) {
            $order->markAsPaid();
            $this->invoiceGenerator->generate($order);
        }

        return $payment;
    }
}
```

## Configuration & Environment

### Use Config Files, Not ENV Directly

```php
// ✅ Good: Access via config
$apiKey = config('services.stripe.key');
$timeout = config('services.payment.timeout', 30);

// config/services.php
return [
    'stripe' => [
        'key' => env('STRIPE_KEY'),
        'secret' => env('STRIPE_SECRET'),
    ],
];

// ❌ Bad: Direct env access in application code
$apiKey = env('STRIPE_KEY');
```

### Use Kebab-Case for Config Files

```php
// ✅ Good: config/payment-processor.php
config('payment-processor.stripe_key');

// ❌ Bad: config/PaymentProcessor.php or config/payment_processor.php
```

## Events & Listeners

### Use Events for Decoupled Architecture

```php
// ✅ Good: Event-driven approach
class UserRegistered
{
    public function __construct(
        public readonly User $user
    ) {}
}

class SendWelcomeEmailListener
{
    public function handle(UserRegistered $event): void
    {
        Mail::to($event->user->email)
            ->send(new WelcomeEmail($event->user));
    }
}

// In service/action
event(new UserRegistered($user));
```

### Event Naming Conventions

- Events: Use past tense (`UserRegistered`, `OrderShipped`, `PaymentProcessed`)
- Listeners: Describe action (`SendWelcomeEmailListener`, `UpdateUserStatisticsListener`)

## Jobs & Queues

### Use Jobs for Async Operations

```php
// ✅ Good: Queueable job
class ProcessOrderJob implements ShouldQueue
{
    use Dispatchable, InteractsWithQueue, Queueable, SerializesModels;

    public function __construct(
        public readonly Order $order
    ) {}

    public function handle(PaymentService $paymentService): void
    {
        $paymentService->processPayment($this->order);
    }

    public function failed(Throwable $exception): void
    {
        // Handle job failure
        Log::error('Order processing failed', [
            'order_id' => $this->order->id,
            'error' => $exception->getMessage(),
        ]);
    }
}

// Dispatch
ProcessOrderJob::dispatch($order);
```

### Job Best Practices

- Make jobs idempotent (safe to run multiple times)
- Implement `failed()` method for error handling
- Use job batching for related operations
- Set appropriate timeout and retry values
- Use unique jobs to prevent duplicates

## Testing

### Use Feature Tests for HTTP Endpoints

```php
// ✅ Good: Feature test
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
```

### Use Factories for Test Data

```php
// ✅ Good: Using factories
$user = User::factory()->create(['email' => 'test@example.com']);
$posts = Post::factory()->count(10)->for($user)->create();

// With states
$admin = User::factory()->admin()->create();
$unverifiedUser = User::factory()->unverified()->create();
```

### Test Naming

- Use descriptive test method names: `test_user_can_update_own_profile`
- Use `test_` prefix or `@test` annotation
- One assertion concept per test
- Arrange-Act-Assert pattern

## Class Naming Conventions

### Controllers

- Plural resource name + `Controller`: `UsersController`, `OrdersController`
- Use singular for single-resource: `ProfileController`, `DashboardController`

### Models

- Singular, PascalCase: `User`, `BlogPost`, `OrderItem`

### Actions

- Verb + noun + `Action`: `CreateUserAction`, `ProcessPaymentAction`, `GenerateInvoiceAction`

### Jobs

- Verb + noun + `Job`: `SendEmailJob`, `ProcessImageJob`, `GenerateReportJob`

### Requests

- Action + resource + `Request`: `StoreUserRequest`, `UpdatePostRequest`, `DeleteCommentRequest`

### Resources (API)

- Model name + `Resource`: `UserResource`, `PostResource`
- Collections: `UserCollection`, `PostCollection`

### Events

- Past tense: `UserRegistered`, `OrderShipped`, `PaymentProcessed`

### Listeners

- Action + `Listener`: `SendWelcomeEmailListener`, `NotifyAdminListener`

## Security Best Practices

### Use CSRF Protection

- Laravel automatically includes CSRF protection for POST, PUT, PATCH, DELETE
- Always include `@csrf` in forms
- For API endpoints, use token-based authentication

### Use Authorization

```php
// ✅ Good: Policy-based authorization
Gate::define('update-post', function (User $user, Post $post) {
    return $user->id === $post->user_id;
});

// In controller
$this->authorize('update', $post);

// Or create Policy class
php artisan make:policy PostPolicy --model=Post
```

### Validate All Input

- Never trust user input
- Use Form Requests for validation
- Sanitize output in views (Blade auto-escapes by default)
- Use parameterized queries (Eloquent does this automatically)

### Use Mass Assignment Protection

- Define `$fillable` or `$guarded` on all models
- Never use `$guarded = []` in production

## Performance Best Practices

### Caching

```php
// ✅ Good: Cache expensive operations
$users = Cache::remember('users.active', 3600, function () {
    return User::active()->with('roles')->get();
});

// Cache tags for granular invalidation
Cache::tags(['users', 'roles'])->remember('users.list', 3600, fn() => ...);
Cache::tags(['users'])->flush();
```

### Query Optimization

- Use `select()` to load only needed columns
- Use `chunk()` or `lazy()` for large datasets
- Add database indexes for frequently queried columns
- Use `toBase()` to convert to base query builder when models aren't needed

### Use Queues for Heavy Tasks

- Email sending
- Image processing
- Report generation
- API calls to external services
- Any operation taking >2 seconds

## Common Anti-Patterns to Avoid

### ❌ Don't Use Models in Views

```php
// ❌ Bad: Query in view
@foreach(User::all() as $user)

// ✅ Good: Pass data from controller
public function index(): View
{
    return view('users.index', ['users' => User::all()]);
}
```

### ❌ Don't Override Conventions Unnecessarily

```php
// ❌ Bad: Fighting conventions
class User extends Model
{
    protected $table = 'user_accounts';
    protected $primaryKey = 'user_id';
    public $timestamps = false;
}

// ✅ Good: Follow conventions
class User extends Model
{
    // Laravel handles table name, primary key, timestamps automatically
}
```

### ❌ Don't Use Raw Queries When Eloquent Works

```php
// ❌ Bad: Raw query
$users = DB::select('SELECT * FROM users WHERE email = ?', [$email]);

// ✅ Good: Eloquent
$user = User::where('email', $email)->first();
```

### ❌ Don't Put Business Logic in Views

```php
// ❌ Bad: Logic in view
@if($user->role === 'admin' && $user->active && $user->permissions->contains('edit'))

// ✅ Good: Move to model method
@if($user->canEdit())

// In User model
public function canEdit(): bool
{
    return $this->role === 'admin'
        && $this->active
        && $this->permissions->contains('edit');
}
```

## Additional Resources

- [Laravel Documentation](https://laravel.com/docs)
- [Laravel Best Practices](https://github.com/alexeymezenin/laravel-best-practices)
- [Spatie Guidelines](https://spatie.be/guidelines/laravel-php)
- [Laravel Beyond CRUD](https://laravel-beyond-crud.com/)
