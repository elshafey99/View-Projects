# 🚀 Enterprise SaaS ERP System

<div align="center">

![Laravel](https://img.shields.io/badge/Laravel-11-FF2D20?style=for-the-badge&logo=laravel&logoColor=white)
![PHP](https://img.shields.io/badge/PHP-8.2+-777BB4?style=for-the-badge&logo=php&logoColor=white)
![MySQL](https://img.shields.io/badge/MySQL-8.0-4479A1?style=for-the-badge&logo=mysql&logoColor=white)
![API](https://img.shields.io/badge/RESTful-API-009688?style=for-the-badge&logo=fastapi&logoColor=white)
![SaaS](https://img.shields.io/badge/SaaS-Multi--Tenant-6366F1?style=for-the-badge&logo=cloud&logoColor=white)

### **نظام SaaS متكامل لإدارة الموارد البشرية والمخزون**

*Enterprise-grade Multi-tenant HR & Inventory Management System*

[Features](#-key-features) • [Architecture](#-saas-architecture) • [Code Examples](#-real-code-examples) • [API Docs](#-api-documentation)

</div>

---

## 🎯 Overview

This is a **Production-Ready SaaS ERP System** built with Laravel, featuring:

- ☁️ **Full Multi-tenancy** with complete data isolation
- 🏢 **HR Module** - Complete employee lifecycle management
- 📦 **Inventory Module** - Full stock tracking with audit trail
- 🔐 **Secure Authentication** - Tenant-based access control
- 🌍 **Multilingual** - Arabic & English support

---

## ☁️ SaaS Architecture

### Multi-Tenant Design

This system is built as a **true SaaS application** where multiple organizations (tenants) can use the same application instance while their data remains completely isolated.

```
┌─────────────────────────────────────────────────────────────────────┐
│                         SaaS Application                            │
├─────────────────────────────────────────────────────────────────────┤
│                                                                     │
│   ┌──────────────┐   ┌──────────────┐   ┌──────────────┐           │
│   │   Tenant A   │   │   Tenant B   │   │   Tenant C   │           │
│   │  (Company 1) │   │  (Company 2) │   │  (Company 3) │           │
│   ├──────────────┤   ├──────────────┤   ├──────────────┤           │
│   │ • Employees  │   │ • Employees  │   │ • Employees  │           │
│   │ • Inventory  │   │ • Inventory  │   │ • Inventory  │           │
│   │ • Warehouses │   │ • Warehouses │   │ • Warehouses │           │
│   │ • KPIs       │   │ • KPIs       │   │ • KPIs       │           │
│   └──────────────┘   └──────────────┘   └──────────────┘           │
│          │                  │                  │                    │
│          └──────────────────┼──────────────────┘                    │
│                             │                                       │
│                    ┌────────▼────────┐                              │
│                    │  Shared Database │                              │
│                    │  (tenant_id FK)  │                              │
│                    └─────────────────┘                              │
│                                                                     │
└─────────────────────────────────────────────────────────────────────┘
```

### How Multi-Tenancy Works

Every table in the database includes a `tenant_id` column, and all queries are automatically scoped to the current tenant:

```php
// Automatic tenant scoping in models
protected static function booted(): void
{
    static::creating(function ($model) {
        if (function_exists('tenant') && tenant()) {
            $model->tenant_id = tenant('id');
        }
    });
}
```

### Tenant Identification Flow

```
┌─────────────────┐     ┌─────────────────┐     ┌─────────────────┐
│   HTTP Request  │────▶│  Middleware     │────▶│  Tenant Context │
│  with subdomain │     │ tenantFromUrl   │     │  Established    │
└─────────────────┘     └─────────────────┘     └─────────────────┘
                                │
                                ▼
                    ┌───────────────────────┐
                    │  All queries scoped   │
                    │  to current tenant    │
                    └───────────────────────┘
```

### Tenant Authentication Guard

```php
// Routes protected by tenant authentication
Route::middleware(['tenantFromUrl', 'setlocaleApi', 'api'])->group(function () {
    Route::middleware('auth:tenant_owner')->group(function () {
        // All tenant-specific routes
    });
});
```

---

## 🏗 System Architecture

```
┌─────────────────────────────────────────────────────────────────────┐
│                       Request Layer                                  │
│  ┌──────────────────────────────────────────────────────────────┐   │
│  │  Middleware: tenantFromUrl → setlocale → auth:tenant_owner    │   │
│  └──────────────────────────────────────────────────────────────┘   │
├─────────────────────────────────────────────────────────────────────┤
│                       Controller Layer                               │
│  • Receives validated requests                                       │
│  • Delegates to Services                                             │
│  • Returns transformed responses (Resources)                         │
├─────────────────────────────────────────────────────────────────────┤
│                       Service Layer                                  │
│  • Contains business logic                                           │
│  • Handles transactions                                              │
│  • Coordinates between repositories                                  │
├─────────────────────────────────────────────────────────────────────┤
│                       Repository Layer                               │
│  • Data access abstraction                                           │
│  • Query building                                                    │
│  • Model operations                                                  │
├─────────────────────────────────────────────────────────────────────┤
│                       Model Layer                                    │
│  • Eloquent models with relationships                                │
│  • Scopes & Accessors                                                │
│  • Automatic tenant scoping                                          │
└─────────────────────────────────────────────────────────────────────┘
```

---

## 💻 Real Code Examples

### 1. Stock Movement Service (Complete Transaction Handling)

This service demonstrates advanced patterns: **Database Transactions**, **Stock Validation**, and **Audit Trail**:

```php
<?php

namespace Modules\Inventory\Services\StockMovements;

use Illuminate\Support\Facades\DB;
use Modules\Inventory\Models\StockMovement;
use Modules\Inventory\Models\Ingredient;
use Modules\Inventory\Models\WarehouseStock;

class StockMovementService
{
    /**
     * Record a generic movement with full audit trail.
     * This method handles all stock operations atomically.
     */
    protected function recordMovement(array $data): StockMovement
    {
        return DB::transaction(function () use ($data) {
            $tenantId = tenant('id');

            if (!$tenantId) {
                throw new RuntimeException(__('inventory::inventory.tenant_not_identified'), 404);
            }

            // 1. Get or validate ingredient
            $ingredient = Ingredient::where('id', $data['ingredient_id'])
                ->where('tenant_id', $tenantId)
                ->firstOrFail();

            // 2. Get or create warehouse stock (atomic operation)
            $warehouseStock = WarehouseStock::firstOrCreate(
                [
                    'tenant_id' => $tenantId,
                    'warehouse_id' => $data['warehouse_id'],
                    'ingredient_id' => $data['ingredient_id'],
                ],
                [
                    'current_stock' => 0,
                    'min_stock' => $ingredient->min_stock ?? 0,
                    'reserved_stock' => 0,
                ]
            );

            // 3. Calculate balances with validation
            $warehouseBalanceBefore = $warehouseStock->current_stock;

            if ($data['direction'] === 'in') {
                $warehouseBalanceAfter = $warehouseBalanceBefore + $data['quantity'];
            } else {
                // Check available stock (current - reserved)
                $availableStock = $warehouseStock->available_stock;

                if ($availableStock < $data['quantity']) {
                    throw new RuntimeException(
                        __('inventory::inventory.insufficient_stock', [
                            'available' => $availableStock,
                            'requested' => $data['quantity'],
                        ]),
                        400
                    );
                }

                $warehouseBalanceAfter = $warehouseBalanceBefore - $data['quantity'];
            }

            // 4. Create stock movement record (audit trail)
            $movement = StockMovement::create([
                'tenant_id' => $tenantId,
                'ingredient_id' => $data['ingredient_id'],
                'warehouse_id' => $data['warehouse_id'],
                'movement_type' => $data['movement_type'],
                'direction' => $data['direction'],
                'quantity' => $data['quantity'],
                'balance_before' => $warehouseBalanceBefore,  // For audit
                'balance_after' => $warehouseBalanceAfter,    // For audit
                'reference_type' => $data['reference_type'],  // Polymorphic
                'reference_id' => $data['reference_id'],
            ]);

            // 5. Update warehouse stock atomically
            $warehouseStock->update([
                'current_stock' => $warehouseBalanceAfter,
                'last_movement_at' => now(),
            ]);

            // 6. Update ingredient total stock (sum across warehouses)
            $ingredient->update([
                'current_stock' => $ingredient->current_stock + 
                    ($data['direction'] === 'in' ? $data['quantity'] : -$data['quantity']),
            ]);

            return $movement->load(['ingredient', 'warehouse']);
        });
    }
}
```

### 2. Stock Transfer Service (Complex Multi-Step Workflow)

This demonstrates a **two-phase transaction** (Send → Receive) with proper state management:

```php
<?php

namespace Modules\Inventory\Services\StockTransfers;

class StockTransferService
{
    /**
     * Send a stock transfer (deduct from source warehouse).
     * Phase 1 of the transfer workflow.
     */
    public function send($id): StockTransfer
    {
        $this->ensureTenantAuth();
        $transfer = $this->getById($id);

        return DB::transaction(function () use ($transfer) {
            // Update status to in_transit
            $transfer = $this->transferRepository->send($transfer);

            // Record transfer_out movements for each item
            foreach ($transfer->items as $item) {
                $this->stockMovementService->recordTransferOut([
                    'ingredient_id' => $item->ingredient_id,
                    'warehouse_id' => $transfer->from_warehouse_id,
                    'quantity' => $item->quantity,
                    'unit_cost' => $item->unit_cost,
                    'reference_type' => StockTransfer::class,  // Polymorphic
                    'reference_id' => $transfer->id,
                    'notes' => "Transfer Out: {$transfer->transfer_number}",
                ]);
            }

            return $transfer->fresh(['fromWarehouse', 'toWarehouse', 'items']);
        });
    }

    /**
     * Receive a stock transfer (add to destination warehouse).
     * Phase 2 of the transfer workflow with partial receive support.
     */
    public function receive($id, array $receivedItems = []): StockTransfer
    {
        $this->ensureTenantAuth();
        $transfer = $this->getById($id);

        return DB::transaction(function () use ($transfer, $receivedItems) {
            // Handle partial receives
            if (!empty($receivedItems)) {
                foreach ($receivedItems as $itemData) {
                    $item = $transfer->items()->find($itemData['item_id']);
                    if ($item) {
                        $receivedQty = $itemData['received_quantity'];

                        // Validation: Cannot receive more than sent
                        if ($receivedQty > $item->quantity) {
                            throw new RuntimeException(
                                __('inventory::inventory.received_quantity_exceeds_sent'),
                                400
                            );
                        }

                        $item->update(['received_quantity' => $receivedQty]);
                    }
                }
            }

            // Record transfer_in movements
            foreach ($transfer->items as $item) {
                if ($item->received_quantity > 0) {
                    $this->stockMovementService->recordTransferIn([
                        'ingredient_id' => $item->ingredient_id,
                        'warehouse_id' => $transfer->to_warehouse_id,
                        'source_warehouse_id' => $transfer->from_warehouse_id,
                        'quantity' => $item->received_quantity,
                        'reference_type' => StockTransfer::class,
                        'reference_id' => $transfer->id,
                    ]);
                }
            }

            return $transfer;
        });
    }
}
```

### 3. Polymorphic Stock Movement Model (Design Pattern)

Using **Polymorphic Relationships** to link movements to any source document:

```php
<?php

namespace Modules\Inventory\Models;

use Illuminate\Database\Eloquent\Model;
use Illuminate\Database\Eloquent\Relations\MorphTo;

class StockMovement extends Model
{
    protected $fillable = [
        'tenant_id',
        'ingredient_id',
        'warehouse_id',
        'source_warehouse_id',
        'movement_type',        // receipt, transfer_in, transfer_out, adjustment, etc.
        'direction',            // in / out
        'quantity',
        'balance_before',       // Audit: balance before this movement
        'balance_after',        // Audit: balance after this movement
        'reference_type',       // Polymorphic: IngredientReceipt, StockTransfer, etc.
        'reference_id',
    ];

    /**
     * Automatic tenant scoping on creation
     */
    protected static function booted(): void
    {
        static::creating(function ($movement) {
            if (function_exists('tenant') && tenant()) {
                $movement->tenant_id = tenant('id');
            }
        });
    }

    /**
     * Polymorphic relation - links to any source document
     * Can return: IngredientReceipt, StockTransfer, InventoryCount, etc.
     */
    public function reference(): MorphTo
    {
        return $this->morphTo('reference');
    }

    /**
     * Query scopes for filtering
     */
    public function scopeIncoming($query)
    {
        return $query->where('direction', 'in');
    }

    public function scopeOutgoing($query)
    {
        return $query->where('direction', 'out');
    }

    public function scopeOfType($query, string $type)
    {
        return $query->where('movement_type', $type);
    }
}
```

### 4. Warehouse Stock Model (Computed Properties)

Using **Eloquent Accessors** for real-time calculations:

```php
<?php

namespace Modules\Inventory\Models;

class WarehouseStock extends Model
{
    protected $fillable = [
        'tenant_id',
        'warehouse_id',
        'ingredient_id',
        'current_stock',
        'min_stock',
        'reserved_stock',
        'last_movement_at',
    ];

    /**
     * Computed property: Available stock for operations
     * available = current - reserved
     */
    public function getAvailableStockAttribute(): float
    {
        return max(0, $this->current_stock - $this->reserved_stock);
    }

    /**
     * Business logic method: Check if reorder needed
     */
    public function isLowStock(): bool
    {
        return $this->current_stock <= $this->min_stock;
    }
}
```

### 5. Leave Request Service (HR Workflow with File Handling)

Demonstrating **file uploads** and **auto-calculation**:

```php
<?php

namespace Modules\HR\Services\LeaveRequests;

class TenantLeaveRequestService
{
    public function create(array $data)
    {
        $this->ensureTenantAuth();
        
        $payload = $this->prepareData($data);
        
        return $this->repository->create($payload);
    }

    /**
     * Prepare data with automatic calculations and file handling
     */
    protected function prepareData(array $data, ?LeaveRequest $existing = null): array
    {
        // Auto-calculate days count
        $start = Carbon::parse($data['start_date']);
        $end = Carbon::parse($data['end_date']);
        $data['days_count'] = $start->diffInDays($end) + 1;
        
        // Default status
        $data['status'] = $data['status'] ?? ($existing?->status ?? 'pending');

        // Handle attachment upload
        $attachment = $data['attachment'] ?? null;

        if ($attachment instanceof UploadedFile) {
            $data['attachment'] = $existing
                ? FileHelper::updateFile($attachment, $existing->attachment, 'uploads/leave-requests')
                : FileHelper::uploadFile($attachment, 'uploads/leave-requests');
        } elseif ($attachment === null && $existing?->attachment) {
            // Delete existing attachment if explicitly set to null
            FileHelper::delete($existing->attachment);
            $data['attachment'] = null;
        }

        return $data;
    }
}
```

### 6. KPI Score Service (Status Workflow with Validation)

Demonstrating **status-based business rules** and **validation**:

```php
<?php

namespace Modules\HR\Services\EmployeeKpiScores;

class EmployeeKpiScoreService
{
    public function update($id, array $data)
    {
        $this->ensureTenantAuth();
        $score = $this->getById($id);

        // Business Rule: Prevent updates on approved scores
        if ($score->status === 'approved' && ($data['status'] ?? null) !== 'rejected') {
            throw new RuntimeException(__('hr::hr.employee_kpi_score_already_approved'), 400);
        }

        // Validate score range (0-100)
        if (isset($data['score'])) {
            $this->validateScore($data['score']);
        }

        // Auto-set timestamps based on status changes
        if (isset($data['status'])) {
            if ($data['status'] === 'submitted' && !$score->submitted_at) {
                $data['submitted_at'] = Carbon::now();
            }
            if ($data['status'] === 'approved' && !$score->approved_at) {
                $data['approved_at'] = Carbon::now();
            }
        }

        return $this->repository->update($score, $data);
    }

    protected function validateScore(float $score): void
    {
        if ($score < 0 || $score > 100) {
            throw new RuntimeException(__('hr::hr.employee_kpi_score_out_of_range'), 422);
        }
    }
}
```

### 7. Recipe Cost Calculation (Automatic Costing)

```php
<?php

namespace Modules\Inventory\Services\Recipes;

class RecipeService
{
    public function create(array $data): Recipe
    {
        return DB::transaction(function () use ($data) {
            // Create recipe
            $recipe = $this->recipeRepository->create($data);

            // Create items with automatic cost fetch
            if (isset($data['items'])) {
                $this->createItems($recipe, $data['items']);
            }

            // Auto-calculate total costs
            $recipe->updateCosts();

            return $recipe;
        });
    }

    protected function createItems(Recipe $recipe, array $items): void
    {
        foreach ($items as $index => $itemData) {
            // Auto-fetch current price from ingredient
            $ingredient = Ingredient::find($itemData['ingredient_id']);
            $unitCost = $ingredient->unit_price ?? 0;
            $quantity = $itemData['quantity'] ?? 0;
            $totalCost = $quantity * $unitCost;  // Auto-calculate

            RecipeItem::create([
                'tenant_id' => tenant('id'),
                'recipe_id' => $recipe->id,
                'ingredient_id' => $itemData['ingredient_id'],
                'quantity' => $quantity,
                'unit_cost' => $unitCost,      // From ingredient
                'total_cost' => $totalCost,    // Calculated
                'sort_order' => $index,
            ]);
        }
    }
}
```

---

## 👥 HR Module Features

### Complete Employee Lifecycle Management

```
┌────────────────────────────────────────────────────────────────┐
│  Onboarding → Active Employment → Performance → Offboarding   │
└────────────────────────────────────────────────────────────────┘
```

| Feature | Description |
|---------|-------------|
| 👤 **Employee Management** | Full profiles, documents (CV, insurance), branch/department assignment |
| 🏢 **Organizational Structure** | Branches, Departments, Positions with hierarchy |
| ⏰ **Shifts & Attendance** | Flexible shifts, biometric device integration |
| 🏖️ **Leave Management** | Multiple leave types, approval workflow, balance tracking |
| 📈 **KPI System** | Categories, Cycles, Assignments, Scoring with audit logs |
| 🎖️ **Rewards & Penalties** | Type definitions, employee assignments |
| 💰 **Salary & Payroll** | Components, advances, installments |

### Performance Management Flow

```
┌─────────────────────────────────────────────────────────────────┐
│                      KPI Architecture                            │
├─────────────────────────────────────────────────────────────────┤
│                                                                   │
│   Categories ──┬── KPIs ──┬── Cycles ──┬── Assignments          │
│                │          │            │                         │
│                └──────────┴────────────┴── Scores & Logs        │
│                                                                   │
│   Status Flow: draft → submitted → approved/rejected             │
│                                                                   │
└─────────────────────────────────────────────────────────────────┘
```

---

## 📦 Inventory Module Features

### Complete Stock Lifecycle

```
┌─────────────────────────────────────────────────────────────────┐
│                                                                   │
│     ┌──────────┐    ┌──────────┐    ┌───────────┐               │
│     │ Receipt  │───▶│  Stock   │───▶│ Transfer  │               │
│     │          │    │          │    │           │               │
│     └──────────┘    └────┬─────┘    └───────────┘               │
│                          │                                        │
│                          ▼                                        │
│     ┌──────────┐    ┌──────────┐    ┌───────────┐               │
│     │  Count   │◀───│ Consume  │───▶│   Waste   │               │
│     │ (Audit)  │    │          │    │           │               │
│     └──────────┘    └──────────┘    └───────────┘               │
│                                                                   │
└─────────────────────────────────────────────────────────────────┘
```

| Feature | Description |
|---------|-------------|
| 📋 **Master Data** | Units, Suppliers, Warehouses, Categories, Waste Reasons |
| 🥗 **Ingredients** | Multi-warehouse tracking, low stock alerts, expiry management |
| 📥 **Receipts** | Supplier invoice linking, batch tracking, auto stock update |
| 🔀 **Transfers** | Inter-warehouse, two-phase (send/receive), partial receive |
| 📊 **Inventory Counts** | Full/partial audit, variance calculation, auto adjustment |
| 🍳 **Recipes** | Multi-ingredient, auto cost calculation, cost per unit |
| 📝 **Movement Audit** | Complete history with balance before/after |

---

## 📡 API Documentation

### Authentication

All endpoints require tenant context and authentication:

```
Authorization: Bearer {token}
X-Tenant: {subdomain}
Accept: application/json
```

### HR Endpoints

| Method | Endpoint | Description |
|--------|----------|-------------|
| `GET` | `/api/employees` | List employees |
| `POST` | `/api/employees` | Create employee |
| `POST` | `/api/leave-requests` | Submit leave request |
| `POST` | `/api/leave-requests/status/{id}` | Approve/Reject |
| `POST` | `/api/employee-kpi-scores` | Record KPI score |
| `POST` | `/api/employee-kpi-scores/status/{id}` | Update score status |

### Inventory Endpoints

| Method | Endpoint | Description |
|--------|----------|-------------|
| `POST` | `/api/ingredient-receipts` | Create receipt |
| `POST` | `/api/ingredient-receipts/{id}/approve` | Approve & update stock |
| `POST` | `/api/stock-transfers` | Create transfer |
| `POST` | `/api/stock-transfers/{id}/send` | Send (phase 1) |
| `POST` | `/api/stock-transfers/{id}/receive` | Receive (phase 2) |
| `POST` | `/api/inventory-counts/{id}/approve` | Apply adjustments |
| `GET` | `/api/stock-movements` | View audit trail |

### Response Format

```json
{
    "success": true,
    "status": 200,
    "message": "Operation successful",
    "data": { ... }
}
```

---

## 🗄 Database Schema

### Core Tables (Both Modules)

| Table | Purpose |
|-------|---------|
| `employees` | Employee master data with tenant isolation |
| `ingredients` | Ingredient master with total stock |
| `warehouse_stocks` | Per-warehouse stock levels |
| `stock_movements` | Complete audit trail |

### Key Design Decisions

1. **Tenant Isolation**: Every table has `tenant_id` FK
2. **Dual Stock Tracking**: `warehouse_stocks` (per location) + `ingredients.current_stock` (total)
3. **Audit Trail**: `balance_before` and `balance_after` on every movement
4. **Polymorphic References**: Movements link to any source document

---

## 🛠 Technologies & Patterns

| Technology | Purpose |
|------------|---------|
| **Laravel 11** | Backend Framework |
| **PHP 8.2+** | Programming Language |
| **MySQL 8** | Database |
| **Laravel Sanctum** | Multi-guard Authentication |
| **Spatie Translatable** | i18n Support |
| **Laravel Modules** | Modular Architecture |

### Design Patterns Applied

| Pattern | Usage |
|---------|-------|
| **Repository Pattern** | Data access abstraction |
| **Service Pattern** | Business logic encapsulation |
| **Resource Pattern** | API response transformation |
| **Polymorphic Relations** | Flexible document linking |
| **Transaction Pattern** | Atomic operations |

---

## 🎯 Key Highlights

✅ **Production-Ready SaaS** - Complete tenant isolation  
✅ **Enterprise Patterns** - Repository, Service, Resource layers  
✅ **Complex Workflows** - Multi-phase operations (Transfer Send/Receive)  
✅ **Full Audit Trail** - Every stock change tracked  
✅ **Type Safety** - Strict PHP 8.2+ types  
✅ **API First** - RESTful with consistent responses  
✅ **Multilingual** - Arabic & English support  

---

## 📞 Contact

**Developer:** Mohamed Elshaf3ey

---

<div align="center">

**Built with ❤️ using Laravel**

*Enterprise-Grade SaaS ERP System*

*© 2025 - All Rights Reserved*

</div>
