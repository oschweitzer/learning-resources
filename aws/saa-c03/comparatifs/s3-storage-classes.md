---
tags:
  - aws
  - saa-c03
  - comparatif
  - cost
status: to-review
source: notion
notion-module: "07. Amazon S3"
---

# S3 storage classes

Pick the class by access frequency, duration and resilience.

| Class | Availability | AZs | Min. duration | Retrieval | Use |
| --- | --- | --- | --- | --- | --- |
| Standard | 99.99% | >= 3 | None | ms, no fee | Frequent access |
| Intelligent-Tiering | 99.9% | >= 3 | None | ms (frequent/IA tiers), no fee | Unknown or changing access |
| Standard-IA | 99.9% | >= 3 | 30 d | ms, fee per GB | Backups, DR copies |
| One Zone-IA | 99.5% | 1 | 30 d | ms, fee per GB | Secondary copies, re-creatable data |
| Glacier Instant Retrieval | 99.9% | >= 3 | 90 d | ms | Archives read about once a quarter |
| Glacier Flexible Retrieval | 99.99% | >= 3 | 90 d | Expedited 1-5 min, Standard 3-5 h, Bulk 5-12 h (free) | Archives with relaxed retrieval |
| Glacier Deep Archive | 99.99% | >= 3 | 180 d | Standard 12 h, Bulk 48 h | Long-term compliance, cheapest |
| Express One Zone | 99.95% | 1 (directory bucket) | None | Single-digit ms | Latency-critical, ML, HPC |

## Decision rule

- Unknown access -> Intelligent-Tiering.
- Rarely read archive, 48 h OK -> Deep Archive (Bulk).
- Re-creatable data -> One Zone-IA.
- Minimum latency co-located with compute -> Express One Zone.

## Exam traps

- Minimum durations: costly for short-lived data.
- Storage Class Analysis only recommends Standard -> Standard-IA.

## See also

[[s3]]
