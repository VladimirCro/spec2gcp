# Example usage

## Greenfield example (new project)

```bash
# Start with your product idea
"I want to create a smart AI agent for elderly care that tracks vitals and alerts caregivers"

# Step 1: Create the PRD
/prd

# Step 2: Break down into features
/frd

# Step 3: Generate agent guidelines from standards (optional, can defer)
/generate-agents

# Step 4: Create technical plans
/plan

# Step 5a: Implement locally
/implement

# OR Step 5b: Delegate to GitHub Copilot
/delegate

# Step 6: Deploy to Google Cloud
/deploy
```

## Brownfield example (existing project)

```bash
# You have an existing codebase with minimal or outdated documentation
"I inherited a marketing campaign management app built in Python/React"

# Step 1: Reverse engineer and document the existing codebase
/rev-eng
# @tech-analyst analyzes the codebase (Python FastAPI backend, React frontend)
# Creates specs/features/ with honest documentation of what exists
# Creates specs/docs/ with architecture, stack, security, API documentation
# Notes gaps: "Email service - stub only, not fully implemented"

# Step 2a: Improve existing code quality (choose ONE path)
/modernize
# @modernizer creates modernization strategy in specs/modernize/
# Identifies technical debt, security issues, outdated dependencies
# Creates actionable tasks in specs/tasks/

# OR Step 2b: Add new features to the existing system
/extend
# @extender gathers requirements from you for new capabilities
# Creates FRDs in specs/features/ for new features
# Creates extension strategy and tasks in specs/tasks/

# Step 3: Break down and implement tasks
/plan   # @dev reviews tasks and confirms breakdown
/implement  # OR /delegate to GitHub Copilot

# Step 4: Deploy evolved application to Google Cloud
/deploy
# @gcloud generates updated Terraform + CI/CD and deploys
```

Back to [docs index](index.md).
