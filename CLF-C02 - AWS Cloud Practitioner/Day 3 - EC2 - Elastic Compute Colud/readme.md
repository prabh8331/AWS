# EC2 - Elastic Compute Cloud

## Amazon EC2

- EC2 is one of the most popular of AWS's offering 
- EC2  = Elastic Compute CLoud = Infrastructure as a service
- It mainly consists in the capability of:
  - Renting virtual machines (EC2)
  - Storing data on virtual drives (EBS)
  - Distributing load across machines (ELB)
  - Scaling the services using an auto-scaling group (ASG)
- Knowing EC2 is fundamental to understand how the Cloud works

### EC2 sizing & configuration options

- Operation System (OS): Linux, Windows or Mac OS
- How much compute power & cores (CPU)
- How much random-access memory (RAM)
- How much storage space:
  - Network-attached (EBS & EFS)
  - hardware (EC2 Instance Store)
- Network card: speed of the card, Public IP address
- Firewall rules: security group
- Bootstrap script (configure at first launch): EC2 User Data

### EC2 User Data (script for bootstrap)

- It is possible to bootstrap out instances using an EC2 User data script
- bootstrapping means launching commands when a machine starts
- That script is only run once at the instance first start
- EC2 user data is used to automate boot tasks such as  
  - Installing updates
  - Installing software
  - Downloading common files from the internet
  - Anything you can think of 
- The EC2 User data Script runs with the root user

### EC2 instance types examples

There are lot many EC2 instance for all the different uses there are 5 examples

| Instance | vCPU | Mem(GiB) | Storage | Network Performance | EBS Bandwidth (Mbps) |
| --- | --- | --- | --- | --- | --- | 
| t2.micro | 1 | 1 | EBS-Only | Low to Moderate | |
| t2.xlarge | 4 | 16 | EBS-Only | Moderate | |
| c5d.4xlarge | 16 | 32 | 1 x 400 NCMe SSD | Up to 10 Gbps | 4750 |
| r5.16xlarge | 64 | 512 | EBS Only | 20 Gbps | 13600 |
| m5.8xlarge | 32 | 128 | EBS Only | 10 Gbps | 6800 |

t2.micro is free tier

### Hands-On: Launching an EC2 Instance running Linux

open EC2 Console = Services --> EC2

1. Launch First EC2 instance
   Instances (Left hand side) --> Launch Instance
   1. Name and tags --> First_instance
   2. Application and OS Images (Amazon Machine Image) - Amazon Linux
        - AMI - Amazon Linux 2023 AMI (because free tier)
        - Architecture - 64-bit (x86)
   3. Instance type
        - t2.micro (1 vCPU, 1 GiB Memory)
   4. Key pair (login)
        - Create key pair
        - Key pair name - EC2_first
        - Key pair type - RSA
        - Private key file format - .pem
   5. Network setting 
        - Network - keep default
        - Subnet - keep default
        - Auto-assign ip - keep default
        - Firewall (Security groups) - this will automatically create new security group called - launch-wizard-1
            - [x] Allow SSH traffic from   (Anywere 0.0.0.0/0)
            - [ ] Allow HTTPS traffic from the internet 
            - [x] Allow HTTP traffic from the internet 
   6. Configure storage
        - 8 GiB gp3 Root volume
        - If click on Advance we will see of storage (volume) it will show Delete on termination is yes
   7. Advance Details
        - User data   (in end of Advance details option)
           - paste the following 

            ```bash

            #!/bin/bash
            # Use this for your user data (script from top to bottom)
            # install httpd (Linux 2 version)
            yum update -y
            yum install -y httpd
            systemctl start httpd
            systemctl enable httpd
            echo "<h1>Hello World from $(hostname -f)</h1>" > /var/www/html/index.html

            ```
    8. Summary 
   
   Launch instance

GO to --> Instances (Left hand side)

Select an Instance (which just created) --> this will show it details in bottom

to check webserver running on instance copy public ip address (3.95.64.114) and launch on browser - 
http://3.95.64.114/

if stop an instance and restart, then its public ip will change

### EC2 Instance Types - Overview

Can use different types of EC2 instances that are optimised for different use cases https://aws.amazon.com/ec2/instance-types/

AWS naming convention of EC2 instance:
e.g.  m5.2xlarge

m: instance class
5: generation (AWS improves them over time)
2xlarge: size within the instance class

compare different instance here - https://instances.vantage.sh/

https://www.ec2instances.info/

Types of EC2 Instance - 

1. General Purpose - Great for a diversity of workloads such as web serves or code repositories, 

- balance between - compute, memory and Networking, 
- eg. Mac, T4g, T3, T2, M6g, M5, M5a, M5zn, M4, A1  (t2.micro)

2. Compute Optimized - Great for compute-intensive tasks that require high performance processors, 

- used for - batch processing workloads, media transcoding, high performance web servers, high performance compute (HPC), scientific modeling & machine learning, dedicated gaming servers, 
- e.g. C6g, C6gn, C5, C5a, C5n, C4

4. Memory Optimized - Fast performance for workloads that process large data sets in memory, 

- used for - High performance relational/non-relation databases, Distributed web scale cache stores, in-memory databases optimized for BI, Applications performing real-time processing of big unstructured data
- e.g. R6g, R5, R5a, R5b, R5n, R4, X1e, X1, High Memory, z1d


5. Accelerated Computing


6. Storage Optimized - Great for storage-intensive tasks that require high, sequential read and write access to large data sets on local storage

- used for - High frequency online transaction processing (OLTP) systems, Relational & NoSql databases, Cache for in-memory databases (for example, Redis), Data warehousing applications, Distributed file system
- e.g. I3, I3en, D2, D3, D3en, H1

7. HPC Optimized
8. Instance Feature
9. Measuring Instance Performance 


### Introduction to Security Groups

1. Security groups are fundamental of network security in AWS, acting as a firewall on EC2 instances and they can attached to multiple instance

each region/ VPC has separate security group, in change region / VPC need to create new rule

security groups are outside from the EC2 instance it is septate layer

2. They control how traffic is allowed into or out of EC2 instances, they regulate - Access to Ports, Authorized IP ranges (IPv4 and Ipv6), control inbound network (other to instance), control outbound network (instance to other)

All inbound traffic is blocked by default
All outbound traffic is authorized by default


3. Security groups only contain allow rules
4. Security groups rules can reference by IP or by security group



- If your application is not accessible (time out), then it’s a security group issue
- If your application gives a “connection refused“ error, then it’s an application error or it’s not launched
- It’s good to maintain one separate security group for SSH access

Classic Ports to Know- 
22 = SSH (Secure Shell) - log into a Linux instance
21 = FTP (File Transfer Protocol) – upload files into a file share
22 = SFTP (Secure File Transfer Protocol) – upload files using SSH
80 = HTTP – access unsecured websites
443 = HTTPS – access secured websites
3389 = RDP (Remote Desktop Protocol) – log into a Windows instance

### Security Group hands on

security group - inbound rule - a way of exposing a port to internet

1. Instance --> security --> can see small idea of security group attached to this instance
2. EC2 console --> Network & Security --> Security Groups
   - here we can see all the security groups, and see there inbound and outbound rules

### SSH Overview

SSH into your EC2 instance - 

while creating EC2 instance we created key pair and downloaded .pem file

copy this .pem file and paste it on the linux server

now from the instance copy the public ipv4 - 34.235.155.192

ec2-user is defaut user setup while creating instance

ssh ec2-user@34.235.155.192
this will fail 


ssh -i EC2_first.pem ec2-user@34.235.155.192


if get permission error use - 
chmod 0400 EC2_first.pem

### EC2 Instance connect

GO to --> Instances --> select the instance --> click "Connect" on top --> connect using EC2 instance connect --> connect

if i close 22 port from inbound rule then ssh/ec2 instance connect will not work



### EC2 Instance Roles Demo

To run aws command from EC2 instance we need to provide this EC2 instance with permissions

```bash 

aws iam list-users

    # Unable to locate credentials. You can configure credentials by running "aws configure".
    # but we don't have to do aws configure and put access keys this is not good idea
    # because anyone on this account can connect to EC2 instance and retrieve the values of IAM APA key
```

Instead we use IAM Roles

IAM > ROles > 
we already have a role created called EC2TestRole which have IAMReadOnlyAccess

Attach roles to EC2 instance 
EC2 Console > Instances > select the instance > Security    -- this will show there is not IAM role
EC2 Console > Instances > select the instance > Action > Security > Modify IAM role > choose IAM role (EC2TestRole) > Save

```bash 

aws iam list-users
# now this will work
```


### EC2 Instance Purchasing Options

1. On-Demand instance - short workload, predictable pricing, pay by second
2. Reserved (1 to 3 years)

     - Reserved Instance - Long workloads
     - Convertible Reserved Instance - Long workloads with flexible instances

3. Saving Plans
4. Spot Instance - short workloads, cheap, can lose instances (less reliable)

    - Most cost-efficient
    - Batch jobs
    - Data analysis
    - Image processing
    - Any distributed workloads
    - Workloads with flexible start and end time

    **Not suitable for critical jobs or databases**

5. Dedicated Hosts - book an entire physical server , control instance placement

    - most expensive option
    - allows address compliance requirements and use existing server- bound software licenses (per-socket, per-core, pe- vm software licenses)
    - useful for software with complicated licensing model
    - or compaines have strong regulatory or compliance needs

6. Dedicated Instance - on other customer will share your hardware
    - Instance run on hardware that's dedicated to you
    - May share hardware with other instance in same account
    - no control over instance placement

7. Capacity Reservation- reserve capacity in a specific AZ for any duration


### Shared responsibility model

AWS

- infrastructure (global network secutiry)
- Isolation on physical hosts
- replacing faulty hardware
- compliance validation

User

- security groups rules
- operation-system patches and updates
- software and utilities installed on the EC2 instance
- IAM roles assigned to EC2 & IAM user access management
- Data security on your instance


