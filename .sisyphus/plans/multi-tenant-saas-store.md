# Multi-Tenant SaaS Online Store Platform

## TL;DR

> **Quick Summary**: Build a production-ready multi-tenant SaaS platform where multiple clients can launch their own branded online stores. Each tenant gets isolated data, custom branding, product management, orders, and analytics.
>
> **Deliverables**:
> - NestJS backend with PostgreSQL RLS multi-tenancy
> - Next.js frontend with tenant-aware routing
> - Super Admin dashboard (platform management)
> - Tenant Admin dashboard (store management)
> - Public storefront (end-user shopping experience)
> - Complete database schema with Prisma
> - Docker setup for local development
> - Test suite for critical paths
>
> **Estimated Effort**: XL (40+ hours, 50+ tasks)
> **Parallel Execution**: YES - 6 waves with 40% speedup
> **Critical Path**: Database Schema → Backend Core → Frontend Core → Integration → Polish

---

## Context

### Original Request
Build a Multi-Tenant SaaS Online Store Platform that allows selling many online stores to different clients. Each client gets their own brand (logo, colors, name), products, customers, and admin dashboard. The platform must feel professional, modern, premium, and competitive with Shopify-like products. Production-ready MVP, not a demo.

### Interview Summary
**Key Decisions**:
- Tech stack: Next.js (App Router), NestJS, PostgreSQL, Prisma, JWT auth
- Multi-tenancy: Shared database with RLS (Row Level Security)
- Tenant resolution: Subdomain + custom domain support
- Clean architecture: Feature-based folder structure
- Auth: JWT + refresh tokens, multi-tenant safe
- Payments: Stripe integration
- Caching: Redis for tenant resolution and rate limiting
- File storage: S3-compatible (MinIO for dev)

**Research Findings**:
- PostgreSQL RLS with Prisma extensions provides automatic tenant isolation
- `nestjs-cls` enables request-scoped tenant context without connection pool exhaustion
- Next.js middleware can extract subdomains and rewrite to dynamic routes
- Feature-based clean architecture supports future microservice migration

### Metis Review (Self-Analysis)
**Identified Gaps** (addressed in plan):
- **Database migration strategy**: Need explicit plan for RLS policy migrations
- **Tenant onboarding flow**: How new tenants are created and configured
- **Custom domain SSL**: Let's Encrypt or similar for custom domains
- **Image optimization**: Need strategy for product image resizing/CDN
- **Rate limiting**: Per-tenant rate limits to prevent abuse
- **Backup strategy**: Tenant data backup considerations
- **Soft deletes**: All entities should support soft deletes for data recovery
- **Audit logging**: Track who changed what in multi-tenant context

---

## Work Objectives

### Core Objective
Build a production-ready multi-tenant SaaS online store platform with true data isolation, professional UI/UX, and extensible architecture that can be sold to real businesses.

### Concrete Deliverables
1. **Backend (NestJS)**:
   - Multi-tenant database layer with RLS
   - Authentication system (JWT + refresh tokens)
   - Feature modules: Auth, Tenants, Products, Orders, Customers, Analytics
   - API endpoints for all operations
   - Stripe integration for payments

2. **Frontend (Next.js)**:
   - Middleware for tenant resolution
   - Super Admin dashboard UI
   - Tenant Admin dashboard UI
   - Public storefront UI
   - Theme system per tenant

3. **Database (PostgreSQL + Prisma)**:
   - Complete schema with tenant isolation
   - RLS policies for all tenant-scoped tables
   - Migration files

4. **Infrastructure**:
   - Docker Compose setup (PostgreSQL, Redis, MinIO, backend, frontend)
   - Environment configuration
   - Seed data for testing

5. **Testing**:
   - Unit tests for domain logic
   - Integration tests for API
   - Tenant isolation verification tests

### Definition of Done
- [ ] All features from MVP scope implemented
- [ ] Tenant isolation verified (no data leakage between tenants)
- [ ] All API endpoints return correct tenant-scoped data
- [ ] Frontend renders tenant-specific branding
- [ ] Checkout flow completes successfully (Stripe test mode)
- [ ] Docker setup runs with single command
- [ ] Test suite passes
- [ ] Code follows clean architecture principles

### Must Have
- True multi-tenant data isolation (RLS policies)
- JWT authentication with refresh tokens
- Subdomain and custom domain tenant resolution
- Product CRUD with image upload
- Shopping cart and checkout
- Order management
- Responsive UI with theming
- Docker-based local development

### Must NOT Have (Guardrails)
- **No email service integration** (post-MVP)
- **No shipping provider integration** (post-MVP)
- **No multi-currency support** (post-MVP)
- **No i18n/multi-language** (post-MVP)
- **No real-time features** (WebSockets, post-MVP)
- **No advanced analytics** (basic only, post-MVP for advanced)
- **No mobile apps** (web-only for MVP)
- **No third-party API integrations** (except Stripe)
- **No coupon/discount system** (post-MVP)
- **No inventory tracking** beyond simple decrement on order
- **No hardcoded secrets** in codebase
- **No business logic in controllers** (must be in services/use cases)
- **No direct database queries** outside repository layer
- **No tenant data in JWT payload** (only tenant_id, use RLS for data)

---

## Verification Strategy

### Test Decision
- **Infrastructure exists**: NO (will be set up)
- **User wants tests**: YES (TDD for critical paths)
- **Framework**: Jest (NestJS default) + bun test for frontend

### Test Setup Tasks
- [ ] 0.1 Setup Backend Test Infrastructure
  - Install: `npm install --save-dev @nestjs/testing jest`
  - Config: Use NestJS default Jest config
  - Verify: `npm test` → runs successfully
  - Example: Create `src/features/auth/auth.service.spec.ts`

- [ ] 0.2 Setup Frontend Test Infrastructure
  - Install: `bun add -d vitest @testing-library/react`
  - Config: Create `vitest.config.ts`
  - Verify: `bun test` → runs successfully
  - Example: Create `src/__tests__/example.test.tsx`

### TDD Workflow
Each critical task follows RED-GREEN-REFACTOR:
1. **RED**: Write failing test first
2. **GREEN**: Implement minimum code to pass
3. **REFACTOR**: Clean up while keeping tests green

---

## Execution Strategy

### Parallel Execution Waves

```
Wave 1 (Foundation - Start Immediately):
├── Task 1: Project setup and folder structure
├── Task 2: Database schema design (Prisma)
├── Task 3: Docker Compose setup
└── Task 4: Backend core module structure

Wave 2 (Backend Core - After Wave 1):
├── Task 5: Multi-tenant Prisma client with RLS
├── Task 6: Tenant context middleware (nestjs-cls)
├── Task 7: Authentication module (JWT + refresh)
└── Task 8: Tenant management module

Wave 3 (Backend Features - After Wave 2):
├── Task 9: Product management module
├── Task 10: Category management module
├── Task 11: Shopping cart module
├── Task 12: Order management module
└── Task 13: Customer management module

Wave 4 (Frontend Core - After Wave 1):
├── Task 14: Next.js project setup with middleware
├── Task 15: Tenant resolution and routing
├── Task 16: Theme system and UI components
├── Task 17: Authentication UI
└── Task 18: API client setup

Wave 5 (Frontend Features - After Wave 4 + partial Wave 2):
├── Task 19: Super Admin dashboard
├── Task 20: Tenant Admin dashboard layout
├── Task 21: Product management UI
├── Task 22: Order management UI
├── Task 23: Store settings UI
└── Task 24: Analytics dashboard UI

Wave 6 (Storefront - After Wave 4 + Wave 3):
├── Task 25: Storefront layout and navigation
├── Task 26: Home page with featured products
├── Task 27: Product listing page
├── Task 28: Product detail page
├── Task 29: Shopping cart UI
└── Task 30: Checkout flow with Stripe

Wave 7 (Integration & Polish - After Waves 3, 5, 6):
├── Task 31: Backend-frontend integration
├── Task 32: Image upload and storage
├── Task 33: Rate limiting implementation
├── Task 34: Soft deletes and audit logging
├── Task 35: Error handling and validation
├── Task 36: Responsive design polish
├── Task 37: Dark mode support
├── Task 38: Performance optimization
└── Task 39: Documentation

Wave 8 (Testing & Deployment Prep - Final):
├── Task 40: Unit tests for domain logic
├── Task 41: Integration tests for APIs
├── Task 42: Tenant isolation tests
├── Task 43: End-to-end checkout test
├── Task 44: Seed data creation
├── Task 45: Environment configuration
├── Task 46: Production build verification
└── Task 47: Final documentation review
```

### Dependency Matrix

| Task | Depends On | Blocks | Can Parallelize With |
|------|------------|--------|---------------------|
| 1 | None | 2, 3, 4, 14 | None |
| 2 | None | 5, 6 | 1, 3, 4, 14 |
| 3 | None | All | 1, 2, 4, 14 |
| 4 | 1 | 5, 6, 7, 8 | 2, 3, 14 |
| 5 | 2, 4 | 9, 10, 11, 12, 13 | 6, 7, 8 |
| 6 | 2, 4 | 9, 10, 11, 12, 13 | 5, 7, 8 |
| 7 | 4 | 9, 10, 11, 12, 13, 19, 20 | 5, 6, 8 |
| 8 | 4 | 19, 20 | 5, 6, 7 |
| 9 | 5, 6, 7 | 21, 26, 27, 28 | 10, 11, 12, 13 |
| 10 | 5, 6, 7 | 21 | 9, 11, 12, 13 |
| 11 | 5, 6, 7 | 29, 30 | 9, 10, 12, 13 |
| 12 | 5, 6, 7 | 22, 30 | 9, 10, 11, 13 |
| 13 | 5, 6, 7 | 24 | 9, 10, 11, 12 |
| 14 | 1 | 15, 16, 17, 18 | 2, 3, 4 |
| 15 | 14 | 19, 20, 21, 22, 23, 24, 25 | 16, 17, 18 |
| 16 | 14 | 19, 20, 21, 22, 23, 24, 25, 26, 27, 28, 29, 30 | 15, 17, 18 |
| 17 | 14 | 19, 20, 21, 22, 23, 24 | 15, 16, 18 |
| 18 | 14 | 19, 20, 21, 22, 23, 24, 25, 26, 27, 28, 29, 30 | 15, 16, 17 |
| 19 | 7, 8, 15, 16, 17 | None | 20, 21, 22, 23, 24 |
| 20 | 7, 8, 15, 16, 17 | 21, 22, 23, 24 | 19 |
| 21 | 9, 10, 15, 16, 17, 18, 20 | None | 22, 23, 24 |
| 22 | 12, 15, 16, 17, 18, 20 | None | 21, 23, 24 |
| 23 | 8, 15, 16, 17, 18, 20 | None | 21, 22, 24 |
| 24 | 13, 15, 16, 17, 18, 20 | None | 21, 22, 23 |
| 25 | 15, 16, 18 | 26, 27, 28, 29, 30 | None |
| 26 | 9, 16, 18, 25 | None | 27, 28, 29, 30 |
| 27 | 9, 16, 18, 25 | None | 26, 28, 29, 30 |
| 28 | 9, 16, 18, 25 | None | 26, 27, 29, 30 |
| 29 | 11, 16, 18, 25 | 30 | 26, 27, 28 |
| 30 | 11, 12, 16, 18, 25, 29 | None | None |
| 31+ | Various | None | Various |

### Critical Path
Task 1 → Task 2 → Task 5 → Task 9 → Task 21 → Task 31 → Task 43

---

## TODOs

### WAVE 1: Foundation

- [ ] 1. Project Setup and Folder Structure

  **What to do**:
  - Create monorepo structure with `apps/backend` and `apps/frontend`
  - Initialize NestJS backend with CLI
  - Initialize Next.js frontend with App Router
  - Set up shared types package
  - Configure TypeScript, ESLint, Prettier for both apps
  - Create root `package.json` with workspace scripts

  **Must NOT do**:
  - Don't add business logic yet
  - Don't create database connections yet
  - Don't add unnecessary dependencies

  **Recommended Agent Profile**:
  - **Category**: `quick`
  - **Skills**: []
  - Reason: This is scaffolding work requiring basic setup commands

  **Parallelization**:
  - **Can Run In Parallel**: YES
  - **Parallel Group**: Wave 1
  - **Blocks**: Tasks 2, 3, 4, 14
  - **Blocked By**: None

  **Acceptance Criteria**:
  - [ ] `apps/backend` exists with NestJS structure
  - [ ] `apps/frontend` exists with Next.js App Router
  - [ ] Root `package.json` has workspace configuration
  - [ ] `npm run dev` starts both backend and frontend
  - [ ] TypeScript compiles without errors in both apps

  **Commit**: YES
  - Message: `chore: initial project setup with NestJS and Next.js`
  - Files: `apps/backend/`, `apps/frontend/`, `package.json`, `tsconfig.json`

- [ ] 2. Database Schema Design (Prisma)

  **What to do**:
  - Set up Prisma in backend
  - Design complete schema with all entities:
    - Platform: SuperAdmin, FeatureFlag
    - Tenant: Tenant, TenantSettings, Domain
    - Auth: User, RefreshToken, Role
    - Store: Product, Category, ProductImage
    - Commerce: Cart, CartItem, Order, OrderItem, Customer
    - Audit: AuditLog (for tracking changes)
  - Add `tenant_id` column to all tenant-scoped tables
  - Add `created_at`, `updated_at`, `deleted_at` (soft delete) to all tables
  - Create initial migration

  **Must NOT do**:
  - Don't add RLS policies yet (Task 5)
  - Don't add indexes yet (will add based on query patterns)
  - Don't create seed data yet (Task 44)

  **Recommended Agent Profile**:
  - **Category**: `ultrabrain`
  - **Skills**: []
  - Reason: Database schema design requires careful thinking about relationships, multi-tenancy, and future extensibility

  **Parallelization**:
  - **Can Run In Parallel**: YES
  - **Parallel Group**: Wave 1
  - **Blocks**: Task 5, 6
  - **Blocked By**: Task 1

  **Acceptance Criteria**:
  - [ ] `prisma/schema.prisma` exists with all entities
  - [ ] All tenant-scoped tables have `tenant_id` column
  - [ ] All tables have timestamp columns
  - [ ] `npx prisma migrate dev` creates migration successfully
  - [ ] `npx prisma generate` generates client successfully
  - [ ] ERD diagram generated (optional but helpful)

  **Commit**: YES
  - Message: `feat(db): design complete multi-tenant schema with Prisma`
  - Files: `apps/backend/prisma/schema.prisma`, `apps/backend/prisma/migrations/`

- [ ] 3. Docker Compose Setup

  **What to do**:
  - Create `docker-compose.yml` with services:
    - PostgreSQL 15+ (with RLS support)
    - Redis 7+ (for caching and rate limiting)
    - MinIO (S3-compatible storage for images)
    - Backend (NestJS)
    - Frontend (Next.js)
  - Create `.env.example` with all required variables
  - Create `Dockerfile` for backend
  - Create `Dockerfile` for frontend
  - Add health checks for dependencies
  - Configure networking between services

  **Must NOT do**:
  - Don't add production optimizations yet
  - Don't add SSL/TLS termination (dev only)
  - Don't use bind mounts for production

  **Recommended Agent Profile**:
  - **Category**: `quick`
  - **Skills**: []
  - Reason: Docker setup is configuration work following standard patterns

  **Parallelization**:
  - **Can Run In Parallel**: YES
  - **Parallel Group**: Wave 1
  - **Blocks**: All subsequent tasks (provides infrastructure)
  - **Blocked By**: Task 1

  **Acceptance Criteria**:
  - [ ] `docker-compose.yml` exists with all services
  - [ ] `docker-compose up` starts all services successfully
  - [ ] PostgreSQL is accessible on port 5432
  - [ ] Redis is accessible on port 6379
  - [ ] MinIO is accessible on port 9000/9001
  - [ ] Backend connects to PostgreSQL and Redis
  - [ ] Frontend can communicate with backend

  **Commit**: YES
  - Message: `chore: add Docker Compose setup with PostgreSQL, Redis, MinIO`
  - Files: `docker-compose.yml`, `apps/backend/Dockerfile`, `apps/frontend/Dockerfile`, `.env.example`

- [ ] 4. Backend Core Module Structure

  **What to do**:
  - Create feature-based folder structure:
    ```
    src/
      features/
        auth/
        tenants/
        products/
        categories/
        carts/
        orders/
        customers/
        analytics/
        audit/
      shared/
        kernel/          # Shared types, utils
        infrastructure/  # Database, cache, storage
    ```
  - Create base classes: BaseEntity, BaseRepository, BaseService
  - Set up module structure with empty modules for each feature
  - Configure global pipes (ValidationPipe)
  - Configure global filters (exception filter)
  - Configure global interceptors (logging, transform)

  **Must NOT do**:
  - Don't implement actual logic yet
  - Don't create controllers yet
  - Don't add business rules yet

  **Recommended Agent Profile**:
  - **Category**: `ultrabrain`
  - **Skills**: []
  - Reason: Core architecture decisions affect entire project maintainability

  **Parallelization**:
  - **Can Run In Parallel**: YES
  - **Parallel Group**: Wave 1
  - **Blocks**: Tasks 5, 6, 7, 8
  - **Blocked By**: Task 1

  **Acceptance Criteria**:
  - [ ] Feature-based folder structure exists
  - [ ] All feature modules are registered in AppModule
  - [ ] Global ValidationPipe is configured
  - [ ] Global exception filter is configured
  - [ ] Application starts without errors

  **Commit**: YES
  - Message: `feat(backend): setup feature-based clean architecture structure`
  - Files: `apps/backend/src/features/`, `apps/backend/src/shared/`

### WAVE 2: Backend Core

- [ ] 5. Multi-Tenant Prisma Client with RLS

  **What to do**:
  - Create PrismaClient extension that:
    - Reads tenant_id from CLS context
    - Executes `SET LOCAL app.current_tenant = 'tenant_id'` before queries
    - Wraps queries in transaction to ensure RLS context
  - Create RLS policies for all tenant-scoped tables:
    ```sql
    CREATE POLICY tenant_isolation ON "Product" 
    USING (tenant_id = current_setting('app.current_tenant')::uuid);
    ```
  - Create bypass policy for super admin operations
  - Add Prisma middleware for automatic tenant_id injection on create
  - Test that queries are automatically filtered by tenant

  **Must NOT do**:
  - Don't create multiple PrismaClient instances per tenant
  - Don't manually add tenant_id to every query (use RLS)
  - Don't skip transaction wrapper (RLS context is per-transaction)

  **Recommended Agent Profile**:
  - **Category**: `ultrabrain`
  - **Skills**: []
  - Reason: RLS implementation is critical for security - must be correct

  **Parallelization**:
  - **Can Run In Parallel**: YES (with Task 6)
  - **Parallel Group**: Wave 2
  - **Blocks**: Tasks 9, 10, 11, 12, 13
  - **Blocked By**: Tasks 2, 4

  **Acceptance Criteria**:
  - [ ] PrismaClient extension exists with RLS support
  - [ ] RLS policies created for all tenant-scoped tables
  - [ ] Test: Query without tenant context returns empty
  - [ ] Test: Query with tenant A context returns only A's data
  - [ ] Test: Query with tenant B context returns only B's data
  - [ ] Test: Super admin bypass works correctly

  **Commit**: YES
  - Message: `feat(backend): implement PostgreSQL RLS for tenant isolation`
  - Files: `apps/backend/src/shared/infrastructure/prisma/rls-extension.ts`, migration files

- [ ] 6. Tenant Context Middleware (nestjs-cls)

  **What to do**:
  - Install and configure `nestjs-cls`
  - Create `TenantContextMiddleware` that:
    - Extracts tenant_id from request header (`x-tenant-id`)
    - Validates tenant exists and is active
    - Sets tenant_id in CLS context
    - Handles custom domain resolution (lookup by domain)
  - Create `TenantContextService` to access tenant_id from anywhere
  - Create `@CurrentTenant()` decorator for controllers
  - Create `TenantGuard` to ensure tenant context is set

  **Must NOT do**:
  - Don't store full tenant object in CLS (only tenant_id)
  - Don't make database calls in middleware (use cache)
  - Don't allow requests without tenant context (except platform routes)

  **Recommended Agent Profile**:
  - **Category**: `ultrabrain`
  - **Skills**: []
  - Reason: Tenant context is foundational - affects all requests

  **Parallelization**:
  - **Can Run In Parallel**: YES (with Task 5)
  - **Parallel Group**: Wave 2
  - **Blocks**: Tasks 9, 10, 11, 12, 13
  - **Blocked By**: Tasks 2, 4

  **Acceptance Criteria**:
  - [ ] `nestjs-cls` configured in AppModule
  - [ ] `TenantContextMiddleware` extracts and validates tenant
  - [ ] `TenantContextService` provides tenant_id access
  - [ ] `@CurrentTenant()` decorator works in controllers
  - [ ] `TenantGuard` rejects requests without valid tenant
  - [ ] Test: Request with valid tenant header succeeds
  - [ ] Test: Request with invalid tenant header fails

  **Commit**: YES
  - Message: `feat(backend): add tenant context middleware with nestjs-cls`
  - Files: `apps/backend/src/shared/infrastructure/tenant-context/`

- [ ] 7. Authentication Module (JWT + Refresh Tokens)

  **What to do**:
  - Install `@nestjs/jwt`, `@nestjs/passport`, `passport`, `passport-jwt`
  - Create JWT strategy with tenant validation
  - Create AuthService with:
    - `login(email, password, tenant_id)` - returns tokens
    - `refreshTokens(refreshToken)` - returns new tokens
    - `logout(refreshToken)` - invalidates refresh token
    - `register(userData)` - creates new user
  - Create AuthController with endpoints:
    - POST `/auth/login`
    - POST `/auth/refresh`
    - POST `/auth/logout`
    - POST `/auth/register` (optional for MVP)
  - Create JWT auth guard with tenant validation
  - Store refresh tokens in database (hashed)
  - Create password hashing with bcrypt

  **Must NOT do**:
  - Don't store passwords in plain text
  - Don't put sensitive data in JWT payload
  - Don't allow cross-tenant token usage
  - Don't implement OAuth/Social login (post-MVP)

  **Recommended Agent Profile**:
  - **Category**: `ultrabrain`
  - **Skills**: []
  - Reason: Auth is security-critical and affects all protected routes

  **Parallelization**:
  - **Can Run In Parallel**: NO (depends on Task 6 for tenant context)
  - **Parallel Group**: Wave 2
  - **Blocks**: Tasks 9, 10, 11, 12, 13, 19, 20
  - **Blocked By**: Tasks 4, 6

  **Acceptance Criteria**:
  - [ ] JWT strategy validates token and tenant
  - [ ] Login returns access_token and refresh_token
  - [ ] Refresh endpoint returns new tokens
  - [ ] Logout invalidates refresh token
  - [ ] Passwords are hashed with bcrypt
  - [ ] Test: Valid login returns tokens
  - [ ] Test: Invalid login returns 401
  - [ ] Test: Token from tenant A doesn't work for tenant B

  **Commit**: YES
  - Message: `feat(backend): implement JWT authentication with refresh tokens`
  - Files: `apps/backend/src/features/auth/`

- [ ] 8. Tenant Management Module

  **What to do**:
  - Create TenantService with:
    - `createTenant(data)` - creates new tenant with settings
    - `updateTenant(id, data)` - updates tenant
    - `suspendTenant(id)` - suspends tenant
    - `activateTenant(id)` - reactivates tenant
    - `getTenantBySubdomain(subdomain)` - for resolution
    - `getTenantByDomain(domain)` - for custom domains
  - Create TenantController with endpoints:
    - POST `/tenants` (super admin only)
    - PATCH `/tenants/:id` (super admin or tenant admin)
    - POST `/tenants/:id/suspend` (super admin)
    - GET `/tenants/:id` (super admin or tenant admin)
    - GET `/tenants` (super admin - list all)
  - Create tenant settings management
  - Create domain management (subdomain + custom)

  **Must NOT do**:
  - Don't allow tenant admins to create new tenants
  - Don't allow duplicate subdomains
  - Don't delete tenants (soft delete only)

  **Recommended Agent Profile**:
  - **Category**: `unspecified-high`
  - **Skills**: []
  - Reason: Tenant management is core business logic

  **Parallelization**:
  - **Can Run In Parallel**: YES (with Task 7)
  - **Parallel Group**: Wave 2
  - **Blocks**: Tasks 19, 20
  - **Blocked By**: Tasks 4, 6

  **Acceptance Criteria**:
  - [ ] Tenant creation works with settings
  - [ ] Subdomain uniqueness enforced
  - [ ] Custom domain can be added
  - [ ] Tenant suspension prevents access
  - [ ] Soft delete works (data preserved)
  - [ ] Test: Duplicate subdomain rejected
  - [ ] Test: Suspended tenant cannot login

  **Commit**: YES
  - Message: `feat(backend): add tenant management module`
  - Files: `apps/backend/src/features/tenants/`

### WAVE 3: Backend Features

- [ ] 9. Product Management Module

  **What to do**:
  - Create ProductService with:
    - `createProduct(data)` - creates product with tenant_id
    - `updateProduct(id, data)` - updates product
    - `deleteProduct(id)` - soft delete
    - `getProductById(id)` - get single product
    - `listProducts(filters)` - list with pagination, search, category filter
    - `updateStock(id, quantity)` - update inventory
  - Create ProductController with endpoints:
    - POST `/products` (tenant admin)
    - PATCH `/products/:id` (tenant admin)
    - DELETE `/products/:id` (tenant admin)
    - GET `/products/:id` (public)
    - GET `/products` (public - list with filters)
  - Implement image upload integration (presigned URLs)
  - Add product search with full-text search (PostgreSQL)

  **Must NOT do**:
  - Don't allow cross-tenant product access
  - Don't hard-delete products (use soft delete)
  - Don't store images in database (use S3)

  **Recommended Agent Profile**:
  - **Category**: `unspecified-high`
  - **Skills**: []
  - Reason: Product management is core e-commerce functionality

  **Parallelization**:
  - **Can Run In Parallel**: YES
  - **Parallel Group**: Wave 3
  - **Blocks**: Tasks 21, 26, 27, 28
  - **Blocked By**: Tasks 5, 6, 7

  **Acceptance Criteria**:
  - [ ] Product CRUD operations work
  - [ ] Products are tenant-isolated
  - [ ] Pagination and filtering work
  - [ ] Search returns relevant results
  - [ ] Soft delete preserves data
  - [ ] Test: Tenant A can't see Tenant B's products

  **Commit**: YES
  - Message: `feat(backend): implement product management with search`
  - Files: `apps/backend/src/features/products/`

- [ ] 10. Category Management Module

  **What to do**:
  - Create CategoryService with:
    - `createCategory(data)` - creates category
    - `updateCategory(id, data)` - updates category
    - `deleteCategory(id)` - soft delete (check for products)
    - `getCategoryById(id)` - get category
    - `listCategories()` - list all categories for tenant
    - `getCategoryTree()` - get hierarchical structure
  - Create CategoryController with endpoints:
    - POST `/categories` (tenant admin)
    - PATCH `/categories/:id` (tenant admin)
    - DELETE `/categories/:id` (tenant admin)
    - GET `/categories` (public)
    - GET `/categories/:id` (public)
  - Support hierarchical categories (parent/child)

  **Must NOT do**:
  - Don't allow deleting categories with products
  - Don't create circular references in hierarchy

  **Recommended Agent Profile**:
  - **Category**: `unspecified-high`
  - **Skills**: []
  - Reason: Category management supports product organization

  **Parallelization**:
  - **Can Run In Parallel**: YES
  - **Parallel Group**: Wave 3
  - **Blocks**: Task 21
  - **Blocked By**: Tasks 5, 6, 7

  **Acceptance Criteria**:
  - [ ] Category CRUD works
  - [ ] Hierarchy is supported
  - [ ] Can't delete category with products
  - [ ] Categories are tenant-isolated

  **Commit**: YES
  - Message: `feat(backend): add category management with hierarchy`
  - Files: `apps/backend/src/features/categories/`

- [ ] 11. Shopping Cart Module

  **What to do**:
  - Create CartService with:
    - `getOrCreateCart(userId/sessionId)` - get existing or create new
    - `addItem(cartId, productId, quantity)` - add item to cart
    - `updateItem(cartId, itemId, quantity)` - update quantity
    - `removeItem(cartId, itemId)` - remove item
    - `clearCart(cartId)` - remove all items
    - `getCartWithItems(cartId)` - get cart with product details
    - `mergeCarts(guestCartId, userCartId)` - merge on login
  - Create CartController with endpoints:
    - GET `/cart` - get current cart
    - POST `/cart/items` - add item
    - PATCH `/cart/items/:id` - update quantity
    - DELETE `/cart/items/:id` - remove item
    - DELETE `/cart` - clear cart
  - Support both authenticated users and guest sessions
  - Handle cart persistence (database-backed)

  **Must NOT do**:
  - Don't use session storage (use database)
  - Don't allow negative quantities
  - Don't allow adding unavailable products

  **Recommended Agent Profile**:
  - **Category**: `unspecified-high`
  - **Skills**: []
  - Reason: Cart is critical for conversion

  **Parallelization**:
  - **Can Run In Parallel**: YES
  - **Parallel Group**: Wave 3
  - **Blocks**: Tasks 29, 30
  - **Blocked By**: Tasks 5, 6, 7

  **Acceptance Criteria**:
  - [ ] Cart creation works for guests and users
  - [ ] Items can be added, updated, removed
  - [ ] Cart totals calculate correctly
  - [ ] Cart merges on login
  - [ ] Cart is tenant-isolated
  - [ ] Test: Cart persists across requests

  **Commit**: YES
  - Message: `feat(backend): implement shopping cart with guest support`
  - Files: `apps/backend/src/features/carts/`

- [ ] 12. Order Management Module

  **What to do**:
  - Create OrderService with:
    - `createOrderFromCart(cartId, customerData)` - create order
    - `processPayment(orderId, paymentData)` - integrate Stripe
    - `confirmOrder(orderId)` - confirm after payment
    - `getOrderById(id)` - get order details
    - `listOrders(filters)` - list with pagination
    - `updateOrderStatus(id, status)` - update status
    - `cancelOrder(id)` - cancel order
  - Create OrderController with endpoints:
    - POST `/orders` - create order from cart
    - GET `/orders/:id` - get order
    - GET `/orders` - list orders (tenant admin)
    - PATCH `/orders/:id/status` - update status (tenant admin)
  - Integrate Stripe for payment processing
  - Create order confirmation email (optional for MVP)
  - Handle inventory decrement on order

  **Must NOT do**:
  - Don't process payments without validation
  - Don't allow modifying confirmed orders
  - Don't expose sensitive payment data

  **Recommended Agent Profile**:
  - **Category**: `ultrabrain`
  - **Skills**: []
  - Reason: Orders and payments are critical business logic

  **Parallelization**:
  - **Can Run In Parallel**: YES
  - **Parallel Group**: Wave 3
  - **Blocks**: Tasks 22, 30
  - **Blocked By**: Tasks 5, 6, 7

  **Acceptance Criteria**:
  - [ ] Order creation from cart works
  - [ ] Stripe payment processing works
  - [ ] Order status transitions work
  - [ ] Inventory decrements on order
  - [ ] Orders are tenant-isolated
  - [ ] Test: Payment success creates confirmed order
  - [ ] Test: Payment failure doesn't create order

  **Commit**: YES
  - Message: `feat(backend): implement order management with Stripe`
  - Files: `apps/backend/src/features/orders/`

- [ ] 13. Customer Management Module

  **What to do**:
  - Create CustomerService with:
    - `createCustomer(data)` - create from order or registration
    - `updateCustomer(id, data)` - update customer
    - `getCustomerById(id)` - get customer
    - `getCustomerByEmail(email)` - lookup by email
    - `listCustomers(filters)` - list with pagination, search
  - Create CustomerController with endpoints:
    - GET `/customers` (tenant admin)
    - GET `/customers/:id` (tenant admin)
    - PATCH `/customers/:id` (tenant admin or self)
  - Link customers to orders
  - Support guest checkout (no account required)

  **Must NOT do**:
  - Don't expose customer data across tenants
  - Don't require account for checkout

  **Recommended Agent Profile**:
  - **Category**: `unspecified-high`
  - **Skills**: []
  - Reason: Customer management supports CRM features

  **Parallelization**:
  - **Can Run In Parallel**: YES
  - **Parallel Group**: Wave 3
  - **Blocks**: Task 24
  - **Blocked By**: Tasks 5, 6, 7

  **Acceptance Criteria**:
  - [ ] Customer creation works
  - [ ] Customer list with search works
  - [ ] Customers are tenant-isolated
  - [ ] Guest checkout doesn't create customer record

  **Commit**: YES
  - Message: `feat(backend): add customer management`
  - Files: `apps/backend/src/features/customers/`

### WAVE 4: Frontend Core

- [ ] 14. Next.js Project Setup with Middleware

  **What to do**:
  - Configure Next.js 14+ with App Router
  - Set up TypeScript, Tailwind CSS
  - Create middleware.ts for tenant resolution:
    - Extract subdomain from hostname
    - Lookup tenant in backend
    - Set headers: x-tenant-id, x-tenant-subdomain
    - Rewrite to dynamic route
  - Create folder structure:
    ```
    app/
      [tenant]/
        (storefront)/
        admin/
      platform/
        admin/
    ```
  - Set up environment variables

  **Must NOT do**:
  - Don't make database calls from middleware (call API)
  - Don't hardcode tenant resolution logic

  **Recommended Agent Profile**:
  - **Category**: `ultrabrain`
  - **Skills**: []
  - Reason: Middleware is critical for routing - affects all pages

  **Parallelization**:
  - **Can Run In Parallel**: YES
  - **Parallel Group**: Wave 4
  - **Blocks**: Tasks 15, 16, 17, 18
  - **Blocked By**: Task 1

  **Acceptance Criteria**:
  - [ ] Middleware extracts subdomain correctly
  - [ ] Tenant lookup API call works
  - [ ] Headers are set on request
  - [ ] URL rewrite to [tenant] works
  - [ ] Custom domain resolution works

  **Commit**: YES
  - Message: `feat(frontend): setup Next.js with tenant middleware`
  - Files: `apps/frontend/middleware.ts`, `apps/frontend/app/[tenant]/`

- [ ] 15. Tenant Resolution and Routing

  **What to do**:
  - Create TenantProvider context
  - Create useTenant() hook
  - Fetch tenant data in layout
  - Handle tenant not found
  - Handle suspended tenants
  - Create error pages for tenant issues
  - Support custom domains (additional middleware logic)

  **Must NOT do**:
  - Don't fetch tenant data on every page (cache it)
  - Don't allow access to suspended tenants

  **Recommended Agent Profile**:
  - **Category**: `unspecified-high`
  - **Skills**: []
  - Reason: Tenant resolution affects all tenant-specific pages

  **Parallelization**:
  - **Can Run In Parallel**: YES
  - **Parallel Group**: Wave 4
  - **Blocks**: Tasks 19, 20, 21, 22, 23, 24, 25
  - **Blocked By**: Task 14

  **Acceptance Criteria**:
  - [ ] Tenant context available throughout app
  - [ ] Tenant not found shows error page
  - [ ] Suspended tenant shows appropriate message
  - [ ] Custom domains resolve correctly

  **Commit**: YES
  - Message: `feat(frontend): implement tenant resolution and context`
  - Files: `apps/frontend/lib/tenant/`

- [ ] 16. Theme System and UI Components

  **What to do**:
  - Create theme configuration system:
    - Colors (primary, secondary, accent)
    - Logo URL
    - Fonts
    - Dark mode support
  - Set up Tailwind with CSS variables for theming
  - Create base UI components:
    - Button, Input, Card, Modal
    - Navigation, Footer
    - ProductCard, CartItem
  - Create theme provider
  - Support dark mode toggle

  **Must NOT do**:
  - Don't hardcode colors (use CSS variables)
  - Don't create overly complex components

  **Recommended Agent Profile**:
  - **Category**: `visual-engineering`
  - **Skills**: []
  - Reason: UI components require design sensibility

  **Parallelization**:
  - **Can Run In Parallel**: YES
  - **Parallel Group**: Wave 4
  - **Blocks**: Tasks 19, 20, 21, 22, 23, 24, 25, 26, 27, 28, 29, 30
  - **Blocked By**: Task 14

  **Acceptance Criteria**:
  - [ ] Theme system supports custom colors
  - [ ] Logo displays correctly
  - [ ] Dark mode works
  - [ ] UI components are reusable
  - [ ] Components are responsive

  **Commit**: YES
  - Message: `feat(frontend): create theme system and base UI components`
  - Files: `apps/frontend/components/ui/`, `apps/frontend/lib/theme/`

- [ ] 17. Authentication UI

  **What to do**:
  - Create login page
  - Create registration page (optional)
  - Create AuthContext for state management
  - Create useAuth() hook
  - Handle token storage (httpOnly cookie preferred)
  - Create protected route wrapper
  - Handle token refresh

  **Must NOT do**:
  - Don't store tokens in localStorage (security risk)
  - Don't allow access to protected routes without auth

  **Recommended Agent Profile**:
  - **Category**: `unspecified-high`
  - **Skills**: []
  - Reason: Auth UI is security-critical

  **Parallelization**:
  - **Can Run In Parallel**: YES
  - **Parallel Group**: Wave 4
  - **Blocks**: Tasks 19, 20, 21, 22, 23, 24
  - **Blocked By**: Task 14

  **Acceptance Criteria**:
  - [ ] Login form works
  - [ ] Tokens stored securely
  - [ ] Protected routes redirect to login
  - [ ] Token refresh works silently
  - [ ] Logout clears tokens

  **Commit**: YES
  - Message: `feat(frontend): implement authentication UI`
  - Files: `apps/frontend/app/[tenant]/login/`, `apps/frontend/lib/auth/`

- [ ] 18. API Client Setup

  **What to do**:
  - Create API client with fetch/axios
  - Add tenant header to all requests
  - Add auth header when logged in
  - Handle 401 errors (token refresh)
  - Handle errors globally
  - Create typed API hooks with SWR or React Query
  - Set up API types from backend

  **Must NOT do**:
  - Don't make API calls without tenant header
  - Don't repeat error handling in every component

  **Recommended Agent Profile**:
  - **Category**: `unspecified-high`
  - **Skills**: []
  - Reason: API client is foundation for all data fetching

  **Parallelization**:
  - **Can Run In Parallel**: YES
  - **Parallel Group**: Wave 4
  - **Blocks**: Tasks 19, 20, 21, 22, 23, 24, 25, 26, 27, 28, 29, 30
  - **Blocked By**: Task 14

  **Acceptance Criteria**:
  - [ ] API client adds tenant header automatically
  - [ ] Auth token included when available
  - [ ] 401 triggers token refresh
  - [ ] Error handling works
  - [ ] Typed hooks available

  **Commit**: YES
  - Message: `feat(frontend): setup API client with tenant and auth headers`
  - Files: `apps/frontend/lib/api/`

### WAVE 5: Admin Dashboards

- [ ] 19. Super Admin Dashboard

  **What to do**:
  - Create platform admin layout
  - Create tenants list page:
    - Table with all tenants
    - Search and filter
    - Suspend/activate actions
  - Create tenant detail page:
    - Tenant info
    - Usage metrics (basic)
    - Settings
  - Create feature flags page:
    - List feature flags
    - Enable/disable per tenant
  - Protect with super admin role check

  **Must NOT do**:
  - Don't allow tenant admins to access
  - Don't show sensitive tenant data unnecessarily

  **Recommended Agent Profile**:
  - **Category**: `visual-engineering`
  - **Skills**: []
  - Reason: Admin UI requires good UX design

  **Parallelization**:
  - **Can Run In Parallel**: YES
  - **Parallel Group**: Wave 5
  - **Blocks**: None
  - **Blocked By**: Tasks 7, 8, 15, 16, 17

  **Acceptance Criteria**:
  - [ ] Super admin can list all tenants
  - [ ] Can suspend/activate tenants
  - [ ] Feature flags can be toggled
  - [ ] Only super admin role can access

  **Commit**: YES
  - Message: `feat(frontend): implement super admin dashboard`
  - Files: `apps/frontend/app/platform/admin/`

- [ ] 20. Tenant Admin Dashboard Layout

  **What to do**:
  - Create tenant admin layout with sidebar
  - Create navigation menu:
    - Dashboard (overview)
    - Products
    - Orders
    - Customers
    - Store Settings
    - Analytics
  - Create dashboard overview page:
    - Key metrics cards
    - Recent orders
    - Quick actions
  - Protect with tenant admin role

  **Must NOT do**:
  - Don't allow customers to access
  - Don't show other tenants' data

  **Recommended Agent Profile**:
  - **Category**: `visual-engineering`
  - **Skills**: []
  - Reason: Dashboard layout is foundation for admin UI

  **Parallelization**:
  - **Can Run In Parallel**: YES
  - **Parallel Group**: Wave 5
  - **Blocks**: Tasks 21, 22, 23, 24
  - **Blocked By**: Tasks 7, 8, 15, 16, 17

  **Acceptance Criteria**:
  - [ ] Sidebar navigation works
  - [ ] Dashboard shows key metrics
  - [ ] Only tenant admin can access
  - [ ] Tenant branding applied

  **Commit**: YES
  - Message: `feat(frontend): create tenant admin dashboard layout`
  - Files: `apps/frontend/app/[tenant]/admin/`

- [ ] 21. Product Management UI

  **What to do**:
  - Create products list page:
    - Table with products
    - Search, filter by category
    - Pagination
    - Edit/Delete actions
  - Create product form (create/edit):
    - Name, description, price
    - Category selection
    - Image upload
    - Stock quantity
  - Create category management page
  - Integrate with product API

  **Must NOT do**:
  - Don't allow negative prices
  - Don't allow empty required fields

  **Recommended Agent Profile**:
  - **Category**: `visual-engineering`
  - **Skills**: []
  - Reason: Product UI requires forms and data tables

  **Parallelization**:
  - **Can Run In Parallel**: YES
  - **Parallel Group**: Wave 5
  - **Blocks**: None
  - **Blocked By**: Tasks 9, 10, 15, 16, 17, 18, 20

  **Acceptance Criteria**:
  - [ ] Product list displays correctly
  - [ ] Can create new product
  - [ ] Can edit existing product
  - [ ] Can upload product images
  - [ ] Categories can be managed

  **Commit**: YES
  - Message: `feat(frontend): implement product management UI`
  - Files: `apps/frontend/app/[tenant]/admin/products/`

- [ ] 22. Order Management UI

  **What to do**:
  - Create orders list page:
    - Table with orders
    - Filter by status
    - Search by order number
    - Pagination
  - Create order detail page:
    - Order info
    - Customer info
    - Items list
    - Status timeline
    - Update status action
  - Integrate with orders API

  **Must NOT do**:
  - Don't allow invalid status transitions
  - Don't expose payment details

  **Recommended Agent Profile**:
  - **Category**: `visual-engineering`
  - **Skills**: []
  - Reason: Order UI requires status management

  **Parallelization**:
  - **Can Run In Parallel**: YES
  - **Parallel Group**: Wave 5
  - **Blocks**: None
  - **Blocked By**: Tasks 12, 15, 16, 17, 18, 20

  **Acceptance Criteria**:
  - [ ] Order list displays correctly
  - [ ] Can view order details
  - [ ] Can update order status
  - [ ] Status history visible

  **Commit**: YES
  - Message: `feat(frontend): implement order management UI`
  - Files: `apps/frontend/app/[tenant]/admin/orders/`

- [ ] 23. Store Settings UI

  **What to do**:
  - Create settings page:
    - Store name
    - Store description
    - Logo upload
    - Brand colors (primary, secondary)
    - Contact info
  - Create domain settings:
    - Subdomain display (read-only)
    - Custom domain input
    - Domain verification status
  - Integrate with tenant API

  **Must NOT do**:
  - Don't allow subdomain changes (read-only)
  - Don't skip validation

  **Recommended Agent Profile**:
  - **Category**: `visual-engineering`
  - **Skills**: []
  - Reason: Settings UI requires forms and file upload

  **Parallelization**:
  - **Can Run In Parallel**: YES
  - **Parallel Group**: Wave 5
  - **Blocks**: None
  - **Blocked By**: Tasks 8, 15, 16, 17, 18, 20

  **Acceptance Criteria**:
  - [ ] Settings form works
  - [ ] Logo uploads successfully
  - [ ] Colors update theme
  - [ ] Custom domain can be set

  **Commit**: YES
  - Message: `feat(frontend): implement store settings UI`
  - Files: `apps/frontend/app/[tenant]/admin/settings/`

- [ ] 24. Analytics Dashboard UI

  **What to do**:
  - Create analytics page:
    - Revenue chart (last 30 days)
    - Orders chart
    - Top products
    - Customer stats
  - Use recharts or similar for charts
  - Date range selector
  - Export data (optional)
  - Integrate with analytics API

  **Must NOT do**:
  - Don't make it too complex (MVP scope)
  - Don't show inaccurate data

  **Recommended Agent Profile**:
  - **Category**: `visual-engineering`
  - **Skills**: []
  - Reason: Analytics requires data visualization

  **Parallelization**:
  - **Can Run In Parallel**: YES
  - **Parallel Group**: Wave 5
  - **Blocks**: None
  - **Blocked By**: Tasks 13, 15, 16, 17, 18, 20

  **Acceptance Criteria**:
  - [ ] Revenue chart displays
  - [ ] Orders chart displays
  - [ ] Top products list shows
  - [ ] Date range works

  **Commit**: YES
  - Message: `feat(frontend): implement basic analytics dashboard`
  - Files: `apps/frontend/app/[tenant]/admin/analytics/`

### WAVE 6: Storefront

- [ ] 25. Storefront Layout and Navigation

  **What to do**:
  - Create storefront layout:
    - Header with logo, navigation, cart icon
    - Footer
    - Responsive design
  - Create navigation:
    - Home
    - Products
    - Categories
    - Cart
  - Apply tenant theme
  - Create mobile menu

  **Must NOT do**:
  - Don't hardcode navigation items
  - Don't ignore mobile experience

  **Recommended Agent Profile**:
  - **Category**: `visual-engineering`
  - **Skills**: []
  - Reason: Storefront layout is customer-facing

  **Parallelization**:
  - **Can Run In Parallel**: YES
  - **Parallel Group**: Wave 6
  - **Blocks**: Tasks 26, 27, 28, 29, 30
  - **Blocked By**: Tasks 15, 16, 18

  **Acceptance Criteria**:
  - [ ] Header displays tenant logo
  - [ ] Navigation works
  - [ ] Cart icon shows item count
  - [ ] Mobile menu works
  - [ ] Theme colors applied

  **Commit**: YES
  - Message: `feat(frontend): create storefront layout and navigation`
  - Files: `apps/frontend/app/[tenant]/(storefront)/layout.tsx`

- [ ] 26. Home Page with Featured Products

  **What to do**:
  - Create hero section
  - Create featured products section
  - Create categories showcase
  - Create promotional banner (optional)
  - Fetch featured products from API
  - Responsive grid layout

  **Must NOT do**:
  - Don't hardcode products
  - Don't make it too cluttered

  **Recommended Agent Profile**:
  - **Category**: `visual-engineering`
  - **Skills**: []
  - Reason: Home page is first impression

  **Parallelization**:
  - **Can Run In Parallel**: YES
  - **Parallel Group**: Wave 6
  - **Blocks**: None
  - **Blocked By**: Tasks 9, 16, 18, 25

  **Acceptance Criteria**:
  - [ ] Hero section displays
  - [ ] Featured products show
  - [ ] Categories visible
  - [ ] Responsive layout

  **Commit**: YES
  - Message: `feat(frontend): implement storefront home page`
  - Files: `apps/frontend/app/[tenant]/(storefront)/page.tsx`

- [ ] 27. Product Listing Page

  **What to do**:
  - Create product grid
  - Create filters sidebar:
    - Categories
    - Price range
    - Sort options
  - Create pagination
  - Create search bar
  - Fetch products from API
  - Empty state

  **Must NOT do**:
  - Don't fetch all products at once
  - Don't ignore filter state in URL

  **Recommended Agent Profile**:
  - **Category**: `visual-engineering`
  - **Skills**: []
  - Reason: Product listing requires filtering and pagination

  **Parallelization**:
  - **Can Run In Parallel**: YES
  - **Parallel Group**: Wave 6
  - **Blocks**: None
  - **Blocked By**: Tasks 9, 16, 18, 25

  **Acceptance Criteria**:
  - [ ] Product grid displays
  - [ ] Filters work
  - [ ] Pagination works
  - [ ] Search works
  - [ ] URL reflects filter state

  **Commit**: YES
  - Message: `feat(frontend): implement product listing page`
  - Files: `apps/frontend/app/[tenant]/(storefront)/products/page.tsx`

- [ ] 28. Product Detail Page

  **What to do**:
  - Create product image gallery
  - Create product info section:
    - Name, price
    - Description
    - Category
  - Create add to cart form:
    - Quantity selector
    - Add to cart button
  - Create related products section
  - Fetch product from API

  **Must NOT do**:
  - Don't allow adding more than stock
  - Don't show incorrect pricing

  **Recommended Agent Profile**:
  - **Category**: `visual-engineering`
  - **Skills**: []
  - Reason: Product detail is conversion-critical

  **Parallelization**:
  - **Can Run In Parallel**: YES
  - **Parallel Group**: Wave 6
  - **Blocks**: None
  - **Blocked By**: Tasks 9, 16, 18, 25

  **Acceptance Criteria**:
  - [ ] Product images display
  - [ ] Product info shows correctly
  - [ ] Can add to cart
  - [ ] Quantity validation works

  **Commit**: YES
  - Message: `feat(frontend): implement product detail page`
  - Files: `apps/frontend/app/[tenant]/(storefront)/products/[id]/page.tsx`

- [ ] 29. Shopping Cart UI

  **What to do**:
  - Create cart page:
    - Items list with images
    - Quantity controls
    - Remove button
    - Price calculations
  - Create cart summary:
    - Subtotal
    - Total
    - Checkout button
  - Create cart icon with badge in header
  - Create mini-cart dropdown (optional)
  - Integrate with cart API

  **Must NOT do**:
  - Don't allow negative quantities
  - Don't show incorrect totals

  **Recommended Agent Profile**:
  - **Category**: `visual-engineering`
  - **Skills**: []
  - Reason: Cart UI affects conversion

  **Parallelization**:
  - **Can Run In Parallel**: YES
  - **Parallel Group**: Wave 6
  - **Blocks**: Task 30
  - **Blocked By**: Tasks 11, 16, 18, 25

  **Acceptance Criteria**:
  - [ ] Cart items display
  - [ ] Can update quantities
  - [ ] Can remove items
  - [ ] Totals calculate correctly
  - [ ] Cart persists across pages

  **Commit**: YES
  - Message: `feat(frontend): implement shopping cart UI`
  - Files: `apps/frontend/app/[tenant]/(storefront)/cart/page.tsx`

- [ ] 30. Checkout Flow with Stripe

  **What to do**:
  - Create checkout page:
    - Order summary
    - Customer info form
    - Shipping info form
  - Integrate Stripe Elements:
    - Card input
    - Payment button
  - Handle payment success:
    - Show confirmation
    - Clear cart
  - Handle payment failure:
    - Show error
    - Allow retry
  - Create order confirmation page

  **Must NOT do**:
  - Don't store card details
  - Don't skip validation
  - Don't allow double submission

  **Recommended Agent Profile**:
  - **Category**: `ultrabrain`
  - **Skills**: []
  - Reason: Checkout is critical and complex

  **Parallelization**:
  - **Can Run In Parallel**: NO (depends on cart)
  - **Parallel Group**: Wave 6
  - **Blocks**: None
  - **Blocked By**: Tasks 11, 12, 16, 18, 25, 29

  **Acceptance Criteria**:
  - [ ] Checkout form works
  - [ ] Stripe payment processes
  - [ ] Success shows confirmation
  - [ ] Failure shows error
  - [ ] Cart clears on success
  - [ ] Order created in backend

  **Commit**: YES
  - Message: `feat(frontend): implement checkout with Stripe`
  - Files: `apps/frontend/app/[tenant]/(storefront)/checkout/`

### WAVE 7: Integration & Polish

- [ ] 31. Backend-Frontend Integration

  **What to do**:
  - Connect all frontend pages to backend APIs
  - Test full user flows:
    - Tenant creation
    - Product creation
    - Cart operations
    - Checkout
  - Fix any API mismatches
  - Handle edge cases
  - Add loading states
  - Add error handling

  **Must NOT do**:
  - Don't leave placeholder data
  - Don't ignore error cases

  **Recommended Agent Profile**:
  - **Category**: `unspecified-high`
  - **Skills**: []
  - Reason: Integration requires attention to detail

  **Parallelization**:
  - **Can Run In Parallel**: NO (depends on previous waves)
  - **Parallel Group**: Wave 7
  - **Blocks**: None
  - **Blocked By**: Waves 3, 5, 6

  **Acceptance Criteria**:
  - [ ] All API calls work
  - [ ] Data flows correctly
  - [ ] Error handling works
  - [ ] Loading states present

  **Commit**: YES
  - Message: `feat: integrate frontend with backend APIs`
  - Files: Various integration points

- [ ] 32. Image Upload and Storage

  **What to do**:
  - Create presigned URL endpoint (backend)
  - Create image upload component (frontend)
  - Integrate with MinIO/S3
  - Handle image resizing (optional)
  - Support multiple images per product
  - Handle upload errors

  **Must NOT do**:
  - Don't store images in database
  - Don't allow huge file uploads

  **Recommended Agent Profile**:
  - **Category**: `unspecified-high`
  - **Skills**: []
  - Reason: File handling requires security care

  **Parallelization**:
  - **Can Run In Parallel**: YES
  - **Parallel Group**: Wave 7
  - **Blocks**: None
  - **Blocked By**: Task 3 (MinIO setup)

  **Acceptance Criteria**:
  - [ ] Presigned URL generation works
  - [ ] Images upload successfully
  - [ ] Images display correctly
  - [ ] File size limits enforced

  **Commit**: YES
  - Message: `feat: implement image upload with S3-compatible storage`
  - Files: `apps/backend/src/features/storage/`, `apps/frontend/components/upload/`

- [ ] 33. Rate Limiting Implementation

  **What to do**:
  - Install `@nestjs/throttler`
  - Configure global rate limits
  - Add tenant-specific rate limits
  - Add stricter limits for auth endpoints
  - Use Redis for rate limit storage
  - Return proper 429 responses

  **Must NOT do**:
  - Don't rate limit health checks
  - Don't allow bypassing limits

  **Recommended Agent Profile**:
  - **Category**: `unspecified-high`
  - **Skills**: []
  - Reason: Rate limiting is security feature

  **Parallelization**:
  - **Can Run In Parallel**: YES
  - **Parallel Group**: Wave 7
  - **Blocks**: None
  - **Blocked By**: Task 3 (Redis setup)

  **Acceptance Criteria**:
  - [ ] Rate limits enforced
  - [ ] Redis stores counters
  - [ ] 429 responses returned
  - [ ] Different limits per endpoint type

  **Commit**: YES
  - Message: `feat: add rate limiting with Redis`
  - Files: `apps/backend/src/shared/infrastructure/rate-limiting/`

- [ ] 34. Soft Deletes and Audit Logging

  **What to do**:
  - Implement soft delete for all entities:
    - Add `deleted_at` column
    - Override delete to set `deleted_at`
    - Filter out deleted records by default
  - Create AuditLog service:
    - Log all create/update/delete operations
    - Include: user_id, tenant_id, action, entity, changes, timestamp
  - Create audit log viewer (super admin)

  **Must NOT do**:
  - Don't hard-delete data
  - Don't log sensitive data (passwords)

  **Recommended Agent Profile**:
  - **Category**: `unspecified-high`
  - **Skills**: []
  - Reason: Data integrity and compliance

  **Parallelization**:
  - **Can Run In Parallel**: YES
  - **Parallel Group**: Wave 7
  - **Blocks**: None
  - **Blocked By**: Task 2 (schema)

  **Acceptance Criteria**:
  - [ ] Soft delete works for all entities
  - [ ] Deleted records filtered by default
  - [ ] Audit logs created
  - [ ] Audit viewer works

  **Commit**: YES
  - Message: `feat: implement soft deletes and audit logging`
  - Files: `apps/backend/src/features/audit/`

- [ ] 35. Error Handling and Validation

  **What to do**:
  - Create global exception filter (backend)
  - Standardize error responses
  - Add validation pipes for all DTOs
  - Create error boundary components (frontend)
  - Add toast notifications for errors
  - Handle network errors gracefully
  - Add form validation feedback

  **Must NOT do**:
  - Don't expose internal errors to client
  - Don't ignore validation errors

  **Recommended Agent Profile**:
  - **Category**: `unspecified-high`
  - **Skills**: []
  - Reason: Error handling affects UX

  **Parallelization**:
  - **Can Run In Parallel**: YES
  - **Parallel Group**: Wave 7
  - **Blocks**: None
  - **Blocked By**: None

  **Acceptance Criteria**:
  - [ ] Errors handled gracefully
  - [ ] Validation errors clear
  - [ ] No internal error exposure
  - [ ] Toast notifications work

  **Commit**: YES
  - Message: `feat: improve error handling and validation`
  - Files: Various error handling files

- [ ] 36. Responsive Design Polish

  **What to do**:
  - Test all pages on mobile
  - Fix layout issues
  - Optimize touch targets
  - Test on tablet
  - Ensure readable font sizes
  - Fix horizontal scroll issues

  **Must NOT do**:
  - Don't ignore mobile experience
  - Don't make text too small

  **Recommended Agent Profile**:
  - **Category**: `visual-engineering`
  - **Skills**: []
  - Reason: Responsive design requires visual attention

  **Parallelization**:
  - **Can Run In Parallel**: YES
  - **Parallel Group**: Wave 7
  - **Blocks**: None
  - **Blocked By**: None

  **Acceptance Criteria**:
  - [ ] All pages work on mobile
  - [ ] No horizontal scroll
  - [ ] Touch targets adequate
  - [ ] Text readable

  **Commit**: YES
  - Message: `style: polish responsive design`
  - Files: Various CSS/styled components

- [ ] 37. Dark Mode Support

  **What to do**:
  - Ensure all components support dark mode
  - Add dark mode toggle
  - Persist preference
  - Test all pages in dark mode
  - Fix any contrast issues

  **Must NOT do**:
  - Don't hardcode light colors
  - Don't ignore accessibility

  **Recommended Agent Profile**:
  - **Category**: `visual-engineering`
  - **Skills**: []
  - Reason: Dark mode requires design system

  **Parallelization**:
  - **Can Run In Parallel**: YES
  - **Parallel Group**: Wave 7
  - **Blocks**: None
  - **Blocked By**: Task 16 (theme system)

  **Acceptance Criteria**:
  - [ ] Dark mode toggle works
  - [ ] All components styled
  - [ ] Preference persists
  - [ ] No contrast issues

  **Commit**: YES
  - Message: `feat: add dark mode support`
  - Files: `apps/frontend/lib/theme/`

- [ ] 38. Performance Optimization

  **What to do**:
  - Add database indexes for common queries
  - Implement Redis caching for:
    - Tenant resolution
    - Product listings
    - Categories
  - Optimize images (Next.js Image component)
  - Add pagination limits
  - Implement query optimization
  - Add performance monitoring (optional)

  **Must NOT do**:
  - Don't premature optimize
  - Don't cache everything

  **Recommended Agent Profile**:
  - **Category**: `ultrabrain`
  - **Skills**: []
  - Reason: Performance requires measurement and analysis

  **Parallelization**:
  - **Can Run In Parallel**: YES
  - **Parallel Group**: Wave 7
  - **Blocks**: None
  - **Blocked By**: None

  **Acceptance Criteria**:
  - [ ] Database indexes added
  - [ ] Redis caching works
  - [ ] Images optimized
  - [ ] Page load times acceptable

  **Commit**: YES
  - Message: `perf: add caching and performance optimizations`
  - Files: Various optimization files

- [ ] 39. Documentation

  **What to do**:
  - Create ARCHITECTURE.md:
    - System overview
    - Multi-tenancy explanation
    - Folder structure
    - Key decisions
  - Create API documentation (Swagger)
  - Create setup instructions (README.md)
  - Create deployment guide
  - Document environment variables
  - Add inline code comments where needed

  **Must NOT do**:
  - Don't write unnecessary documentation
  - Don't duplicate information

  **Recommended Agent Profile**:
  - **Category**: `writing`
  - **Skills**: []
  - Reason: Documentation requires clear communication

  **Parallelization**:
  - **Can Run In Parallel**: YES
  - **Parallel Group**: Wave 7
  - **Blocks**: None
  - **Blocked By**: None

  **Acceptance Criteria**:
  - [ ] ARCHITECTURE.md complete
  - [ ] API docs accessible
  - [ ] README has setup instructions
  - [ ] Environment variables documented

  **Commit**: YES
  - Message: `docs: add comprehensive documentation`
  - Files: `ARCHITECTURE.md`, `README.md`, `DEPLOYMENT.md`

### WAVE 8: Testing & Deployment Prep

- [ ] 40. Unit Tests for Domain Logic

  **What to do**:
  - Write tests for:
    - Auth service (login, refresh, logout)
    - Product service (CRUD, search)
    - Cart service (add, update, remove)
    - Order service (create, status update)
  - Mock external dependencies
  - Test edge cases
  - Aim for 70%+ coverage on services

  **Must NOT do**:
  - Don't test implementation details
  - Don't mock what you don't own (unless necessary)

  **Recommended Agent Profile**:
  - **Category**: `unspecified-high`
  - **Skills**: []
  - Reason: Testing requires understanding of logic

  **Parallelization**:
  - **Can Run In Parallel**: YES
  - **Parallel Group**: Wave 8
  - **Blocks**: None
  - **Blocked By**: Waves 2, 3

  **Acceptance Criteria**:
  - [ ] Auth service tests pass
  - [ ] Product service tests pass
  - [ ] Cart service tests pass
  - [ ] Order service tests pass
  - [ ] Coverage report generated

  **Commit**: YES
  - Message: `test: add unit tests for domain services`
  - Files: `apps/backend/src/**/*.spec.ts`

- [ ] 41. Integration Tests for APIs

  **What to do**:
  - Write tests for all API endpoints:
    - Auth endpoints
    - Product endpoints
    - Cart endpoints
    - Order endpoints
  - Use test database
  - Test request/response formats
  - Test error cases
  - Test authentication requirements

  **Must NOT do**:
  - Don't test against production database
  - Don't leave test data

  **Recommended Agent Profile**:
  - **Category**: `unspecified-high`
  - **Skills**: []
  - Reason: Integration tests require API knowledge

  **Parallelization**:
  - **Can Run In Parallel**: YES
  - **Parallel Group**: Wave 8
  - **Blocks**: None
  - **Blocked By**: Waves 2, 3

  **Acceptance Criteria**:
  - [ ] All endpoints tested
  - [ ] Test database isolated
  - [ ] Auth requirements verified
  - [ ] Error cases covered

  **Commit**: YES
  - Message: `test: add API integration tests`
  - Files: `apps/backend/test/`

- [ ] 42. Tenant Isolation Tests

  **What to do**:
  - Write tests to verify tenant isolation:
    - Tenant A cannot access Tenant B's data
    - RLS policies work correctly
    - Cross-tenant queries return empty
  - Test with multiple tenants
  - Test edge cases (deleted tenants, suspended)

  **Must NOT do**:
  - Don't assume isolation works
  - Don't skip negative tests

  **Recommended Agent Profile**:
  - **Category**: `ultrabrain`
  - **Skills**: []
  - Reason: Tenant isolation is security-critical

  **Parallelization**:
  - **Can Run In Parallel**: YES
  - **Parallel Group**: Wave 8
  - **Blocks**: None
  - **Blocked By**: Tasks 5, 6

  **Acceptance Criteria**:
  - [ ] Cross-tenant access blocked
  - [ ] RLS policies enforced
  - [ ] Test data properly isolated
  - [ ] All isolation tests pass

  **Commit**: YES
  - Message: `test: add tenant isolation verification tests`
  - Files: `apps/backend/test/tenant-isolation.spec.ts`

- [ ] 43. End-to-End Checkout Test

  **What to do**:
  - Write complete checkout flow test:
    - Add products to cart
    - Proceed to checkout
    - Fill customer info
    - Complete Stripe payment (test mode)
    - Verify order created
    - Verify cart cleared
  - Use Stripe test keys
  - Mock or use Stripe test card

  **Must NOT do**:
  - Don't use real payments
  - Don't skip payment verification

  **Recommended Agent Profile**:
  - **Category**: `ultrabrain`
  - **Skills**: []
  - Reason: E2E tests require full system understanding

  **Parallelization**:
  - **Can Run In Parallel**: NO (depends on checkout)
  - **Parallel Group**: Wave 8
  - **Blocks**: None
  - **Blocked By**: Task 30

  **Acceptance Criteria**:
  - [ ] Full checkout flow works
  - [ ] Payment processes
  - [ ] Order created
  - [ ] Cart cleared

  **Commit**: YES
  - Message: `test: add end-to-end checkout test`
  - Files: `apps/backend/test/checkout.e2e-spec.ts`

- [ ] 44. Seed Data Creation

  **What to do**:
  - Create seed script:
    - Super admin user
    - Sample tenants (2-3)
    - Sample categories
    - Sample products with images
    - Sample orders
  - Make idempotent (can run multiple times)
  - Use for development and demos
  - Document seed data

  **Must NOT do**:
  - Don't use production passwords
  - Don't create too much data

  **Recommended Agent Profile**:
  - **Category**: `quick`
  - **Skills**: []
  - Reason: Seed data is straightforward data creation

  **Parallelization**:
  - **Can Run In Parallel**: YES
  - **Parallel Group**: Wave 8
  - **Blocks**: None
  - **Blocked By**: Task 2

  **Acceptance Criteria**:
  - [ ] Seed script runs successfully
  - [ ] Super admin created
  - [ ] Sample data created
  - [ ] Script is idempotent

  **Commit**: YES
  - Message: `chore: add database seed script`
  - Files: `apps/backend/prisma/seed.ts`

- [ ] 45. Environment Configuration

  **What to do**:
  - Create `.env.example` with all variables
  - Document each variable
  - Separate dev/prod configs
  - Add validation for required variables
  - Create config service
  - Never commit real .env files

  **Must NOT do**:
  - Don't commit secrets
  - Don't leave hardcoded values

  **Recommended Agent Profile**:
  - **Category**: `quick`
  - **Skills**: []
  - Reason: Configuration is setup work

  **Parallelization**:
  - **Can Run In Parallel**: YES
  - **Parallel Group**: Wave 8
  - **Blocks**: None
  - **Blocked By**: None

  **Acceptance Criteria**:
  - [ ] `.env.example` complete
  - [ ] All variables documented
  - [ ] Config validation works
  - [ ] No secrets in repo

  **Commit**: YES
  - Message: `chore: add environment configuration`
  - Files: `.env.example`, `apps/backend/src/config/`

- [ ] 46. Production Build Verification

  **What to do**:
  - Test production build:
    - Build backend
    - Build frontend
    - Run with production env
  - Verify all features work
  - Check for console errors
  - Verify environment variables loaded
  - Test Docker production build

  **Must NOT do**:
  - Don't skip production testing
  - Don't ignore build warnings

  **Recommended Agent Profile**:
  - **Category**: `unspecified-high`
  - **Skills**: []
  - Reason: Production verification requires thoroughness

  **Parallelization**:
  - **Can Run In Parallel**: NO (final verification)
  - **Parallel Group**: Wave 8
  - **Blocks**: None
  - **Blocked By**: All previous tasks

  **Acceptance Criteria**:
  - [ ] Backend builds successfully
  - [ ] Frontend builds successfully
  - [ ] No console errors
  - [ ] All features work
  - [ ] Docker build works

  **Commit**: YES
  - Message: `chore: verify production builds`
  - Files: Build configurations

- [ ] 47. Final Documentation Review

  **What to do**:
  - Review all documentation:
    - README.md
    - ARCHITECTURE.md
    - API docs
    - Environment docs
  - Fix any inaccuracies
  - Add missing information
  - Ensure setup instructions work
  - Add screenshots (optional)

  **Must NOT do**:
  - Don't leave outdated docs
  - Don't skip this step

  **Recommended Agent Profile**:
  - **Category**: `writing`
  - **Skills**: []
  - Reason: Documentation review requires attention to detail

  **Parallelization**:
  - **Can Run In Parallel**: NO (final step)
  - **Parallel Group**: Wave 8
  - **Blocks**: None
  - **Blocked By**: All previous tasks

  **Acceptance Criteria**:
  - [ ] All docs reviewed
  - [ ] Inaccuracies fixed
  - [ ] Setup instructions tested
  - [ ] Ready for handoff

  **Commit**: YES
  - Message: `docs: final documentation review and updates`
  - Files: `README.md`, `ARCHITECTURE.md`, etc.

---

## Commit Strategy

| After Task | Message | Files | Verification |
|------------|---------|-------|--------------|
| 1 | `chore: initial project setup` | apps/backend/, apps/frontend/ | npm run dev works |
| 2 | `feat(db): design multi-tenant schema` | prisma/schema.prisma | migrate succeeds |
| 3 | `chore: add Docker Compose setup` | docker-compose.yml | docker-compose up works |
| 4 | `feat(backend): setup clean architecture` | src/features/, src/shared/ | app starts |
| 5 | `feat(backend): implement RLS` | rls-extension.ts | isolation tests pass |
| 6 | `feat(backend): add tenant context` | tenant-context/ | middleware works |
| 7 | `feat(backend): implement JWT auth` | features/auth/ | login tests pass |
| 8 | `feat(backend): add tenant management` | features/tenants/ | CRUD works |
| 9 | `feat(backend): implement products` | features/products/ | product tests pass |
| 10 | `feat(backend): add categories` | features/categories/ | category tests pass |
| 11 | `feat(backend): implement cart` | features/carts/ | cart tests pass |
| 12 | `feat(backend): implement orders` | features/orders/ | order tests pass |
| 13 | `feat(backend): add customers` | features/customers/ | customer tests pass |
| 14 | `feat(frontend): setup Next.js` | middleware.ts, app/[tenant]/ | middleware works |
| 15 | `feat(frontend): tenant resolution` | lib/tenant/ | context works |
| 16 | `feat(frontend): theme system` | components/ui/, lib/theme/ | theming works |
| 17 | `feat(frontend): auth UI` | app/[tenant]/login/, lib/auth/ | login works |
| 18 | `feat(frontend): API client` | lib/api/ | API calls work |
| 19 | `feat(frontend): super admin` | app/platform/admin/ | dashboard works |
| 20 | `feat(frontend): tenant admin layout` | app/[tenant]/admin/ | layout works |
| 21 | `feat(frontend): product UI` | app/[tenant]/admin/products/ | product management works |
| 22 | `feat(frontend): order UI` | app/[tenant]/admin/orders/ | order management works |
| 23 | `feat(frontend): settings UI` | app/[tenant]/admin/settings/ | settings work |
| 24 | `feat(frontend): analytics UI` | app/[tenant]/admin/analytics/ | analytics work |
| 25 | `feat(frontend): storefront layout` | app/[tenant]/(storefront)/ | layout works |
| 26 | `feat(frontend): home page` | app/[tenant]/(storefront)/page.tsx | home works |
| 27 | `feat(frontend): product listing` | app/[tenant]/(storefront)/products/ | listing works |
| 28 | `feat(frontend): product detail` | app/[tenant]/(storefront)/products/[id]/ | detail works |
| 29 | `feat(frontend): cart UI` | app/[tenant]/(storefront)/cart/ | cart works |
| 30 | `feat(frontend): checkout` | app/[tenant]/(storefront)/checkout/ | checkout works |
| 31 | `feat: integrate frontend-backend` | Various | integration works |
| 32 | `feat: image upload` | features/storage/, components/upload/ | upload works |
| 33 | `feat: rate limiting` | rate-limiting/ | limits enforced |
| 34 | `feat: soft deletes and audit` | features/audit/ | audit works |
| 35 | `feat: error handling` | Various | errors handled |
| 36 | `style: responsive polish` | Various | responsive works |
| 37 | `feat: dark mode` | lib/theme/ | dark mode works |
| 38 | `perf: optimizations` | Various | performance improved |
| 39 | `docs: documentation` | *.md | docs complete |
| 40 | `test: unit tests` | **/*.spec.ts | tests pass |
| 41 | `test: integration tests` | test/ | tests pass |
| 42 | `test: tenant isolation` | test/tenant-isolation.spec.ts | isolation verified |
| 43 | `test: e2e checkout` | test/checkout.e2e-spec.ts | checkout works |
| 44 | `chore: seed data` | prisma/seed.ts | seed works |
| 45 | `chore: environment config` | .env.example, config/ | config works |
| 46 | `chore: production build` | Various | build succeeds |
| 47 | `docs: final review` | *.md | docs reviewed |

---

## Success Criteria

### Verification Commands

```bash
# Start all services
docker-compose up -d

# Run backend tests
npm run test

# Run frontend tests
bun test

# Verify tenant isolation
npm run test:tenant-isolation

# Build for production
npm run build

# Verify production build
docker-compose -f docker-compose.prod.yml up -d
```

### Final Checklist

- [ ] All 47 tasks completed
- [ ] All tests passing
- [ ] Tenant isolation verified
- [ ] Checkout flow works end-to-end
- [ ] Docker setup works with single command
- [ ] Documentation complete
- [ ] Code follows clean architecture
- [ ] No hardcoded secrets
- [ ] Responsive design works
- [ ] Dark mode works
- [ ] Production build succeeds
- [ ] Ready for deployment

### Quality Gates

1. **Security**: Tenant isolation verified, no data leakage
2. **Functionality**: All MVP features work
3. **Performance**: Page loads < 3s, API responses < 500ms
4. **Code Quality**: Clean architecture, meaningful names
5. **Testing**: Critical paths covered
6. **Documentation**: Setup and architecture documented

---

## Architecture Decisions

### Multi-Tenancy: PostgreSQL RLS
**Decision**: Use PostgreSQL Row Level Security with Prisma extensions
**Rationale**: Automatic tenant filtering at database level, no query modifications needed, secure by default
**Trade-off**: Slightly more complex setup, but safer than application-level filtering

### Tenant Resolution: Next.js Middleware + Headers
**Decision**: Extract subdomain in middleware, pass tenant_id via headers
**Rationale**: Clean separation, works with both subdomains and custom domains
**Trade-off**: Requires API call in middleware (cached)

### Clean Architecture: Feature-Based
**Decision**: Organize by feature (auth, products, orders) not by layer
**Rationale**: Easier to understand, ready for microservice extraction
**Trade-off**: Some code duplication across features

### Authentication: JWT + Refresh Tokens
**Decision**: Short-lived access tokens (15min), long-lived refresh tokens (7 days)
**Rationale**: Balance security and UX
**Trade-off**: Requires refresh token storage and rotation

### File Storage: S3-Compatible (MinIO)
**Decision**: Use MinIO for dev, S3 for production
**Rationale**: Same API, easy local development
**Trade-off**: Additional service to run locally

---

## Post-MVP Roadmap

### Phase 2: Enhanced Features
- Email notifications (SendGrid/Resend)
- Shipping provider integration
- Inventory management
- Coupon/discount system
- Customer reviews
- Wishlist
- Advanced analytics

### Phase 3: Scale & Enterprise
- Microservice extraction
- Multi-region deployment
- Advanced caching (CDN)
- Webhook system
- API for integrations
- White-label mobile app

---

## Notes for Executor

1. **Start with Wave 1**: Foundation must be solid before building features
2. **Test tenant isolation early**: Task 5 and 42 are critical - don't skip
3. **Use the draft**: Reference `.sisyphus/drafts/multi-tenant-saas-store.md` for context
4. **Commit frequently**: Each task has a commit - don't batch changes
5. **Ask if stuck**: Some tasks (5, 7, 30) are complex - consult if needed
6. **Focus on MVP**: Don't add post-MVP features no matter how tempting
7. **Quality over speed**: This is production code, not a demo

**Estimated Total Time**: 40-60 hours of focused work
**Recommended Team**: 2-3 developers (1 backend, 1 frontend, 1 floating)
**Solo Timeline**: 4-6 weeks part-time

---

*Plan generated by Prometheus - Strategic Planning Consultant*
*Draft reference: `.sisyphus/drafts/multi-tenant-saas-store.md`*
