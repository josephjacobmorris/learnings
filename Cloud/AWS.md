# AWS (Amazon Web Services)

## Region

Region is chosen based on different factors such as 

* Compilance
* Latency
* Availability of services
* Pricing

## Availability zones

each region will have 3 to 6 availability zones connected by high speed n/w.

## Types of Services

### Global Services

* IAM
* Route 53 (DNS service)
* CloudFront (CDN) 
* WAF (Web application firewall)

### Regional Services

* EC2 (IaaS)
* Lambda (FaaS)
* Elastic Beanstalk (PaaS)
* Rekognition (SaaS)

## Services

All aws service are governed by shared responsibility model. AWS ensures the security of infrastructure and what they are providing.
It is the user's responsibility to configure things correctly.

### IAM (Identity Access Management)

This aws service is used for authentication. Users and Groups can be created. Groups are similar to roles in `SQL`. Users w/o group also can be created. Users can also belong to multiple groups.
IAM Security Tools

* IAM Credentials Report (account-level)
* IAM Access Advisor (user-level)

### EC2 (Elastic Cloud Compute)

EC2 instances have aws cli installed on them but never provide aksk 
as these might be accessible to other users in your account.
In AMAZON Linux `ec2-user` is the default user configured and 
Use `.pem` (Supported by `ssh` command) format key for windows > 10, linux and Mac. 
Use `.ppk` (Supported by putty) format key for windows < 10. 

Only instances in the running state incur costs.

>**Note:** The public IP of an instance might change after restarting the instance but private IP
won't change

#### EC2 Instance Types

> INFO: <br>
>Naming Convention : m5.2xlarge<br>
>m denotes instance class <br>
>5 denotes generation (Hardware updates etc) <br>
>2xlarge all resources are 2x the normal<br>

| Name                         | Uses                                                                                                                                                                                                                | Examples        |
|------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------|
| General Purpose    Instances | Balance between compute,memory ,Networking . Ideal for web servers and code repositories                                                                                                                            | t3,t2 etc       |
| Compute Optimized Instances  | High performance processors suitable for batch processing workloads, Media transcoding, High performance web servers, High performance computing, Scientific modelling & Machine learning, Dedicated gaming servers | c6g,c6gn,C5 etc |
| Memory Optimized Instances   | High performance relation/non-relational databases, Distributed cache stores, In-memory databases optimized for business intelligence  , Apps for processing big unstructured data                                  | R6g,R5 etc      |
| Storage Optimized Instances  | OLTP, Relational /  non-relational databases, DFS, cache for in-memory databases, Data warehousing applications                                                                                                     | I3,I3en etc     |


#### EC2 Purchasing Options
| Type                      | Description                                                                                                                                                                                              |
|---------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| On-Demand                 | Pay as we use, predictable pricing                                                                                                                                                                       |
| Reserved                  | with 1 or 3 years reserved (Instance type, region, tenancy, os ), very cheap compared to on-demand(based on payment upfrontness and longer reservation period)                                           |
| Savings Plan              | with 1 or 3 years committed amount of usage beyond that will be charged based on on-demand price. Instance family and region cannot be changed                                                           |
| Spot Instances            | short workloads, cheap can lose instances and hence not reliable , most cost effective in aws                                                                                                            |
| Dedicated Host Instances  | book a physical server ... etc. No-one else will use the same machine . used for compliance purposes or to use license which are based on per-socket, per-core , pe-VM software licenses. Most expensive |
| EC2 Capacity Reservations | On Demand Price                                                                                                                                                                                          |                                                                                                                                                                                                       |

#### Security Groups

These govern inbound and outbound traffic, if your request gets timed out then 100% it is due toSimilar to Amaz
misconfiguration of the security groups.

#### EC2 Instance storage

|Type| Description|Remarks|
|-----|-----------|-----| 
|EBS (Elastic Block Storage) | n/w storage attached per EC2 instance | AZ dependant, can use EBS snapshot to transfer/copy between AZs|
|EFS (Elastic File System) | can be shared between mutliple EC2 instances|costlier|
|EBS instance store | Where drive and machine is connected | data is lost once instance is deleted|
|EFS-IA| Price is 92% less compared to EFS | optimized for Infrequent access and hence cost is reduced|
|FSx for Windows |Network f/s for windows servers |    |     
|FSx for Lusture (Linux + Cluster) |High performance Linux Ec2 instances|    |

AMI - Amazon Machine Image (eg Amazon Linux), can be customized, bought from Amazon Marketplace.

EC2 Image Builder is a aws service which automatically test, build, and distribute AMIs.

### Elastic Load Balancing (ELB)

ELB is a managed load balancer meaning aws takes of upgrades, maintenance etc.
There are kinds of ELBs provided by AWS EC2 namely Application Load Balancer (ALB - Layer 7), Network Load Balancer (NLB  - Layer 4), 
and Gateway load balancer (GLB - Layer 3).

#### Auto Scaling Groups
A instance group which changes the instances number to match the load.
Different strategies
* Manual Scaling 
* Dynamic Scaling 
    * Simple /Step Scaling
    * Target Tracking Scaling
    * Scheduled Scaling
* Predictive Scaling


### AWS S3 Service
AWS S3 Service can be used for object versioning.

#### S3 Security

* User Based
    * IAM Policy based
* Resource Based

#### S3 Replication
Will only replicate objects after the replication rule is created. Can create objects between same region as well as different region.

#### S3 Storage Class

* Amazon S3 Standard - General Purpose
    * Big Data Analytics,mobile & gaming Applications ,content distribution etc.
    * 99.99% availability
    * can support 2 concurrent facility failures
    * Used for frequently accessed objects/data
* Amazon S3 Standard-Infrequent Access
  * Low cost than S3 Standard
  * 99.9% availability
  * Used for Disaster Recovery,backups
* Amazon S3 One Zone-Infrequent Access
    * 99.999999999% durability in a Availability Zone and is lost in-case the AZ is destroyed
    * 99.95% availability
    * Used to store secondary copies of backups and as well as data which can be easily re-created
* Amazon S3 One Glacier-Instant Retrieval
    * Low cost backup/archive solution
    * Pricing : price for storage + price for retrieval
    * Milli-seconds retrieval
    * Minimum storage duration is 90 days
* Amazon S3 Glacier-Flexible Retrieval
  * Low cost backup/archive solution
  * Pricing : price for storage + price for retrieval
  * Used to be called Amazon S3 Glacier
  * Retrieval - Expedited (1to 5 minutes), Standard (3-5 hours), Bulk (5-12 hours) - free
  * Minimum storage duration is 90 days
* Amazon S3 Glacier-Deep Archive
  * Low cost backup/archive solution
  * Pricing : price for storage + price for retrieval
  * Retrieval - Standard (12 hours), Bulk (48 hours) 
  * Minimum storage duration is 180 days
* Amazon S3 Intelligent Tiering
  * Small monthly monitoring and auto-tiering fee
  * There are no retrieval charges in S3 intelligent tiering

### AWS Snow Family
Transfer data physically rather than over the network.

* Data Migration
    * Snowball edge
        * Pay per data transfer job
        * Both block and s3-compatible object storage
    * Snowcone
       * Lightweight
       * Device used for edge computing,storage and data transfer
       * For space constrained environments
       * We have to provide the power and cables
       * Data transfer can be offline or online using AWS DataSync
    * Snowmobile
       * Truck
       * 1EB = 1000PB
* Edge Computing

### AWS Storage Gateway
Integrate on-premises storage with Amazon AWS Storage

## References

* <https://aws.amazon.com/about-aws/global-infrastructure/>
* <https://aws.amazon.com/ec2/instance-types/>
* <https://instances.vantage.sh/>