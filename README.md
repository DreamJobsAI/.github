# DreamJobsAI — Shared GitHub Actions

Reusable workflows and composite actions for the DreamJobs AI platform.

## Contents

| Path | Purpose |
|------|---------|
| `.github/workflows/docker-build.yml` | Reusable Docker build + ECR push |
| `.github/workflows/deploy.yml` | Reusable SSM deploy to EC2 |
| `.github/actions/checkout-platform/` | Multi-repo checkout for `ai-common` consumers |

## Documentation

- [Platform Build Contract](../setup/docs/02_platform-build-contract.md)
- [CI Secrets & Onboarding](../setup/docs/03_ci-secrets-and-onboarding.md)
- [Docker CI/CD Setup](../infra/docs/02_docker-ci-cd-setup.md)

## Quick reference

### Test workflow (service with ai-common)

```yaml
- uses: DreamJobsAI/.github/.github/actions/checkout-platform@main
  with:
    service_name: ai_gateway
    dependency_ref: ${{ github.head_ref || github.ref_name }}
    app_id: ${{ secrets.GH_APP_ID }}
    app_private_key: ${{ secrets.GH_APP_PRIVATE_KEY }}
```

### Docker workflow

```yaml
uses: DreamJobsAI/.github/.github/workflows/docker-build.yml@main
secrets:
  AWS_ROLE_ARN: ${{ secrets.AWS_ECR_ROLE_ARN }}
  GH_APP_ID: ${{ secrets.GH_APP_ID }}
  GH_APP_PRIVATE_KEY: ${{ secrets.GH_APP_PRIVATE_KEY }}
```

Org secrets `GH_APP_ID`, `GH_APP_PRIVATE_KEY`, and `AWS_ECR_ROLE_ARN` are configured
once at the organization level.
