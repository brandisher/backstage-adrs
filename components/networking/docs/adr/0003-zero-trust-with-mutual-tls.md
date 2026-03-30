# Zero Trust with Mutual TLS

- Status: accepted
- Date: 2026-03-22

## Context and Problem Statement

Pod-to-pod traffic within our clusters is unencrypted. Any compromised workload can sniff network traffic or impersonate other services. Our security team requires encryption in transit and cryptographic identity verification for all inter-service communication to meet zero-trust compliance requirements.

## Decision Drivers

- All inter-service traffic must be encrypted in transit
- Services must authenticate peers via cryptographic identity, not network position
- Minimal application code changes — mTLS should be transparent to workloads
- Certificate rotation must be automated

## Considered Options

- Cilium mutual authentication with WireGuard encryption
- Istio sidecar-based mTLS
- Application-level TLS managed per service

## Decision Outcome

Chosen option: "Cilium mutual authentication with WireGuard encryption", because it provides transparent node-to-node encryption via WireGuard and mutual authentication using SPIFFE identities, all without sidecar proxies. This avoids the resource overhead and latency penalty of sidecar injection while satisfying the zero-trust requirement. Cilium's identity-aware policy enforcement ensures that encryption and authentication are tied to the same eBPF-based network layer already in use.

### Consequences

- WireGuard encryption is enabled cluster-wide via Cilium configuration; all pod-to-pod traffic is encrypted transparently
- Cilium's mutual authentication is enabled, issuing SPIFFE SVIDs to workloads for peer verification
- No sidecar injection is required, keeping pod resource overhead unchanged
- Istio remains available for teams that need L7 traffic management features (retries, circuit breaking) beyond what Cilium provides
