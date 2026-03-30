# Use OpenAPI for REST API Contracts

- Status: accepted
- Date: 2026-03-05

## Context and Problem Statement

Teams are building REST APIs with inconsistent documentation practices. Some have Swagger annotations, some have hand-written Markdown, and some have no documentation at all. Consumers resort to reading source code or asking in Slack to understand request/response shapes.

## Decision Drivers

- Machine-readable API contracts that can drive code generation, validation, and documentation
- Single source of truth for API shape that stays in sync with the implementation
- Broad tooling ecosystem for linting, mocking, and client generation
- Backstage catalog integration for API discovery

## Considered Options

- OpenAPI 3.1 specification
- gRPC with Protocol Buffers
- GraphQL schema

## Decision Outcome

Chosen option: "OpenAPI 3.1 specification", because it is the industry standard for describing REST APIs, supports JSON Schema for request/response validation, and integrates directly with Backstage's API entity type. OpenAPI specs enable automated client generation (via openapi-generator), contract testing, and interactive documentation via Swagger UI or Redoc.

### Consequences

- Every REST API must have an OpenAPI 3.1 spec checked in alongside its source code
- Specs are linted in CI using Spectral with our custom ruleset
- API entities in the Backstage catalog reference the OpenAPI spec as their definition
- gRPC remains the standard for internal service-to-service communication where streaming or performance is critical
