# 1. What is Cloud Computing?
Cloud computing means delivering computing services — servers, storage, databases, networking, software — over the internet (the cloud) on a pay-as-you-go basis.

Instead of buying and maintaining physical hardware, you rent computing resources from a cloud provider like AWS, Azure, or GCP.

Traditional IT                     Cloud Computing
────────────────                   ─────────────────
Buy hardware upfront               Pay per use
Weeks to provision                 Minutes to provision
You manage everything              Provider manages hardware
Fixed capacity                     Elastic (scale up/down)
High CapEx                         Low OpEx

## Key characteristics of cloud computing (NIST definition):

- On-demand self-service — provision resources without human interaction from provider
- Broad network access — access via standard mechanisms (internet, phone, tablet)
- Resource pooling — shared infrastructure across multiple customers (multi-tenancy)
- Rapid elasticity — scale up and down quickly as demand changes
- Measured service — pay only for what you use
_______________________________________________________________________________________________________
# 2. Why Cloud Computing?

Problems with Traditional Data Centers
```
Traditional DC Pain Points:
┌────────────────────────────────────────┐
│ 1. Capital Expense (servers, racks)    │
│ 2. Space, power, cooling costs         │
│ 3. Long procurement time (weeks/months)│
│ 4. Over-provisioning (waste)           │
│ 5. Under-provisioning (outages)        │
│ 6. Maintenance burden on your team     │
│ 7. Disaster recovery is expensive      │
└────────────────────────────────────────┘
```
Cloud Benefits

| Benefit	| Description |
|---|---|
| Trade CapEx for OpEx	| No upfront hardware investment |
| Massive economies of scale	| AWS buys at huge volume, passes savings to you | 
| Stop guessing capacity	| Scale exactly as needed |
| Speed and agility	| Launch globally in minutes |
| Go global in minutes	| Deploy to multiple regions instantly |
| Focus on business	| AWS handles undifferentiated heavy lifting |
_______________________________________________________________________________________________________
# 3. Cloud Service Models

These are the three fundamental service models. Think of them as layers of responsibility:
```
┌──────────────────────────────────────────────────────────┐
│                     SaaS                                 │
│         (Gmail, Salesforce, Office 365)                  │
│   Provider manages everything. You just USE the app.     │
├──────────────────────────────────────────────────────────┤
│                     PaaS                                 │
│       (Elastic Beanstalk, Heroku, App Engine)            │
│   You manage: App code, Data                             │
│   Provider manages: OS, runtime, servers, networking     │
├──────────────────────────────────────────────────────────┤
│                     IaaS                                 │
│          (AWS EC2, Azure VM, Google Compute)             │
│   You manage: OS, runtime, app, data                     │
│   Provider manages: Physical hardware, virtualization    │
└──────────────────────────────────────────────────────────┘
```
## Detailed Breakdown

IaaS — Infrastructure as a Service
- You get raw compute, storage, and networking
- You install your own OS, middleware, and apps
- Maximum control, maximum responsibility
- Example: Launching an EC2 instance and installing your own web server

PaaS — Platform as a Service
- You get a managed platform (OS + runtime pre-configured)
- You deploy your application code
- Moderate control, less management overhead
- Example: Deploying a Python app to AWS Elastic Beanstalk

SaaS — Software as a Service
- You get a fully managed application
- No infrastructure to manage at all
- Minimum control, zero management overhead
- Example: Using Gmail, Slack, or AWS WorkMail

# 4. Cloud Deployment Models

Public Cloud

- Infrastructure is owned and operated by a third-party cloud provider (AWS, Azure, GCP)
- Shared infrastructure across many customers
- You access resources over the internet
- Best for: startups, variable workloads, cost-sensitive applications

Private Cloud

- Infrastructure is dedicated to a single organization
- Can be on-premises or hosted by a provider
- Complete control and isolation
- Best for: highly regulated industries (banking, healthcare), compliance requirements

Hybrid Cloud

- Mix of public and private cloud
- Data and apps can move between private and public cloud
- Common pattern: keep sensitive data on-prem, run workloads in public cloud
```
Hybrid Cloud Architecture:
┌──────────────────┐          ┌──────────────────┐
│   On-Premises    │          │   AWS (Public)   │
│   Private Cloud  │◄────────►│   Cloud          │
│                  │  VPN /   │                  │
│ ■ Sensitive DB   │ Direct   │ ■ Web servers    │
│ ■ Legacy apps    │ Connect  │ ■ Dev/test envs  │
│ ■ Compliance data│          │ ■ Analytics      │
└──────────────────┘          └──────────────────┘
```
Multi-Cloud

- Using multiple cloud providers simultaneously
- Avoids vendor lock-in
- Example: Running workloads on both AWS and Azure
_________________________________________________________________________________________________________________

# 5. AWS Global Infrastructure
AWS has the largest cloud infrastructure in the world. It is organized into:
```
AWS Global Infrastructure:
─────────────────────────
Regions
  └── Availability Zones (AZs)
        └── Data Centers
              └── Servers, Storage, Networking
```
By the Numbers (approximate, as of 2025)
- 33+ Regions across the world
- 105+ Availability Zones
- 600+ Edge Locations (CloudFront PoPs)
- 200+ services available
___________________________________________________________________________________________________________________

# 6. Regions and Availability Zones

What is a Region?
>A Region is a geographic area where AWS has a cluster of data centers. Each region is completely independent — a failure in one region does not affect another.

Examples of AWS Regions:
```
us-east-1        → N. Virginia (oldest, most services)
us-west-2        → Oregon
eu-west-1        → Ireland
ap-south-1       → Mumbai
ap-southeast-1   → Singapore
```
How to choose a region?

- Latency — pick the region closest to your users
- Compliance — data residency laws (EU data stays in EU)
- Service availability — not all services are in all regions
- Pricing — varies by region (us-east-1 is usually cheapest)

What is an Availability Zone (AZ)?
>An AZ is one or more discrete data centers within a Region, each with:

- Redundant power
- Networking
- Connectivity

AZs within a region are connected via low-latency links but are physically separate enough to be isolated from failures.
```
Region: us-east-1 (N. Virginia)
┌─────────────────────────────────────────────────┐
│                                                 │
│  ┌──────────────┐  ┌──────────────┐  ┌──────┐  │
│  │  AZ: us-east │  │  AZ: us-east │  │  AZ  │  │
│  │    -1a       │  │    -1b       │  │  -1c │  │
│  │              │  │              │  │      │  │
│  │ Data Center 1│  │ Data Center 2│  │  DC3 │  │
│  └──────────────┘  └──────────────┘  └──────┘  │
│        ◄──── Low-latency links ────►            │
└─────────────────────────────────────────────────┘
```
> Best Practice: Always deploy across at least 2 AZs for high availability. If one AZ fails, your app keeps running in the other.
_______________________________________________________________________________________________________________________
# 7. Edge Locations and CloudFront

## What is an Edge Location?
An Edge Location is a site where AWS caches content for fast delivery to end users. These are part of AWS CloudFront (CDN).

- Located in major cities worldwide (600+ locations)
- Much more numerous than regions
- Content is cached here, closer to users
```
User in Sydney                  AWS Origin (us-east-1)
       │                                │
       ▼                                │
┌─────────────────┐                     │
│  Edge Location  │◄────────────────────┘
│  Sydney, AUS    │   Content cached here
│  (CloudFront PoP)│
└─────────────────┘
       │
       ▼
  Fast response to user (ms latency vs seconds from US)
```
________________________________________________________________________________________________________________________
# 8. AWS Pricing Models
AWS pricing follows several models. Choose the right one to optimize costs.

## On-Demand
- Pay by the second/hour with no commitment
- Most expensive per unit
- Best for: unpredictable workloads, short-term experiments

## Reserved Instances (RI)
- Commit to 1 or 3 years in exchange for up to 72% discount
- Types:
- - Standard RI — fixed instance type, biggest discount
- - Convertible RI — can change instance type, smaller discount
- Best for: steady-state workloads, production databases

## Spot Instances
- Bid on unused AWS capacity — up to 90% cheaper than On-Demand
- AWS can reclaim with 2-minute warning
- Best for: batch processing, CI/CD, stateless workloads

## Savings Plans
- Flexible pricing model — commit to a dollar/hour spend for 1-3 years
- Applies automatically to eligible usage
- Best for: flexible teams that mix instance types

## Dedicated Hosts
- Physical server dedicated to you
- Required for regulatory compliance or BYOL (bring your own license)
- Most expensive option

```
Cost (High → Low):    On-Demand > Reserved > Savings Plans > Spot
Control (High → Low): Dedicated > On-Demand > Reserved > Spot
```
__________________________________________________________________________________________________________________________
# 9. AWS Free Tier

AWS offers free usage for new accounts for 12 months on many services:

| Service |	Free Tier |
|---|---|
| EC2	| 750 hours/month of t2.micro or t3.micro |
| S3	| 5 GB storage, 20,000 GET requests |
| RDS	| 750 hours/month of db.t2.micro |
| Lambda	| 1 million requests/month (always free) |
| CloudFront   | 1 TB data transfer out/month |

> Warning: Always set up billing alerts when using AWS — unexpected charges are common for beginners who forget to stop resources.
__________________________________________________________________________________________________________________________
# 10. AWS Management Console, CLI, and SDK

## Three Ways to Interact with AWS
1. AWS Management Console (Web UI)
- Browser-based GUI at console.aws.amazon.com
- Best for exploration and learning
- Visual representation of resources

2. AWS CLI (Command Line Interface)
- Command-line tool for managing AWS services
- Install
```
# Linux
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
unzip awscliv2.zip && sudo ./aws/install
```
- aws configure
# Prompts for: Access Key ID, Secret Key, Region, Output format
