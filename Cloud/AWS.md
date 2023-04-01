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

### IAM

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

| Name                        | Uses                                                                                     | Examples  |
|-----------------------------|------------------------------------------------------------------------------------------|-----------|
| General Purpose    Instances| Balance between compute,memory ,Networking . Ideal for web servers and code repositories | t3,t2 etc |
| Compute Optimized Instances |  |  |

#### Security Groups
These govern inbound and outbound traffic, if your request gets timed out then 100% it is due to
misconfiguration of the security groups.

## References

* <https://aws.amazon.com/about-aws/global-infrastructure/>