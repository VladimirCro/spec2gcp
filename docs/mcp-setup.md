# MCP Server Setup

Spec2GCP agents use **MCP (Model Context Protocol) servers** to access external tools and data sources during their work — fetching library documentation, querying GitHub, deploying to Google Cloud, and more. Without MCP servers, agents fall back to their training knowledge only, which is less accurate and up-to-date.

The MCP configuration lives in `.vscode/mcp.json` at the root of the repository and is automatically loaded by GitHub Copilot inside VS Code.

---

## Configured servers

The framework configures **9 MCP servers** out of the box:

| Server | Type | Purpose | Required env variable |
|--------|------|---------|----------------------|
| `context7` | HTTP | Up-to-date library and framework documentation | — |
| `github` | HTTP | GitHub repository access, issue management, PR creation | `GITHUB_COPILOT_PAT` |
| `deepwiki` | HTTP | Deep analysis of GitHub repositories and codebases | — |
| `playwright` | npx | Browser automation for E2E testing and web scraping | — |
| `gcloud` | npx | Google Cloud CLI operations from agent context | `GOOGLE_APPLICATION_CREDENTIALS` |
| `observability` | npx | Cloud Monitoring, Cloud Logging, Cloud Trace access | `GOOGLE_APPLICATION_CREDENTIALS` |
| `storage` | npx | Cloud Storage operations (read, write, manage buckets) | `GOOGLE_APPLICATION_CREDENTIALS` |
| `analytics` | pipx | BigQuery and Analytics Hub access | `GOOGLE_APPLICATION_CREDENTIALS`, `GOOGLE_CLOUD_PROJECT` |
| `developer-knowledge` | HTTP | Google Developer Knowledge base (APIs, GCP products) | `GOOGLE_DEVELOPER_KNOWLEDGE_API_KEY` |

---

## How MCP servers connect

There are three connection types used in `.vscode/mcp.json`:

### HTTP servers (remote, no install needed)
These connect to hosted MCP endpoints over HTTPS. GitHub Copilot connects to them directly — no local process required.

```json
"context7": {
  "type": "http",
  "url": "https://mcp.context7.com/mcp"
}
```

Servers using this type: `context7`, `github`, `deepwiki`, `developer-knowledge`

### npx servers (local, Node.js required)
These run as local processes started with `npx`. Node.js must be installed (the dev container includes it).

```json
"gcloud": {
  "command": "npx",
  "args": ["-y", "@google-cloud/gcloud-mcp"]
}
```

Servers using this type: `playwright`, `gcloud`, `observability`, `storage`

### pipx servers (local, Python required)
These run as local processes started with `pipx`. The dev container installs `pipx` via `postCreateCommand`.

```json
"analytics": {
  "command": "pipx",
  "args": ["run", "analytics-mcp"],
  "env": { ... }
}
```

Servers using this type: `analytics`

---

## Setup inside the dev container

If you are using the dev container (recommended), MCP servers that require Node.js or pipx work automatically — all dependencies are pre-installed.

The only manual steps are setting environment variables **before** opening the container:

```bash
# Add to ~/.bashrc or ~/.zshrc on your host machine

# Required for GitHub MCP (github/*)
export GITHUB_COPILOT_PAT="ghp_..."

# Required for GCloud, Observability, Storage, Analytics MCPs
export GOOGLE_APPLICATION_CREDENTIALS="/path/to/service-account.json"
export GOOGLE_CLOUD_PROJECT="your-gcp-project-id"

# Required for Developer Knowledge MCP
export GOOGLE_DEVELOPER_KNOWLEDGE_API_KEY="AIza..."
```

> The dev container reads these from your host shell via `localEnv`. A `.env` file in the repo root is NOT picked up automatically.

---

## Setup without the dev container

If you are not using the dev container, install the dependencies manually:

```bash
# Node.js (required for npx servers)
# Install from https://nodejs.org or via nvm

# pipx (required for analytics MCP)
pip install --user pipx
pipx ensurepath

# Verify Node.js and npx are available
node --version
npx --version

# Verify pipx is available
pipx --version
```

The `.vscode/mcp.json` file is already present in the repository — VS Code and GitHub Copilot will pick it up automatically when you open the project.

---

## Verifying MCP servers are connected

1. Open VS Code with the project
2. Open Copilot Chat (`Ctrl+Shift+I`)
3. Click the **Tools** icon (plug icon) in the chat toolbar
4. You should see all 9 MCP servers listed and connected

If a server shows as disconnected:

| Server | Common cause | Fix |
|--------|-------------|-----|
| `github` | Missing or expired PAT | Regenerate `GITHUB_COPILOT_PAT` in GitHub Settings |
| `gcloud` | No Node.js | Install Node.js or open in dev container |
| `analytics` | Missing pipx | Run `pip install --user pipx && pipx ensurepath` |
| `developer-knowledge` | Missing API key | Set `GOOGLE_DEVELOPER_KNOWLEDGE_API_KEY` in shell |
| `gcloud`, `observability`, `storage` | No GCP auth | Run `gcloud auth application-default login` |

---

## Which agents use which servers

| MCP server | Used by |
|------------|---------|
| `context7` | architect, planner, dev, modernizer, extender |
| `github` | dev, planner, spec2gcp orchestrator, delegate workflow |
| `deepwiki` | architect, planner, dev, modernizer |
| `playwright` | dev (E2E testing), tech-analyst |
| `gcloud` | gcloud agent, deploy workflow |
| `observability` | gcloud agent (monitoring setup) |
| `storage` | gcloud agent (Cloud Storage operations) |
| `analytics` | gcloud agent (BigQuery, Analytics Hub) |
| `developer-knowledge` | architect, gcloud agent (GCP product documentation) |

---

## Optional: Developer Knowledge API key

The `developer-knowledge` MCP gives agents access to official Google API documentation and GCP product knowledge. It is optional — agents will use `context7` and `deepwiki` as fallbacks.

To enable it:
1. Go to [Google Cloud Console → APIs & Services](https://console.cloud.google.com/apis)
2. Search for **Developer Knowledge API** and enable it
3. Create an API key and restrict it to this API
4. Add it to your shell: `export GOOGLE_DEVELOPER_KNOWLEDGE_API_KEY="AIza..."`

Back to [docs index](index.md).
