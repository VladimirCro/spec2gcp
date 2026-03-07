# spec2gcp Primer

> **Specification-driven development with AI agents — from idea to Google Cloud in a structured, repeatable way.**

## What is spec2gcp?

spec2gcp is a set of patterns and primitives that enable developers to create complex applications using **specifications instead of manual coding**. It provides an opinionated yet flexible framework for AI-assisted software development, where specifications serve as the source of truth throughout the entire development lifecycle.

Instead of diving straight into code, spec2gcp guides you through a structured process: define what you want to build (PRD), break it into features (FRDs), let specialized AI agents generate the implementation, and deploy to Google Cloud — all while maintaining traceability from business requirements to deployed infrastructure.

```
┌─────────────────────────────────────────────────────────────────────┐
│                         spec2gcp Flow                               │
├─────────────────────────────────────────────────────────────────────┤
│  Idea → PRD → FRDs → ADRs → Implementation Plan → Code → GCP       │
│    ↑                                                          ↓     │
│    └──────────── Specifications as Source of Truth ───────────┘     │
└─────────────────────────────────────────────────────────────────────┘
```

---

## Core Philosophy

### Specs as the Source of Truth

In spec2gcp, specifications are not just documentation — they are the **primary artifacts** that drive everything:

- **PRD (Product Requirements Document)** defines *what* to build and *why*
- **FRDs (Feature Requirements Documents)** break down features with clear acceptance criteria
- **ADRs (Architecture Decision Records)** capture *how* key technical decisions are made
- **AGENTS.md** provides coding agents with project-specific guidelines

When you update a spec, agents can regenerate code. When you need to understand a feature, the spec tells the story. This creates a **living documentation system** that evolves with your project.

### Opinionated but Flexible

spec2gcp provides:

- **Predefined agents** with specific roles (PM, Architect, Dev Lead, etc.)
- **Structured prompts** for common workflows
- **MCP server configurations** for enhanced capabilities
- **Template shells** for rapid bootstrapping

But everything is **customizable** — you can modify agents, create new prompts, add MCP servers, or adapt shells to your specific needs.

---

## The Three Flows

### 1. Greenfield Flow

Start from scratch with a blank repository and use spec2gcp agents to build everything:

```
User Idea
    ↓
┌─────────────────┐
│   PM Agent      │ → Creates PRD in specs/prd.md
└────────┬────────┘
         ↓
┌─────────────────┐
│   PM Agent      │ → Breaks PRD into FRDs in specs/features/
└────────┬────────┘
         ↓
┌─────────────────┐
│ DevLead Agent   │ → Reviews for technical completeness
└────────┬────────┘
         ↓
┌─────────────────┐
│ Architect Agent │ → Creates ADRs for key decisions
└────────┬────────┘
         ↓
┌─────────────────┐
│ Planner Agent   │ → Creates implementation plan with diagrams
└────────┬────────┘
         ↓
┌─────────────────┐
│   Dev Agent     │ → Implements features based on specs
└────────┬────────┘
         ↓
┌─────────────────┐
│  GCloud Agent   │ → Deploys to GCP with Terraform and CI/CD
└─────────────────┘
```

### 2. Greenfield with Shell

Start with a **shell** — a pre-configured template repository that includes:

- Technical bootstrapping (backend + frontend scaffold)
- Infrastructure as Code (Terraform)
- Local development setup
- Pre-configured spec2gcp agents, prompts, and MCP servers
- Project-specific AGENTS.md

This accelerates development by providing a working foundation that's already spec-enabled:

```
Choose Shell (e.g., agentic-shell-python)
    ↓
Clone/Initialize
    ↓
Define PRD & FRDs
    ↓
Agents implement on top of shell scaffold
    ↓
Deploy to Google Cloud
```

### 3. Brownfield Flow

Bring spec2gcp to an **existing repository** that wasn't built with specifications:

```
Existing Codebase
    ↓
┌─────────────────────┐
│ Tech-Analyst Agent  │ → Reverse engineers the codebase
│                     │ → Creates technical docs in specs/docs/
│                     │ → Generates FRDs from existing features
└─────────┬───────────┘
          ↓
┌─────────────────────┐
│ Bootstrap AGENTS.md │ → Creates AGENTS.md based on technical context
└─────────┬───────────┘
          ↓
Spec-Enabled Repository
    ↓
Use spec2gcp agents for future development
```

The key outcome: your existing repo now has **proper specifications** and an **AGENTS.md** file to guide coding agents going forward.

---

## The Layer Model

spec2gcp operates in three layers:

```
┌──────────────────────────────────────────────────────────┐
│                    TEMPLATES                              │
│  Ready-to-use project starters with full implementation  │
│  Examples: AI agent starters, Cloud Run apps             │
│  Built using shells + spec2gcp                           │
├──────────────────────────────────────────────────────────┤
│                      SHELLS                               │
│  Technical bootstrapping with scaffolded code + IaC      │
│  • agentic-shell-python (Python + AI agents + GCP)       │
│  • agentic-shell-node (Node.js + AI agents + GCP)        │
│  Includes spec2gcp agents/prompts/MCP config             │
├──────────────────────────────────────────────────────────┤
│                    SPEC2GCP (Base)                        │
│  Core patterns and primitives:                           │
│  • Copilot Agents with defined roles                     │
│  • Structured prompts for workflows                      │
│  • MCP server configurations (GCP + GitHub + context7)  │
│  • Dev container with GCP tooling                        │
└──────────────────────────────────────────────────────────┘
```

### Layer Details

| Layer | Purpose | Examples |
|-------|---------|----------|
| **spec2gcp** | Base patterns, agents, prompts, MCP config | This repository |
| **Shells** | Technical bootstrapping + spec2gcp | agentic-shell-python, agentic-shell-node |
| **Templates** | Complete, ready-to-customize solutions | AI agent starters, Cloud Run templates |

---

## Key Primitives

### 1. Copilot Agents

Predefined agents with specialized roles, available in `.github/agents/`:

| Agent | Role |
|-------|------|
| **spec2gcp** | Orchestrator — routes requests to specialized agents |
| **pm** | Product Manager — creates PRD and FRDs |
| **devlead** | Dev Lead — reviews specs for technical completeness |
| **architect** | Architect — creates ADRs and manages guidelines |
| **planner** | Planner — creates implementation plans and diagrams |
| **dev** | Developer — implements features and manages standards |
| **gcloud** | GCP Specialist — deploys with Terraform and CI/CD |
| **tech-analyst** | Reverse Engineer — analyzes existing codebases |
| **modernizer** | Modernization Strategist — creates upgrade roadmaps |
| **extender** | Extension Strategist — plans new feature additions |

### 2. Prompts

Structured prompts in `.github/prompts/` for common workflows:

- `/prd` — Create a Product Requirements Document
- `/frd` — Break PRD into Feature Requirements Documents
- `/adr` — Create Architecture Decision Records
- `/plan` — Create implementation plans with diagrams
- `/deploy` — Deploy to Google Cloud with Terraform
- `/rev-eng` — Reverse engineer an existing codebase
- `/modernize` — Create a modernization plan
- And more...

### 3. MCP Servers

Model Context Protocol servers configured in `.vscode/mcp.json`:

| Server | Purpose |
|--------|---------|
| **gcloud** | Google Cloud resource management |
| **observability** | Cloud Monitoring, Logging, Tracing |
| **storage** | Cloud Storage operations |
| **analytics** | Google Analytics data access |
| **developer-knowledge** | Google Developer documentation |
| **context7** | Library and framework documentation |
| **github** | GitHub repository operations |
| **deepwiki** | Deep research and analysis |
| **playwright** | Browser automation for testing |

### 4. AGENTS.md

A critical file in each project that provides **coding guidelines** to agents:

- Technology stack and versions
- Project structure conventions
- Coding standards and patterns
- Testing requirements
- Security guidelines

Without AGENTS.md, coding agents lack project-specific context. With it, they generate code that follows your team's standards.

---

## The Agent Ecosystem

```
                        ┌─────────────────┐
                        │    spec2gcp     │
                        │  (Orchestrator) │
                        └────────┬────────┘
                                 │
        ┌────────────────────────┼────────────────────────┐
        │            │           │           │            │
        ▼            ▼           ▼           ▼            ▼
   ┌─────────┐ ┌──────────┐ ┌─────────┐ ┌─────────┐ ┌─────────┐
   │   pm    │ │ devlead  │ │architect│ │ planner │ │   dev   │
   │         │ │          │ │         │ │         │ │         │
   │ PRD/FRD │ │  Review  │ │  ADRs   │ │  Plans  │ │  Code   │
   └─────────┘ └──────────┘ └─────────┘ └─────────┘ └────┬────┘
                                                         │
                                                         ▼
                                                    ┌─────────┐
                                                    │  gcloud │
                                                    │         │
                                                    │ Deploy  │
                                                    └─────────┘

   ┌─────────────────────────────────────────────────────────┐
   │               Brownfield Specialists                    │
   │  ┌──────────────┐              ┌──────────────┐        │
   │  │ tech-analyst │ ──────────▶  │  modernizer  │        │
   │  │              │              │              │        │
   │  │  Analyze     │              │  Modernize   │        │
   │  └──────────────┘              └──────────────┘        │
   └─────────────────────────────────────────────────────────┘
```

### Agent Responsibilities

| Agent | Responsibilities |
|-------|------------------|
| **spec2gcp** | Analyzes user intent, delegates to specialized agents, coordinates multi-agent workflows |
| **pm** | Creates PRD and FRDs, focuses on WHAT not HOW, defines acceptance criteria |
| **devlead** | Reviews specs for technical feasibility, identifies missing requirements, advocates simplicity |
| **architect** | Creates ADRs, researches technology options, maintains architecture standards |
| **planner** | Creates multi-level Mermaid diagrams (L0-L3), breaks down work into tasks, planning only |
| **dev** | Implements features, maintains code standards, follows AGENTS.md guidelines |
| **gcloud** | Deploys to GCP using gcloud CLI and Terraform, sets up Cloud Build CI/CD |
| **tech-analyst** | Reverse engineers existing codebases, extracts specs from code, documents architecture |
| **modernizer** | Analyzes legacy systems, identifies technical debt, creates modernization roadmaps |
| **extender** | Gathers requirements for new features, plans integration with existing systems |

---

## Artifacts Generated

spec2gcp produces a structured set of artifacts:

```
project/
├── specs/
│   ├── prd.md                    # Product Requirements Document
│   ├── features/                 # Feature Requirements Documents
│   │   ├── user-auth.md
│   │   ├── dashboard.md
│   │   └── ...
│   ├── adr/                      # Architecture Decision Records
│   │   ├── 0001-database-choice.md
│   │   ├── 0002-auth-strategy.md
│   │   └── ...
│   ├── tasks/                    # Implementation tasks
│   └── docs/                     # Technical documentation
│       ├── architecture/
│       ├── technology/
│       └── integration/
├── AGENTS.md                     # Coding guidelines for agents
└── infra/                        # Infrastructure as Code (Terraform)
```

| Artifact | Purpose | Created By |
|----------|---------|------------|
| **PRD** | Product vision, goals, success metrics | pm |
| **FRDs** | Feature specs with acceptance criteria | pm |
| **ADRs** | Key technical decisions with rationale | architect |
| **AGENTS.md** | Project-specific coding guidelines | architect / bootstrap |
| **Tasks** | Actionable implementation items | planner / modernizer |
| **Technical Docs** | Architecture, stack, integrations | tech-analyst |
| **Diagrams** | Mermaid diagrams (L0-L3) | planner |
| **Terraform** | GCP infrastructure as code | gcloud |

---

## Technology Foundation

spec2gcp leverages:

| Technology | Purpose |
|------------|---------|
| **GitHub Copilot** | AI-powered code generation and chat |
| **Copilot Agents** | Custom agents with specialized roles |
| **VS Code** | Primary IDE with integrated agent experience |
| **MCP (Model Context Protocol)** | Extended capabilities via GCP and third-party servers |
| **Google Cloud** | Cloud deployment target |
| **gcloud CLI** | Infrastructure provisioning and deployment |
| **Terraform** | Infrastructure as Code |
| **Cloud Build** | CI/CD pipelines on GCP |

---

## Getting Started

### Option 1: Dev Container (Recommended)

The fastest way — everything pre-installed:

1. Clone this repository
2. Open in VS Code → **Reopen in Container**
3. Set GCP env vars locally before opening:
   ```bash
   export GOOGLE_CLOUD_PROJECT="your-project-id"
   export GOOGLE_APPLICATION_CREDENTIALS="/path/to/key.json"
   ```
4. Start with `@spec2gcp create a PRD for [your idea]`

### Option 2: Start with a Shell

1. Choose a shell based on your tech stack:
   - `agentic-shell-python` — Python + AI agents + GCP
   - `agentic-shell-node` — Node.js + AI agents + GCP

2. Clone and open in VS Code
3. Start with `@spec2gcp create a PRD for [your idea]`

### Option 3: Spec-Enable an Existing Repo

1. Copy spec2gcp agents and config to your repo
2. Run `@tech-analyst analyze this codebase`
3. Generate AGENTS.md with the bootstrap prompt
4. Use spec2gcp agents for future development

---

## Key Scenarios

| Scenario | Flow | Key Agents |
|----------|------|------------|
| **New GCP Application** | Greenfield | pm → architect → planner → dev → gcloud |
| **Rapid Prototyping** | Greenfield with Shell | pm → planner → dev → gcloud |
| **Modernization** | Brownfield | tech-analyst → modernizer → planner → dev |
| **Add New Features** | Extension | extender → planner → dev → gcloud |
| **Platform Engineering** | Custom shells/templates | architect → dev |

---

## Learn More

- [INTEGRATION.md](https://github.com/VladimirCro/spec2gcp/blob/main/INTEGRATION.md) — How to integrate spec2gcp into existing projects
- [getting-started.md](getting-started.md) — Step-by-step setup guide
- [workflows.md](workflows.md) — Detailed workflow documentation
- [APM docs](apm.md) — Agent Package Manager for engineering standards
