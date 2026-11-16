---
tags:
  - aws
  - saa-c03
  - cost
  - resilient
  - secure
  - performant
status: to-review
source: notion
notion-module: "21. Architecture patterns, other services and Well-Architected"
---

# Well-Architected Framework and Trusted Advisor

## What it is for

A best-practice framework (Well-Architected) and automatic account analysis (Trusted Advisor).

## Key concepts

- **Principles**: stop guessing capacity, test at production scale, automate, allow evolutionary architectures, drive decisions with data, improve through game days.
- **Six pillars**: Operational excellence, Security, Reliability, Performance efficiency, Cost optimisation, Sustainability. They reinforce each other rather than being trade-offs.
- **Well-Architected Tool**: free; answer questions about a workload, review against the six pillars, get advice, reports and improvement plans. Reference architectures: AWS Architecture Center and AWS Solutions Library.
- **Trusted Advisor**: analyses accounts with nothing to install; recommendations across cost optimisation, performance, security, fault tolerance, service limits and operational excellence. Core checks for everyone; the full set and programmatic access through the AWS Support API require Business or Enterprise Support.

## Limits and quotas

- Nothing specific in my Notion notes.

## Pricing (logic)

- Well-Architected Tool is free. Trusted Advisor: core checks for everyone, full set with Business/Enterprise Support.

## When to use it vs alternatives

- Architecture review -> Well-Architected Tool; automatic recommendations -> Trusted Advisor; precise right-sizing -> [[cost-management]].

## Exam traps

- Six pillars (including Sustainability).

## See also

[[cost-management]], [[reference-architectures]]
