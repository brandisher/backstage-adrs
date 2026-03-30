# Standardize Error Response Format

- Status: accepted
- Date: 2026-03-19

## Context and Problem Statement

Each API returns errors in a different shape. Some return `{ "error": "message" }`, some return `{ "code": 123, "msg": "..." }`, and some return plain text. Client teams must write per-service error handling logic, and observability tooling cannot parse error responses consistently for alerting.

## Decision Drivers

- Consistent error structure across all APIs for client SDK and UI error handling
- Machine-parseable error codes for programmatic retry/fallback decisions
- Human-readable messages for debugging and logging
- Alignment with an established standard to avoid inventing a custom format

## Considered Options

- RFC 9457 (Problem Details for HTTP APIs)
- Custom internal error envelope
- Google Cloud API error model

## Decision Outcome

Chosen option: "RFC 9457 (Problem Details for HTTP APIs)", because it is an IETF standard with a well-defined JSON schema (`type`, `title`, `status`, `detail`, `instance`) that covers the needs of both human debugging and programmatic error handling. It uses `application/problem+json` as the content type, making errors distinguishable from successful responses at the HTTP level. Libraries exist for all our server languages (Go, TypeScript, Python).

### Consequences

- All APIs must return `application/problem+json` for 4xx and 5xx responses
- The `type` field must be a URI referencing our internal error catalog for programmatic identification
- The `detail` field must not expose internal implementation details or stack traces
- Client SDKs will include a shared error parser that maps Problem Details responses to typed error objects
- An internal error type registry will be maintained in the API platform docs
