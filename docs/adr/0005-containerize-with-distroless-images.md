# Containerize with Distroless Images

- Status: accepted
- Date: 2026-03-28

## Context and Problem Statement

Our container images are built on `ubuntu:22.04` and average 850MB. Security scans flag hundreds of CVEs from OS packages we never use. Build times are slow and image pulls during scaling events add noticeable latency.

## Decision Drivers

- Reduce attack surface by removing unnecessary OS packages and shells
- Shrink image size to improve pull times and reduce storage costs
- Meet compliance requirements for minimal container images
- Maintain debuggability for production incidents

## Considered Options

- Google Distroless images
- Alpine Linux base images
- Scratch (empty) base images
- Continue with Ubuntu base

## Decision Outcome

Chosen option: "Google Distroless images", because they contain only the application runtime and its dependencies — no shell, package manager, or extraneous OS libraries. This reduces CVE surface area by over 90% compared to Ubuntu, and image sizes drop to 30-80MB. Unlike scratch images, distroless includes CA certificates, timezone data, and glibc, avoiding common runtime surprises.

### Consequences

- All production Dockerfiles will use multi-stage builds with `gcr.io/distroless/base-nossl-debian12` or language-specific variants
- Debug images (`gcr.io/distroless/base:debug`) will be available for on-demand debugging via ephemeral containers in Kubernetes
- Alpine is not chosen due to musl libc incompatibilities with some Go and Node.js native modules
- CI will enforce a maximum image size threshold of 150MB per service
