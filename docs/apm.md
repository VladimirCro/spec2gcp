# Managing standards with APM

This repository uses **[APM (Agent Package Manager)](https://github.com/danielmeppiel/apm)** for managing engineering standards. Install it with:

```bash
pip install apm-cli
```

APM provides:

- ✅ **Zero-config setup** - `apm install` reads `apm.yml` and installs all dependencies
- ✅ **Semantic versioning** - Lock to specific versions or use latest
- ✅ **Automatic AGENTS.md** - `apm compile` generates guardrails from all packages
- ✅ **Mix any standards** - Combine Google, community, and custom packages
- ✅ **One-command updates** - `apm update` to get latest standards

## Built-in standards

By default, spec2gcp includes three packages:

- **[spec2gcp-guidelines](https://github.com/VladimirCro/spec2gcp-guidelines)** - General engineering, documentation, agent-first patterns, CI/CD, security
- **[spec2gcp-guidelines-backend](https://github.com/VladimirCro/spec2gcp-guidelines-backend)** - Backend standards (Python and .NET)
- **[spec2gcp-guidelines-frontend](https://github.com/VladimirCro/spec2gcp-guidelines-frontend)** - Frontend standards (React/Next.js)

## Adding more standards

Edit `apm.yml` to add technology-specific standards:

```yaml
dependencies:
  apm:
    - VladimirCro/spec2gcp-guidelines
    - VladimirCro/spec2gcp-guidelines-backend
    - VladimirCro/spec2gcp-guidelines-frontend
    - your-org/your-custom-standards  # Add your own standards
```

Then run:

```bash
apm install  # Install new packages
apm compile  # Regenerate AGENTS.md with new standards
```

## Creating custom standards

Create your own APM package:

```bash
my-standards/
├── apm.yml
├── README.md
└── .apm/
    └── instructions/
        ├── api-design.instructions.md
        ├── database-patterns.instructions.md
        └── security-rules.instructions.md
```

Install from any GitHub repo:

```bash
# Public repo
apm install your-org/your-standards

# Private repo (requires GITHUB_APM_PAT)
export GITHUB_APM_PAT=your_token
apm install your-org/private-standards
```

## Updating standards

```bash
# Update all packages to latest
apm update

# Update specific package
apm update VladimirCro/spec2gcp-guidelines

# Regenerate AGENTS.md after updates
apm compile
```

Back to [docs index](index.md).
