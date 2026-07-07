# DreamJobsAI — Shared GitHub Actions

Reusable workflows and composite actions for the DreamJobs AI platform.

## Contents

| Path | Purpose |
|------|---------|
| `.github/workflows/docker-build.yml` | Reusable Docker build + ECR push |
| `.github/workflows/deploy.yml` | Reusable SSM deploy to EC2 |
| `.github/actions/checkout-platform/` | Multi-repo checkout for `ai-common` consumers |

## Documentation

Platform-wide contracts (authoritative):

- [Platform Build Contract](../setup/docs/02_platform-build-contract.md) — layout, Docker, CI rules
- [CI Secrets & Onboarding](../setup/docs/03_ci-secrets-and-onboarding.md) — org secrets, IAM, checklist
- [Docker CI/CD Setup](../infra/docs/02_docker-ci-cd-setup.md) — ECR, versioning, troubleshooting

## Quick reference

### Test workflow (service with ai-common)

```yaml
- uses: DreamJobsAI/.github/.github/actions/checkout-platform@main
  with:
    service_name: ai_gateway
    dependency_ref: ${{ github.head_ref || github.ref_name }}
    checkout_token: ${{ secrets.GH_PLATFORM_READ_TOKEN }}
```

### Docker workflow

```yaml
uses: DreamJobsAI/.github/.github/workflows/docker-build.yml@main
secrets:
  AWS_ROLE_ARN: ${{ secrets.AWS_ECR_ROLE_ARN }}
  CHECKOUT_TOKEN: ${{ secrets.GH_PLATFORM_READ_TOKEN }}  # when using ai-common
```

Org secrets `GH_PLATFORM_READ_TOKEN` and `AWS_ECR_ROLE_ARN` must be configured once
at the organization level. See [03_ci-secrets-and-onboarding.md](../setup/docs/03_ci-secrets-and-onboarding.md).
