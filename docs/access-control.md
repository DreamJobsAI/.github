# Access Control & Team Permissions

How GitHub permissions are organized in the DreamJobsAI organization.

## Team Structure

GitHub Free orgs use **team-based permissions** — each team grants a specific access level across all repositories.

| Team | GitHub Role | What it allows |
|------|------------|----------------|
| **ai-leads** | `maintain` | Manage branches & protection rules, merge PRs, push to protected branches, manage releases. No destructive admin actions (delete repo, change visibility). |
| **ai-developers** | `write` | Push branches, create & merge PRs, manage issues & labels, trigger workflows. The standard developer role. |
| **ai-reviewers** | `triage` | Review PRs (approve/request changes), manage issues & labels, read code. Cannot push code. |
| **platform-ci-read** | `read` | Read-only access. For CI service accounts, external auditors, or temporary observers. |

All four teams have access to **every repository** in the org.

## Adding a New Developer

A new team member who will write code should be added to **ai-developers** (write access):

```bash
# 1. Invite to the org (they'll receive an email)
gh api orgs/DreamJobsAI/invitations -X POST \
  -f email="dev@example.com" \
  -f role="direct_member"

# 2. Once they accept, add to ai-developers
gh api orgs/DreamJobsAI/teams/ai-developers/memberships/GITHUB_USERNAME \
  -X PUT -f role="member"
```

Or if you know their GitHub username upfront:

```bash
# Invite by username + add to team in one go
gh api orgs/DreamJobsAI/invitations -X POST \
  -f invitee_id=$(gh api users/GITHUB_USERNAME --jq '.id') \
  -f role="direct_member" \
  -f team_ids="[18625530]"
```

> The `team_ids` value `18625530` is the `ai-developers` team ID. Look up other team IDs with:
> `gh api orgs/DreamJobsAI/teams --jq '.[] | "\(.name): \(.id)"'`

## Changing a Developer's Role

Promote a developer to lead:

```bash
gh api orgs/DreamJobsAI/teams/ai-leads/memberships/GITHUB_USERNAME \
  -X PUT -f role="member"
```

Add review-only access (e.g. for a QA engineer or external reviewer):

```bash
gh api orgs/DreamJobsAI/teams/ai-reviewers/memberships/GITHUB_USERNAME \
  -X PUT -f role="member"
```

Remove from a team:

```bash
gh api orgs/DreamJobsAI/teams/ai-developers/memberships/GITHUB_USERNAME \
  -X DELETE
```

## Permission Matrix

What each role can do:

| Action | read | triage | write | maintain | admin |
|--------|:----:|:------:|:-----:|:--------:|:-----:|
| View code & clone | x | x | x | x | x |
| Create & comment on issues | x | x | x | x | x |
| Manage issues & labels | | x | x | x | x |
| Review PRs (approve/request changes) | | x | x | x | x |
| Push branches | | | x | x | x |
| Create & merge PRs | | | x | x | x |
| Trigger & cancel workflows | | | x | x | x |
| Manage branch protection rules | | | | x | x |
| Push to protected branches | | | | x | x |
| Manage releases & tags | | | | x | x |
| Manage repo settings & webhooks | | | | | x |
| Delete repo | | | | | x |

## Current Members

| User | Org Role | Teams |
|------|----------|-------|
| atr-ip | admin | ai-leads (maintainer), ai-developers |
| laszlobedo | admin | ai-leads (maintainer) |
| AndrasBocsardi | member | ai-developers |

## Org-Level Settings

| Setting | Value |
|---------|-------|
| Plan | Free |
| Default repo permission | `read` |
| Members can create repos | yes |
| 2FA required | no (recommended to enable) |

## Quick Decision Guide

| Scenario | Team |
|----------|------|
| New full-time developer | `ai-developers` |
| Tech lead / senior who manages releases | `ai-leads` |
| QA engineer or external code reviewer | `ai-reviewers` |
| CI bot or read-only service account | `platform-ci-read` |
| Temporary contractor (read code only) | `platform-ci-read` |
