# Use Trunk-Based Development

- Status: accepted
- Date: 2026-03-22

## Context and Problem Statement

Our team has grown from 4 to 12 engineers and long-lived feature branches are causing painful merge conflicts, delayed integration feedback, and unpredictable release cycles. We need a branching strategy that supports continuous delivery.

## Decision Drivers

- Reduce integration risk by merging small changes frequently
- Enable continuous deployment to production multiple times per day
- Minimize merge conflicts and stale branches
- Keep the CI/CD pipeline fast and reliable

## Considered Options

- Trunk-based development with short-lived branches
- Git Flow (develop, release, hotfix branches)
- GitHub Flow (feature branches off main, merged via PR)

## Decision Outcome

Chosen option: "Trunk-based development with short-lived branches", because it enforces small, incremental changes that are integrated into `main` at least daily. This pairs well with our CI/CD pipeline and feature flag infrastructure, which allows incomplete features to be merged safely behind flags.

### Consequences

- Feature branches must be merged within 24 hours of creation
- All changes require a passing CI pipeline and one approval before merge
- Incomplete features must be gated behind feature flags (LaunchDarkly)
- Release branches are cut from `main` only when a hotfix is needed for a prior release
- Git Flow is explicitly deprecated; existing long-lived branches will be closed out within two sprints
