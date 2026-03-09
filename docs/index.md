# Spec2GCP Documentation

**Spec2GCP** is an AI-powered multi-agent framework that turns natural language specifications into production-ready applications on Google Cloud. You describe what you want to build — specialized agents handle requirements, architecture, planning, implementation, and deployment.

---

## Two ways to start

### Greenfield — new project from scratch
```
/prd → /frd → /generate-agents → /plan → /implement or /delegate → /deploy
```
Start with a product idea. Agents write the spec, break it into features, create tasks, implement code, and deploy to Cloud Run with Terraform-generated infrastructure.

### Brownfield — existing codebase
```
/rev-eng → /modernize or /extend → /plan → /deploy
```
Start with an existing codebase. The reverse engineering agent documents what exists, then you choose to improve quality (`/modernize`) or add new features (`/extend`).

---

## Agents

| Agent | Slash command | What it does |
|-------|---------------|--------------|
| `@spec2gcp` | — | Orchestrator — routes all requests to the right agent |
| `@pm` | `/prd`, `/frd` | Product Manager — writes PRDs and FRDs |
| `@devlead` | — | Dev Lead — reviews specs for technical feasibility |
| `@architect` | `/adr`, `/generate-agents` | Architect — creates ADRs and synthesizes AGENTS.md |
| `@planner` | — | Planner — produces L0-L3 architectural diagrams |
| `@dev` | `/plan`, `/implement`, `/delegate` | Developer — breaks down and implements features |
| `@gcloud` | `/deploy` | GCP Specialist — deploys with Terraform and GitHub Actions |
| `@tech-analyst` | `/rev-eng` | Analyst — reverse engineers existing codebases |
| `@modernizer` | `/modernize` | Modernizer — plans technical debt reduction |
| `@extender` | `/extend` | Extender — plans new feature additions |

---

## Documentation

### Getting started
- [Getting Started](getting-started.md) — prerequisites, dev container setup, environment variables, GCP IAM roles
- [MCP Server Setup](mcp-setup.md) — configure the 9 MCP servers that agents use for research and cloud access
- [Shell baselines](shells.md) — pre-built project scaffolds for faster greenfield starts

### Workflows & architecture
- [Workflows (Greenfield + Brownfield)](workflows.md) — step-by-step diagrams for both paths
- [Architecture (Dev Container, MCP, Agents)](architecture.md) — how the system is assembled
- [Generated specs layout (`specs/`)](specs-structure.md) — folder structure created by agents

### Standards & configuration
- [Managing standards with APM](apm.md) — install and version engineering standards
- [Example usage](examples.md) — complete greenfield and brownfield walkthroughs
- [Key benefits](benefits.md) — what Spec2GCP gives you over manual development

---

## Related guides

- [Integration & installation guide](https://github.com/VladimirCro/spec2gcp/blob/main/INTEGRATION.md)
- [Contributing](contributing.md)
