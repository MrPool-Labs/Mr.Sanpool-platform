# Repository Boundaries

## Purpose

This document defines the architectural boundaries between the main repositories of the MrPool / Mr.Sanpool ecosystem.

The goal is to reduce duplication, preserve separation of concerns, protect commercial value, and make future evolution more predictable.

## Repository Roles

### `mrpool-core`
Technical core and reusable foundation.

This repository should contain generic, reusable, product-neutral components that can be shared by other repositories without depending on SaaS, tenancy, billing, or enterprise-specific orchestration.

### `mrpool-enterprise`
Enterprise capabilities and specialized operational integrations.

This repository should contain enterprise-specific services, connectors, operational logic, and advanced runtime capabilities that are consumed by higher-level products.

### `mrsanpool-platform`
SaaS product layer, governance, and control plane.

This repository should contain the platform-facing product logic: tenants, auth, plans, onboarding, billing, admin operations, entitlements, and orchestration of enterprise capabilities.

---

## What Belongs in Each Repository

## `mrpool-core`

### Include
- shared technical contracts
- base runtime primitives
- generic utility libraries
- retry, validation, serialization, and shared helper logic
- reusable observability foundations
- neutral worker and pipeline abstractions
- components that do not depend on tenant, billing, or enterprise context

### Do Not Include
- tenant management
- onboarding
- auth for the product surface
- plans or subscriptions
- billing logic
- SaaS APIs
- enterprise orchestration specifics
- commercial or governance rules

### Decision Rule
If a component can be reused by another project without knowing anything about tenant, plan, enterprise deployment, or product rules, it probably belongs in `mrpool-core`.

---

## `mrpool-enterprise`

### Include
- enterprise services
- policy engine
- graph builder
- enterprise runtime logic
- deployment, reconcile, and recovery capabilities
- operational connectors
- infrastructure-facing integrations
- enterprise observability and hardening logic
- schemas and contracts specific to enterprise operations

### Do Not Include
- tenant onboarding
- platform users/admin management
- subscriptions and plans
- billing and commercial rules
- product-level auth flows
- SaaS admin APIs
- tenant governance as a product concern

### Decision Rule
If a capability exists because of enterprise operations and can be consumed by the platform as a service/capability, it belongs in `mrpool-enterprise`.

---

## `mrsanpool-platform`

### Include
- public/platform API surface
- onboarding
- JWT auth
- tenants and users
- plans and subscriptions
- billing / usage ledger
- platform admin endpoints
- entitlement enforcement
- audit trails of platform actions
- SaaS database models and migrations
- control plane orchestration of enterprise capabilities
- tenant isolation rules
- governance and administrative flows

### Do Not Include
- deep enterprise connector implementations
- generic utilities that belong in core
- enterprise logic duplicated from `mrpool-enterprise`

### Decision Rule
If it is part of the SaaS product behavior the customer or operator experiences directly, it belongs in `mrsanpool-platform`.

---

## Dependency Direction

The dependency direction should remain:

- `mrpool-core` -> no dependency on the others
- `mrpool-enterprise` -> may depend on `mrpool-core`
- `mrsanpool-platform` -> may depend on `mrpool-core`
- `mrsanpool-platform` -> may depend on `mrpool-enterprise`

The following should be avoided:

- `mrpool-core` depending on `mrpool-enterprise`
- `mrpool-core` depending on `mrsanpool-platform`
- `mrpool-enterprise` depending on `mrsanpool-platform`

---

## Source of Truth Rules

## `mrpool-core` is the source of truth for
- shared neutral technical primitives
- generic contracts and helpers
- product-agnostic runtime building blocks

## `mrpool-enterprise` is the source of truth for
- enterprise deployment capabilities
- reconcile and recovery logic
- policy and graph domain logic
- enterprise integrations and connectors
- enterprise operational schemas

## `mrsanpool-platform` is the source of truth for
- tenants
- auth context
- users and roles
- plans and subscriptions
- billing usage
- onboarding/admin flows
- entitlements and governance
- platform-facing control plane APIs

---

## Duplication Rules

Avoid duplicating logic between repositories.

### Do not duplicate between `platform` and `enterprise`
- deployment logic
- reconcile logic
- recovery logic
- enterprise schemas
- connector logic
- policy/graph logic

The platform should consume enterprise capabilities, not reimplement them.

### Do not duplicate between `core` and `enterprise`
- generic helpers
- neutral shared types
- reusable utilities

### Do not duplicate between `core` and `platform`
- product auth
- SaaS database models
- admin/product API contracts
- onboarding, billing, tenant logic

---

## Data Ownership

## Platform-owned data
- tenants
- users
- plans
- subscriptions
- usage events
- auth context
- onboarding state

## Enterprise-owned data
- deployment intent
- service topology
- runtime operational metadata
- policy state
- graph state
- recovery decisions
- connector-specific metadata

## Core-owned data
- generic shared schemas
- reusable neutral technical abstractions
- technical base-level shared structures

---

## Practical Rules for New Files

Before creating a new file, ask:

1. Is this product or governance logic?
   - Put it in `mrsanpool-platform`.

2. Is this an enterprise capability or operational integration?
   - Put it in `mrpool-enterprise`.

3. Is this neutral and reusable across contexts?
   - Put it in `mrpool-core`.

---

## Recommended Target Layout

## `mrpool-core`
- `contracts/`
- `runtime/`
- `shared/`
- `utils/`
- `observability_base/`

## `mrpool-enterprise`
- `services/`
- `connectors/`
- `integrations/`
- `recovery/`
- `policy/`
- `graph/`
- `deploy/`
- `schemas/`

## `mrsanpool-platform`
- `api/`
- `auth/`
- `db/`
- `services/`
- `state/`
- `audit/`
- `control_plane/`
- `billing/` (if expanded)
- `admin/` (if expanded)

---

## Current Recommended Operating Model

### `mrpool-core`
Keep it lean and neutral.

### `mrpool-enterprise`
Keep it focused on enterprise capabilities and operational engines.

### `mrsanpool-platform`
Keep it as the product-facing SaaS/control-plane layer.

This separation allows:
- cleaner ownership
- lower duplication
- better commercial protection
- more maintainable integration boundaries
- easier future refactoring

---

## Next Steps

1. Add this document to the repositories as a reference.
2. Review duplicated logic between `mrsanpool-platform` and `mrpool-enterprise`.
3. Extract only truly neutral reusable pieces into `mrpool-core`.
4. Keep `mrsanpool-platform` as the only place for:
   - tenancy
   - auth
   - plans
   - subscriptions
   - billing
   - onboarding
   - entitlements
5. Keep `mrpool-enterprise` as the only place for:
   - deployment
   - reconcile
   - recovery
   - connectors
   - enterprise operational logic
