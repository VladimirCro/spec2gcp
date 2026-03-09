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
git clone https://github.com/VladimirCro/spec2gcp.git
cd spec2gcp
```

### 2. Open in Dev Container (Recommended)

The easiest way to get started is using the included dev container — all tools are pre-installed:

1. Open the folder in VS Code
2. Press `F1` → **Dev Containers: Reopen in Container**
3. Wait for the container to build (~2-3 min on first run)

The container includes: Python 3.12, Node.js, gcloud CLI, Docker-in-Docker, pipx, and all MCP servers.

### 3. Configure Environment Variables

The dev container reads environment variables from your **local shell** — set them before opening the container. Add these to your `~/.bashrc`, `~/.zshrc`, or equivalent:

```bash
# Required: GitHub tokens
export GITHUB_COPILOT_PAT="ghp_..."        # GitHub PAT with Copilot access
export GITHUB_APM_PAT="ghp_..."            # GitHub PAT for APM package manager

# Required: Google Cloud
export GOOGLE_CLOUD_PROJECT="my-project-id"  # Your GCP project ID

# Optional: for local gcloud auth via service account
export GOOGLE_APPLICATION_CREDENTIALS="/path/to/service-account.json"

# Optional: Google Developer Knowledge MCP
export GOOGLE_DEVELOPER_KNOWLEDGE_API_KEY="AIza..."
```

**How to get each value:**

| Variable | Where to get it |
|----------|----------------|
| `GITHUB_COPILOT_PAT` | GitHub → Settings → Developer settings → Personal access tokens → New token. Scopes: `repo`, `copilot` |
| `GITHUB_APM_PAT` | Same as above, or reuse the same token. Scope: `read:packages` |
| `GOOGLE_CLOUD_PROJECT` | Run `gcloud projects list` or find it in [GCP Console](https://console.cloud.google.com) |
| `GOOGLE_APPLICATION_CREDENTIALS` | Create a service account in GCP IAM, download the JSON key. For local dev, `gcloud auth application-default login` is simpler (no file needed) |
| `GOOGLE_DEVELOPER_KNOWLEDGE_API_KEY` | [Google Cloud Console → APIs → Developer Knowledge API](https://console.cloud.google.com) — only needed for the developer-knowledge MCP server |

> **Note**: The dev container uses `localEnv` to read from your host shell — a `.env` file in the repo root will NOT be picked up automatically by the container.

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

## Required GCP IAM Roles

The service account used by GitHub Actions (or your local credentials) needs these IAM roles to deploy with Spec2GCP workflows:

| Role | Purpose |
|------|---------|
| `roles/run.admin` | Deploy and manage Cloud Run services |
| `roles/iam.serviceAccountUser` | Allow Cloud Run to use a runtime service account |
| `roles/artifactregistry.writer` | Push container images to Artifact Registry |
| `roles/secretmanager.secretAccessor` | Read secrets at deploy time |
| `roles/storage.admin` | Manage Cloud Storage buckets (Terraform state, static assets) |
| `roles/iam.workloadIdentityUser` | Authenticate via Workload Identity Federation |
| `roles/serviceusage.serviceUsageConsumer` | Enable and use GCP APIs |
| `roles/cloudbuild.builds.editor` | Trigger Cloud Build jobs (if using Cloud Build) |

Grant the minimum set of roles needed for your deployment. For a basic Cloud Run deployment, `run.admin` + `iam.serviceAccountUser` + `artifactregistry.writer` is usually sufficient.

```bash
# Grant roles to the GitHub Actions service account
SA_EMAIL="github-actions-sa@YOUR_PROJECT_ID.iam.gserviceaccount.com"
PROJECT="YOUR_PROJECT_ID"

for ROLE in roles/run.admin roles/iam.serviceAccountUser roles/artifactregistry.writer roles/secretmanager.secretAccessor; do
  gcloud projects add-iam-policy-binding $PROJECT \
    --member="serviceAccount:$SA_EMAIL" \
    --role=$ROLE
done
```

## Next Steps

- Learn about the [Benefits](benefits.md) of Spec2GCP
- Read the full [Workflow Guide](workflows.md)
- Check the [SPEC2GCP.md](https://github.com/VladimirCro/spec2gcp/blob/main/SPEC2GCP.md) template metadata
