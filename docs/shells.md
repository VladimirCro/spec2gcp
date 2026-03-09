# Shell baselines (Greenfield)

Spec2GCP supports a "shell-first" greenfield approach: start from a predefined baseline ("shell"), then use coding agents to translate natural language requirements into the missing code, wiring, and configuration.

## What is a shell?

A shell is a pre-built project scaffold that gives agents a working skeleton to build on. Rather than generating boilerplate from scratch (which agents can get wrong or inconsistently), a shell provides:

| Component | What the shell includes |
|-----------|------------------------|
| **Project structure** | Monorepo layout, folder conventions, module boundaries |
| **CI/CD config** | GitHub Actions workflows for lint, test, build, deploy |
| **Dev container** | `.devcontainer/` with Python, Node, gcloud CLI, Docker-in-Docker |
| **MCP config** | Pre-configured MCP servers (context7, deepwiki, github, gcloud) |
| **IaC scaffold** | Terraform modules for Cloud Run, IAM, and Secret Manager |
| **Standards** | `AGENTS.md` stubs and `/standards/` baseline guidelines |
| **Test harness** | Pytest/Vitest setup with coverage reporting |

Agents then fill in only the application-specific logic—endpoints, UI components, domain models, and business rules—so they focus entirely on your requirements rather than project wiring.

## Available shells

- **Agentic Python shell** — FastAPI + Cloud Run + React frontend baseline
  Repository: https://github.com/VladimirCro/agentic-shell-python
- More GCP-specific shells can be created using spec2gcp as the base

## Shell vs blank project

| Scenario | Recommended approach |
|----------|---------------------|
| New application on GCP | **Use a shell** — faster start, agents skip boilerplate |
| Existing codebase you want to modernize | **Blank project** — run `/rev-eng` instead |
| Proof-of-concept or spike | **Blank project** — shell overhead may not be worth it |
| Production application from scratch | **Use a shell** — CI/CD and IaC come pre-wired |

## How to use shells with agents

1. **Pick a shell** that matches your language/runtime from the list above.
2. **Clone the shell repo** as your new project's starting point:
   ```bash
   git clone https://github.com/VladimirCro/agentic-shell-python my-app
   cd my-app
   git remote set-url origin https://github.com/YOUR_ORG/my-app
   ```
3. **Open in dev container** — the shell's `.devcontainer/` has all tools pre-installed.
4. **Run the Spec2GCP greenfield workflow** — the shell is ready for agents:
   ```
   /prd → /frd → /generate-agents → /plan → /implement → /deploy
   ```
5. **Let agents fill the gaps** — they implement endpoints, UI, storage, tests, and deployment configs against the shell's existing structure.

## Creating a custom shell

If your team has a preferred stack not covered by existing shells, create a custom shell using spec2gcp as the base:

1. Scaffold your preferred project structure (e.g., Django + Vue, Go + Cloud Run)
2. Add a `.devcontainer/` with required tools
3. Add `.github/` with standard CI/CD workflows
4. Add a minimal `AGENTS.md` or run `/generate-agents` to populate it
5. Publish as a template repository so teams can clone it as a starting point

Back to [docs index](index.md).
