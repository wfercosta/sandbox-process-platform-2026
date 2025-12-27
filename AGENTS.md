# AGENTS.md (Codex)

This repository is a **technical sandbox** to design and test **process-oriented architectures** applied to a realistic **mortgage lending** case study (proposal simulation, underwriting, contracting, and custody management).

The system is intentionally modeled as a **distributed architecture**, composed of:
- **microfrontends** (Next.js)
- **microservices** (Kotlin / Spring Boot and Go – stdlib-first)
- **process orchestration** (Camunda 8 / Zeebe)

Codex is used to accelerate implementation, refactoring, and documentation — while keeping architectural decisions explicit, reviewable, and auditable.

---

## 1) Required reading (before any change)

Always read these files first:

1. `.ai/pinned-context.md`
2. `docs/01_architecture/ARCHITECTURE.md` (full architecture)
3. `docs/01_architecture/decisions/` (ADRs)

If any required document is missing, create a **minimal stub** and proceed with conservative assumptions.

---

## 2) How Codex should work in this repository

### Operating mode
- Always propose a **short plan** (3–7 steps) before touching multiple files.
- Prefer **small, incremental changes** over broad refactors.
- Keep changes scoped to **one microfrontend, one microservice, or one workflow** whenever possible.
- Avoid cross-cutting changes unless explicitly requested or justified by an ADR.

### Communication & outputs
- Clearly state which **microfrontend(s)**, **microservice(s)** or **workflow(s)** are impacted.
- When adding a new microfrontend or microservice, update:
  - repository structure documentation
  - contracts (if applicable)
  - architecture docs (at least at a high level)
- When uncertain, choose the **simplest viable approach** and document assumptions.

---

## 3) Architectural boundaries (hard rules)

### Microfrontends (Next.js)
- The web layer follows a **microfrontend architecture**.
- Each microfrontend:
  - is independently deployable
  - owns its UI scope and routing boundaries
  - communicates with backend services only via contracts
- Shared UI components or design systems must live in explicit shared packages.
- Avoid implicit coupling between microfrontends (no direct imports across apps unless explicitly designed).

### Microservices
- Each microservice is:
  - independently deployable
  - independently versioned
  - owner of its own data (unless an ADR states otherwise)
- No shared databases between microservices.
- Integration happens via **contracts**, not direct code or database access.

### Application vs Infrastructure
- **Microfrontends and microservices do not provision cloud resources.**
- **All Infrastructure as Code (Terraform) lives under `infra/`.**
- Applications provide:
  - source code
  - Dockerfiles
  - configuration contracts (ports, env vars, health endpoints)
- Infrastructure consumes deployable artifacts (Docker images) via variables.

### Contracts-first
- API and event contracts live under `apps/shared/contracts/`.
- Contracts are treated as the **source of truth**.
- Breaking changes require:
  - explicit versioning
  - coordination across impacted microfrontends and microservices
  - an ADR when architectural boundaries change.

### Process orchestration boundaries (Camunda 8 / Zeebe)
- Camunda 8 (Zeebe) is used strictly for **orchestration**:
  - long-lived state
  - retries, timeouts, compensations
  - coordination across microservices
- **Domain logic remains inside microservices**, not embedded as complex workflow logic.
- Changes to workflow ↔ microservice responsibilities require an ADR.

---

## 4) High-level repository structure (map)

- `apps/web/` — Next.js **microfrontends**
  - each subdirectory represents an independent microfrontend

- `apps/mobile/` — Expo (React Native) mobile application

- `apps/microservices/`
  - `kotlin/` — Kotlin + Spring Boot microservices
  - `go/` — Go (stdlib-first) microservices and workers

- `apps/process/camunda/` — Camunda 8 / Zeebe (BPMN, DMN, workers, config)

- `apps/shared/contracts/` — OpenAPI / AsyncAPI / JSON Schema (source of truth)

- `infra/` — Terraform modules and environment definitions (AWS/ECS/etc.)
- `platform/` — Local development platform assets (Docker Compose, scripts)
- `docs/` — Architecture, ADRs, engineering documentation
- `.ai/` — Pinned context, prompts, and memory for Codex

---

## 5) Standards to follow

### Commit messages (Conventional Commits)

Format:
- `feat(scope): ...`
- `fix(scope): ...`
- `docs(scope): ...`
- `refactor(scope): ...`
- `test(scope): ...`
- `chore(scope): ...`

Typical scopes:
- `microfrontend-<name>`
- `mobile`
- `process`
- `microservice-kotlin-<name>`
- `microservice-go-<name>`
- `contracts`
- `infra`
- `docs`
- `ci`

---

## 6) Branching model

- Trunk-based development
- `main` is the trunk
- Short-lived branches: `feat/...`, `fix/...`, `chore/...`
- Prefer small, reviewable pull requests

---

## 7) Definition of Done (DoD)

A change is considered done when:

- The affected microfrontend(s), microservice(s), or workflow(s) build successfully
- Minimal tests run (unit and/or integration, where applicable)
- Linting and formatting pass (if configured)
- Contracts are updated (if integration changed)
- Architecture docs or ADRs are updated (if boundaries or decisions changed)

---

## 8) When to create an ADR

Create an ADR when introducing or changing:

- microfrontend boundaries or composition model
- microservice boundaries or responsibilities
- workflow vs microservice logic split
- synchronous vs asynchronous communication
- contract versioning strategy
- data ownership or persistence model
- execution model (ECS, EKS, Lambda, etc.)
- observability or security posture

ADRs live in:
