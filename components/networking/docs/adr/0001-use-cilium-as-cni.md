# Use Cilium as CNI

- Status: accepted
- Date: 2026-03-08

## Context and Problem Statement

We need to select a Container Network Interface (CNI) plugin for our Kubernetes clusters. The CNI must support network policy enforcement, provide visibility into pod-to-pod traffic, and scale to clusters with 500+ nodes without introducing significant latency overhead.

## Decision Drivers

- Kernel-level performance using eBPF instead of iptables
- Native Kubernetes NetworkPolicy support plus extended L7 policies
- Built-in observability via Hubble for flow logs and service maps
- Active CNCF project with broad adoption

## Considered Options

- Cilium
- Calico
- Flannel with kube-proxy

## Decision Outcome

Chosen option: "Cilium", because it uses eBPF to implement networking, security, and observability directly in the Linux kernel, bypassing iptables entirely. This eliminates the performance degradation seen with iptables at scale. Cilium's Hubble component provides real-time network flow visibility without sidecar proxies, and its CiliumNetworkPolicy CRD supports L7-aware policies beyond what standard Kubernetes NetworkPolicy offers.

### Consequences

- Cilium replaces kube-proxy via its eBPF-based kube-proxy replacement mode
- Hubble is enabled on all clusters for network flow observability
- Teams must use CiliumNetworkPolicy for L7 policies; standard NetworkPolicy is supported for L3/L4 rules
- Kernel version 5.10+ is required on all nodes to support the eBPF features Cilium depends on
