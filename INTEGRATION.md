# Spec2GCP Integration Guide

This guide explains how to integrate spec2gcp into your existing projects to enable AI-powered development workflows.

## 🎯 What is Spec2GCP?

Spec2GCP is a collection of specialized GitHub Copilot agents and workflows that transform how you build software on Google Cloud:

- **Greenfield**: Turn product ideas into production-ready applications
- **Brownfield**: Reverse engineer existing codebases into comprehensive documentation

## 📦 Installation Methods

### Method 1: Quick Install (Recommended)

One-line installation from GitHub releases:

```bash
# Full installation (agents, prompts, devcontainer, MCP)
curl -fsSL https://raw.githubusercontent.com/VladimirCro/spec2gcp/main/scripts/quick-install.sh | bash

# Minimal installation (agents and prompts only)
curl -fsSL https://raw.githubusercontent.com/VladimirCro/spec2gcp/main/scripts/quick-install.sh | bash -s -- --minimal

# Install to specific directory
curl -fsSL https://raw.githubusercontent.com/VladimirCro/spec2gcp/main/scripts/quick-install.sh | bash -s -- --target /path/to/project
```

### Method 2: Manual Download

Download and extract manually:

```bash
# Download latest release
curl -L https://github.com/VladimirCro/spec2gcp/releases/latest/download/spec2gcp-full-latest.zip -o spec2gcp.zip

# Extract
unzip spec2gcp.zip -d spec2gcp

# Run installer
cd spec2gcp
./scripts/install.sh --full

# Or on Windows
.\scripts\install.ps1 -Full
```

### Method 3: GitHub Release Download

1. Visit [Releases](https://github.com/VladimirCro/spec2gcp/releases)
2. Download the desired package:
   - `spec2gcp-full-*.zip` - Complete package with all features
   - `spec2gcp-minimal-*.zip` - Agents and prompts only
3. Extract and run the installer

## 🔧 Installation Options

### Full Installation

Includes everything:
- ✅ 10 specialized AI agents
- ✅ 12 workflow prompts
- ✅ MCP server configuration
- ✅ Dev container setup
- ✅ APM configuration
- ✅ Directory structure templates

```bash
# Linux/Mac
./scripts/install.sh --full

# Windows
.\scripts\install.ps1 -Full
```

### Minimal Installation

Includes only:
- ✅ 10 specialized AI agents
- ✅ 12 workflow prompts

```bash
# Linux/Mac
./scripts/install.sh --agents-only

# Windows
.\scripts\install.ps1 -AgentsOnly
```

### Installation Flags

| Flag | Description |
|------|-------------|
| `--full` / `-Full` | Install all components |
| `--agents-only` / `-AgentsOnly` | Install only agents and prompts |
| `--merge` / `-Merge` | Merge with existing files (default) |
| `--force` / `-Force` | Overwrite without prompting |
| `--no-color` / `-NoColor` | Disable colored output |

## 📁 What Gets Installed

### Directory Structure

After installation, your project will have:

```
your-project/
├── .github/
│   ├── agents/              # 10 specialized AI agents
│   │   ├── architect.agent.md
│   │   ├── gcloud.agent.md
│   │   ├── dev.agent.md
│   │   ├── devlead.agent.md
│   │   ├── extender.agent.md
│   │   ├── modernizer.agent.md
│   │   ├── planner.agent.md
│   │   ├── pm.agent.md
│   │   ├── spec2gcp.agent.md
│   │   └── tech-analyst.agent.md
│   └── prompts/             # 12 workflow prompts
│       ├── adr.prompt.md
│       ├── bootstrap-agents.prompt.md
│       ├── delegate.prompt.md
│       ├── deploy.prompt.md
│       ├── extend.prompt.md
│       ├── frd.prompt.md
│       ├── generate-agents.prompt.md
│       ├── implement.prompt.md
│       ├── modernize.prompt.md
│       ├── plan.prompt.md
│       ├── prd.prompt.md
│       └── rev-eng.prompt.md
├── .vscode/
│   └── mcp.json             # MCP server configuration (full install)
├── .devcontainer/
│   └── devcontainer.json    # Dev container config (full install)
├── specs/                   # Documentation will be generated here
│   ├── features/
│   ├── tasks/
│   └── docs/
└── apm.yml                  # APM configuration (full install)
```

## 🔄 Integration Scenarios

### Scenario 1: New Project

Starting fresh? Install spec2gcp and start building:

```bash
mkdir my-new-project
cd my-new-project
git init

# Install spec2gcp
curl -fsSL https://raw.githubusercontent.com/VladimirCro/spec2gcp/main/scripts/quick-install.sh | bash

# Open in VS Code
code .

# Start with /prd workflow
```

### Scenario 2: Existing Codebase (Documentation Needed)

Have existing code but no documentation? Use brownfield workflows:

```bash
cd my-existing-project

# Install spec2gcp
curl -fsSL https://raw.githubusercontent.com/VladimirCro/spec2gcp/main/scripts/quick-install.sh | bash

# Open in VS Code
code .

# Reverse engineer your codebase
# Use /rev-eng workflow
```

### Scenario 3: Active Project (Non-Destructive)

Installing into an active project? Spec2GCP respects existing files:

```bash
cd my-active-project

# Install with merge mode (default)
curl -fsSL https://raw.githubusercontent.com/VladimirCro/spec2gcp/main/scripts/quick-install.sh | bash

# Existing .github files preserved
# New agents/prompts added
# Conflicting configs saved as *.spec2gcp for manual merge
```

## ⚙️ Configuration

### MCP Servers

If you have existing `.vscode/mcp.json`, the installer will:
1. Save spec2gcp's MCP config as `mcp.json.spec2gcp`
2. Allow you to manually merge configurations

Example merge:

```json
{
  "servers": {
    "my-existing-server": {
      "command": "node",
      "args": ["server.js"]
    },
    "gcloud": {
      "command": "npx",
      "args": ["-y", "@google-cloud/gcloud-mcp"]
    },
    "observability": {
      "command": "npx",
      "args": ["-y", "@google-cloud/observability-mcp"]
    },
    "storage": {
      "command": "npx",
      "args": ["-y", "@google-cloud/storage-mcp"]
    },
    "context7": {
      "type": "http",
      "url": "https://mcp.context7.com/mcp"
    },
    "github": {
      "type": "http",
      "url": "https://api.githubcopilot.com/mcp/"
    }
  }
}
```

### Dev Container

If you have existing `.devcontainer/devcontainer.json`, the installer will:
1. Save spec2gcp's config as `devcontainer.json.spec2gcp`
2. Allow you to manually merge configurations

Key features to consider merging:
- Python 3.12, Node.js, Google Cloud CLI
- Docker-in-Docker support
- GitHub Copilot extensions
- Google Cloud Code and AI Toolkit extensions

### Environment Variables

The dev container expects these variables set in your local environment:

```bash
export GOOGLE_CLOUD_PROJECT="your-gcp-project-id"
export GOOGLE_APPLICATION_CREDENTIALS="/path/to/service-account.json"
export GOOGLE_DEVELOPER_KNOWLEDGE_API_KEY="your-api-key"  # optional
```

### APM Configuration

If you have existing `apm.yml`, the installer will skip creating a new one.

To use spec2gcp standards:

```yaml
dependencies:
  apm:
    - VladimirCro/spec2gcp-guidelines
    - VladimirCro/spec2gcp-guidelines-backend
    - VladimirCro/spec2gcp-guidelines-frontend
```

Then run:
```bash
apm install
apm compile
```

## 🚀 Using Spec2GCP

### Greenfield Workflows

For new features and projects:

1. **`/prd`** - Create Product Requirements Document
   - Describes the product vision, goals, and requirements

2. **`/frd`** - Create Feature Requirements Documents
   - Breaks down PRD into individual features

3. **`/generate-agents`** (Optional) - Generate Agent Guidelines
   - Consolidates engineering standards from `standards/` directory

4. **`/plan`** - Create Technical Task Breakdown
   - Translates features into implementation tasks

5. **`/implement`** - Implement Features Locally
   - Dev agent writes code directly

6. **`/delegate`** - Delegate to GitHub Copilot
   - Creates GitHub issues for Copilot Coding Agent

7. **`/deploy`** - Deploy to Google Cloud
   - Generates Terraform IaC and CI/CD pipelines

### Brownfield Workflows

For existing codebases:

1. **`/rev-eng`** - Reverse Engineer Codebase
   - Analyzes code and creates documentation
   - Generates tasks, features, and product vision

2. **`/modernize`** (Optional) - Create Modernization Plan
   - Assesses technical debt and upgrade opportunities

3. **`/plan`** (Optional) - Implement Modernization
   - Executes modernization tasks

4. **`/deploy`** (Optional) - Deploy to Google Cloud
   - Deploys modernized application

## 🔍 Troubleshooting

### Issue: Agents Not Showing in Copilot Chat

**Solution**: Reload VS Code window
1. Press `Ctrl+Shift+P` (Windows/Linux) or `Cmd+Shift+P` (Mac)
2. Type "Reload Window"
3. Press Enter

### Issue: MCP Servers Not Loading

**Solution**: Check MCP configuration
1. Open `.vscode/mcp.json`
2. Verify server configurations
3. Check that required tools are installed (Node.js, npx, pipx)
4. Restart VS Code

### Issue: GCP Authentication Errors

**Solution**: Set up Application Default Credentials
```bash
# Inside dev container
gcloud auth application-default login

# Or point to service account key
export GOOGLE_APPLICATION_CREDENTIALS="/path/to/key.json"
```

### Issue: Installation Script Permission Denied

**Solution**: Make script executable
```bash
chmod +x scripts/install.sh
./scripts/install.sh --full
```

### Issue: Conflicting Configuration Files

**Solution**: Manually merge `.spec2gcp` files
1. Find `*.spec2gcp` files in your project
2. Compare with your existing configurations
3. Merge desired settings
4. Delete `.spec2gcp` files after merging

### Issue: APM Not Found

**Solution**: Install APM
```bash
# Install APM
pip install apm-cli
```

### Issue: Prompts Not Working

**Solution**: Verify file structure
```bash
# Check agents
ls .github/agents/*.agent.md

# Check prompts
ls .github/prompts/*.prompt.md

# Should see 10 agents and 12 prompts
```

## 📊 Verification

After installation, verify everything is working:

```bash
# 1. Check file structure
tree .github/

# 2. Count installed components
find .github/agents -name "*.agent.md" | wc -l   # Should be 10
find .github/prompts -name "*.prompt.md" | wc -l  # Should be 12

# 3. Open in VS Code
code .

# 4. Open GitHub Copilot Chat
# Press Ctrl+Shift+I (Windows/Linux) or Cmd+Shift+I (Mac)

# 5. Type @ and verify agents appear
# Should see: @spec2gcp, @pm, @devlead, @architect, @planner, @dev, @gcloud, @tech-analyst, @modernizer, @extender

# 6. Type / and verify prompts appear
# Should see: /prd, /frd, /plan, /implement, /deploy, /delegate, /rev-eng, /modernize, /extend, /adr, etc.
```

## 🔄 Updating Spec2GCP

To update to a newer version:

```bash
# Re-run quick install with desired version
curl -fsSL https://raw.githubusercontent.com/VladimirCro/spec2gcp/main/scripts/quick-install.sh | bash -s -- --version v1.1.0

# Or download and run installer with --force
./scripts/install.sh --full --force
```

## 🗑️ Uninstalling

To remove spec2gcp:

```bash
# Remove agents and prompts
rm -rf .github/agents
rm -rf .github/prompts

# Remove specs directory (be careful - may contain your work!)
# Only if you want to remove generated documentation
rm -rf specs/

# Remove configurations (if no conflicts)
rm .vscode/mcp.json
rm .devcontainer/devcontainer.json
rm apm.yml

# Remove any .spec2gcp backup files
find . -name "*.spec2gcp" -delete
```

## 💡 Best Practices

### 1. Start Small
- Begin with minimal installation
- Test workflows on a small feature
- Upgrade to full installation if needed

### 2. Document as You Go
- Use `/prd` before coding new features
- Run `/rev-eng` on inherited code
- Keep specs/ directory in version control

### 3. Leverage Standards
- Install APM packages for your tech stack
- Run `apm compile` to generate `AGENTS.md`
- Agents will follow your standards

### 4. Use Dev Container
- Consistent environment across team
- All tools pre-installed
- MCP servers configured

### 5. Version Control
- Commit `.github/agents` and `.github/prompts`
- Commit `specs/` directory
- Include `apm.yml` and `AGENTS.md`
- Add `.spec2gcp` to `.gitignore`

## 📚 Additional Resources

- **Main Documentation**: [README.md](README.md)
- **Workflow Guide**: [SPEC2GCP.md](SPEC2GCP.md)
- **GitHub Repository**: https://github.com/VladimirCro/spec2gcp
- **APM Documentation**: https://github.com/danielmeppiel/apm
- **GitHub Copilot**: https://github.com/features/copilot

## 🤝 Support

Need help?

1. **Check Documentation**: Start with README.md and this guide
2. **GitHub Issues**: Report bugs or request features
3. **GitHub Discussions**: Ask questions and share experiences

## 📝 License

See [LICENSE.md](LICENSE.md) for details.

---

**Ready to transform your development workflow?** Install spec2gcp and start building!
