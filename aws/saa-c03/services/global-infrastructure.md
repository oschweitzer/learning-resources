---
tags:
  - aws
  - saa-c03
  - secure
status: to-review
source: notion
notion-module: "01. Global infrastructure and IAM"
---

# AWS global infrastructure and the shared responsibility model

## What it is for

Choose where to deploy (region, AZ) and know who is responsible for what.

## Key concepts

- A **region** is a cluster of data centres in one geographic area (e.g. eu-west-3 Paris, us-east-1 N. Virginia). Most services are regional.
- An **Availability Zone (AZ)** is one or more discrete data centres with redundant power, networking and connectivity. AZs are isolated from each other but linked by high-bandwidth, low-latency networking.
- **Points of presence** (edge locations + regional edge caches): more than 400. Used by CloudFront, Route 53 and Global Accelerator.
- **Local Zones** put compute close to large cities. **Wavelength** puts it inside 5G networks. **Outposts** brings AWS racks into your own data centre.
- Criteria for choosing a region: compliance, latency, service availability, pricing.
- Global services: IAM, Route 53, CloudFront, WAF (with CloudFront). Regional services: EC2, Elastic Beanstalk, Lambda, Rekognition, most of the catalogue.
- **Shared responsibility**: AWS = security *of* the cloud (hardware, facilities, network, hypervisor, managed parts of managed services). You = security *in* the cloud (data, IAM, encryption settings, security groups, OS patching on EC2, application code).
- The line moves with the service: EC2 -> you patch the OS; RDS -> AWS patches the engine, you control network access, encryption and users; S3 -> you own bucket policies and encryption settings.

## Limits and quotas

- A region usually has three AZs: never fewer than three, up to six.

## Pricing (logic)

- Prices differ from region to region (a region selection criterion).

## When to use it vs alternatives

- Compute in your own data centre -> Outposts; inside 5G networks -> Wavelength; close to a large city -> Local Zones.

## Exam traps

- Data never leaves a region unless you move it.
- New services and features do not reach every region on day one.

## See also

[[iam]], [[other-services]], [[dr-and-multi-region]]
