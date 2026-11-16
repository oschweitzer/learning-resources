---
tags:
  - aws
  - saa-c03
  - secure
status: to-review
source: notion
notion-module: "12. Serverless"
---

# Amazon Cognito

## What it is for

Authentication (user pools) and authorisation to AWS resources (identity pools) for web/mobile app users.

## Key concepts

- **User pools**: *who are you*; user sign-in; gives **JWT tokens**; password, reset, email/phone verification, MFA, federation (Google, Facebook, SAML, OIDC); integrates with API Gateway and ALB.
- **Identity pools**: *what can you access in AWS*; gives **temporary AWS credentials through STS** and an IAM role; identities from user pools, social logins, SAML or guest access; policies can use the user ID to restrict to the user's own S3 prefix or DynamoDB rows; direct calls to S3, DynamoDB, or through API Gateway.
- Mobile app with per-user storage: user pool for sign-in, API Gateway + Lambda + DynamoDB for the API, identity pool so users write directly to their own S3 prefix.

## Limits and quotas

- Nothing specific in my Notion notes.

## Pricing (logic)

- Nothing specific in my Notion notes.

## When to use it vs alternatives

- App user authentication -> user pools. Direct AWS access -> identity pools. Workforce -> [[identity-center-and-directory-service]]. Comparison -> [[cognito-user-vs-identity-pools]].

## Exam traps

- Temporary AWS credentials for app users come from identity pools, not user pools.

## See also

[[cognito-user-vs-identity-pools]], [[api-gateway]], [[iam]], [[identity-center-and-directory-service]]
