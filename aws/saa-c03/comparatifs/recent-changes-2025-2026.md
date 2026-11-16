---
tags:
  - aws
  - saa-c03
  - comparatif
  - resilient
  - performant
  - secure
  - cost
status: to-review
source: notion
notion-module: "Root page: What changed recently"
---

# Recent changes to know (up to November 2026)

Newer than many training materials. The exam can lag behind the services: learn the classic answer and the current state.

| Date | Change | Note |
| --- | --- | --- |
| June 2025 | DynamoDB global tables: multi-region strong consistency (RPO zero) | [[dynamodb]] |
| June 2025 | Timestream for LiveAnalytics closed to new customers; InfluxDB recommended | [[specialized-databases]] |
| August 2025 | SQS: max message size 256 KiB -> 1 MiB | [[sqs]] |
| September 2025 | EBS gp3 up to 64 TiB, 80,000 IOPS, 2,000 MiB/s | [[ebs]] |
| October 2025 | QuickSight -> Amazon Quick Suite, then Amazon Quick | [[opensearch-emr-and-quick]] |
| November 2025 | Snowball Edge and S3 Object Lambda closed to new customers | [[data-transfer]], [[s3]] |
| November 2025 | Regional NAT gateway | [[nat]] |
| December 2025 | S3: max object size 5 TB -> 50 TB | [[s3]] |
| December 2025 | Standalone Glacier (vaults) closed to new customers; S3 Glacier classes unaffected | [[s3]] |
| April 2026 | Amazon S3 Files: an S3 bucket mounted as a shared file system | [[fsx-and-s3-files]] |
| October 2026 | End of support for Amazon Pinpoint | [[other-services]] |
| October 2026 | Exam guide 1.1: Elastic Transcoder removed from the in-scope services | [[other-services]] |

## Decision rule

- Learn the classic answer AND the current state of the service.
- Lambda also accepts asynchronous payloads up to 1 MB ([[lambda]]).

## Exam traps

- Nothing specific in my Notion notes.

## See also

[[s3]], [[sqs]], [[ebs]], [[nat]], [[dynamodb]]
