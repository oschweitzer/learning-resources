---
tags:
  - aws
  - saa-c03
  - performant
  - secure
status: to-review
source: notion
notion-module: "12. Serverless"
---

# Amazon API Gateway

## What it is for

Serverless front door for APIs: versions, stages, authentication, API keys, usage plans, throttling, caching, validation, OpenAPI, WebSocket.

## Key concepts

- Types: **REST** (most features), **HTTP** (cheaper, lower latency, fewer features), **WebSocket**.
- Integrations: Lambda (the classic serverless REST API); HTTP backends (on-premises, ALB) to add rate limiting, caching, auth, keys; AWS services directly (SendMessage to SQS, PutRecord to Kinesis, start a Step Functions execution) without Lambda.
- **Endpoints**: *edge-optimised* (default, global clients via CloudFront, API stays in one region); *regional* (clients in the same region; own CloudFront possible); *private* (from your VPC through an interface VPC endpoint + resource policy).
- **Security**: IAM (SigV4) for internal/AWS callers; **Cognito user pools** for external users; **Lambda authorisers** for custom logic (third-party tokens); WAF can protect REST APIs; custom domains with ACM (edge-optimised: certificate in **us-east-1**; regional: the API's region) then CNAME/alias in Route 53.

## Limits and quotas

- Nothing specific in my Notion notes.

## Pricing (logic)

- Nothing specific in my Notion notes.

## When to use it vs alternatives

- Serverless API -> API Gateway + [[lambda]]; user authentication -> [[cognito]]; API caching -> see [[reference-architectures]].

## Exam traps

- Certificate of an edge-optimised endpoint in us-east-1.
- Private API = interface VPC endpoint + resource policy.

## See also

[[lambda]], [[cognito]], [[acm]], [[waf-shield-firewall-manager]], [[step-functions]]
