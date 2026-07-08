# DreamJobsAI — Shared GitHub Actions

Reusable workflows and composite actions for the DreamJobs AI platform.

## Workflows

| Workflow | Purpose | Caller trigger |
|----------|---------|---------------|
| `docker-build.yml` | Build & push Docker image to ECR | `workflow_call` |
| `deploy.yml` | SSM-based deploy to EC2 | `workflow_call` |
| `docs-check.yml` | Validate README required sections on PR | `workflow_call` |
| `local-docs-aggregator.yml` | Aggregate local docs into root `docs/` on push | `workflow_call` |

## Composite Actions

| Action | Purpose |
|--------|---------|
| `checkout-platform` | Check out service + ai-common dependency |

## Documentation

- [Platform Build Contract](../setup/docs/02_platform-build-contract.md)
- [CI Secrets & Onboarding](../setup/docs/03_ci-secrets-and-onboarding.md)

## Quick reference (ai-common consumers)

Generate a GitHub App installation token in the **caller workflow**, then pass it to
`checkout-platform` as `checkout_token`. Do not pass App secrets into the composite
action — nested actions cannot receive them reliably.

```yaml
- name: Create GitHub App token
  id: app-token
  uses: actions/create-github-app-token@v1
  with:
    app-id: ${{ secrets.GH_APP_ID }}
    private-key: ${{ secrets.GH_APP_PRIVATE_KEY }}
    owner: DreamJobsAI
    repositories: ai-common

- uses: DreamJobsAI/.github/.github/actions/checkout-platform@main
  with:
    service_name: ai_gateway
    dependency_ref: ${{ github.head_ref || github.ref_name }}
    checkout_token: ${{ steps.app-token.outputs.token }}
```

Docker builds: `docker-build.yml` creates the App token automatically when
`dependency_repository` is set.
