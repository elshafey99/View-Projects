# 🚀 Enterprise Resource Planning System (ERP)

<div align="center">

![Laravel](https://img.shields.io/badge/Laravel-11-FF2D20?style=for-the-badge&logo=laravel&logoColor=white)
![PHP](https://img.shields.io/badge/PHP-8.2+-777BB4?style=for-the-badge&logo=php&logoColor=white)
![MySQL](https://img.shields.io/badge/MySQL-8.0-4479A1?style=for-the-badge&logo=mysql&logoColor=white)
![API](https://img.shields.io/badge/RESTful-API-009688?style=for-the-badge&logo=fastapi&logoColor=white)

**نظام متكامل لإدارة الموارد البشرية والمخزون**

*Enterprise-grade HR & Inventory Management System*

</div>

---

## 📋 Table of Contents

- [Overview](#-overview)
- [Architecture](#-architecture)
- [HR Module](#-hr-module-human-resources)
- [Inventory Module](#-inventory-module)
- [Key Technical Highlights](#-key-technical-highlights)
- [API Documentation](#-api-documentation)
- [Database Design](#-database-design)

---

## 🎯 Overview

This is a comprehensive **Multi-tenant ERP System** built with Laravel, featuring modular architecture for managing:

- **Human Resources (HR)** - Complete employee lifecycle management
- **Inventory Management** - Full stock tracking with audit trail

### ✨ Core Features

| Feature | Description |
|---------|-------------|
| 🏢 **Multi-tenancy** | Complete tenant isolation with automatic scoping |
| 🌍 **Multilingual** | Arabic & English support with Spatie Translatable |
| 🔐 **Authentication** | Laravel Sanctum with role-based access |
| 📊 **RESTful APIs** | Fully documented API endpoints |
| 🗄️ **Clean Architecture** | Repository-Service pattern |

---

## 🏗 Architecture

```
┌─────────────────────────────────────────────────────────────────────┐
│                         Application Layer                           │
├───────────────────────────────┬─────────────────────────────────────┤
│         HR Module             │          Inventory Module           │
├───────────────────────────────┼─────────────────────────────────────┤
│  • Controllers                │  • Controllers                      │
│  • Services                   │  • Services                         │
│  • Repositories               │  • Repositories                     │
│  • Models                     │  • Models                           │
│  • Transformers               │  • Transformers                     │
└───────────────────────────────┴─────────────────────────────────────┘
                                  │
                                  ▼
              ┌────────────────────────────────────────┐
              │           Core Infrastructure          │
              │  • Multi-tenancy • Auth • Localization │
              └────────────────────────────────────────┘
```

### Design Patterns Used

- **Repository Pattern** - Data access abstraction
- **Service Pattern** - Business logic encapsulation
- **Resource Pattern** - API response transformation
- **Polymorphic Relationships** - Flexible data associations

---

## 👥 HR Module (Human Resources)

### 🌟 Features Overview

The HR module provides complete workforce management capabilities:

#### 1. **Employee Management** 👤
```
┌────────────────────────────────────────────────────────────────┐
│                     Employee Lifecycle                          │
├────────────────────────────────────────────────────────────────┤
│  Onboarding → Active Employment → Performance → Offboarding    │
└────────────────────────────────────────────────────────────────┘
```

- Complete employee profiles with personal & professional info
- Document management (CV, Insurance documents)
- Branch & Department assignment
- Shift scheduling integration
- Employee status management

#### 2. **Organizational Structure** 🏢

| Component | Description |
|-----------|-------------|
| **Branches** | Multi-location support with hierarchical structure |
| **Departments** | Organizational units with manager assignment |
| **Positions** | Job titles with role definitions |
| **Education Levels** | Academic qualification tracking |

#### 3. **Attendance & Shifts** ⏰

```
┌─────────────────────────────────────────────────────────────┐
│                    Shift Management                          │
├─────────────────────────────────────────────────────────────┤
│  • Flexible shift creation with start/end times             │
│  • Break duration configuration                              │
│  • Shift swap capability                                     │
│  • Biometric device integration                              │
└─────────────────────────────────────────────────────────────┘
```

**Biometric Integration Features:**
- Device registration & management
- Automatic attendance logging
- Real-time sync capabilities

#### 4. **Leave Management** 🏖️

Complete leave request workflow:

```
Request → Pending Review → Approved/Rejected → Leave Balance Update
```

- Multiple leave types configuration
- Day count calculation
- Attachment support for documentation
- Status tracking (pending, approved, rejected)

#### 5. **Performance Management (KPI System)** 📈

```
┌─────────────────────────────────────────────────────────────────┐
│                      KPI Architecture                            │
├─────────────────────────────────────────────────────────────────┤
│                                                                   │
│   Categories ──┬── KPIs ──┬── Cycles ──┬── Assignments          │
│                │          │            │                         │
│                └──────────┴────────────┴── Scores & Logs        │
│                                                                   │
└─────────────────────────────────────────────────────────────────┘
```

| Feature | Description |
|---------|-------------|
| **KPI Categories** | Organize KPIs by type (Sales, Quality, etc.) |
| **KPI Definitions** | Define metrics with calculation methods |
| **Evaluation Cycles** | Periodic evaluation periods |
| **Employee Assignments** | Assign KPIs with targets & weights |
| **Scoring System** | Track actual vs target performance |
| **Audit Logs** | Complete history of score changes |

#### 6. **Rewards & Penalties** 🎖️

- Define reward/penalty types
- Assign to employees with documentation
- Track history for payroll integration

#### 7. **Salary & Compensation** 💰

```
┌──────────────────────────────────────────────────────────────┐
│                   Payroll Components                          │
├──────────────────────────────────────────────────────────────┤
│  Base Salary + Allowances + Bonuses - Deductions = Net Pay   │
└──────────────────────────────────────────────────────────────┘
```

- Salary components (additions & deductions)
- Employee-specific salary configuration
- Payroll run preparation
- Advance & installment tracking

---

## 📦 Inventory Module

### 🌟 Features Overview

Enterprise-grade inventory management with complete audit trail:

#### 1. **Master Data Management** 📋

| Entity | Features |
|--------|----------|
| **Units** | Measurement units (kg, piece, liter) |
| **Suppliers** | Vendor management with contact info |
| **Warehouses** | Multi-location with hierarchy support |
| **Categories** | Ingredient classification |
| **Waste Reasons** | Waste tracking categories |

#### 2. **Ingredient Management** 🥗

```
┌─────────────────────────────────────────────────────────────────┐
│                    Ingredient Features                           │
├─────────────────────────────────────────────────────────────────┤
│  • Multi-warehouse stock tracking                                │
│  • Automatic stock level monitoring                              │
│  • Low stock alerts                                              │
│  • Expiry date management                                        │
│  • Unit price tracking                                           │
└─────────────────────────────────────────────────────────────────┘
```

#### 3. **Complete Inventory Lifecycle** 🔄

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

#### 4. **Ingredient Receipts (Procurement)** 📥

Complete receiving workflow:

```
Draft → Items Added → Approval → Stock Updated → Movement Logged
```

**Features:**
- Supplier invoice linking
- Batch number tracking
- Expiry date recording
- Automatic stock updates on approval
- Full audit trail

#### 5. **Stock Transfers** 🔀

Inter-warehouse transfer management:

```
┌─────────────┐         ┌─────────────┐         ┌─────────────┐
│   Create    │   ───▶  │    Send     │   ───▶  │   Receive   │
│   (Draft)   │         │ (In Transit)│         │  (Complete) │
└─────────────┘         └─────────────┘         └─────────────┘
      │                        │                       │
      ▼                        ▼                       ▼
  Warehouse A            Deduct from A            Add to B
```

**Features:**
- Source validation (check available stock)
- Partial receive support
- Variance tracking
- Dual movement recording

#### 6. **Inventory Counts (Stock Audit)** 📊

Physical inventory verification:

```
System Quantity ───┐
                   ├──▶ Variance ──▶ Automatic Adjustment
Counted Quantity ──┘
```

**Features:**
- Full or partial count types
- Automatic variance calculation
- Adjustment movement creation
- Cost variance reporting

#### 7. **Recipe Management** 🍳

Product recipe costing:

```
┌─────────────────────────────────────────────────────────────────┐
│                      Recipe Costing                              │
├─────────────────────────────────────────────────────────────────┤
│                                                                   │
│   Ingredient 1 (qty × cost) ─┐                                   │
│   Ingredient 2 (qty × cost) ─┼──▶ Total Cost ──▶ Cost/Unit      │
│   Ingredient 3 (qty × cost) ─┘        │                          │
│                                        │                          │
│                               Yield Quantity                      │
│                                                                   │
└─────────────────────────────────────────────────────────────────┘
```

**Features:**
- Multi-ingredient recipes
- Automatic cost calculation
- Cost per unit computation
- Preparation time tracking
- Cost update on ingredient price changes

#### 8. **Stock Movements (Audit Trail)** 📝

Every stock change is tracked:

| Movement Type | Direction | Trigger |
|---------------|-----------|---------|
| `receipt` | IN | Ingredient Receipt Approval |
| `transfer_out` | OUT | Stock Transfer Send |
| `transfer_in` | IN | Stock Transfer Receive |
| `adjustment` | IN/OUT | Inventory Count Approval |
| `consumption` | OUT | Recipe Usage |
| `waste` | OUT | Waste Recording |
| `return` | IN | Ingredient Return |

**Each movement records:**
- Balance before & after
- Reference to source document (Polymorphic)
- User & timestamp
- Cost information

---

## 🔧 Key Technical Highlights

### 1. **Service Layer Architecture**

```php
// Example: Stock Transfer Service
class StockTransferService
{
    public function send($id)
    {
        return DB::transaction(function () use ($id) {
            $transfer = $this->getById($id);
            
            // Validate stock availability
            // Create transfer_out movements
            // Update warehouse stocks
            // Update transfer status
            
            return $transfer;
        });
    }
}
```

### 2. **Polymorphic References**

```php
// Stock movements reference multiple document types
public function reference()
{
    return $this->morphTo();
}

// Usage: $movement->reference returns IngredientReceipt, StockTransfer, etc.
```

### 3. **Automatic Stock Calculation**

```php
// Warehouse Stock Model
public function getAvailableStockAttribute()
{
    return $this->current_stock - $this->reserved_stock;
}

public function isLowStock()
{
    return $this->current_stock <= $this->min_stock;
}
```

### 4. **Transaction Safety**

All complex operations wrapped in database transactions:

```php
DB::transaction(function () {
    // Multiple related updates
    // Automatic rollback on failure
});
```

---

## 📡 API Documentation

### HR Module Endpoints

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/api/employees` | List all employees |
| POST | `/api/employees` | Create employee |
| GET | `/api/employees/{id}` | Get employee details |
| POST | `/api/employees/{id}` | Update employee |
| POST | `/api/leave-requests` | Submit leave request |
| POST | `/api/leave-requests/status/{id}` | Approve/Reject leave |
| GET | `/api/kpis` | List KPIs |
| POST | `/api/employee-kpi-scores` | Record KPI score |

### Inventory Module Endpoints

| Method | Endpoint | Description |
|--------|----------|-------------|
| POST | `/api/ingredient-receipts` | Create receipt |
| POST | `/api/ingredient-receipts/{id}/approve` | Approve receipt |
| POST | `/api/stock-transfers` | Create transfer |
| POST | `/api/stock-transfers/{id}/send` | Send transfer |
| POST | `/api/stock-transfers/{id}/receive` | Receive transfer |
| POST | `/api/inventory-counts/{id}/approve` | Apply count adjustments |
| GET | `/api/stock-movements` | View all movements |
| GET | `/api/warehouses/{id}/movements` | Warehouse movements |

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

## 🗄 Database Design

### HR Module Tables

```
├── employees
├── branches
├── departments
├── positions
├── education
├── shifts
├── biometric_devices
├── attendance_logs
├── leave_types
├── leave_requests
├── penalties
├── employee_penalties
├── rewards
├── employee_rewards
├── salary_components
├── employee_salary_components
├── advances
├── advance_installments
├── payroll_runs
├── payroll_items
├── kpi_categories
├── kpis
├── kpi_cycles
├── employee_kpi_assignments
├── employee_kpi_scores
└── employee_kpi_logs
```

### Inventory Module Tables

```
├── units
├── suppliers
├── warehouses
├── ingredient_categories
├── waste_reasons
├── ingredients
├── warehouse_stocks          ⭐ (Stock per warehouse per ingredient)
├── stock_movements           ⭐ (Complete audit trail)
├── ingredient_receipts
├── ingredient_receipt_items
├── stock_transfers
├── stock_transfer_items
├── inventory_counts
├── inventory_count_items
├── recipes
└── recipe_items
```

---

## 🛠 Technologies Used

| Technology | Purpose |
|------------|---------|
| **Laravel 11** | Backend Framework |
| **PHP 8.2+** | Programming Language |
| **MySQL 8** | Database |
| **Laravel Sanctum** | API Authentication |
| **Spatie Translatable** | Multi-language Support |
| **Laravel Modules** | Modular Architecture |
| **Repository Pattern** | Data Access Layer |

---

## 📞 Contact

**Developer:** Mohamed Elshaf3ey

Feel free to reach out for any questions or opportunities!

---

<div align="center">

**Built with ❤️ using Laravel**

*© 2025 - All Rights Reserved*

</div>

