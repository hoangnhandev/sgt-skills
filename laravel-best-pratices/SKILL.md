---
name: laravel
description: Laravel best practices skill for MVC + Service architecture. Use when working with Laravel applications, creating controllers, models, services, or following Laravel conventions. Covers Service Layer pattern, thin controllers, fat models, Form Request validation, naming conventions, PSR standards, routing, Blade views, Eloquent relationships, eager loading, pagination, authentication, authorization, caching, events, queues, and testing. Ideal for building maintainable Laravel applications with proper separation of concerns.
metadata:
  version: "1.2.0"
  author: "sgt-skills"
  architecture: "MVC + Service Pattern"
  php_version: "8.2+"
  laravel_version: "11.x+"
  topics:
    - mvc
    - service-pattern
    - psr-standards
    - eloquent
    - routing
    - blade
    - authentication
    - authorization
  related_skills:
    - laravel-mvc-service-overview
    - laravel-project-structure
    - laravel-naming-conventions
    - laravel-coding-standards
---

# Laravel Best Practices - MVC + Service Architecture

> **Architecture**: MVC + Service Pattern | **PHP**: 8.2+ | **Laravel**: 11.x+

## When to Use This Skill

Dùng skill này khi:

- **Tạo mới Laravel code**: Controllers, Models, Services, Form Requests
- **Refactor code**: Chuyển đổi sang kiến trúc MVC + Service chuẩn
- **Tạo API endpoints**: RESTful API với validation và responses
- **Implement business logic**: Logic phức tạp cần tách riêng khỏi controllers
- **Setup cấu trúc dự án**: Organization theo Laravel conventions
- **Fix naming issues**: Đặt tên đúng PSR và Laravel conventions
- **Coding standards**: Viết code theo PSR-2, PSR-12
- **Eloquent queries**: Scopes, relationships, eager loading
- **Routing**: Resource routes, route groups, model binding
- **Blade templates**: Components, layouts, directives
- **Authentication & Authorization**: Guards, Policies, Gates
- **Testing**: PHPUnit, Pest tests

## Overview

### MVC + Service Architecture

```
┌─────────────────────────────────────────────────────────────────┐
│                    MVC + SERVICE ARCHITECTURE                   │
├─────────────────────────────────────────────────────────────────┤
│                                                                  │
│  ┌──────────────┐     ┌──────────────┐     ┌──────────────┐   │
│  │   ROUTES     │────▶│ CONTROLLERS  │────▶│   SERVICES   │   │
│  │              │     │              │     │              │   │
│  │ web.php      │     │ - Request    │     │ - Business   │   │
│  │ api.php      │     │ - Response   │     │   Logic      │   │
│  └──────────────┘     │ - Delegate   │     │ - Orchestrate│   │
│                       └──────────────┘     └──────┬───────┘   │
│                                                      │          │
│                              ┌───────────────────────┼────────┐ │
│                              ▼                       ▼        │ │
│                       ┌──────────────┐     ┌──────────────┐  │
│                       │   MODELS     │     │  REPOSITORIES│  │
│                       │              │     │   (Optional)  │  │
│                       │ - Eloquent   │     │ - Query      │  │
│                       │ - Scopes     │     │   Builder     │  │
│                       │ - Relations  │     └──────────────┘  │
│                       └──────────────┘                       │
│                              │                                │
│                              ▼                                │
│                       ┌──────────────┐                       │
│                       │   DATABASE   │                       │
│                       └──────────────┘                       │
└─────────────────────────────────────────────────────────────────┘
```

### Data Flow

```
User Request
    │
    ▼
┌─────────┐
│  Route  │
└────┬────┘
     │
     ▼
┌──────────────┐
│ Form Request │ ← Validation & Authorization
└────┬─────────┘
     │
     ▼
┌─────────────┐
│ Controller  │ ← Thin: Delegate only
└────┬────────┘
     │
     ▼
┌─────────────┐
│   Service   │ ← Business logic, orchestrate
└────┬────────┘
     │
     ▼
┌─────────────┐
│    Model    │ ← Eloquent, scopes, relationships
└────┬────────┘
     │
     ▼
┌─────────────┐
│  Database   │
└─────────────┘
```

### Project Structure

```
app/
├── Http/
│   ├── Controllers/
│   │   ├── Controller.php
│   │   └── UserController.php        # Skinny controllers
│   ├── Middleware/
│   ├── Requests/
│   │   ├── StoreUserRequest.php      # Form Request validation
│   │   └── UpdateUserRequest.php
│   └── Resources/
│       └── UserResource.php          # API Resources
├── Models/
│   └── User.php                       # Fat models with scopes
├── Services/
│   ├── UserService.php
│   └── Contracts/
│       └── UserServiceInterface.php
└── Providers/
    ├── AppServiceProvider.php
    └── AuthServiceProvider.php
```

### Key Principles

| Principle | Description |
|-----------|-------------|
| **Thin Controllers** | Chỉ xử lý Request/Response, delegate cho Services |
| **Fat Models** | Data queries, scopes, accessors trong Models |
| **Form Requests** | Validation tách riêng khỏi controllers |
| **Services** | Business logic, orchestrate nhiều models |
| **Single Responsibility** | Mỗi class có một trách nhiệm rõ ràng |

## How to Use

### 1. Creating CRUD Workflow

**Step 1: Tạo Service Class**

```bash
php .claude/skills/laravel/scripts/make_service.php UserService
```

Hoặc tạo thủ công:

```php
<?php

namespace App\Services;

use App\Models\User;
use Illuminate\Database\Eloquent\Collection;
use Illuminate\Pagination\LengthAwarePaginator;

class UserService
{
    public function __construct(
        private User $user
    ) {}

    public function getAllUsers(): Collection
    {
        return $this->user->all();
    }

    public function getPaginatedUsers(int $perPage = 15): LengthAwarePaginator
    {
        return $this->user->paginate($perPage);
    }

    public function getUserById(int $id): User
    {
        return $this->user->findOrFail($id);
    }

    public function createUser(array $data): User
    {
        return $this->user->create($data);
    }

    public function updateUser(int $id, array $data): User
    {
        $user = $this->getUserById($id);
        $user->update($data);
        return $user;
    }

    public function deleteUser(int $id): bool
    {
        $user = $this->getUserById($id);
        return $user->delete();
    }
}
```

**Step 2: Tạo Form Requests**

```bash
php artisan make:request StoreUserRequest
php artisan make:request UpdateUserRequest
```

```php
<?php

namespace App\Http\Requests;

use Illuminate\Foundation\Http\FormRequest;

class StoreUserRequest extends FormRequest
{
    public function authorize(): bool
    {
        return true;
    }

    public function rules(): array
    {
        return [
            'name' => 'required|string|max:255',
            'email' => 'required|email|unique:users,email',
            'password' => 'required|string|min:8|confirmed',
        ];
    }
}
```

**Step 3: Tạo Controller với Dependency Injection**

```bash
php artisan make:controller UserController --api
```

```php
<?php

namespace App\Http\Controllers;

use App\Http\Requests\StoreUserRequest;
use App\Http\Requests\UpdateUserRequest;
use App\Services\UserService;
use Illuminate\Http\JsonResponse;

class UserController extends Controller
{
    public function __construct(
        private UserService $userService
    ) {}

    public function index(): JsonResponse
    {
        $users = $this->userService->getPaginatedUsers();
        return response()->json($users);
    }

    public function show(int $id): JsonResponse
    {
        $user = $this->userService->getUserById($id);
        return response()->json($user);
    }

    public function store(StoreUserRequest $request): JsonResponse
    {
        $user = $this->userService->createUser($request->validated());
        return response()->json($user, 201);
    }

    public function update(UpdateUserRequest $request, int $id): JsonResponse
    {
        $user = $this->userService->updateUser($id, $request->validated());
        return response()->json($user);
    }

    public function destroy(int $id): JsonResponse
    {
        $this->userService->deleteUser($id);
        return response()->json(null, 204);
    }
}
```

**Step 4: Tạo Model với Scopes**

```bash
php artisan make:model User -m
```

```php
<?php

namespace App\Models;

use Illuminate\Database\Eloquent\Model;
use Illuminate\Database\Eloquent\Relations\HasMany;

class User extends Model
{
    protected $fillable = [
        'name',
        'email',
        'password',
        'is_active',
    ];

    protected $hidden = [
        'password',
        'remember_token',
    ];

    protected $casts = [
        'email_verified_at' => 'datetime',
        'is_active' => 'boolean',
    ];

    // ===== SCOPES =====
    public function scopeActive($query)
    {
        return $query->where('is_active', true);
    }

    // ===== RELATIONSHIPS =====
    public function posts(): HasMany
    {
        return $this->hasMany(Post::class);
    }

    // ===== ACCESSORS =====
    public function getFullNameAttribute(): string
    {
        return $this->name;
    }
}
```

**Step 5: Định nghĩa Routes**

```php
// routes/api.php
Route::apiResource('users', UserController::class);
```

### 2. Naming Conventions Quick Reference

| Loại | Pattern | Ví dụ Đúng |
|------|---------|------------|
| Controller | singular + Controller | `UserController` |
| Model | singular | `User` |
| Service | singular + Service | `UserService` |
| Request | singular + Request | `StoreUserRequest` |
| Resource | singular + Resource | `UserResource` |
| Table | plural snake_case | `user_profiles` |
| Route | plural | `users/1` |
| Route name | dot notation | `users.index` |

### 3. Common Tasks Reference

**Create CRUD for Entity:**
```bash
# 1. Tạo Service
php .claude/skills/laravel/scripts/make_service.php EntityService

# 2. Tạo Model với migration
php artisan make:model Entity -m

# 3. Tạo Form Requests
php artisan make:request StoreEntityRequest
php artisan make:request UpdateEntityRequest

# 4. Tạo Controller
php artisan make:controller EntityController --api

# 5. Định nghĩa routes
# routes/api.php: Route::apiResource('entities', EntityController::class);
```

**N+1 Query Prevention:**
```php
// ❌ Bad - N+1 queries
$posts = Post::all();
foreach ($posts as $post) {
    echo $post->user->name; // Separate query for each post
}

// ✅ Good - Eager loading
$posts = Post::with('user')->get();
foreach ($posts as $post) {
    echo $post->user->name; // No additional queries
}
```

**Transaction Handling:**
```php
use Illuminate\Support\Facades\DB;

public function createUserWithProfile(array $userData, array $profileData): User
{
    return DB::transaction(function () use ($userData, $profileData) {
        $user = User::create($userData);
        $user->profile()->create($profileData);
        return $user;
    });
}
```

## Examples

### Complete CRUD Example

**Routes** (`routes/api.php`):
```php
Route::apiResource('users', UserController::class);
```

**Form Request** (`app/Http/Requests/StoreUserRequest.php`):
```php
class StoreUserRequest extends FormRequest
{
    public function rules(): array
    {
        return [
            'name' => 'required|string|max:255',
            'email' => 'required|email|unique:users',
            'password' => 'required|min:8',
        ];
    }
}
```

**Controller** (`app/Http/Controllers/UserController.php`):
```php
class UserController extends Controller
{
    public function __construct(private UserService $service) {}

    public function store(StoreUserRequest $request): JsonResponse
    {
        $user = $this->service->createUser($request->validated());
        return response()->json($user, 201);
    }
}
```

**Service** (`app/Services/UserService.php`):
```php
class UserService
{
    public function createUser(array $data): User
    {
        return User::create($data);
    }
}
```

**Model** (`app/Models/User.php`):
```php
class User extends Model
{
    protected $fillable = ['name', 'email', 'password'];
}
```

### Model Scopes

```php
// In Model
public function scopeActive($query)
{
    return $query->where('is_active', true);
}

// Usage
User::active()->get();
```

### Blade Components

```blade
<!-- resources/views/components/user-card.blade.php -->
@props(['user'])

<div class="user-card">
    <h2>{{ $user->name }}</h2>
    <p>{{ $user->email }}</p>
</div>

<!-- Usage -->
<x-user-card :user="$user" />
```

## Resources

### References (Detailed Documentation)

| File | Content |
|------|---------|
| `references/service-layer.md` | Service class patterns, when to use/not use |
| `references/controller-best-practices.md` | Thin controllers, RESTful conventions, DI |
| `references/request-validation.md` | Form Request validation, custom rules |
| `references/eloquent-best-practices.md` | Model scopes, accessors, relationships |
| `references/api-resources.md` | Transform models to JSON responses |
| `references/middleware.md` | Request filtering, auth, logging, CORS |
| `references/naming-conventions.md` | PSR & Laravel naming conventions |
| `references/coding-standards.md` | PSR-2, PSR-12, PHPDoc rules |
| `references/routing.md` | Route groups, resource routes, model binding |
| `references/views-blade.md` | Blade templates, components, layouts |
| `references/eager-loading.md` | Eager loading, prevent N+1 queries |
| `references/pagination.md` | Pagination for Blade & API |
| `references/authentication.md` | Guards, providers, Sanctum |
| `references/authorization.md` | Policies, gates, permissions |
| `references/caching.md` | Cache strategies, drivers |
| `references/events-listeners.md` | Events, listeners, observers |
| `references/testing.md` | PHPUnit, Pest testing |

### Scripts (Automation Tools)

| Script | Usage |
|--------|-------|
| `scripts/make_service.php` | Generate new Service class |

## See Also

### Related Skills

- **laravel-mvc-service-overview** - Tổng quan kiến trúc MVC + Service Pattern
- **laravel-project-structure** - Cấu trúc thư mục chuẩn Laravel
- **laravel-naming-conventions** - Quy tắc đặt tên chi tiết
- **laravel-coding-standards** - PSR-2, PSR-12, PHPDoc
- **laravel-models-best-practices** - Fat Models với Scopes, Relationships
- **laravel-controllers-guide** - Skinny Controllers best practices
- **laravel-views-blade** - Blade templates và Components
- **laravel-routing** - Routing conventions
- **laravel-service-pattern** - Service Pattern fundamentals
- **laravel-service-creation** - Tạo Service class
- **laravel-form-requests** - Form Request validation
- **laravel-service-workflows** - CRUD hoàn chỉnh với Service

### External References

- **Laravel Documentation**: https://laravel.com/docs
- **Laravel Contribution Guide**: https://laravel.com/docs/contributions
- **Spatie Guidelines**: https://spatie.be/guidelines/laravel-php
- **Laravel Best Practices**: https://github.com/alexeymezenin/laravel-best-practices

---

**Version**: 1.2.0 | **Last Updated**: 2025-01-28
