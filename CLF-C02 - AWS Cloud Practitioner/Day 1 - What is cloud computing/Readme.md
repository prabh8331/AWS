# AWS

Getting started - 
Go to 
https://portal.aws.amazon.com/
and sign up 


## What is Cloud Computing 

Cloud computing is the on-demand delivery of compute power, database storage, 
applications, and other IT resources

### The Deployment Models of the Cloud 

Private Cloud: provider - rackspace - cloud service used by single organization, not exposed to the public

Public Cloud: provider - AWS, GCP, Azure
cloud resources owned and operated by a third party cloud service provider delivered over the internet

Hybrid Cloud :
keep some server on premises and extend some capabilities to the cloud

### The 5 Characteristics of Cloud Computing

- On demand self service
- Broad network access
- Multi-tenancy and resource pooling
- Rapid elasticity and scalability
- Measured service  

### Six Advantages of Cloud Computing

- Trade capital expense (CAPEX) for operational expense (OPEX)
- Benefit from massive economics of scale
- Stop guessing capacity
- Increase speed and agility
- Stop spending money running and marinating data centers
- Go global in minutes

### Problems solved by the Cloud

- Flexibility
- Cost-Effectiveness
- Scalability
- Elasticity
- High-availability and fault-tolerance
- Agility

### Types of Cloud Computing

![Alt text](<Types of cloud computing.png>)

- Infrastructure as a Service (IaaS)
  - Provider building blocks for cloud IT
  - Provides networking, computers, data storage space
  - Highest level of flexibility
  - Easy parallel with traditional on-premises IT

        e.g.

        - Amazon EC2
        - GCP, Azure, Rackspace, Digital Ocean, Linode

- Platform as a Service (PaaS)
  - Removes the need for your organization to manage the underlying infrastructure 
  - Focus on the deployment and management of your applications

        e.g.

        - Elastic Beanstalk (on AWS)
        - Heroku, Google App Engine (GCP), Windows Azure (Microsoft)

- Software as a Service (SaaS)
  - Completed product that is run and managed by the service provider

        e.g.

        - Many AWS services (ex: Rekognition for Machine Learning)
        - Google Apps (Gmail), Dropbox, Zoom


Pricing of the Cloud
AWS has 3 pricing fundamentals, following the pay-as-you-go pricing model (solves the expensive issue of traditional IT)
- Compute
  - pay for compute time
- Storage
  - Pay for data stored in the Cloud
- Data transfer OUT of the cloud
  - Data transfer IN is free


### AWS Regions 

- AWS has Regions all around the world
- Names can be us-east-I, eu-west-3
- A region is a cluster of data centers
- Most AWS service are region-scoped


How to choose an AWS Region?

- Compliance with data governance and legal requirements: data never leaves a region without your explicit permission
- Proximity to customers: reduced latency 
- Available service within a Region: new services and new features aren't available in every Region
- Pricing: pricing varies to region and is transparent in the service pricing page 

AWS Availability Zones

- Each region has many availability zones (eg. ap-southeast-2a, ap-southeast-2b, ap-southeast-2c)
- Each availability zone (AZ) is one or more discrete data centers with redundant power, networking, and connectivity 
- They're separate from each other, so that they're isolated from disasters
- They're connected with high bandwidth, ultra-low latency networking

AWS Points of Presence (Edge Locations)

- Amazon has 400+ Points of Presence (400+ Edge Locations & 10+ Regional Caches) in 90+ cities cross 40+ countries
- COntent is delivered to end users with lower latency



### Tour of the AWS Console

1. AWS has Global services: 
   - Identity and Access Management (IAM)
   - Route 53 (DNS service)
   - CloudFront (Content Delivery Network)
   - WAF (Web Application Firewall)

2. Most AWS services are Region-scoped:
   - Amazon EC2 (Infrastructure as a Service)
   - Elastic Beanstalk (Platform as a Service)
   - Lambda (Function as a Service)


- chose region geographically close
- In top right shows Global/region selection
- check AWS service regionaly - https://aws.amazon.com/about-aws/global-infrastructure/regional-product-services/


### Shared Responsibility Model diagram

https://aws.amazon.com/compliance/shared-responsibility-model/

Customer (responsibility for security in the cloud)- 
- customer data
- platform, applications, identity & access management
- operation system, network and firewall configuration 
- client-side data encryption & data integrity
- server-side encryption (file system and/or data)
- networking traffic protection (encryption, integrity, Identity)


AWS (responsibility for security 'of' the cloud)
- Software
- compute
- storage
- database 
- networking
- hardware/aws global infrastructure 
- regions
- availability zones
- edge locations 


### AWS Acceptable Use policy 
https://aws.amazon.com/aup/

- No Illigal, harmful, or Offensive Use of content
- No Security violations 
- No Network Abuse
- No E-mail or other message Abuse