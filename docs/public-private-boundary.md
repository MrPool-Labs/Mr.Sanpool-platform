# Public–Private Boundary Policy

## 1. Purpose

This document formalizes the boundary between the public technical layer of the Mr.Sanpool ecosystem and its private commercial and sovereign layers.

The policy exists to protect three things at the same time:

1. the public credibility and adoption value of `mrpool-core`
2. the proprietary commercial value of `mrpool-enterprise`
3. the sovereign operational value of `mrsanpool-platform`

Integration between these layers is **technical only**. It does not change ownership, licensing, distribution rights, or access rules.

---

## 2. Ecosystem classification

### 2.1 Public layer

#### `mrpool-core`

Classification: **public technical layer**

Primary role:

- statistical inference
- coordination analysis
- control recommendation primitives
- benchmarking
- experimentation
- technical proof of competence

Public distribution allowed:

- source code
- documentation
- benchmarks
- research notes
- examples
- technical contracts intentionally published

Must not include:

- tenant governance
- private policy bundles
- customer-specific connectors
- secret material
- deployment control plane logic
- commercial licensing enforcement logic
- sovereign node management

---

### 2.2 Private product layer

#### `mrpool-enterprise`

Classification: **private commercial product layer**

Primary role:

- event ingestion
- operational scoring
- policy decisioning
- explainability for product/API consumption
- anti-replay and integrity enforcement
- enterprise connectors
- operational workers
- observability of protected workflows

Access model:

- unlocked only under contract
- deployable only under authorized license terms
- source access only if explicitly granted
- managed access may be API-only, binary-only, hosted-only, or code access under negotiated terms

Must not be treated as public merely because it imports `mrpool-core`.

---

### 2.3 Private sovereign platform layer

#### `mrsanpool-platform`

Classification: **private sovereign platform layer**

Primary role:

- control plane
- tenant governance
- service registration
- deployment orchestration
- runtime state
- reconciliation
- secrets/config governance
- audit trail
- installation profiles
- multi-node operational governance

Access model:

- unlocked only under contract
- reserved for sovereign operation, managed operation, or strategic licensing
- source access only by explicit grant
- may be operated by owner, customer, or dedicated managed deployment depending on contract

Must not be treated as public merely because it hosts `mrpool-enterprise`.

---

## 3. Core policy statement

The following rule remains active even after technical integration:

> `mrpool-core` is public; `mrpool-enterprise` and `mrsanpool-platform` are private and unlocked only under contract.

Technical integration does **not** convert a private layer into a public layer.

Technical dependency does **not** imply source distribution rights.

Operational hosting does **not** imply code disclosure rights.

---

## 4. Boundary rules

### 4.1 Code boundary

The ecosystem must preserve repository-level separation.

#### Public repository

- `mrpool-core`

#### Private repositories

- `mrpool-enterprise`
- `mrsanpool-platform`

No private source file may be mirrored into the public repository.

No platform-only operational logic may be committed into the public core.

### 4.2 Dependency direction

Allowed dependency direction:

- `mrpool-enterprise` -> `mrpool-core`
- `mrsanpool-platform` -> `mrpool-enterprise`
- `mrsanpool-platform` -> `mrpool-core` only for explicit integration utilities if needed

Forbidden dependency direction:

- `mrpool-core` -> `mrpool-enterprise`
- `mrpool-core` -> `mrsanpool-platform`
- `mrpool-enterprise` -> `mrsanpool-platform` for internal control-plane coupling

### 4.3 Contract boundary

Public contracts may exist for the public core API.

Private contracts may exist for:

- enterprise API behavior
- platform deployment descriptors
- audit and runtime schemas
- licensing, support, installation, and managed operation

A technical contract is not the same thing as a public license grant.

### 4.4 Distribution boundary

Customers may receive one or more of the following under contract:

- hosted access
- API access
- packaged deployment artifacts
- private binary/image access
- source access when explicitly negotiated

Distribution of `mrpool-enterprise` or `mrsanpool-platform` is always governed by private terms.

### 4.5 Secret and policy boundary

The following must never be published in `mrpool-core`:

- secrets
- tenant metadata
- operational credentials
- private policy bundles
- private deployment topology
- customer-specific configuration
- proprietary threat logic not intentionally designated public

---

## 5. What integration is allowed to do

Integration is allowed to:

- let `mrpool-enterprise` consume `mrpool-core` through adapters
- let `mrsanpool-platform` register and deploy the `mrpool-enterprise` stack
- expose stable interfaces between layers
- standardize payloads and runtime descriptors

Integration is **not** allowed to:

- blur repository ownership
- force public disclosure of private source
- move sovereign platform code into the public core
- embed private customer logic into the public repository

---

## 6. Commercial unlocking model

### 6.1 Open entry

`mrpool-core` may be used as:

- a public technical benchmark
- a public library
- a public demonstration of competence
- an adoption funnel into higher-value offerings

### 6.2 Contracted product unlock

`mrpool-enterprise` is unlocked under contract for customers who need:

- operational risk scoring
- explainability
- enforcement
- private connectors
- private deployments
- support and maintenance

### 6.3 Sovereign platform unlock

`mrsanpool-platform` is unlocked under contract for customers or partners who need:

- sovereign hosting
- dedicated runtime governance
- tenant-aware deployment control
- auditable operational control plane
- hardened single-node or multi-node operation

---

## 7. Engineering enforcement requirements

The following engineering practices are mandatory to preserve this boundary:

1. separate repositories for public and private layers
2. explicit package boundaries and imports
3. adapter-based integration only
4. clear license files for each repository
5. `.gitignore` and secret hygiene for all private repos
6. no customer-specific material in the public core
