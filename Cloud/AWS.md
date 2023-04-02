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
These govern inbound and outbound traffic, if your request gets timed out then 100% it is due to
misconfiguration of the security groups.

## References

* <https://aws.amazon.com/about-aws/global-infrastructure/>
* <https://aws.amazon.com/ec2/instance-types/>
* <https://instances.vantage.sh/>