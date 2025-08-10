# Repository Pattern

## Repositories / Custom Query Objects

When working with databases in an application, you often need to retrieve, store, and manipulate data. Repositories are
a design pattern that provides an abstraction layer between your application and the database, allowing you to interact
with data without directly accessing the database.

Here’s an example of using the Repository Pattern in a Laravel application.

```php
namespace App\Repositories;

use App\Models\Post;

class PostRepository implements PostRepositoryInterface
{
    public function getAll(): array
    {
        return Post::all()->toArray();
    }

    public function findById(int $id): ?array
    {
        $post = Post::find($id);
        return $post ? $post->toArray() : null;
    }

    public function create(array $data): array
    {
        $post = Post::create($data);
        return $post->toArray();
    }

    public function update(int $id, array $data): bool
    {
        $post = Post::find($id);
        return $post ? $post->update($data) : false;
    }

    public function delete(int $id): bool
    {
        $post = Post::find($id);
        return $post ? $post->delete() : false;
    }
}
```

Now in your application rather than calling the Post model directly you can use the repostiory. This pattern will
allow you to group together the logic you need when interacting with the database from your application.

In Laravel applications you have a way of ecapulating the logic of the repository pattern by using the Eloquent custom
query builder.

In the model you can define a newEloquentBuilder method that returns a custom query builder.

```php
namespace App\Models;

use Illuminate\Database\Eloquent\Model;

class Post extends Model
{
    protected $fillable = ['title', 'content', 'published_at'];

    public function newEloquentBuilder($query)
    {
        return new PostQueryBuilder($query);
    }
}
```

Then you can define the custom query builder.

```php
namespace App\QueryBuilders;

use Illuminate\Database\Eloquent\Builder;

class PostQueryBuilder extends Builder
{
    public function published()
    {
        return $this->whereNotNull('published_at')->where('published_at', '<=', now());
    }

    public function search(string $term)
    {
        return $this->where('title', 'like', "%{$term}%")
                    ->orWhere('content', 'like', "%{$term}%");
    }
}
```

You can now use these method like scopes on the model. If you want to get published posts you will use

```php
$posts = Post::published()->get();
```

If you want to search for posts you can use

```php
$posts = Post::search('laravel')->get();
```

### Why Custom Eloquent Query Builders Are Better Than the Repository Pattern

**Simpler and Less Boilerplate:**

The repository pattern introduces additional layers (interfaces, bindings, and concrete implementations), which can lead
to unnecessary complexity, especially in smaller projects.
Custom query builders allow you to extend functionality directly within Eloquent, avoiding this overhead.

**Leverages Laravel's Built-in Features:**

Custom query builders integrate naturally with Eloquent's lifecycle, preserving relationships, scopes, and other ORM
features.
Repositories, on the other hand, require manual management of such features.

**Follows Laravel's "Active Record" Pattern:**
Eloquent uses the active record pattern, where models are responsible for their own queries. Custom query builders align
with this philosophy.
Repositories deviate from this approach, leading to potential friction with the rest of Laravel's design.

**Improved Readability:**
Custom query builders maintain a clean and fluent syntax directly within model queries, enhancing code readability.
Repositories can sometimes obscure the logic by separating queries into another layer.

**No Dependency on an Interface:**

Repositories rely on interfaces for flexibility, but this flexibility is often unnecessary for most applications.
Custom query builders provide the same reusability without requiring an interface or binding.

### When to Use Custom Query Builders Over Repositories

Use custom query builders when you want to keep your codebase simple and align with Laravel's conventions.
Consider repositories if you need to completely decouple the data access layer (e.g., supporting multiple data sources
like APIs and databases in the same application).

In most Laravel applications, custom query builders are sufficient and provide a cleaner, more maintainable approach to
encapsulating query logic.
