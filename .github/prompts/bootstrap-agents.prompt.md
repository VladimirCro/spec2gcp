---
agent: devlead
---
# Bootstrap Project Agents

You are the Dev Lead Agent. Your task is to bootstrap the agent guidelines for this project by installing the spec2gcp engineering standards and generating the `AGENTS.md` file.

## What This Does

This workflow installs the spec2gcp engineering standards (via APM) and compiles them into a single `AGENTS.md` file that all agents in this project will automatically follow.

## Steps

### Step 1: Verify Prerequisites

Check that `apm.yml` exists in the project root:
- If it exists: proceed to Step 2
- If it does NOT exist: create it using `templates/apm.yml.template` as the base, then ask the user to customize it before proceeding

### Step 2: Install APM CLI (if not installed)

Run in terminal:
```bash
npm install -g @apm-cli/apm
```

Verify installation:
```bash
apm --version
```

### Step 3: Install Dependencies

Run in terminal:
```bash
apm install
```

This downloads all packages listed in `apm.yml` into `apm_modules/`.

### Step 4: Compile AGENTS.md

Run in terminal:
```bash
apm compile
```

This reads all installed standards and generates:
- `AGENTS.md` — consolidated agent guidelines (auto-injected to all Copilot agents)
- `CLAUDE.md` — same content, for Claude Code users

### Step 5: Verify Output

Check that `AGENTS.md` was created successfully:
- It should be in the project root
- It should contain sections for: General Engineering, Backend, Frontend standards (based on what's in `apm.yml`)
- If the file is missing or empty, check `apm.yml` for correct dependency sources

### Step 6: Commit Results

Add `apm.yml` and `apm.lock` to version control (do NOT commit `AGENTS.md` or `CLAUDE.md` — they are auto-generated and listed in `.gitignore`):

```bash
git add apm.yml apm.lock
git commit -m "chore: bootstrap agent guidelines with APM"
```

## After Bootstrapping

All agents in `.github/agents/` will now automatically follow the standards defined in `AGENTS.md`. You do not need to manually reference it — GitHub Copilot injects it automatically.

To update standards in the future:
```bash
apm install   # pulls latest versions
apm compile   # regenerates AGENTS.md
```

To add more standards packages, edit `apm.yml` and re-run the above commands.
