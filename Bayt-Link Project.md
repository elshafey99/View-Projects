# 🏠 Bayt Link - Property Owners Association Management System

<div align="center">

![Laravel](https://img.shields.io/badge/Laravel-12.0-FF2D20?style=for-the-badge&logo=laravel&logoColor=white)
![Livewire](https://img.shields.io/badge/Livewire-3.6-FB70A9?style=for-the-badge&logo=livewire&logoColor=white)
![PHP](https://img.shields.io/badge/PHP-8.2+-777BB4?style=for-the-badge&logo=php&logoColor=white)
![Sanctum](https://img.shields.io/badge/Sanctum-4.0-FF2D20?style=for-the-badge&logo=laravel&logoColor=white)

**A comprehensive property management platform for homeowner associations in the Middle East**

[🇸🇦 العربية](#arabic) | [🇬🇧 English](#english)

</div>

---

<a name="english"></a>

## 📋 Project Overview

**Bayt Link** is a full-stack property management application designed to digitize and streamline homeowner association operations. The platform enables property owners, tenants, and association members to manage all property affairs efficiently and transparently through mobile and web interfaces.

---

## 🎯 My Role & Contributions

As a **Backend Developer**, I was responsible for building the entire backend infrastructure and API system for this project. My work included:

### 🔧 Architecture & Design
- Implemented **Repository Pattern** for clean data access layer separation
- Designed **Service Layer Architecture** for business logic encapsulation
- Built scalable **RESTful API** following industry best practices
- Structured the codebase for maintainability and extensibility

### 🔐 Authentication & Security
- Implemented secure authentication system using **Laravel Sanctum**
- Built complete **OTP verification** system for phone number authentication
- Developed **password reset flow** with OTP verification
- Created multi-guard authentication (Owner Login, Member Login, Admin Login)
- Implemented role-based access control with **Spatie Permissions Pattern**

### 📱 API Development (30+ Controllers)
Built comprehensive RESTful APIs for:
- **Property Management**: Full CRUD for properties with profile management
- **Building Management**: Hierarchical structure (Property → Building → Floor → Unit)
- **Unit Assignments**: Managing residents, owners, and tenants relationships
- **Subscription System**: Trial, activation, upgrade, downgrade, renewal, cancellation
- **Voting/Poll System**: Complete voting mechanism with results and analytics
- **Announcements**: Advertisement system with file attachments
- **Complaints & Suggestions**: Issue tracking with status management
- **Service Providers**: Directory of technicians with ratings

### 🖥️ Admin Dashboard (Livewire)
Developed admin panel components using **Livewire 3**:
- Properties Management (CRUD)
- Buildings Management
- Subscription Plans Management
- User & Admin Management
- Role & Permission Management
- Location Management (Countries, Governorates, Centers)
- Settings Management

---

## 🛠️ Technical Stack

| Category | Technology |
|----------|------------|
| **Framework** | Laravel 12 |
| **PHP Version** | 8.2+ |
| **API Authentication** | Laravel Sanctum |
| **Frontend (Dashboard)** | Livewire 3 |
| **Database** | MySQL / SQLite |
| **Localization** | Laravel Localization (AR/EN) |
| **Translations** | Spatie Laravel Translatable |
| **OTP System** | ichtrojan/laravel-otp |
| **Notifications** | PHP Flasher |

---

## 📁 Project Structure

```
app/
├── Channels/           # WhatsApp notification channel
├── Helpers/            # API Response & File helpers
├── Http/
│   ├── Controllers/    # 38 API & Dashboard controllers
│   ├── Requests/       # 53 Form Request validators
│   └── Resources/      # 41 API Resources for response formatting
├── Livewire/           # 32 Livewire components for dashboard
├── Models/             # 48 Eloquent models
├── Notifications/      # Custom notification classes
├── Repositories/       # 29 Repository classes
├── Services/           # 30 Service classes
└── View/               # Custom Blade components
```

---

## 📊 Database Design

Designed and implemented **49 database migrations** covering:

### Core Entities
- `users` - Multi-type user management
- `properties` - Property/compound data
- `buildings` - Building records
- `floors` - Floor levels
- `units` - Individual units (apartments, shops, offices)
- `unit_assignments` - User-to-unit relationships

### Business Features
- `subscriptions` - Subscription management
- `plans` & `plan_prices` - Pricing tiers per country
- `polls`, `poll_options`, `poll_votes` - Voting system
- `advertisements` - Announcement system
- `complaints_suggestions` - Issue tracking
- `service_providers` & `service_provider_rates` - Technician directory

### Supporting Tables
- `countries`, `governorates`, `center_govs` - Location hierarchy
- `attachments` - File management with visibility rules
- `invoices`, `payments` - Financial records

---

## 🔌 API Endpoints Summary

### Authentication (`/api/auth`)
```
POST /register          - User registration
POST /login             - Owner login
POST /owner/login       - Owner-specific login
POST /member/login      - Member-specific login
POST /logout            - Logout (protected)
POST /otp/send          - Send OTP
POST /otp/verify        - Verify OTP
POST /password/forgot   - Forgot password
POST /password/reset    - Reset password
```

### Properties (`/api/properties`)
```
POST   /                          - Register new property
GET    /{id}                      - Get property details
GET    /{id}/profile              - Get property profile
POST   /{id}/profile              - Update property profile
POST   /{id}/attachments          - Upload attachments
GET    /{id}/attachments          - List attachments
```

### Buildings & Units
```
POST   /{property}/buildings                        - Create building
POST   /{property}/buildings/create-with-structure  - Create with floors & units
GET    /{property}/buildings/{building}/structure   - Get building structure
POST   /{property}/buildings/{b}/floors/{f}/units   - Create unit
POST   /{property}/buildings/{b}/floors/{f}/units/{u}/assignments - Assign resident
```

### Voting System
```
GET    /polls                     - List polls
POST   /polls                     - Create poll
POST   /polls/{id}/vote           - Cast vote
GET    /polls/{id}/results        - Get results
POST   /polls/{id}/close          - Close poll
POST   /polls/{id}/final-decision - Set final decision
```

### Subscriptions
```
GET    /subscriptions/current     - Current subscription
POST   /subscriptions/trial       - Start trial
POST   /subscriptions/activate    - Activate subscription
POST   /subscriptions/upgrade     - Upgrade plan
POST   /subscriptions/renew       - Renew subscription
```

---

## 🎨 Key Features Implemented

### 1. **Multi-Level Property Structure**
```
Property (Compound)
    └── Buildings
        └── Floors
            └── Units
                └── Unit Assignments (Residents)
```

### 2. **Smart Subscription System**
- 14-day free trial
- Monthly & yearly billing cycles
- Country-specific pricing
- Unit-based add-ons
- Automatic expiration handling

### 3. **Complete Voting Mechanism**
- Multiple vote types (Board only, Owners, Tenants, All)
- Real-time vote counting
- Percentage-based results
- Final decision recording
- Poll lifecycle (Active → Closed)

### 4. **Service Provider Directory**
- Categorized technicians (Electrical, Plumbing, Carpentry, etc.)
- Rating system
- Contact information management

### 5. **Multilingual Support**
- Arabic & English interfaces
- Translatable model attributes
- Localized validation messages
- RTL/LTR support

---

## 💻 Code Examples

### 1. API Response Helper
A standardized helper class for consistent API responses across the application:

```php
<?php

namespace App\Helpers;

class ApiResponse
{
    public static function success($data = null, string $message = 'Success', int $code = 200): JsonResponse
    {
        return response()->json([
            'success' => true,
            'status' => $code,
            'message' => $message,
            'data' => $data,
        ], $code);
    }

    public static function error(string $message = 'Error', int $code = 400, $errors = null): JsonResponse
    {
        $response = [
            'success' => false,
            'status' => $code,
            'message' => $message,
        ];

        if ($errors) {
            $response['errors'] = $errors;
        }

        return response()->json($response, $code);
    }

    public static function paginated($data, $paginator, string $message = 'Success', int $code = 200): JsonResponse
    {
        return response()->json([
            'success' => true,
            'status' => $code,
            'message' => $message,
            'data' => $data,
            'pagination' => [
                'current_page' => $paginator->currentPage(),
                'last_page' => $paginator->lastPage(),
                'per_page' => $paginator->perPage(),
                'total' => $paginator->total(),
            ],
        ], $code);
    }
}
```

### 2. Repository Pattern Implementation
Example of the Poll Repository with clean data access methods:

```php
<?php

namespace App\Repositories\Api\Poll;

class PollRepository
{
    public function getByBuildingId(int $buildingId, array $filters = [], int $perPage = 15): LengthAwarePaginator
    {
        $query = Poll::where('building_id', $buildingId)
            ->with(['category', 'voteType', 'starter'])
            ->withCount('votes');

        // Apply filters dynamically
        if (!empty($filters['status'])) {
            $query->where('status', $filters['status']);
        }

        if (!empty($filters['category_id'])) {
            $query->where('category_id', $filters['category_id']);
        }

        if (!empty($filters['year'])) {
            $query->whereYear('start_date', $filters['year']);
        }

        return $query->orderBy('created_at', 'desc')->paginate($perPage);
    }

    public function hasUserVoted(int $pollId, int $userId): bool
    {
        return PollVote::where('poll_id', $pollId)
            ->where('user_id', $userId)
            ->exists();
    }
}
```

### 3. Service Layer - Subscription Management
Business logic encapsulation for subscription handling:

```php
<?php

namespace App\Services\Api\Subscription;

class SubscriptionService
{
    protected SubscriptionRepository $subscriptionRepository;

    public function startTrial(int $propertyId, int $planId): Subscriptions
    {
        $plan = Plan::findOrFail($planId);

        if ($plan->isFree()) {
            throw new \Exception(__('front.cannot_trial_free_plan'));
        }

        // Cancel current subscription
        $this->subscriptionRepository->cancelCurrentSubscription($propertyId);

        return $this->subscriptionRepository->create([
            'property_id' => $propertyId,
            'plan_id' => $plan->id,
            'cycle' => 'monthly',
            'is_first_year' => true,
            'starts_at' => now(),
            'trial_ends_at' => now()->addDays($plan->trial_days),
            'status' => 'trial',
        ]);
    }

    public function upgrade(int $propertyId, int $newPlanId, string $cycle, int $countryId): Subscriptions
    {
        $newPlan = Plan::findOrFail($newPlanId);

        if ($newPlan->isFree()) {
            return $this->downgradeToFree($propertyId, $countryId);
        }

        $this->subscriptionRepository->cancelCurrentSubscription($propertyId);

        $endsAt = $cycle === 'yearly' ? now()->addYear() : now()->addMonth();

        return $this->subscriptionRepository->create([
            'property_id' => $propertyId,
            'plan_id' => $newPlan->id,
            'cycle' => $cycle,
            'starts_at' => now(),
            'ends_at' => $endsAt,
            'status' => 'active',
        ]);
    }
}
```

### 4. API Resource with Localization
Transforming data with multi-language support:

```php
<?php

namespace App\Http\Resources\Poll;

class PollResource extends JsonResource
{
    public function toArray(Request $request): array
    {
        $locale = app()->getLocale();

        return [
            'id' => $this->id,
            'title' => $this->getTranslation('title', $locale),
            'description' => $this->getTranslation('description', $locale),
            'category' => $this->when($this->relationLoaded('category'), fn() => [
                'id' => $this->category->id,
                'name' => $this->category->getTranslation('name', $locale),
            ]),
            'vote_type' => $this->when($this->relationLoaded('voteType'), fn() => [
                'id' => $this->voteType->id,
                'key' => $this->voteType->key,
                'name' => $this->voteType->getTranslation('name', $locale),
            ]),
            'status' => $this->status,
            'can_vote' => $this->canVote(),
            'options' => PollOptionResource::collection($this->whenLoaded('options')),
            'results' => $this->when($this->relationLoaded('votes'), fn() => $this->calculateResults()),
            'total_votes' => $this->votes_count ?? $this->votes->count(),
        ];
    }

    protected function calculateResults(): array
    {
        $totalWeight = $this->votes->sum('vote_weight');

        return $this->options->map(fn($option) => [
            'option_id' => $option->id,
            'option_text' => $option->getTranslation('option_text', app()->getLocale()),
            'votes_count' => $this->votes->where('option_id', $option->id)->count(),
            'percentage' => $totalWeight > 0 
                ? round(($this->votes->where('option_id', $option->id)->sum('vote_weight') / $totalWeight) * 100, 2) 
                : 0,
        ])->toArray();
    }
}
```

### 5. Controller with Dependency Injection
Clean controller using services and authorization:

```php
<?php

namespace App\Http\Controllers\Api\Poll;

class PollController extends Controller
{
    protected PollService $pollService;
    protected PropertyService $propertyService;
    protected BuildingService $buildingService;

    public function __construct(
        PollService $pollService,
        PropertyService $propertyService,
        BuildingService $buildingService
    ) {
        $this->pollService = $pollService;
        $this->propertyService = $propertyService;
        $this->buildingService = $buildingService;
    }

    public function index(int $propertyId, int $buildingId, Request $request): JsonResponse
    {
        // Check if user has access to property
        if (!$this->propertyService->userHasAccess($propertyId, Auth::id())) {
            return ApiResponse::error(__('api.unauthorized_access'), 403);
        }

        // Check if building belongs to property
        if (!$this->buildingService->belongsToProperty($buildingId, $propertyId)) {
            return ApiResponse::error(__('api.building_not_found'), 404);
        }

        $filters = [
            'status' => $request->query('status'),
            'category_id' => $request->query('category_id'),
            'year' => $request->query('year'),
        ];

        $polls = $this->pollService->getByBuildingId($buildingId, $filters, $request->query('per_page', 15));

        return ApiResponse::success([
            'polls' => PollListResource::collection($polls),
            'pagination' => [
                'current_page' => $polls->currentPage(),
                'last_page' => $polls->lastPage(),
                'total' => $polls->total(),
            ],
        ]);
    }
}
```

---

## 🚀 What I Learned

- Advanced Laravel architecture patterns (Repository, Service)
- Building scalable multi-tenant SaaS applications
- Complex hierarchical data modeling
- Arabic/English localization best practices
- Mobile-first API design
- Subscription & billing system implementation

---

<a name="arabic"></a>

## 🇸🇦 نظرة عامة (العربية)

### دوري في المشروع

قمت ببناء **البنية التحتية الكاملة للـ Backend** لهذا المشروع، شاملة:

✅ نظام API كامل باستخدام Laravel Sanctum  
✅ لوحة تحكم تفاعلية باستخدام Livewire 3  
✅ نظام مصادقة متقدم مع OTP  
✅ إدارة العقارات والمباني والوحدات  
✅ نظام التصويت والاستفتاءات  
✅ نظام الاشتراكات والباقات  
✅ نظام الشكاوي والاقتراحات  
✅ دليل مزودي الخدمات والفنيين  
✅ دعم كامل للغة العربية والإنجليزية  

---

## 📞 Contact

**Developer:** Mohamed Elshafey  
**Role:** Backend Developer (Laravel)

---

<div align="center">

**© 2025 Bayt Link - All Rights Reserved**

</div>

