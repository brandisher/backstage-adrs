# Adopt Gateway API for Ingress

- Status: accepted
- Date: 2026-03-15

## Context and Problem Statement

Our clusters use a mix of Ingress resources with nginx-ingress annotations and raw Istio VirtualServices for external traffic routing. The inconsistency makes it difficult for application teams to expose services, and annotation-driven configuration is error-prone and hard to validate.

## Decision Drivers

- Standardized, expressive API that supports HTTP routing, TLS, and traffic splitting natively
- Role-oriented resource model separating infrastructure (Gateway) from application (HTTPRoute) concerns
- Vendor-neutral specification with multiple conformant implementations
- Path toward deprecating custom annotations and vendor-specific CRDs

## Considered Options

- Kubernetes Gateway API
- Continue with Ingress + nginx annotations
- Istio VirtualService for all ingress

## Decision Outcome

Chosen option: "Kubernetes Gateway API", because it provides a role-oriented model where platform teams manage Gateway resources (listeners, TLS certificates) and application teams manage HTTPRoute resources (path matching, backends). This separation of concerns reduces misconfiguration risk. Gateway API is a graduated Kubernetes SIG-Network project with conformance tests, and Cilium implements it natively.

### Consequences

- All new external-facing services must use HTTPRoute resources attached to shared Gateway resources
- Platform team owns Gateway resources and TLS certificate management via cert-manager
- Existing Ingress resources will be migrated to HTTPRoute over the next two quarters
- Istio VirtualService remains supported for mesh-internal traffic but is no longer used for ingress
