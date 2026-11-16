---
tags:
  - aws
  - saa-c03
  - comparatif
  - secure
  - performant
  - resilient
status: to-review
source: notion
notion-module: "19. VPC networking"
---

# Site-to-Site VPN vs Direct Connect

Quick encrypted connection over the internet, or a dedicated private link.

|  | Site-to-Site VPN | Direct Connect |
| --- | --- | --- |
| Network | IPsec over the public internet | Dedicated private link through a DX location |
| Encryption | Yes | Not by default (IPsec VPN on top or MACsec) |
| Lead time | Quick to set up | Often more than a month |
| Bandwidth | ~1.25 Gbps per tunnel to a VGW | Dedicated 1 to 400 Gbps; hosted 50 Mbps to 25 Gbps |
| Performance | Variable (internet) | More consistent, large volumes at lower cost |

## Decision rule

- Immediate need -> VPN first.
- Large volumes, consistent performance -> Direct Connect, VPN as backup.

## Exam traps

- DX is not encrypted by default.

## See also

[[vpc-connectivity]]
