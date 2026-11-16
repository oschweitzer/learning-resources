---
tags:
  - aws
  - saa-c03
  - secure
status: to-review
source: notion
notion-module: "17. Advanced identity and governance"
---

# AWS Control Tower

## What it is for

Set up and govern a secure, compliant multi-account environment (landing zone) in a few clicks, on top of Organizations.

## Key concepts

- Creates a **Security OU** with a *Log Archive* account (central logs) and an *Audit* account (security team access); usually configures IAM Identity Center; **Account Factory** creates new compliant accounts.
- **Controls (guardrails)**: *preventive* (SCPs, e.g. restrict regions), *detective* (Config rules, e.g. untagged resources, public buckets), *proactive* (CloudFormation hooks, block a non-compliant resource before deployment).
- Compliance dashboard across accounts; detects and remediates drift.

## Limits and quotas

- Nothing specific in my Notion notes.

## Pricing (logic)

- Nothing specific in my Notion notes.

## When to use it vs alternatives

- Landing zone + ongoing governance -> Control Tower; manual guardrails -> [[organizations-and-scp]].

## Exam traps

- Detective = Config; preventive = SCP; proactive = CloudFormation hooks.

## See also

[[organizations-and-scp]], [[config]], [[identity-center-and-directory-service]], [[cloudformation]]
