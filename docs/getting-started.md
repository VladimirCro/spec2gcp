# Getting Started

This guide will help you get started with Spec2GCP.

## Prerequisites

Before you begin, ensure you have:

- Python 3.10 or higher
- Node.js 18 or higher
- Google Cloud project (for deployment)
- [gcloud CLI](https://cloud.google.com/sdk/docs/install) installed and authenticated
- Git
- VS Code with GitHub Copilot

## Installation

### 1. Clone the Repository

```bash
git clone https://github.com/YOUR_ORG/spec2gcp.git
cd spec2gcp
```

### 2. Open in Dev Container (Recommended)

The easiest way to get started is using the included dev container — all tools are pre-installed:

1. Open the folder in VS Code
2. Press `F1` → **Dev Containers: Reopen in Container**
3. Wait for the container to build (~2-3 min on first run)

The container includes: Python 3.12, Node.js, gcloud CLI, Docker-in-Docker, pipx, and all MCP servers.

### 3. Configure Environment Variables

Set these variables in your local shell **before** opening the dev container:

```bash
export GOOGLE_CLOUD_PROJECT="your-gcp-project-id"
export GOOGLE_APPLICATION_CREDENTIALS="/path/to/service-account.json"
export GOOGLE_DEVELOPER_KNOWLEDGE_API_KEY="your-api-key"  # optional
```

Or create a `.env` file in the root directory:

```env
GOOGLE_CLOUD_PROJECT=your-project-id
GOOGLE_APPLICATION_CREDENTIALS=path/to/service-account.json
GOOGLE_DEVELOPER_KNOWLEDGE_API_KEY=your-api-key
```

### 4. Authenticate with Google Cloud

Inside the dev container:

```bash
# Authenticate for local development
gcloud auth login
gcloud auth application-default login
gcloud config set project $GOOGLE_CLOUD_PROJECT
```

## Running the Workflows

Once inside VS Code with GitHub Copilot:

1. Open Copilot Chat (`Ctrl+Shift+I`)
2. Type `@spec2gcp` to start the orchestrator agent
3. Or use a workflow prompt directly (e.g., `/prd`, `/rev-eng`)

### Available Agents

| Agent | Purpose |
|-------|---------|
| `@spec2gcp` | Main orchestrator — routes to the right agent |
| `@pm` | Product Manager — creates PRDs |
| `@architect` | Solution Architect — designs the system |
| `@planner` | Technical Planner — breaks work into tasks |
| `@dev` | Developer — implements features |
| `@gcloud` | GCP Specialist — deploys to Google Cloud |
| `@devlead` | Dev Lead — reviews and coordinates |
| `@tech-analyst` | Tech Analyst — reverse engineers codebases |
| `@modernizer` | Modernizer — plans technical debt reduction |
| `@extender` | Extender — plans new feature additions |

## Next Steps

- Learn about the [Benefits](benefits.md) of Spec2GCP
- Read the full [Workflow Guide](workflows.md)
- Check the [SPEC2GCP.md](https://github.com/VladimirCro/spec2gcp/blob/main/SPEC2GCP.md) template metadata
