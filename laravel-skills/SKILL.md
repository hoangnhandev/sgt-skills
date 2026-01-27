---
name: laravel
description: Laravel best practices skill for MVC Service architecture. Use this skill when working with Laravel applications, creating controllers, models, services, or following Laravel conventions. Covers Service Layer pattern, thin controllers, Form Request validation, and Eloquent best practices. Ideal for building maintainable Laravel applications with proper separation of concerns.
---

# Laravel Best Practices

MVC Service architecture for Laravel applications. Follow modern Laravel conventions with Service Layer pattern.

## When to Use

- Creating new Controllers, Models, or Services
- Refactoring code to follow best practices
- Creating API endpoints with proper validation
- Implementing complex business logic
- Setting up Laravel project structure

## Core Architecture

```
app/
├── Http/
│   ├── Controllers/     # Thin controllers
│   └── Requests/        # Form Request validation
├── Models/              # Fat models with scopes
└── Services/            # Business logic
```

## Workflow

### 1. Create Service Class

Use the script to generate a new Service class:

```bash
cd /path/to/laravel/project
php .claude/skills/laravel/scripts/make_service.php <ServiceName>
```

Example: `php .claude/skills/laravel/scripts/make_service.php UserService`

### 2. Create Form Request

```bash
php artisan make:request StoreUserRequest
php artisan make:request UpdateUserRequest
```

### 3. Implement Controller

Inject Service into Controller via constructor:

```php
class UserController extends Controller
{
    public function __construct(
        private UserService $userService
    ) {}
}
```

### 4. Create Model with Scopes

```php
class User extends Model
{
    public function scopeActive($query) {
        return $query->where('is_active', true);
    }
}
```

## Key Principles

1. **Thin Controllers** - Handle HTTP only, delegate business logic to Services
2. **Fat Models** - Data queries, scopes, accessors in Models
3. **Form Requests** - Separate validation from controllers
4. **Single Responsibility** - Each class has one clear purpose

## Resources

### References

| File | Content |
|------|---------|
| `references/service-layer.md` | Service class patterns, when to use/not use |
| `references/controller-best-practices.md` | Thin controllers, RESTful conventions |
| `references/request-validation.md` | Form Request validation, custom rules |
| `references/eloquent-best-practices.md` | Model scopes, accessors, relationships, eager loading |
| `references/api-resources.md` | Transform models to JSON responses, collections |
| `references/middleware.md` | Request filtering, auth, logging, CORS |

### Scripts

| Script | Usage |
|--------|-------|
| `scripts/make_service.php` | Generate new Service class |

## Quick Examples

### Controller + Service

```php
// Controller
public function store(StoreUserRequest $request)
{
    $user = $this->userService->createUser($request->validated());
    return response()->json($user, 201);
}

// Service
public function createUser(array $data): User
{
    return User::create($data);
}
```

### Model Scope

```php
// In Model
public function scopeActive($query) { ... }

// Usage
User::active()->get();
```

### Validation

```php
// StoreUserRequest.php
public function rules(): array
{
    return ['email' => ['required', 'email', 'unique:users']];
}
```

## Common Tasks

**Create CRUD for Entity:**
1. `make_service.php EntityService`
2. `make:model Entity -m` (with migration)
3. `make:request StoreEntityRequest`
4. `make:request UpdateEntityRequest`
5. `make:controller EntityController --api`

**N+1 Query Prevention:**
```php
// ❌ Bad
Post::all()->each(fn($p) => $p->user->name);

// ✅ Good
Post::with('user')->get();
```

---

**See `references/` for detailed pattern documentation.**
