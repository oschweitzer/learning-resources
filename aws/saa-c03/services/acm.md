---
tags:
  - aws
  - saa-c03
  - secure
status: to-review
source: notion
notion-module: "18. Security and encryption"
---

# AWS Certificate Manager (ACM)

## What it is for

Provision, manage and deploy TLS certificates (public ones free and auto-renewed; private ones via AWS Private CA).

## Key concepts

- Integrates with ELB (ALB, NLB, CLB), CloudFront, API Gateway... **Not directly with EC2 instances**: a public ACM certificate cannot be extracted (exportable public certificates exist as a paid option).
- Requesting a public certificate: list FQDNs or wildcards (`*.example.com`), choose **DNS** validation (preferred, a CNAME record, ideal with Route 53) or email validation, wait for validation. Automatic renewal **60 days before expiry**.
- **Imported** certificates (generated elsewhere): no automatic renewal; ACM sends daily expiry events to EventBridge (from 45 days by default, configurable); the Config rule `acm-certificate-expiration-check` flags them.
- **Regions**: CloudFront and edge-optimised API Gateway -> certificate in **us-east-1**; regional API -> the API's region; then CNAME/alias in Route 53.

## Limits and quotas

- Automatic renewal 60 days before expiry; expiry events from 45 days (configurable).

## Pricing (logic)

- Public certificates are free; private ones via AWS Private CA.

## When to use it vs alternatives

- TLS on ELB/CloudFront/API Gateway -> ACM. Installing on EC2 -> not possible with a public ACM certificate.

## Exam traps

- ACM cannot install certificates on EC2.
- CloudFront: us-east-1.

## See also

[[elb]], [[cloudfront]], [[api-gateway]], [[route-53]], [[config]]
