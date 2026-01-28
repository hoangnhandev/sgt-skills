# Laravel Skills Plan - MVC + Service Pattern Architecture

> **Version**: 1.2
> **Created**: 2025-01-27
> **Updated**: 2025-01-27
> **Architecture**: MVC + Service Pattern

---

## Table of Contents

- [Overview](#overview)
- [Architecture](#architecture)
- [Skills List](#skills-list)
- [Skill Folder Structure](#skill-folder-structure)
- [Detailed Skills](#detailed-skills)
- [Indexing & Retrieval Strategy](#indexing--retrieval-strategy)
- [Implementation Priority](#implementation-priority)

---

## Overview

Kế hoạch tạo bộ Laravel Skills tập trung vào kiến trúc **MVC + Service Pattern** - một pattern phổ biến trong cộng đồng Laravel để tổ chức code theo cách:

- **Controllers**: Chịu trách nhiệm Request/Response, thin và delegate cho Services
- **Services**: Chứa business logic, orchestrate nhiều Models
- **Models**: Fat models với Scopes, Relationships, Accessors
- **Form Requests**: Validation và Authorization tách riêng

---

## Architecture

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
│ Controller  │ ← Delegate, không chứa business logic
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

---

## Skills List

### 🔴 PART 1: FOUNDATION (4 skills)

| # | Skill | Description | Priority |
|---|-------|-------------|----------|
| 1 | `laravel-mvc-service-overview` | Tổng quan kiến trúc MVC + Service | 🔴 High |
| 2 | `laravel-project-structure` | Cấu trúc thư mục chuẩn Laravel | 🔴 High |
| 3 | `laravel-naming-conventions` | Quy tắc đặt tên PSR & Laravel | 🔴 High |
| 4 | `laravel-coding-standards` | PSR-2, PSR-12, PHPDoc rules | 🔴 High |

### 🟡 PART 2: MVC LAYERS (4 skills)

| # | Skill | Description | Priority |
|---|-------|-------------|----------|
| 5 | `laravel-models-best-practices` | Fat Models, Scopes, Relationships | 🟡 Medium |
| 6 | `laravel-controllers-guide` | Skinny Controllers, DI, Resource | 🟡 Medium |
| 7 | `laravel-views-blade` | Blade templates, components | 🟡 Medium |
| 8 | `laravel-routing` | Routes, groups, API routes | 🟡 Medium |

### 🟢 PART 3: SERVICE PATTERN (4 skills)

| # | Skill | Description | Priority |
|---|-------|-------------|----------|
| 9 | `laravel-service-pattern` | Service Pattern fundamentals | 🟢 Medium |
| 10 | `laravel-service-creation` | Tạo Service class, Artisan command | 🟢 Medium |
| 11 | `laravel-form-requests` | Form Request validation | 🟢 Medium |
| 12 | `laravel-service-workflows` | CRUD hoàn chỉnh với Service | 🟢 Medium |

### 🔵 PART 4: ADVANCED TOPICS (10 skills)

| # | Skill | Description | Priority |
|---|-------|-------------|----------|
| 13 | `laravel-eloquent-relationships` | Relationships best practices | 🔵 Low |
| 14 | `laravel-eager-loading` | Eager loading, tránh N+1 | 🔵 Low |
| 15 | `laravel-api-resources` | API Resources, Transformers | 🔵 Low |
| 16 | `laravel-pagination` | Blade & API pagination | 🔵 Low |
| 17 | `laravel-authentication` | Auth, guards, providers | 🔵 Low |
| 18 | `laravel-authorization` | Policies, Gates, permissions | 🔵 Low |
| 19 | `laravel-caching` | Cache strategies | 🔵 Low |
| 20 | `laravel-events-listeners` | Events, observers | 🔵 Low |
| 21 | `laravel-queues-jobs` | Queues, jobs, workers | 🔵 Low |
| 22 | `laravel-testing` | PHPUnit, Pest testing | 🔵 Low |

---

## Skill Folder Structure

### Standard Skill Structure

Mỗi skill theo format chuẩn của Agent Skills với cấu trúc:

```
skill-name/
├── SKILL.md                    # REQUIRED - Metadata + Instructions
├── scripts/                    # OPTIONAL - Executable code
│   └── *.php                  # Artisan commands, helpers
├── references/                 # OPTIONAL - Additional documentation
│   ├── TOPIC-1.md
│   ├── TOPIC-2.md
│   └── TOPIC-N.md
└── assets/                     # OPTIONAL - Templates, resources
    ├── templates/
    └── examples/
```

### Detailed Structure Breakdown

```
┌─────────────────────────────────────────────────────────────────┐
│                        SKILL.md (REQUIRED)                     │
├─────────────────────────────────────────────────────────────────┤
│                                                                  │
│  ---                                                             │
│  name: skill-name                                                │
│  description: When to use this skill...                          │
│  metadata:                                                       │
│    version: "1.0"                                                │
│  ---                                                             │
│                                                                  │
│  # Skill Title                                                   │
│                                                                  │
│  ## When to use this skill                                       │
│  ...                                                             │
│                                                                  │
│  ## Overview                                                     │
│  ...                                                             │
│                                                                  │
│  ## How to use                                                   │
│  ...                                                             │
│                                                                  │
│  ## Examples                                                     │
│  ...                                                             │
│                                                                  │
│  ## See Also                                                     │
│  ...                                                             │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────────┐
│                     scripts/ (OPTIONAL)                         │
├─────────────────────────────────────────────────────────────────┤
│                                                                  │
│  make-service.php          # Artisan command generator          │
│  create-crud.php           # CRUD scaffolding script            │
│  validate-skill.php        # Skill validation tool              │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────────┐
│                   references/ (OPTIONAL)                        │
├─────────────────────────────────────────────────────────────────┤
│                                                                  │
│  ARCHITECTURE.md           # Architecture diagrams              │
│  PATTERNS.md               # Design patterns                    │
│  EXAMPLES.md               # Real-world examples                │
│  TROUBLESHOOTING.md        # Common issues & solutions         │
│  BEST-PRACTICES.md         # Best practices summary            │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────────┐
│                     assets/ (OPTIONAL)                          │
├─────────────────────────────────────────────────────────────────┤
│                                                                  │
│  templates/               # Code templates                      │
│  ├── service.stub.php                                             │
│  ├── controller.stub.php                                         │
│  └── model.stub.php                                             │
│                                                                  │
│  examples/                # Full working examples               │
│  ├── user-management/                                             │
│  └── blog-system/                                               │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘
```

### Line Count Guidelines

| Component | Lines | Description |
|-----------|-------|-------------|
| **SKILL.md** | 150-400 | Main instruction file |
| ├─ Frontmatter | ~10 | YAML metadata |
| ├─ When to use | ~20 | Trigger conditions |
| ├─ Overview | ~50 | Core concepts |
| ├─ How to use | ~100 | Step-by-step guide |
| └─ Examples | ~150 | Code samples |
| **Reference files** | ~100 each | Detailed documentation |
| **Scripts** | ~50-100 each | Executable code |

### Example: Complete Skill Structure

```
laravel-service-pattern/
├── SKILL.md                           (~250 lines)
│   ├── Frontmatter                     (~10 lines)
│   ├── When to use this skill          (~20 lines)
│   ├── What is Service Pattern         (~40 lines)
│   ├── When to use Services            (~30 lines)
│   ├── Quick Example                   (~50 lines)
│   ├── Service vs Other Patterns       (~40 lines)
│   ├── See Also                        (~20 lines)
│   └── References                      (~40 lines)
│
├── scripts/                            (OPTIONAL)
│   └── make-service.php                (~80 lines)
│       ├── Command signature
│       ├── File creation logic
│       ├── Stub template
│       └── Error handling
│
└── references/                         (OPTIONAL)
    ├── SERVICE-BASICS.md               (~120 lines)
    │   ├── Definition
    │   ├── Responsibilities
    │   ├── Benefits
    │   └── Drawbacks
    │
    ├── WHEN-USE-SERVICES.md            (~100 lines)
    │   ├── Use cases
    │   ├── Anti-patterns
    │   └── Decision tree
    │
    └── SERVICE-VS-ACTION.md            (~150 lines)
        ├── Comparison table
        ├── Service example
        ├── Action example
        └── When to choose which
```

### File Naming Conventions

```
SKILL.md                              # REQUIRED - PascalCase for display
references/
├── ARCHITECTURE.md                    # UPPER_SNAKE for main concepts
├── DATA-FLOW.md                       # UPPER_SNAKE for diagrams
├── PATTERNS.md                        # UPPER_SNAKE for patterns
├── EXAMPLES.md                        # UPPER_SNAKE for examples
└── TROUBLESHOOTING.md                 # UPPER_SNAKE for problems

scripts/
├── make-service.php                   # kebab-case for commands
├── create-controller.php
└── generate-crud.php

assets/
├── templates/
│   ├── service.stub.php               # .stub for templates
│   └── controller.stub.php
└── examples/
    └── user-management/               # kebab-case for example folders
        ├── User.php
        └── UserService.php
```

### Content Organization Principles

```
┌─────────────────────────────────────────────────────────────────┐
│                  PROGRESSIVE DISCLOSURE PRINCIPLE               │
├─────────────────────────────────────────────────────────────────┤
│                                                                  │
│  Phase 1: SKILL.md (Quick Reference)                            │
│  ┌─────────────────────────────────────────────────────────┐   │
│  │ • Frontmatter: name + description                       │   │
│  │ • When to use: Quick decision                           │   │
│  │ • Overview: High-level understanding                     │   │
│  │ • Quick Example: Get started fast                        │   │
│  └─────────────────────────────────────────────────────────┘   │
│                           │                                     │
│                           ▼                                     │
│  Phase 2: references/ (Deep Dive)                               │
│  ┌─────────────────────────────────────────────────────────┐   │
│  │ • ARCHITECTURE.md: Detailed diagrams                     │   │
│  │ • PATTERNS.md: Pattern implementations                   │   │
│  │ • EXAMPLES.md: Real-world scenarios                      │   │
│  └─────────────────────────────────────────────────────────┘   │
│                           │                                     │
│                           ▼                                     │
│  Phase 3: scripts/ (Automation)                                 │
│  ┌─────────────────────────────────────────────────────────┐   │
│  │ • make-*.php: Generate code                             │   │
│  │ • validate-*.php: Check compliance                      │   │
│  └─────────────────────────────────────────────────────────┘   │
└─────────────────────────────────────────────────────────────────┘
```

### Reference File Types

| File Type | Purpose | Typical Size |
|-----------|---------|--------------|
| **ARCHITECTURE.md** | Diagrams, structure explanations | 100-150 lines |
| **PATTERNS.md** | Design pattern applications | 100-150 lines |
| **EXAMPLES.md** | Real-world code examples | 150-200 lines |
| **BEST-PRACTICES.md** | Do's and Don'ts | 80-120 lines |
| **TROUBLESHOOTING.md** | Common issues & fixes | 80-100 lines |
| **COMPARISON.md** | Compare approaches | 100-150 lines |
| **GUIDE.md** | Step-by-step tutorials | 150-200 lines |
| **REFERENCE.md** | Technical reference | 150-250 lines |

### Skill Interconnections

```
┌─────────────────────────────────────────────────────────────────┐
│                    SKILL RELATIONSHIP MAPPING                   │
├─────────────────────────────────────────────────────────────────┤
│                                                                  │
│  laravel-mvc-service-overview (ROOT)                            │
│  ├── ├─ laravel-project-structure                               │
│  ├── ├─ laravel-naming-conventions                              │
│  ├── ├─ laravel-coding-standards                                │
│  ├── │                                                           │
│  ├── ├─ laravel-models-best-practices ───┐                      │
│  ├── │                                   │                      │
│  ├── ├─ laravel-controllers-guide ───────┤── laravel-service-workflows
│  ├── │                                   │                      │
│  ├── ├─ laravel-service-pattern ─────────┤                      │
│  │   │                                   │                      │
│  │   └─ laravel-service-creation ────────┘                      │
│  │                                                                 │
│  └── laravel-form-requests                                        │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘
```

Each skill's SKILL.md should include:

```markdown
## See Also

Related skills to learn more:
- [laravel-service-creation](../laravel-service-creation/SKILL.md) - Creating Service classes
- [laravel-controllers-guide](../laravel-controllers-guide/SKILL.md) - Controller best practices
- [laravel-form-requests](../laravel-form-requests/SKILL.md) - Form Request validation
```

---

## Detailed Skills

### 01. laravel-mvc-service-overview

**Mô tả**: Giới thiệu kiến trúc MVC + Service Pattern trong Laravel

**Nội dung chính**:
- Cấu trúc MVC + Service
- Vai trò từng layer
- Flow dữ liệu: Route → Controller → Service → Model → Database
- Khi nào dùng Service Pattern
- So sánh các architecture patterns

**Files**:
```
01-laravel-mvc-service-overview/
├── SKILL.md
└── references/
    ├── ARCHITECTURE.md      # Chi tiết kiến trúc
    ├── DATA-FLOW.md         # Flow dữ liệu
    └── COMPARISON.md        # So sánh patterns
```

---

### 02. laravel-project-structure

**Mô tả**: Cấu trúc thư mục dự án Laravel chuẩn

**Nội dung chính**:
```
app/
├── Http/
│   ├── Controllers/
│   │   ├── Controller.php
│   │   └── UserController.php
│   ├── Middleware/
│   ├── Requests/
│   │   ├── UserRequest.php
│   │   └── AuthRequest.php
│   └── Resources/
│       └── UserResource.php
├── Models/
│   └── User.php
├── Services/
│   ├── UserService.php
│   └── Contracts/
│       └── UserServiceInterface.php
└── Providers/
    ├── AppServiceProvider.php
    └── AuthServiceProvider.php
```

**Files**:
```
02-laravel-project-structure/
├── SKILL.md
└── references/
    ├── APP-STRUCTURE.md      # Cấu trúc app/
    ├── NAMESPACE-RULES.md    # PSR-4 autoloading
    └── SERVICE-STRUCTURE.md  # Tổ chức Services
```

---

### 03. laravel-naming-conventions

**Mô tả**: Quy tắc đặt tên theo PSR & Laravel

**Nội dung chính**:

| Loại | Pattern | Ví dụ Đúng | Ví dụ Sai |
|------|---------|------------|-----------|
| Controller | singular + Controller | `UserController` | `UsersController` |
| Model | singular | `User` | `Users` |
| Service | singular + Service | `UserService` | `UserManagementService` |
| Request | singular + Request | `StoreUserRequest` | `UserFormRequest` |
| Resource | singular + Resource | `UserResource` | `UsersResource` |
| Collection | singular + Collection | `UserCollection` | `UsersCollection` |
| Contract | Interface + suffix | `UserServiceInterface` | `IUserService` |
| Route | plural | `users/1` | `user/1` |
| Route name | dot notation | `users.index` | `user-index` |
| Table | plural snake_case | `user_profiles` | `userProfile` |
| Column | snake_case | `first_name` | `firstName` |
| Migration | verb_table | `create_users_table` | `create_table_users` |
| Foreign key | singular_id | `user_id` | `idUser` |
| Pivot table | singular alphabetical | `role_user` | `user_role` |
| Method | camelCase | `getFullName()` | `get_full_name()` |
| Variable | camelCase | `$userData` | `$user_data` |
| Constant | UPPER_SNAKE | `MAX_LOGIN_ATTEMPTS` | `maxLoginAttempts` |
| Enum | singular | `UserStatus` | `UserStatuses` |
| Config file | snake_case | `google_calendar.php` | `GoogleCalendar.php` |
| View file | kebab-case | `user-profile.blade.php` | `userProfile.blade.php` |

**Files**:
```
03-laravel-naming-conventions/
├── SKILL.md
└── references/
    ├── NAMING-TABLE.md      # Bảng tra cứu
    └── EXAMPLES.md          # Ví dụ thực tế
```

---

### 04. laravel-coding-standards

**Mô tả**: PSR-2, PSR-12, PHPDoc rules

**Nội dung chính**:

**PSR Compliance**:
- PSR-1: Basic coding standard
- PSR-2: Coding style guide
- PSR-4: Autoloading standard
- PSR-12: Extended coding style

**Code Style**:
- 4 spaces indent (không dùng tabs)
- Line length ~120 characters
- Opening braces on same line
- No trailing whitespace

**Type Hints**:
```php
// Đúng - có return type
public function getUser(int $id): ?User
{
    return User::find($id);
}

// Sai - thiếu return type
public function getUser($id)
{
    return User::find($id);
}
```

**Docblocks**:
```php
// Không cần docblock khi đã type hint đầy đủ
public function createUser(array $data): User
{
    return User::create($data);
}

// Cần docblock cho generic types
/**
 * @return Collection<int, User>
 */
public function getActiveUsers(): Collection
{
    return User::active()->get();
}
```

**Files**:
```
04-laravel-coding-standards/
├── SKILL.md
└── references/
    ├── PSR-COMPLIANCE.md    # Chi tiết PSR
    ├── PHPDOC-RULES.md      # Quy tắc Docblocks
    └── EXAMPLES.md          # Code examples
```

---

### 05. laravel-models-best-practices

**Mô tả**: Fat Models - Scopes, Relationships, Accessors, Mutators

**Nội dung chính**:

```php
<?php

namespace App\Models;

use Illuminate\Database\Eloquent\Model;
use Illuminate\Database\Eloquent\Relations\BelongsTo;
use Illuminate\Database\Eloquent\Relations\HasMany;

class User extends Model
{
    protected $fillable = [
        'first_name',
        'last_name',
        'email',
        'status',
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

    public function scopeWithRecentPosts($query)
    {
        return $query->with(['posts' => function ($query) {
            $query->where('created_at', '>=', now()->subWeek());
        }]);
    }

    // ===== RELATIONSHIPS =====
    public function posts(): HasMany
    {
        return $this->hasMany(Post::class);
    }

    public function profile(): BelongsTo
    {
        return $this->belongsTo(Profile::class);
    }

    // ===== ACCESSORS =====
    public function getFullNameAttribute(): string
    {
        return "{$this->first_name} {$this->last_name}";
    }

    public function getAvatarUrlAttribute(): string
    {
        return $this->avatar ?? 'https://default.com/avatar.png';
    }

    // ===== MUTATORS =====
    public function setPasswordAttribute(string $value): void
    {
        $this->attributes['password'] = bcrypt($value);
    }
}
```

**Files**:
```
05-laravel-models-best-practices/
├── SKILL.md
└── references/
    ├── ELOQUENT-PATTERNS.md    # Patterns cho Models
    ├── SCOPES-GUIDE.md         # Local & Global Scopes
    ├── RELATIONSHIPS.md        # Relationships types
    └── ACCESSORS-MUTATORS.md   # Getters & Setters
```

---

### 06. laravel-controllers-guide

**Mô tả**: Skinny Controllers - chỉ Request/Response

**Nội dung chính**:

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
        $users = $this->userService->getAllUsers();
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

**Quy tắc**:
- Controller KHÔNG chứa business logic
- Controller KHÔNG query trực tiếp Database
- Controller chỉ: validate, delegate, return response
- Dùng Dependency Injection cho Services

**Files**:
```
06-laravel-controllers-guide/
├── SKILL.md
└── references/
    ├── SKINNY-CONTROLLERS.md      # Best practices
    ├── RESOURCE-CONTROLLERS.md    # Resource routing
    ├── DEPENDENCY-INJECTION.md    # Constructor injection
    └── RESPONSE-PATTERNS.md       # JSON responses
```

---

### 07. laravel-views-blade

**Mô tả**: Blade templates best practices

**Nội dung chính**:

**Components over @include**:
```blade
<!-- resources/views/components/user-card.blade.php -->
@props(['user'])

<div class="user-card">
    <h2>{{ $user->full_name }}</h2>
    <p>{{ $user->email }}</p>
</div>

<!-- Usage -->
<x-user-card :user="$user" />
```

**Layouts**:
```blade
<!-- resources/views/layouts/app.blade.php -->
<!DOCTYPE html>
<html>
<head>
    <title>@slot('title', 'Default')</title>
</head>
<body>
    @yield('content')

    @slot('footer')
        Default footer
    @endslot
</body>
</html>

<!-- Usage -->
@extends('layouts.app')

@section('title', 'Page Title')

@section('content')
    <h1>Content here</h1>
@endsection

@slot('footer')
    Custom footer
@endslot
```

**Quy tắc**:
- KHÔNG đặt logic trong Blade
- Sử dụng directives: @auth, @guest, @can, @foreach
- Components để reuse
- Sử dụng {{ }} thay vì {!! !!} trừ khi cần thiết

**Files**:
```
07-laravel-views-blade/
├── SKILL.md
└── references/
    ├── BLADE-BEST-PRACTICES.md  # Best practices
    ├── COMPONENTS.md            # Anonymous & Class components
    └── LAYOUTS.md               # Layout inheritance
```

---

### 08. laravel-routing

**Mô tả**: Routing best practices

**Nội dung chính**:

```php
// routes/web.php
Route::prefix('admin')
    ->middleware(['auth', 'admin'])
    ->group(function () {
        Route::resource('users', UserController::class);
        Route::resource('posts', PostController::class);
    });

// routes/api.php
Route::middleware('auth:sanctum')->group(function () {
    Route::apiResource('users', UserController::class);
    Route::apiResource('posts', PostController::class);
});
```

**Quy tắc**:
- Dùng resource controllers khi có thể
- Group routes với common prefix/middleware
- API routes dùng `apiResource`
- Route model binding thay vì find()

**Files**:
```
08-laravel-routing/
├── SKILL.md
└── references/
    ├── ROUTE-GROUPS.md           # Route grouping
    ├── API-ROUTES.md             # API routing
    └── ROUTE-MODEL-BINDING.md    # Implicit & explicit binding
```

---

### 09. laravel-service-pattern

**Mô tả**: Service Pattern fundamentals

**Nội dung chính**:

**Service là gì?**
- Class chứa business logic
- Orchestrate nhiều Models
- Handle transactions
- Coordinate giữa các components

**Khi nào dùng Service?**
- Logic phức tạp hơn CRUD đơn giản
- Cần interact với nhiều Models
- Cần transaction management
- Cần reusable business logic

**Ví dụ Service**:
```php
<?php

namespace App\Services;

use App\Models\User;
use App\Models\Profile;
use Illuminate\Support\Facades\DB;
use Illuminate\Support\Facades\Hash;

class UserService
{
    public function __construct(
        private User $userModel,
        private Profile $profileModel
    ) {}

    public function createUserWithProfile(array $userData, array $profileData): User
    {
        return DB::transaction(function () use ($userData, $profileData) {
            $user = $this->userModel->create([
                'name' => $userData['name'],
                'email' => $userData['email'],
                'password' => Hash::make($userData['password']),
            ]);

            $user->profile()->create($profileData);

            return $user;
        });
    }
}
```

**Files**:
```
09-laravel-service-pattern/
├── SKILL.md
└── references/
    ├── SERVICE-BASICS.md         # Fundamentals
    ├── WHEN-USE-SERVICES.md      # When & why
    └── SERVICE-VS-ACTION.md      # Comparison
```

---

### 10. laravel-service-creation

**Mô tả**: Tạo Service class đúng cách

**Nội dung chính**:

**Cấu trúc Service**:
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

    // CRUD Operations
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

**Artisan Command**:
```php
<?php

namespace App\Console\Commands;

use Illuminate\Console\Command;

class MakeServiceCommand extends Command
{
    protected $signature = 'make:service {name}';
    protected $description = 'Create a new service class';

    public function handle()
    {
        $name = $this->argument('name');
        $path = app_path("Services/{$name}.php");

        if (file_exists($path)) {
            $this->error('Service already exists!');
            return Command::FAILURE;
        }

        // Create service file
        file_put_contents($path, $this->getStub($name));

        $this->info("Service created successfully: {$name}");
        return Command::SUCCESS;
    }

    private function getStub(string $name): string
    {
        return <<<PHP
        <?php

        namespace App\Services;

        class {$name}
        {
            public function __construct()
            {
                //
            }

            //
        }
        PHP;
    }
}
```

**Files**:
```
10-laravel-service-creation/
├── SKILL.md
├── scripts/
│   └── make-service.php      # Artisan command stub
└── references/
    ├── SERVICE-TEMPLATES.md      # Service templates
    ├── DEPENDENCY-INJECTION.md   # DI in Services
    └── INTERFACE-CONTRACTS.md    # Contracts & Interfaces
```

---

### 11. laravel-form-requests

**Mô tả**: Form Request validation & authorization

**Nội dung chính**:

```php
<?php

namespace App\Http\Requests;

use Illuminate\Foundation\Http\FormRequest;
use Illuminate\Contracts\Validation\Validator;
use Illuminate\Http\Exceptions\HttpResponseException;

class StoreUserRequest extends FormRequest
{
    public function authorize(): bool
    {
        // Authorization logic
        return true;
        // Or use Gates/Policies
        // return $this->user()->can('create', User::class);
    }

    public function rules(): array
    {
        return [
            'name' => 'required|string|max:255',
            'email' => 'required|email|unique:users,email',
            'password' => 'required|string|min:8|confirmed',
        ];
    }

    public function messages(): array
    {
        return [
            'email.required' => 'Email là bắt buộc',
            'email.email' => 'Email không đúng định dạng',
        ];
    }

    protected function failedValidation(Validator $validator)
    {
        throw new HttpResponseException(
            response()->json([
                'errors' => $validator->errors()
            ], 422)
        );
    }
}
```

**Files**:
```
11-laravel-form-requests/
├── SKILL.md
└── references/
    ├── VALIDATION-RULES.md       # Common rules
    ├── AUTHORIZATION.md          # authorize() method
    ├── ERROR-RESPONSES.md        # Custom error handling
    └── CONDITIONAL-RULES.md      # sometimes(), requiredIf()
```

---

### 12. laravel-service-workflows

**Mô tả**: CRUD hoàn chỉnh với Service Pattern

**Nội dung chính**:

**Complete Flow**:
```
┌─────────────┐    ┌─────────────┐    ┌─────────────┐    ┌─────────────┐
│   Request   │ -> │ Controller  │ -> │   Service   │ -> │    Model    │
│ FormRequest │    │ Delegate    │    │ Logic       │    │ Eloquent    │
└─────────────┘    └─────────────┘    └─────────────┘    └─────────────┘
```

**Ví dụ hoàn chỉnh**:

**1. Routes** (`routes/api.php`):
```php
Route::apiResource('users', UserController::class);
```

**2. Form Request** (`app/Http/Requests/StoreUserRequest.php`):
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

**3. Controller** (`app/Http/Controllers/UserController.php`):
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

**4. Service** (`app/Services/UserService.php`):
```php
class UserService
{
    public function createUser(array $data): User
    {
        return User::create($data);
    }
}
```

**5. Model** (`app/Models/User.php`):
```php
class User extends Model
{
    protected $fillable = ['name', 'email', 'password'];
}
```

**Transaction Handling**:
```php
public function createUserWithSubscription(array $userData, array $subscriptionData): User
{
    return DB::transaction(function () use ($userData, $subscriptionData) {
        $user = User::create($userData);
        $user->subscription()->create($subscriptionData);

        // Dispatch event
        event(new UserCreated($user));

        return $user;
    });
}
```

**Error Handling**:
```php
use Illuminate\Database\Eloquent\ModelNotFoundException;
use Illuminate\Database\QueryException;

public function deleteUser(int $id): bool
{
    try {
        $user = User::findOrFail($id);
        return $user->delete();
    } catch (ModelNotFoundException $e) {
        throw new UserNotFoundException("User with ID {$id} not found");
    } catch (QueryException $e) {
        throw new UserDeletionException("Failed to delete user");
    }
}
```

**Files**:
```
12-laravel-service-workflows/
├── SKILL.md
└── references/
    ├── CRUD-PATTERN.md           # CRUD patterns
    ├── TRANSACTION-HANDLING.md   # Database transactions
    ├── ERROR-HANDLING.md         # Exception handling
    └── COMPLETE-EXAMPLE.md       # Full CRUD example
```

---

### 13-22. ADVANCED SKILLS

Chi tiết cho các skills nâng cao (13-22) sẽ được bổ sung sau:

| # | Skill | Key Topics |
|---|-------|------------|
| 13 | `laravel-eloquent-relationships` | OneToOne, OneToMany, ManyToMany, Polymorphic |
| 14 | `laravel-eager-loading` | with(), load(), eager loading limits |
| 15 | `laravel-api-resources` | JsonResource, ResourceCollection |
| 16 | `laravel-pagination` | paginate(), simplePaginate(), cursorPaginate() |
| 17 | `laravel-authentication` | Guards, Providers, Sanctum |
| 18 | `laravel-authorization` | Policies, Gates, Permissions |
| 19 | `laravel-caching` | Cache drivers, tags, locking |
| 20 | `laravel-events-listeners` | Events, Listeners, Observers |
| 21 | `laravel-queues-jobs` | Jobs, Dispatching, Workers |
| 22 | `laravel-testing` | PHPUnit, Pest, Feature tests |

---

## Implementation Priority

### Phase 1: Core Foundation (Week 1-2)

```
✓ 01-laravel-mvc-service-overview
✓ 02-laravel-project-structure
✓ 03-laravel-naming-conventions
✓ 04-laravel-coding-standards
```

### Phase 2: MVC Basics (Week 3-4)

```
✓ 05-laravel-models-best-practices
✓ 06-laravel-controllers-guide
✓ 07-laravel-views-blade
✓ 08-laravel-routing
```

### Phase 3: Service Pattern (Week 5-6)

```
✓ 09-laravel-service-pattern
✓ 10-laravel-service-creation
✓ 11-laravel-form-requests
✓ 12-laravel-service-workflows
```

### Phase 4: Advanced (Week 7+)

```
○ 13-22: Advanced skills (optional)
```

---

## Reference Sources

Kế hoạch này được xây dựng dựa trên:

1. **Laravel 12.x Official Documentation**
   - https://laravel.com/docs/12.x
   - Contribution Guide, PSR Standards

2. **Spatie Laravel & PHP Guidelines**
   - https://spatie.be/guidelines/laravel-php
   - Coding standards, naming conventions

3. **alexeymezenin/laravel-best-practices** (12k+ stars)
   - https://github.com/alexeymezenin/laravel-best-practices
   - Community best practices

4. **Community Resources**
   - Strapi Laravel Best Practices
   - Medium articles, Dev.to posts
   - Reddit discussions

---

## Indexing & Retrieval Strategy

### Overview

Laravel Skills được thiết kế để hoạt động với **RAG/Vector Database system** sử dụng **BM25 enhanced scoring**. Kế hoạch indexing đảm bảo agents có thể discover và sử dụng skills hiệu quả.

### Progressive Disclosure Indexing

```
┌─────────────────────────────────────────────────────────────────┐
│                    AGENT SKILLS DISCOVERY                      │
├─────────────────────────────────────────────────────────────────┤
│                                                                  │
│  Phase 1: DISCOVERY (~100 tokens)                               │
│  ┌─────────────────────────────────────────────────────────┐   │
│  │ Agent loads at startup:                                 │   │
│  │ - skill_name (weight: 3.0)                             │   │
│  │ - skill_description (weight: 2.5)                      │   │
│  └─────────────────────────────────────────────────────────┘   │
│                           │                                     │
│                           ▼                                     │
│  Phase 2: ACTIVATION (<5000 tokens)                           │
│  ┌─────────────────────────────────────────────────────────┐   │
│  │ When task matches description, load full SKILL.md:      │   │
│  │ - when_to_use (weight: 2.2)                            │   │
│  │ - overview (weight: 1.8)                               │   │
│  │ - examples (weight: 2.0)                               │   │
│  │ - how_to_use (weight: 1.5)                             │   │
│  └─────────────────────────────────────────────────────────┘   │
│                           │                                     │
│                           ▼                                     │
│  Phase 3: EXECUTION (as needed)                               │
│  ┌─────────────────────────────────────────────────────────┐   │
│  │ Load reference files on demand:                         │   │
│  │ - references/*.md (weight: 1.2)                        │   │
│  │ - scripts/*.php (weight: 1.5)                          │   │
│  └─────────────────────────────────────────────────────────┘   │
└─────────────────────────────────────────────────────────────────┘
```

### BM25 Enhancement for Laravel

#### Laravel Term Boosts

```python
# Domain-specific term weights
LARAVEL_TERM_BOOSTS = {
    # Framework keywords - highest boost
    'eloquent': 2.5,
    'artisan': 2.5,
    'blade': 2.5,
    'migration': 2.3,
    'service': 2.2,
    'controller': 2.2,
    'model': 2.2,
    'request': 2.1,
    'validation': 2.1,
    'middleware': 2.1,

    # Architecture patterns
    'mvc': 2.0,
    'service pattern': 2.3,
    'fat models': 2.0,
    'skinny controllers': 2.0,

    # Database/ORM
    'relationship': 2.0,
    'scope': 2.0,
    'eager loading': 2.2,

    # Coding standards
    'psr-2': 1.8,
    'psr-4': 1.8,
    'naming convention': 1.9,
}
```

#### Field-Specific Weights

```python
FIELD_WEIGHTS = {
    # Frontmatter - Discovery phase
    'skill_name': 3.0,
    'skill_description': 2.5,

    # SKILL.md sections - Activation phase
    'when_to_use': 2.2,
    'examples': 2.0,
    'overview': 1.8,
    'how_to_use': 1.5,

    # Reference files - Execution phase
    'references': 1.2,
    'code_blocks': 1.8,
}
```

### Document Structure for Indexing

```json
{
    "skill_name": "laravel-service-pattern",
    "skill_description": "Service Pattern fundamentals for Laravel...",
    "when_to_use": "Use when: business logic spans multiple models...",
    "overview": "Service Pattern adalah...",
    "examples": ["class UserService {...}"],
    "references": ["references/SERVICE-BASICS.md"],
    "related_skills": [
        "laravel-service-creation",
        "laravel-service-workflows",
        "laravel-controllers-guide"
    ],
    "metadata": {
        "author": "sgt-skills",
        "version": "1.0"
    }
}
```

### Hybrid Retrieval Strategy

```
┌─────────────────────────────────────────────────────────────────┐
│                    HYBRID RETRIEVAL ARCHITECTURE               │
├─────────────────────────────────────────────────────────────────┤
│                                                                  │
│  User Query: "how to create service in laravel"                 │
│                                                                  │
│  ┌─────────────────┐     ┌─────────────────┐                   │
│  │ Laravel Skills  │     │ Official Docs   │                   │
│  │ (Domain-specific)│     │ (Comprehensive) │                   │
│  │                 │     │                 │                   │
│  │ Weight: 70%     │     │ Weight: 30%     │                   │
│  └────────┬────────┘     └────────┬────────┘                   │
│           │                      │                              │
│           └──────────┬───────────┘                              │
│                      ▼                                          │
│           ┌─────────────────────┐                               │
│           │ Query Intent Class  │                               │
│           │ - how_to (→ Skills) │                               │
│           │ - reference (→ Docs)│                               │
│           │ - example (→ Skills)│                               │
│           └─────────────────────┘                               │
│                      │                                          │
│                      ▼                                          │
│           ┌─────────────────────┐                               │
│           │ Related Skills Chain│                               │
│           │ Expand depth=1      │                               │
│           └─────────────────────┘                               │
└─────────────────────────────────────────────────────────────────┘
```

### Query Expansion

```python
# Laravel-specific query expansion
QUERY_EXPANSIONS = {
    'create': ['store', 'insert', 'make'],
    'read': ['index', 'show', 'get', 'find', 'retrieve'],
    'update': ['update', 'edit', 'modify'],
    'delete': ['destroy', 'delete', 'remove'],
    'database': ['model', 'eloquent', 'query', 'migration'],
    'service': ['service', 'business logic', 'orchestrate'],
}
```

### Indexing Implementation Checklist

```markdown
## BM25 & Indexing Tasks

### Phase 1: Core Indexing
- [ ] Parse SKILL.md frontmatter (name, description)
- [ ] Extract sections (when_to_use, overview, examples)
- [ ] Index related_skills references
- [ ] Create document schema for RAG

### Phase 2: BM25 Enhancement
- [ ] Implement Laravel term boosts
- [ ] Add field-specific weights
- [ ] Implement code pattern matching
- [ ] Add query expansion

### Phase 3: Hybrid Retrieval
- [ ] Index Laravel official docs
- [ ] Implement skills + docs hybrid search
- [ ] Add query intent classification
- [ ] Implement related skills chaining

### Phase 4: Optimization
- [ ] Add caching for common queries
- [ ] Benchmark relevance improvements
- [ ] Optimize query performance
```

### Expected Impact

| Metric | Before BM25 | After Enhancement |
|--------|-------------|-------------------|
| **Relevance** | Baseline | +30-50% |
| **Speed** | Baseline | +10-20% (with caching) |
| **User Satisfaction** | Baseline | +40% |

---

## Notes

- Mỗi skill nên giữ **SKILL.md dưới 500 dòng** (theo Agent Skills spec)
- Chi tiết đặt trong `references/` để progressive disclosure
- Scripts được cung cấp trong thư mục `scripts/`
- Mọi code examples theo **PSR-12** và **Laravel conventions**

---

*Document Version: 1.2*
*Last Updated: 2025-01-27*
*Changes:*
  - v1.1: Added Indexing & Retrieval Strategy section with BM25 optimization
  - v1.2: Added Skill Folder Structure section with detailed breakdown
