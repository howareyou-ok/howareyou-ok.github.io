---
title: "Laravel Framework: Elegant PHP Web Development"
date: 2024-01-18T15:00:00+08:00
tags: ["php", "laravel", "framework", "web development"]
---

Laravel is the most popular PHP framework, known for its elegant syntax, powerful features, and developer-friendly tools. It follows the MVC (Model-View-Controller) pattern and includes everything you need to build modern web applications.

## Why Choose Laravel?

- **Elegant Syntax**: Clean, expressive code that's easy to read and write
- **Rich Ecosystem**: Comprehensive set of tools and packages
- **Built-in Features**: Authentication, routing, sessions, caching out of the box
- **Artisan CLI**: Powerful command-line interface for development tasks
- **Eloquent ORM**: Beautiful, simple ActiveRecord implementation
- **Blade Templating**: Powerful templating engine with inheritance

## Installation and Setup

### Requirements

- PHP >= 8.1
- Composer (PHP dependency manager)
- Node.js and npm (for frontend assets)

### Install Laravel

```bash
# Install Laravel via Composer
composer global require laravel/installer

# Create new Laravel project
laravel new blog

# Or via Composer
composer create-project laravel/laravel blog

# Navigate to project
cd blog

# Start development server
php artisan serve
```

Visit `http://localhost:8000` to see your Laravel application!

### Project Structure

```
blog/
├── app/                    # Application core
│   ├── Http/
│   │   ├── Controllers/    # Controllers
│   │   └── Middleware/     # Middleware
│   ├── Models/            # Eloquent models
│   └── Providers/         # Service providers
├── config/                # Configuration files
├── database/
│   ├── migrations/        # Database migrations
│   └── seeders/          # Database seeders
├── public/               # Web server document root
├── resources/
│   ├── views/            # Blade templates
│   └── js/               # JavaScript files
├── routes/               # Route definitions
│   ├── web.php           # Web routes
│   └── api.php           # API routes
├── storage/              # Logs, cache, sessions
└── tests/                # Automated tests
```

## Routing

### Basic Routes

```php
<?php
// routes/web.php

use Illuminate\Support\Facades\Route;
use App\Http\Controllers\UserController;

// Basic GET route
Route::get('/', function () {
    return view('welcome');
});

// Route with parameters
Route::get('/user/{id}', function ($id) {
    return "User ID: " . $id;
});

// Optional parameters
Route::get('/posts/{id?}', function ($id = null) {
    if ($id) {
        return "Post ID: " . $id;
    }
    return "All posts";
});

// Route constraints
Route::get('/user/{id}', function ($id) {
    return "User ID: " . $id;
})->where('id', '[0-9]+');

// Named routes
Route::get('/profile', function () {
    return view('profile');
})->name('profile');

// Route groups
Route::prefix('admin')->group(function () {
    Route::get('/users', function () {
        return "Admin users";
    });
    Route::get('/posts', function () {
        return "Admin posts";
    });
});
```

### Controller Routes

```php
// Route to controller method
Route::get('/users', [UserController::class, 'index']);
Route::post('/users', [UserController::class, 'store']);
Route::get('/users/{user}', [UserController::class, 'show']);

// Resource routes (creates all CRUD routes)
Route::resource('posts', PostController::class);
```

## Controllers

### Creating Controllers

```bash
# Create controller
php artisan make:controller UserController

# Create resource controller
php artisan make:controller PostController --resource

# Create controller with model
php artisan make:controller PostController --model=Post
```

### Basic Controller

```php
<?php
// app/Http/Controllers/UserController.php

namespace App\Http\Controllers;

use Illuminate\Http\Request;
use App\Models\User;

class UserController extends Controller
{
    public function index()
    {
        $users = User::all();
        return view('users.index', compact('users'));
    }
    
    public function show($id)
    {
        $user = User::findOrFail($id);
        return view('users.show', compact('user'));
    }
    
    public function create()
    {
        return view('users.create');
    }
    
    public function store(Request $request)
    {
        $validatedData = $request->validate([
            'name' => 'required|max:255',
            'email' => 'required|email|unique:users',
            'password' => 'required|min:8',
        ]);
        
        $validatedData['password'] = bcrypt($validatedData['password']);
        
        User::create($validatedData);
        
        return redirect()->route('users.index')
                        ->with('success', 'User created successfully');
    }
    
    public function edit($id)
    {
        $user = User::findOrFail($id);
        return view('users.edit', compact('user'));
    }
    
    public function update(Request $request, $id)
    {
        $user = User::findOrFail($id);
        
        $validatedData = $request->validate([
            'name' => 'required|max:255',
            'email' => 'required|email|unique:users,email,' . $user->id,
        ]);
        
        $user->update($validatedData);
        
        return redirect()->route('users.index')
                        ->with('success', 'User updated successfully');
    }
    
    public function destroy($id)
    {
        $user = User::findOrFail($id);
        $user->delete();
        
        return redirect()->route('users.index')
                        ->with('success', 'User deleted successfully');
    }
}
```

## Models and Eloquent ORM

### Creating Models

```bash
# Create model
php artisan make:model Post

# Create model with migration
php artisan make:model Post -m

# Create model with migration, factory, and seeder
php artisan make:model Post -mfs
```

### Basic Model

```php
<?php
// app/Models/Post.php

namespace App\Models;

use Illuminate\Database\Eloquent\Factories\HasFactory;
use Illuminate\Database\Eloquent\Model;

class Post extends Model
{
    use HasFactory;
    
    protected $fillable = [
        'title',
        'content',
        'user_id',
        'published_at'
    ];
    
    protected $casts = [
        'published_at' => 'datetime',
    ];
    
    // Relationships
    public function user()
    {
        return $this->belongsTo(User::class);
    }
    
    public function comments()
    {
        return $this->hasMany(Comment::class);
    }
    
    public function tags()
    {
        return $this->belongsToMany(Tag::class);
    }
    
    // Scopes
    public function scopePublished($query)
    {
        return $query->whereNotNull('published_at');
    }
    
    public function scopeByUser($query, $userId)
    {
        return $query->where('user_id', $userId);
    }
    
    // Accessors
    public function getTitleAttribute($value)
    {
        return ucfirst($value);
    }
    
    // Mutators
    public function setTitleAttribute($value)
    {
        $this->attributes['title'] = strtolower($value);
    }
}
```

### Using Eloquent

```php
<?php
// Create
$post = Post::create([
    'title' => 'My First Post',
    'content' => 'This is the content of my first post.',
    'user_id' => 1
]);

// Read
$posts = Post::all();
$post = Post::find(1);
$post = Post::where('title', 'My First Post')->first();

// Update
$post = Post::find(1);
$post->update(['title' => 'Updated Title']);

// Delete
$post = Post::find(1);
$post->delete();

// Query with relationships
$posts = Post::with('user', 'comments')->get();

// Using scopes
$publishedPosts = Post::published()->get();
$userPosts = Post::byUser(1)->get();

// Pagination
$posts = Post::paginate(10);
```

## Database Migrations

### Creating Migrations

```bash
# Create migration
php artisan make:migration create_posts_table

# Create migration for existing table
php artisan make:migration add_published_at_to_posts_table --table=posts
```

### Migration Example

```php
<?php
// database/migrations/2024_01_18_000000_create_posts_table.php

use Illuminate\Database\Migrations\Migration;
use Illuminate\Database\Schema\Blueprint;
use Illuminate\Support\Facades\Schema;

return new class extends Migration
{
    public function up()
    {
        Schema::create('posts', function (Blueprint $table) {
            $table->id();
            $table->string('title');
            $table->text('content');
            $table->foreignId('user_id')->constrained()->onDelete('cascade');
            $table->timestamp('published_at')->nullable();
            $table->timestamps();
            
            $table->index(['user_id', 'published_at']);
        });
    }
    
    public function down()
    {
        Schema::dropIfExists('posts');
    }
};
```

### Running Migrations

```bash
# Run migrations
php artisan migrate

# Rollback last migration
php artisan migrate:rollback

# Reset all migrations
php artisan migrate:reset

# Refresh migrations (rollback and re-run)
php artisan migrate:refresh

# Check migration status
php artisan migrate:status
```

## Blade Templates

### Layout Template

```blade
{{-- resources/views/layouts/app.blade.php --}}
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>@yield('title', 'My Blog')</title>
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.1.3/dist/css/bootstrap.min.css" rel="stylesheet">
</head>
<body>
    <nav class="navbar navbar-expand-lg navbar-dark bg-dark">
        <div class="container">
            <a class="navbar-brand" href="{{ route('home') }}">My Blog</a>
            <div class="navbar-nav ms-auto">
                @auth
                    <a class="nav-link" href="{{ route('posts.create') }}">Create Post</a>
                    <a class="nav-link" href="{{ route('logout') }}">Logout</a>
                @else
                    <a class="nav-link" href="{{ route('login') }}">Login</a>
                    <a class="nav-link" href="{{ route('register') }}">Register</a>
                @endauth
            </div>
        </div>
    </nav>
    
    <div class="container mt-4">
        @if(session('success'))
            <div class="alert alert-success">
                {{ session('success') }}
            </div>
        @endif
        
        @if(session('error'))
            <div class="alert alert-danger">
                {{ session('error') }}
            </div>
        @endif
        
        @yield('content')
    </div>
    
    <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.1.3/dist/js/bootstrap.bundle.min.js"></script>
</body>
</html>
```

### Child Template

```blade
{{-- resources/views/posts/index.blade.php --}}
@extends('layouts.app')

@section('title', 'All Posts')

@section('content')
<div class="d-flex justify-content-between align-items-center mb-4">
    <h1>All Posts</h1>
    @auth
        <a href="{{ route('posts.create') }}" class="btn btn-primary">Create New Post</a>
    @endauth
</div>

@forelse($posts as $post)
    <div class="card mb-3">
        <div class="card-body">
            <h5 class="card-title">
                <a href="{{ route('posts.show', $post) }}">{{ $post->title }}</a>
            </h5>
            <p class="card-text">{{ Str::limit($post->content, 150) }}</p>
            <div class="d-flex justify-content-between align-items-center">
                <small class="text-muted">
                    By {{ $post->user->name }} on {{ $post->created_at->format('M d, Y') }}
                </small>
                @can('update', $post)
                    <div>
                        <a href="{{ route('posts.edit', $post) }}" class="btn btn-sm btn-outline-primary">Edit</a>
                        <form action="{{ route('posts.destroy', $post) }}" method="POST" class="d-inline">
                            @csrf
                            @method('DELETE')
                            <button type="submit" class="btn btn-sm btn-outline-danger" 
                                    onclick="return confirm('Are you sure?')">Delete</button>
                        </form>
                    </div>
                @endcan
            </div>
        </div>
    </div>
@empty
    <div class="alert alert-info">
        No posts found. <a href="{{ route('posts.create') }}">Create the first post!</a>
    </div>
@endforelse

{{ $posts->links() }}
@endsection
```

### Form Template

```blade
{{-- resources/views/posts/create.blade.php --}}
@extends('layouts.app')

@section('title', 'Create Post')

@section('content')
<h1>Create New Post</h1>

<form action="{{ route('posts.store') }}" method="POST">
    @csrf
    
    <div class="mb-3">
        <label for="title" class="form-label">Title</label>
        <input type="text" class="form-control @error('title') is-invalid @enderror" 
               id="title" name="title" value="{{ old('title') }}" required>
        @error('title')
            <div class="invalid-feedback">{{ $message }}</div>
        @enderror
    </div>
    
    <div class="mb-3">
        <label for="content" class="form-label">Content</label>
        <textarea class="form-control @error('content') is-invalid @enderror" 
                  id="content" name="content" rows="10" required>{{ old('content') }}</textarea>
        @error('content')
            <div class="invalid-feedback">{{ $message }}</div>
        @enderror
    </div>
    
    <div class="mb-3 form-check">
        <input type="checkbox" class="form-check-input" id="published" name="published" value="1">
        <label class="form-check-label" for="published">
            Publish immediately
        </label>
    </div>
    
    <button type="submit" class="btn btn-primary">Create Post</button>
    <a href="{{ route('posts.index') }}" class="btn btn-secondary">Cancel</a>
</form>
@endsection
```

## Authentication

### Setup Authentication

```bash
# Install Laravel Breeze (simple authentication)
composer require laravel/breeze --dev
php artisan breeze:install
npm install && npm run dev
php artisan migrate
```

### Manual Authentication

```php
<?php
// app/Http/Controllers/AuthController.php

namespace App\Http\Controllers;

use Illuminate\Http\Request;
use Illuminate\Support\Facades\Auth;
use Illuminate\Support\Facades\Hash;
use App\Models\User;

class AuthController extends Controller
{
    public function showLogin()
    {
        return view('auth.login');
    }
    
    public function login(Request $request)
    {
        $credentials = $request->validate([
            'email' => 'required|email',
            'password' => 'required',
        ]);
        
        if (Auth::attempt($credentials, $request->boolean('remember'))) {
            $request->session()->regenerate();
            return redirect()->intended('/dashboard');
        }
        
        return back()->withErrors([
            'email' => 'The provided credentials do not match our records.',
        ]);
    }
    
    public function logout(Request $request)
    {
        Auth::logout();
        $request->session()->invalidate();
        $request->session()->regenerateToken();
        
        return redirect('/');
    }
    
    public function showRegister()
    {
        return view('auth.register');
    }
    
    public function register(Request $request)
    {
        $validatedData = $request->validate([
            'name' => 'required|string|max:255',
            'email' => 'required|string|email|max:255|unique:users',
            'password' => 'required|string|min:8|confirmed',
        ]);
        
        $user = User::create([
            'name' => $validatedData['name'],
            'email' => $validatedData['email'],
            'password' => Hash::make($validatedData['password']),
        ]);
        
        Auth::login($user);
        
        return redirect('/dashboard');
    }
}
```

### Protecting Routes

```php
<?php
// routes/web.php

// Protect single route
Route::get('/dashboard', function () {
    return view('dashboard');
})->middleware('auth');

// Protect group of routes
Route::middleware('auth')->group(function () {
    Route::resource('posts', PostController::class);
    Route::get('/profile', [ProfileController::class, 'show']);
});

// Guest only routes
Route::middleware('guest')->group(function () {
    Route::get('/login', [AuthController::class, 'showLogin']);
    Route::post('/login', [AuthController::class, 'login']);
});
```

## Validation

### Form Request Validation

```bash
# Create form request
php artisan make:request StorePostRequest
```

```php
<?php
// app/Http/Requests/StorePostRequest.php

namespace App\Http\Requests;

use Illuminate\Foundation\Http\FormRequest;

class StorePostRequest extends FormRequest
{
    public function authorize()
    {
        return true; // or implement authorization logic
    }
    
    public function rules()
    {
        return [
            'title' => 'required|string|max:255|unique:posts',
            'content' => 'required|string|min:10',
            'published_at' => 'nullable|date|after:now',
            'tags' => 'array',
            'tags.*' => 'string|max:50',
        ];
    }
    
    public function messages()
    {
        return [
            'title.required' => 'The post title is required.',
            'title.unique' => 'A post with this title already exists.',
            'content.min' => 'The post content must be at least 10 characters.',
        ];
    }
}
```

### Using Form Request

```php
<?php
public function store(StorePostRequest $request)
{
    $validatedData = $request->validated();
    
    $post = Post::create([
        'title' => $validatedData['title'],
        'content' => $validatedData['content'],
        'user_id' => auth()->id(),
        'published_at' => $validatedData['published_at'] ?? now(),
    ]);
    
    if (isset($validatedData['tags'])) {
        $post->tags()->sync($validatedData['tags']);
    }
    
    return redirect()->route('posts.show', $post)
                    ->with('success', 'Post created successfully!');
}
```

## API Development

### API Routes

```php
<?php
// routes/api.php

use App\Http\Controllers\Api\PostController;

Route::middleware('auth:sanctum')->group(function () {
    Route::apiResource('posts', PostController::class);
});

// Public API routes
Route::get('/posts', [PostController::class, 'index']);
Route::get('/posts/{post}', [PostController::class, 'show']);
```

### API Controller

```php
<?php
// app/Http/Controllers/Api/PostController.php

namespace App\Http\Controllers\Api;

use App\Http\Controllers\Controller;
use App\Models\Post;
use Illuminate\Http\Request;

class PostController extends Controller
{
    public function index()
    {
        $posts = Post::with('user')->paginate(10);
        
        return response()->json([
            'data' => $posts->items(),
            'meta' => [
                'current_page' => $posts->currentPage(),
                'total' => $posts->total(),
                'per_page' => $posts->perPage(),
            ]
        ]);
    }
    
    public function show(Post $post)
    {
        $post->load('user', 'comments');
        
        return response()->json([
            'data' => $post
        ]);
    }
    
    public function store(Request $request)
    {
        $validatedData = $request->validate([
            'title' => 'required|string|max:255',
            'content' => 'required|string',
        ]);
        
        $post = $request->user()->posts()->create($validatedData);
        
        return response()->json([
            'data' => $post,
            'message' => 'Post created successfully'
        ], 201);
    }
    
    public function update(Request $request, Post $post)
    {
        $this->authorize('update', $post);
        
        $validatedData = $request->validate([
            'title' => 'required|string|max:255',
            'content' => 'required|string',
        ]);
        
        $post->update($validatedData);
        
        return response()->json([
            'data' => $post,
            'message' => 'Post updated successfully'
        ]);
    }
    
    public function destroy(Post $post)
    {
        $this->authorize('delete', $post);
        
        $post->delete();
        
        return response()->json([
            'message' => 'Post deleted successfully'
        ]);
    }
}
```

## Testing

### Feature Tests

```php
<?php
// tests/Feature/PostTest.php

namespace Tests\Feature;

use App\Models\Post;
use App\Models\User;
use Illuminate\Foundation\Testing\RefreshDatabase;
use Tests\TestCase;

class PostTest extends TestCase
{
    use RefreshDatabase;
    
    public function test_user_can_create_post()
    {
        $user = User::factory()->create();
        
        $response = $this->actingAs($user)->post('/posts', [
            'title' => 'Test Post',
            'content' => 'This is a test post content.',
        ]);
        
        $response->assertRedirect();
        $this->assertDatabaseHas('posts', [
            'title' => 'Test Post',
            'user_id' => $user->id,
        ]);
    }
    
    public function test_guest_cannot_create_post()
    {
        $response = $this->post('/posts', [
            'title' => 'Test Post',
            'content' => 'This is a test post content.',
        ]);
        
        $response->assertRedirect('/login');
    }
    
    public function test_post_requires_title_and_content()
    {
        $user = User::factory()->create();
        
        $response = $this->actingAs($user)->post('/posts', []);
        
        $response->assertSessionHasErrors(['title', 'content']);
    }
}
```

### Unit Tests

```php
<?php
// tests/Unit/PostTest.php

namespace Tests\Unit;

use App\Models\Post;
use App\Models\User;
use Illuminate\Foundation\Testing\RefreshDatabase;
use Tests\TestCase;

class PostTest extends TestCase
{
    use RefreshDatabase;
    
    public function test_post_belongs_to_user()
    {
        $user = User::factory()->create();
        $post = Post::factory()->create(['user_id' => $user->id]);
        
        $this->assertInstanceOf(User::class, $post->user);
        $this->assertEquals($user->id, $post->user->id);
    }
    
    public function test_post_can_be_published()
    {
        $post = Post::factory()->create(['published_at' => null]);
        
        $this->assertFalse($post->isPublished());
        
        $post->publish();
        
        $this->assertTrue($post->isPublished());
    }
}
```

## Best Practices

### Service Classes

```php
<?php
// app/Services/PostService.php

namespace App\Services;

use App\Models\Post;
use App\Models\User;

class PostService
{
    public function createPost(User $user, array $data): Post
    {
        $post = $user->posts()->create([
            'title' => $data['title'],
            'content' => $data['content'],
            'published_at' => $data['publish_now'] ? now() : null,
        ]);
        
        if (isset($data['tags'])) {
            $post->tags()->sync($data['tags']);
        }
        
        // Send notification, update cache, etc.
        
        return $post;
    }
    
    public function getPublishedPosts(int $perPage = 10)
    {
        return Post::published()
                   ->with('user', 'tags')
                   ->latest('published_at')
                   ->paginate($perPage);
    }
}
```

### Repository Pattern

```php
<?php
// app/Repositories/PostRepository.php

namespace App\Repositories;

use App\Models\Post;

class PostRepository
{
    public function findPublished()
    {
        return Post::published()->get();
    }
    
    public function findByUser($userId)
    {
        return Post::where('user_id', $userId)->get();
    }
    
    public function create(array $data)
    {
        return Post::create($data);
    }
}
```

## Deployment

### Environment Configuration

```bash
# .env.production
APP_ENV=production
APP_DEBUG=false
APP_URL=https://yourdomain.com

DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=your_database
DB_USERNAME=your_username
DB_PASSWORD=your_password

CACHE_DRIVER=redis
SESSION_DRIVER=redis
QUEUE_CONNECTION=redis
```

### Deployment Commands

```bash
# Optimize for production
php artisan config:cache
php artisan route:cache
php artisan view:cache

# Run migrations
php artisan migrate --force

# Clear caches
php artisan cache:clear
php artisan config:clear
php artisan route:clear
php artisan view:clear
```

## Next Steps

After mastering Laravel basics:

1. **Learn Advanced Features**: Queues, Events, Broadcasting
2. **Explore Packages**: Spatie packages, Laravel Nova
3. **Study Testing**: Advanced testing techniques
4. **Learn DevOps**: Docker, CI/CD, server management
5. **Build Real Projects**: E-commerce, CMS, API services

Laravel's elegant syntax and comprehensive feature set make it an excellent choice for modern web development. Its active community and extensive documentation ensure you'll have support throughout your development journey.

Remember to follow Laravel conventions, write tests, and leverage the framework's built-in features to build maintainable and scalable applications.