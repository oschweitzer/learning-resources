---
tags:
  - aws
  - saa-c03
  - secure
  - resilient
  - performant
status: to-review
source: notion
notion-module: "19. VPC networking"
---

# Peering, Transit Gateway, VPN, Direct Connect

## What it is for

Connect VPCs to each other and to the on-premises network.

## Key concepts

- **VPC peering**: connects two VPCs privately over the AWS network; non-overlapping CIDRs; **non-transitive** (A-B and B-C != A-C); update route tables on both sides; cross-account and cross-region; SG references between peered VPCs in the same region.
- **Site-to-Site VPN**: IPsec over the internet; **VGW** on the VPC side (custom ASN possible), **CGW** on your side (public IP, or that of a NAT device with NAT traversal); enable **route propagation**; allow ICMP inbound to ping; quick, encrypted, limited bandwidth (~1.25 Gbps per tunnel to a VGW). **CloudHub**: several on-premises sites through one VGW (hub-and-spoke, dynamic routing, over the internet, low cost). **Client VPN**: OpenVPN-based access for individual users.
- **Direct Connect**: dedicated private connection through a DX location; VGW (or Transit Gateway); *private VIF* -> VPCs, *public VIF* -> public AWS services (S3), on the same connection; IPv4 and IPv6; more bandwidth for large datasets at lower cost, more consistent performance; **DX gateway**: one DX to VPCs in many regions. *Dedicated* 1 to 400 Gbps (physical port); *Hosted* 50 Mbps to 25 Gbps through partners (capacity changeable). Lead time often > 1 month -> VPN first. **Not encrypted by default** (but private) -> IPsec VPN on top or MACsec (dedicated). Resiliency: *high* = one connection at two locations; *maximum* = separate connections on separate devices in more than one location; a VPN is a cheaper backup.
- **Transit Gateway**: hub-and-spoke with **transitive** routing between thousands of VPCs and on-premises networks (VPN, DX gateway); regional with inter-TGW peering; shared across accounts via **RAM**; route tables per VPC; **the only AWS service supporting IP multicast**; **ECMP** on VPN to a TGW aggregates tunnels (~2.5 Gbps per VPN connection instead of 1.25 to a VGW, more by adding connections); a DX can be shared between accounts through a TGW.

## Limits and quotas

- VPN: ~1.25 Gbps per tunnel to a VGW; ~2.5 Gbps per connection to a TGW (ECMP).
- Direct Connect dedicated: 1 to 400 Gbps; hosted: 50 Mbps to 25 Gbps.

## Pricing (logic)

- Direct Connect: best cost for large volumes; egress via DX cheaper; VPN: low cost (CloudHub).

## When to use it vs alternatives

- A few VPCs -> peering; many VPCs + on premises, transitive -> Transit Gateway; quick and encrypted -> VPN; private, constant, high throughput -> Direct Connect. Comparison -> [[vpn-vs-direct-connect]].

## Exam traps

- Peering is not transitive.
- Direct Connect is not encrypted by default and takes weeks to set up.
- Many VPCs + on premises = Transit Gateway.

## See also

[[vpn-vs-direct-connect]], [[vpc]], [[route-53]], [[vpc-endpoints]], [[dr-and-multi-region]]
