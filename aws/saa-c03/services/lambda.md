---
tags:
  - aws
  - saa-c03
  - performant
  - resilient
  - cost
status: to-review
source: notion
notion-module: "12. Serverless"
---

# AWS Lambda

## What it is for

On-demand functions with automatic scaling, integrated with most AWS services (monitoring through CloudWatch).

## Key concepts

- EC2 = a virtual server limited by RAM/CPU, running continuously. Lambda = a virtual function limited by duration, on demand, auto-scaled.
- Runtimes: Node.js, Python, Java, .NET, Ruby, custom runtimes (Rust, Go), container images implementing the Lambda Runtime API. Arbitrary Docker images -> ECS/Fargate.
- **Invocation**: *synchronous* (API Gateway, ALB, SDK; when throttled -> 429, the caller retries); *asynchronous* (S3, SNS, EventBridge; queued, retries up to 6 hours, failures go to a DLQ or on-failure destination); *event source mapping* (SQS, Kinesis, DynamoDB Streams: Lambda polls in batches).
- **Reserved concurrency**: guarantees and caps one function (without it a runaway function can throttle all others). **Provisioned concurrency**: pre-initialised environments, no cold starts, paid, scalable with Application Auto Scaling. **SnapStart** (Java, Python, .NET): snapshot after init, up to 10x faster starts at no extra cost.
- **Cold start**: a new execution environment loads code and runs initialisation outside the handler.
- **In a VPC**: by default Lambda runs in an AWS-owned VPC (internet yes, your private resources no). Attached to your VPC (subnets + SGs) -> ENIs; internet via **NAT gateway**; AWS services via VPC endpoints (DynamoDB: free gateway endpoint). Many database connections -> **RDS Proxy** (the function must be in the VPC).
- RDS for PostgreSQL and Aurora MySQL can invoke Lambda from inside the database. RDS event notifications report on the instance, not the data (SNS/EventBridge).
- Newer options: Lambda Managed Instances (EC2 managed by Lambda), durable functions (checkpoint/resume). Good to know; the exam still revolves around the classic limits.
- **Edge**: CloudFront Functions (JavaScript; millions of req/s; < 1 ms; 2 MB; no network/file access; ~1/6 the cost of Lambda@Edge; viewer request/response; cache key normalisation, header manipulation, URL rewrites, simple auth) vs Lambda@Edge (Node.js/Python; thousands of req/s; up to 30 s; 128 MB for viewer, up to 10 GB for origin events; network + file access; viewer and origin; longer logic, external services, body access).

## Limits and quotas

- Memory 128 MB to 10,240 MB (CPU grows with memory).
- Timeout **900 s (15 min)**, cannot be raised.
- /tmp 512 MB to 10,240 MB.
- Environment variables 4 KB.
- Concurrency 1,000 per region by default (can be raised; new accounts start lower).
- Package 50 MB zipped, 250 MB unzipped; container images up to 10 GB.
- Payload: 6 MB synchronous, 1 MB asynchronous, 200 MB streamed response.

## Pricing (logic)

- Pay per request and per duration (1 ms increments); monthly free tier (1 million requests, 400,000 GB-seconds).
- Provisioned concurrency: paid. SnapStart: no extra cost.

## When to use it vs alternatives

- Job > 15 min, > 10 GB RAM or big image -> ECS/Fargate or AWS Batch ([[lambda-vs-batch]]). Orchestration -> [[step-functions]]. API -> [[api-gateway]].

## Exam traps

- A job longer than 15 minutes cannot run on Lambda.
- One function starving others = reserved concurrency.
- Cold starts = provisioned concurrency or SnapStart.
- Lambda in a VPC needs a NAT gateway for internet.
- Edge-optimised API certificates in us-east-1.

## See also

[[lambda-vs-batch]], [[api-gateway]], [[step-functions]], [[dynamodb]], [[rds]], [[sqs]], [[cloudfront]], [[ecs-fargate-ecr]]
