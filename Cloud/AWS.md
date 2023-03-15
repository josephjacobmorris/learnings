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

## References

* <https://aws.amazon.com/about-aws/global-infrastructure/>