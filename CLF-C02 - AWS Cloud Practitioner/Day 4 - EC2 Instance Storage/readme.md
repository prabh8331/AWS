# EC2 Instance Storage Section

•EBS volumes:
    •network drives attached to one EC2 instance at a time
    •Mapped to an Availability Zones
    •Can use EBS Snapshots for backups / transferring EBS volumes across AZ
•AMI: create ready-to-use EC2 instances with our customizations
•EC2 Image Builder : automatically build, test and distribute AMIs
•EC2 Instance Store:
    •High performance hardware disk attached to our EC2 instance
    •Lost if our instance is stopped / terminated
•EFS: network file system, can be attached to 100s of instances in a region
•EFS-IA: cost-optimized storage class for infrequent accessed files
•FSx for Windows: Network File System for Windows servers
•FSx for Lustre: High Performance Computing Linux file system



## What's an EBS Volume

- EBS (Elastic BLock Store) volume is a network drive (not a physical drive), we can attach to instance while they run
      - It uses network to communicate instance, (so there might be a bit of latency)
      - It can be detached from an EC2 instance and attached to another one
- It allows your instance to persist data, even after their termination
- They can only be mounted to one instance at a time
- They are bound to a specific availability zone
      - If EBS volume is us-east-1a it can't used in us-east-1b
      - to move a volume across, need to take snapshot
- Have a provisioned capacity (size in GBs and IOPS)
      - You get billed for all the provisioned capacity
      - YOu can increase the capacity of the drive over time

EBS multi - attach - (FOr io1 and io2 volume types, it can be attached to multiple instance)


EBS - Delete on Termination attribute

- Controls the EBS behavior when an EC2 instance terminates
      - By default, the root EBS volume is deleted (attribute enabled)
      - By default, any other attached EBS volume is not deleted (attribute disabled)
- This can be controlled by the AWS console / AWS CLI
- Use case: preserve root volume when instance is terminated



### ESB hands on

**see volume attached with EC2 instance**
EC2 console --> Instance --> select instance --> volume --> it will show there is a root volume attached with instance

**See all the volumes**
EC2 console --> Elastic Block Store --> Volumes --> can see the above volume here

**create a new volume**
EC2 console --> Elastic Block Store --> Volumes --> Create Volume

Volume settings-

1. Volume type --> General Purpose SSD (gp3)
2. Size (GiB) - 2
3. IOPS- default (3000)
4. Throughput (MiB/s)  - (125)
5. Availability Zone - eu-north-1b  (same as EC2 instance)
6. Snapshot ID - DOn't create volume from a snapshot

Create Volume


**Attach volume to EC2 Instance**

EC2 console --> Elastic Block Store --> Volumes --> Select volume --> Actions --> Attach Volume --> 

1. Instance -- select First_instance
2. Device name -- /dev/sdf

Attach volume


**Check if attached**

EC2 console --> Instance --> select Instance --> Storage --> Block device --> here it will show both volume



If we create a volume to different availability zone then we can not attach it to EC2 instance

If we terminate the EC2 instance the root volume will also be gone


### EBS Snapshots

- Can make a backup (or snapshot) of EBS volume at a point in time
- Not necessary to detach volume to do snapshot, but recommended
- can copy snapshots across AZ or Region

EBS Snapshots Features

- **EBS Snapshot Archive**
        - Move a Snapshot to an "archive tier" that is 75% cheaper
        - Takes within a 24 to 72 hours for restoring the archive
- **Recycle Bin** for EBS Snapshots
        - Setup rules to retain deleted snapshots so you can recover them after an accidental deletion
        - Specify retention (from 1 day to 1 year)

Hands on

EC2 console --> Elastic Block Store --> Volumes --> Select volume --> Actions --> Create snapshot

Description -> Demosnapshort-20240425

EC2 console --> Elastic Block Store --> Snapshots --> we can see this new snapshot


**Copy snapshot to another region**

EC2 console --> Elastic Block Store --> Snapshots --> select snapshots --> action --> copy snapshot

Description - [Copied snap-0cd3830327c2545fc from eu-north-1] Demosnapshort-20240425

Destination Region- eu-west-3  (any region you want good for degistor managment stategy)


**recreate volume from snapshot**

EC2 console --> Elastic Block Store --> Snapshots --> select snapshots --> action --> create volume from snapshot

1. Volume type 
2. size
3. IOPS
4. Throughput
5. Availability zone - eu-north-1b

Create Volume 


**Recycle bin**

EC2 console --> Elastic Block Store --> Snapshots --> Recycle bin

Create retention rule

1. Retention rule name --> DemoRetention
2. Resource type --> EBS snapshots
3. [x] Apply to all resouces
4. Retention period - 1 day


EC2 console --> Elastic Block Store --> Snapshots --> select snapshots --> actions --> delete snapshots

Recycle bin COnsole --> Resources --> can see deleted snapshots 
select snapshot --> recover 
now an see it in Snapshots page again

**Archive snapshot**

EC2 console --> Elastic Block Store --> Snapshots --> select snapshots --> actions --> Archiving -- >Archive snapshot

this will move snapshot to Archive which is different pricing tier (but will take some time to restore)


### AMI Overview

- AMI = Amazon Machine Image
- AMI are customization of an EC2 Instance 
      - add software, configuration, operating system, monitoring
      - Faster boot / configuration time because all your software is pre-packaged
- AMI are built for a specific region (and can be copied across regions)
- You can launch EC2 instances from
      - A public AMI: AWS provided
      - Your own AMI: you make and maintain them yourself
      - An AWS Marketplace AMI: an AMI someone else made (and potentially sells)

**AMI Process (from an EC2 instance)**

1. Start an EC2 instance and customize it
2. Stop the instance (for data integrity)
3. Build an AMI - this will also create EBS snapshots
4. Launch instance from other AMIs


**Hands ON**

Launch a new EC2 instance -->

only paste following in the user data
```bash
#!/bin/bash
# Use this for your user data (script from top to bottom)
# install httpd (Linux 2 version)
yum update -y
yum install -y httpd
systemctl start httpd
systemctl enable httpd
```

right click new EC2 instance --> Image and templets --> create image

1. Image name --> AMIdemo
2. keep other default

create image

EC2 console --> Images --> AMIs --> this will show AMIdemo

EC2 console --> Instances --> Launch Instances

1. Name- ForAMI
2. Application and OS Images (Amazon Machine Image) ---> MyAMIs --> select AMIdemo
3. Key pair --> select keypair
4. Network settings --> Firewall (secutiy group) --> select existing secutiry group --> launch-wized-1
5. user data

```bash
#!/bin/bash
# Use this for your user data (script from top to bottom)
# install httpd (Linux 2 version)

echo "<h1>Hello World from $(hostname -f)</h1>" > /var/www/html/index.html
```

Lanuch Instance 


this will have faster boot time becuse we are not installing httpd


### EC2 Image Builder

- used to automate the creation of Virtual Machines or container images
- Automate the creation, maintain, validate and test EC2 AMIs
      - EC2 Image Builder
        - create a new EC2 Instance --> build components applied (customize software on instance)
        - create a New AMI 
        - Test EC2 instance --> Test suite in run (is the AMI working, secure?) 
        - AMI is distributed (can be multiple regions)
- Can be run on schedule (weekly, whenever packges are updated etc)
- Free service ( only pay for the underlying resources)


### EC2 Instance Store

- EBS volumes are network drives with good but limited performance
- If need a high-performance hardware disk, use EC2 Instance Store

- Better I/O performance 
- EC2 Instance Store lose their storage if they're stopped (ephemeral)
- Good for buffer/ cache/ scratch data / temporary content
- Risk of data loss if hardware fails
- Backups and Replication are your responsibility


### EFS - Elastic File System

- Managed NFS (network file system) that can be mounted to 100s of EC2
- EFS works with Linux EC2 instances in multi-AZ
- Highly available, scalable, expensive (3x gp2), pey per use, no capacity planning

EFS Infrequent Access (EFS-IA)

- Storage class that is cost-optimized for files not accessed every day
- Up to 92% lower cost compared to EFS standard
- EFS will automatically move your files to EFS-IA based on the last time they were accessed
- Enable EFS-IA with a Lifecycle Policy , Example- move files that are not accessed for 60 days to EFS-IA
- Transparent to the applications accessing EFS


### Shared Responsibility Model for EC2 storage  

AWS

- Infrastructure
- Replication for data for EBS volumes & EFS drives
- Replacing faulty hardware
- Ensuring their employees cannot access your data

Customer

- Setting up backup/ snapshot procedures
- Setting up data encryption
- Responsibility of any data on the drives
- Understanding the risk of using EC2 instance store

### Amazon FSx - Overview

Launch 3rd party high-performance file systems on AWS
Fully Managed service 

FSx For Lustre, FSx for Windows file Server, FSx for NetApp ONTAP

1. Amazon FSx for Windows File Server
     - A fully managed, highly reliable, and scalable Windows native shared file system
     - Built on Windows File Server
     - Supports SMB protocol & Windows NTFS
     - Integrated with Microsoft Active Directory
     - Can be accessed from AWS (EC2 instance) or your on-premise infrastructure (Windows client)

2. Amazon FSx for Luster
      - A fully managed, high-performance, scalable file storage for High Performance Computing (HPC)
      - Luster = Linux + Cluster
      - Machine Learning, Analytics, Video Processing, Financial modeling
      - Scales up to 100s GB/s millions of IOPS, sub-ms latencies 





### Section Cleanup

EC2 console --> EC2 Dashboard --> see all the resources running 

Instance --> Terminate 
Volumes --> Delete
AMIs--> Deregister 
snapshot - Delete