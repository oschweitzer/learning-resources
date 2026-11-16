---
tags:
  - aws
  - saa-c03
  - resilient
  - performant
status: to-review
source: notion
notion-module: "16. Monitoring and audit"
---

# Amazon CloudWatch (metrics, logs, alarms)

## What it is for

Observe performance, aggregate logs, alert. Answers 'how is it performing?'.

## Key concepts

- **Metrics**: every service publishes metrics; namespaces, dimensions (30 max), timestamps; custom metrics at standard (1 min) or high resolution (down to 1 s). **EC2: RAM and disk are not in the default metrics** -> CloudWatch agent. Cross-region/account dashboards. **Metric Streams**: near real-time to Firehose or third parties (Datadog, Dynatrace, New Relic, Splunk).
- **Logs**: log groups (often one per application) -> log streams. Retention 1 day to 10 years or never expire; encrypted by default. Sources: SDK, agent, Beanstalk, ECS, Lambda, VPC Flow Logs, API Gateway, CloudTrail, Route 53 query logs. **Logs Insights** = a query language (not real time). **S3 export** (CreateExportTask) up to 12 h: not real time. **Subscription filters**: real time to Kinesis Data Streams, Firehose, Lambda; cross-account through a destination + access policy (centralise logs). **Metric filters**: log patterns -> metrics -> alarms.
- From EC2/servers: nothing by default -> install the **unified agent** (logs + system metrics: RAM, swap, disk, processes, netstat) with IAM permissions; the old Logs agent is legacy.
- **Alarms**: states OK, INSUFFICIENT_DATA, ALARM; period 10 or 30 s (high resolution) or multiples of 60. Targets: EC2 actions (stop, terminate, reboot, recover), Auto Scaling, SNS, Systems Manager. **Composite alarms** (AND/OR) reduce noise. **EC2 recovery**: on a failed system status check, moves the instance to new hardware keeping private/public/Elastic IPs, metadata, placement group. Test: `aws cloudwatch set-alarm-state`.
- **Other**: Container Insights (ECS, EKS, Kubernetes), Lambda Insights, **Contributor Insights** (top-N, e.g. IPs generating most traffic in Flow Logs), Application Insights, Network Synthetic Monitor (ICMP/TCP probes VPC -> on premises).
- Outside CloudWatch: X-Ray (traces across distributed services), Health Dashboard (AWS events affecting your resources), Managed Grafana and Managed Service for Prometheus.

## Limits and quotas

- Dimensions: 30 max per metric.
- Log retention: 1 day to 10 years, or never expire.
- S3 export up to 12 h.

## Pricing (logic)

- Nothing specific in my Notion notes.

## When to use it vs alternatives

- Performance/logs/alerts -> CloudWatch; who did what -> [[cloudtrail]]; configuration compliance -> [[config]]. Comparison -> [[cloudwatch-vs-cloudtrail-vs-config]].

## Exam traps

- RAM on EC2 = CloudWatch agent.
- Real-time log processing = subscription filters, not S3 export.

## See also

[[cloudtrail]], [[config]], [[eventbridge]], [[cloudwatch-vs-cloudtrail-vs-config]], [[vpc-flow-logs-and-network-firewall]], [[auto-scaling]]
