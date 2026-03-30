# Adopt OpenTelemetry for Observability

- Status: accepted
- Date: 2026-03-18

## Context and Problem Statement

Our services use a mix of vendor-specific agents and libraries for metrics, tracing, and logging. This creates inconsistent telemetry data, vendor lock-in, and makes it difficult to correlate signals across services during incident response.

## Decision Drivers

- Unified instrumentation across traces, metrics, and logs
- Vendor neutrality to allow switching backends without code changes
- Industry momentum and broad language support
- Correlation of signals (e.g., linking a trace to the log lines it produced)

## Considered Options

- OpenTelemetry (OTel) SDK + Collector
- Datadog agent with proprietary SDK
- Maintain current mixed approach (Prometheus client + Jaeger SDK + structured logging)

## Decision Outcome

Chosen option: "OpenTelemetry SDK + Collector", because it provides a single, vendor-neutral instrumentation layer across all three signal types. The OTel Collector acts as a pipeline that can export to any backend, eliminating lock-in. Auto-instrumentation support for our primary languages (Go, TypeScript) reduces adoption effort.

### Consequences

- All new services must use OTel SDKs for instrumentation
- An OTel Collector will be deployed as a sidecar in Kubernetes to receive, process, and export telemetry
- Existing services will be migrated incrementally, prioritizing critical-path services first
- Backend choice (Grafana Cloud, Datadog, etc.) becomes a Collector config change rather than a code change
