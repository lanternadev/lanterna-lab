# Homelab DNS Setup

This document describes the DNS design for the `lanterna-lab` environment.

## Goals
- Human-readable names for all VMs and services.
- Smooth integration with K3s (CoreDNS + ExternalDNS).
- Standards-compliant private zones (`*.home.arpa`).

## Architecture
- **Authoritative DNS**: PowerDNS serving `lab.home.arpa` and `k3s.home.arpa`.
- **Recursive resolver**: Unbound for forwarding external queries.
- **Cluster DNS**: CoreDNS (bundled with K3s) handling `*.svc.cluster.local`.
- **Dynamic updates**: ExternalDNS publishing Ingress records to PowerDNS.

## Example Zones
- `db1.lab.home.arpa` → VM hosting PostgreSQL
- `grafana.k3s.home.arpa` → Ingress published via ExternalDNS
- `*.svc.cluster.local` → internal service discovery

## Next Steps
1. Stand up Unbound + PowerDNS on `lab-1-mgmt1`.
2. Configure DHCP/macOS resolver entries for `*.home.arpa`.
3. Deploy MetalLB + ExternalDNS in K3s.
