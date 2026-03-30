# Require API Versioning via URL Path

- Status: accepted
- Date: 2026-03-12

## Context and Problem Statement

We have no consistent API versioning strategy. Some services use header-based versioning, some embed versions in the URL, and some have no versioning at all. Breaking changes have caused production incidents when clients and servers disagree on the contract.

## Decision Drivers

- Versioning must be explicit and visible to API consumers
- Must support running multiple API versions concurrently during migration periods
- Simple to implement and test without custom middleware
- Easy to route at the load balancer / gateway level

## Considered Options

- URL path versioning (`/v1/resources`)
- Accept header versioning (`Accept: application/vnd.api.v1+json`)
- Query parameter versioning (`/resources?version=1`)

## Decision Outcome

Chosen option: "URL path versioning", because it makes the API version immediately visible in every request, requires no custom content negotiation, and is trivially routable at the gateway layer. Consumers can bookmark, share, and curl versioned endpoints without special headers. It is the most widely understood approach and aligns with the conventions of the public APIs our customers already integrate with.

### Consequences

- All APIs must include a major version prefix in the URL path (e.g., `/v1/`, `/v2/`)
- Minor and patch-level changes must be backward compatible within a major version
- Deprecated versions must continue serving traffic for at least 6 months after the successor version is generally available
- API gateway routes are configured per version, allowing independent scaling and canary deployment of new versions
