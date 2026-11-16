---
tags:
  - aws
  - saa-c03
  - comparatif
  - secure
status: to-review
source: notion
notion-module: "12. Serverless"
---

# Cognito user pools vs identity pools

Authentication vs authorisation to AWS.

|  | User pools | Identity pools |
| --- | --- | --- |
| Purpose | Authentication (sign-in) of app users | Authorisation to access AWS resources |
| Gives | JWT tokens | Temporary AWS credentials (STS + IAM role) |
| Features | Password, reset, verification, MFA, federation (Google, Facebook, SAML, OIDC) | Identities from user pools, social logins, SAML or guest; policies by user ID |
| Integrates with | API Gateway, ALB | Direct calls to S3, DynamoDB, or through API Gateway |

## Decision rule

- Sign in -> user pools. Direct access to S3/DynamoDB -> identity pools.

## Exam traps

- Temporary AWS credentials = identity pools.

## See also

[[cognito]]
