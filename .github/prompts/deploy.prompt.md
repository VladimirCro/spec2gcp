---
agent: gcloud
---
# Google Cloud Deployment Flow

Analyze the codebase and deploy it to Google Cloud Platform with best practices, infrastructure as code, and CI/CD pipelines.

## Your Responsibilities

### Pre-Deployment Analysis
1. Read `specs/prd.md` and FRDs to understand requirements
2. Review `AGENTS.md` for technology stack decisions
3. Consult `specs/adr/*.md` for architectural decisions
4. Analyze codebase structure (`src/backend`, `src/frontend`, etc.)
5. Identify GCP services needed

### Deployment Workflow

**Step 1: GCP Project Setup**
- Confirm GCP project ID and region with the user
- Enable required APIs: `gcloud services enable run.googleapis.com cloudbuild.googleapis.com secretmanager.googleapis.com artifactregistry.googleapis.com monitoring.googleapis.com`
- Configure `gcloud config set project <PROJECT_ID>`
- Define environments (dev, staging, prod)

**Step 2: Infrastructure as Code (Terraform)**
- Generate Terraform configuration in `infra/` folder
- **Use Google-provided Terraform modules** when available
- Create `main.tf` orchestrator with provider block
- Define `variables.tf` and `outputs.tf`
- Follow proper resource naming conventions
- Configure resource labels for cost management

**Step 3: GitHub Actions CI/CD**
- Create `.github/workflows/deploy.yml`
- **Use Workload Identity Federation** for keyless GCP authentication (no key files)
- Configure build, test, and push to Artifact Registry steps
- Add deployment steps using `gcloud` CLI or `terraform apply`
- Set up environments and approval gates
- Configure secrets in GitHub (never hardcode)

**Step 4: Security & Monitoring**
- Configure **Workload Identity Federation** for GitHub Actions authentication
- Store secrets in **Secret Manager** (never in code or env vars)
- Configure **Cloud Monitoring** dashboards and alerts
- Set up **Cloud Trace** and **Cloud Logging** for all services
- Enable **IAM** with least-privilege roles

**Step 5: Deploy & Verify**
- Run `terraform init && terraform plan` to preview changes
- Run `terraform apply` to provision infrastructure
- Deploy application: `gcloud run deploy` / `gcloud functions deploy`
- Verify deployment health and endpoints
- Configure custom domains and managed SSL certificates if needed
- Set up uptime checks in Cloud Monitoring

**Step 6: Documentation**
- Create `docs/deployment.md` with deployment instructions
- Document all GCP resources created
- Document environment variables and Secret Manager secrets
- Add runbook for common operations (redeploy, rollback, scaling)

## If Deployment Fails

**Terraform errors:**
- Run `terraform plan` first and review the output before `terraform apply`
- On failure, run `terraform destroy` only for newly created resources — never for existing ones
- Check GCP API enablement: missing APIs are the most common cause of Terraform failures

**Cloud Run deployment errors:**
- Check image build logs: `gcloud builds list --limit=5`
- Check service logs: `gcloud run services logs read <SERVICE_NAME>`
- Roll back to previous revision: `gcloud run services update-traffic <SERVICE_NAME> --to-revisions=PREVIOUS=100`

**GitHub Actions / Workload Identity errors:**
- Verify the WIF pool and provider are correctly configured
- Verify the service account has the correct IAM bindings
- Check that `GOOGLE_CLOUD_PROJECT` and `WIF_PROVIDER` secrets are set in the repository

**General rule:** If something fails, do not retry blindly — read the error output, surface it to the user, and propose a fix before proceeding.

## Tools to Use

Priority order:
1. **gcloud CLI** - Primary tool for GCP operations
2. **Terraform** - Infrastructure as Code for all GCP resources
3. **Google GitHub Actions** (`google-github-actions/*`) - For CI/CD workflows
4. **Cloud Build** - For native GCP CI/CD pipelines (alternative to GitHub Actions)
5. **GitHub MCP** - For creating workflows and managing repository

## Important Notes

- **Prefer Cloud Run** as the default compute option — scales to zero, minimal configuration
- **Always enable required APIs** before provisioning resources
- **Use Workload Identity Federation** instead of service account key files for GitHub Actions
- **Enable all logging** from day one (Cloud Logging, Cloud Trace)
- **Label all resources** with environment, project, owner, managed-by=terraform
- **Plan for scalability** even if starting small (Cloud Run handles this automatically)
