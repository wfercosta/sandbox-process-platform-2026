# sandbox-process-platform-2025

> Sandbox for designing and testing **long-running process architectures**, applied to a realistic **mortgage lending** domain.

---

## 🎯 Purpose

This repository is a **technical sandbox** focused on exploring architectural patterns and engineering practices for **long-running business processes**.

The case study is intentionally based on **mortgage lending**, a domain that naturally involves:
- multi-step workflows
- asynchronous integrations
- human and automated decisions
- long-lived state and compensations
- strict non-functional requirements (traceability, reliability, auditability)

The goal is **architectural learning**, not a production-ready financial system.

---

## 🧩 Business Context (Case Study)

The sandbox models the lifecycle of a mortgage financing process, including:

- proposal simulation
- credit analysis and underwriting
- contract execution and signing
- custody and post-contract management

This domain is used as a **realistic backdrop** to stress-test architectural decisions and processing models.

> The patterns explored here are **domain-agnostic** and transferable to other industries such as insurance, marketplace, logistics and internal enterprise platforms.

---

## 🏗️ Architectural Focus

This project emphasizes:

- **Long-running process orchestration**
- Explicit modeling of process state and transitions
- Clear separation between:
  - orchestration
  - domain logic
  - infrastructure
- Contracts-first integration
- Incremental and documented architectural evolution (ADRs)

Process orchestration is treated as a **first-class architectural concern**, not as incidental glue code.

---

## 🛠️ Technology Stack

### Front-end
- **Web:** Next.js
- **Mobile:** Expo (React Native)

### Back-end
- **Kotlin + Spring Boot**  
  Used for domain-centric services and APIs.
- **Go (stdlib-first approach)**  
  Used to explore minimalistic service design, workers and integrations.

### Process Orchestration
- **Camunda 8 (Zeebe)**  
  Used to model and execute long-running processes, including:
  - orchestration of service interactions
  - timeouts and retries
  - compensations
  - human-in-the-loop steps (where applicable)

### Integration & Contracts
- **OpenAPI** for synchronous APIs
- **AsyncAPI / JSON Schema** for events and shared payloads
- Contracts are treated as the **source of truth** and live in a shared location in the repository.

### Infrastructure & Platform
- **Docker** for local development and packaging
- **Terraform** for infrastructure as code
- **AWS (ECS, ECR, IAM, networking)** as the primary cloud execution model (subject to evolution via ADRs)

---

## 🤖 Use of AI in Engineering (Codex)

This repository is intentionally structured to support **AI-assisted software engineering**, with a strong focus on **Codex**.

AI is used as:
- a design and implementation assistant
- a reviewer of architectural consistency
- a productivity multiplier for repetitive tasks

### AI-specific structure
- `AGENTS.md` defines explicit rules and expectations for AI agents
- `.ai/pinned-context.md` provides stable architectural context
- `.ai/prompts/` contains reusable prompts for common engineering tasks
- Architectural decisions and boundaries are documented to reduce ambiguity for both humans and AI

### Guiding principle
> AI is treated as a **collaborator**, not an autonomous actor.  
> All meaningful architectural decisions remain explicit and documented.

---

## 📁 Repository Structure (High Level)

```text
apps/        # web, mobile, backend services and process workers
infra/       # infrastructure as code (Terraform)
docs/        # architecture, ADRs, patterns and runbooks
.ai/         # context and memory for Codex
