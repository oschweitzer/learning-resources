---
tags:
  - aws
  - saa-c03
  - secure
status: to-review
source: notion
notion-module: "19. VPC networking"
---

# Security groups and NACLs

## What it is for

Two complementary firewalls: instance level (SG) and subnet level (NACL).

## Key concepts

- A request crosses the subnet's **NACL**, then the instance's **SG**, and the response travels back the same way.
- **NACL**: one per subnet (new subnets get the default NACL which allows everything in/out: do not modify it, create custom ones); rules numbered 1 to 32766, evaluated in ascending order, **first match wins**; the last rule (*) denies the rest.
- **Ephemeral ports**: the client connects to a fixed port and expects the reply on a high port (49152 to 65535 on Windows; 32768 to 60999 on many Linux kernels). NACLs are **stateless** -> explicitly allow return traffic on 1024 to 65535, in both subnets.
- **SG**: instance (ENI) level, *allow* rules only, **stateful**, all rules evaluated together. Can reference another SG; an SG of a peered VPC in the same region can be referenced.
- **Blocking one specific IP**: NACL deny (or WAF); SGs cannot deny.
- Pattern: LB opens 80/443; instances only accept the LB's SG; database only accepts the instances' SG.

## Limits and quotas

- NACL: rule numbers 1 to 32766.

## Pricing (logic)

- Nothing specific in my Notion notes.

## When to use it vs alternatives

- Comparison -> [[security-group-vs-nacl]]. Blocking IPs in general -> [[reference-architectures]].

## Exam traps

- NACLs are stateless: remember ephemeral ports.
- Security groups cannot deny.
- Blocking an IP = NACL deny (or WAF).

## See also

[[security-group-vs-nacl]], [[ec2]], [[vpc]], [[vpc-flow-logs-and-network-firewall]], [[waf-shield-firewall-manager]], [[reference-architectures]]
