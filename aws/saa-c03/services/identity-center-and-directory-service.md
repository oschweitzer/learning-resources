---
tags:
  - aws
  - saa-c03
  - secure
status: to-review
source: notion
notion-module: "17. Advanced identity and governance"
---

# IAM Identity Center and Directory Service

## What it is for

Workforce SSO across accounts (Identity Center) and Active Directory in/with AWS (Directory Service).

## Key concepts

- **IAM Identity Center** (formerly AWS SSO): one login for all accounts of the organisation, SAML 2.0 business apps (Salesforce, Microsoft 365, Box) and EC2 Windows instances. Identity sources: built-in store or external (Active Directory, Okta, Microsoft Entra ID, OneLogin). **Permission sets** = collections of IAM policies assigned to users/groups per account (roles created in each account). ABAC with attributes from the source. Recommended way to give workforce users access to many accounts (instead of IAM users per account).
- **Directory Service**: *AWS Managed Microsoft AD* (a real AD managed by AWS, MFA, **trusts** with on-premises AD); *AD Connector* (a proxy forwarding authentication to your on-premises AD, no copy); *Simple AD* (AD-compatible, Samba-based, no trust, small standalone).
- Identity Center integrates out of the box with AWS Managed Microsoft AD; for a self-managed AD: two-way trust with Managed AD, or AD Connector.

## Limits and quotas

- Nothing specific in my Notion notes.

## Pricing (logic)

- Nothing specific in my Notion notes.

## When to use it vs alternatives

- Workforce across accounts -> Identity Center; app users -> [[cognito]]; existing AD without replication -> AD Connector. Comparison -> [[directory-service-options]].

## Exam traps

- Existing on-premises AD without a copy in AWS = AD Connector.

## See also

[[iam]], [[organizations-and-scp]], [[directory-service-options]], [[cognito]], [[control-tower]]
