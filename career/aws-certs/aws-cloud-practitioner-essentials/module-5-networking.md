# Networking

Learn about what a VPC is and what it does.
What is a subnet and what does it do?
Public and private subnet.

## Introduction To Networking

- Allows you to limit access from the outside world to resources within your VPC.
- Subnets are used to organize resources, setting these to be public or private that don't have access to the internet.
- Public subnets have a route to the internet, while private subnets do not.
- A public resource will be a customer facing website
- A private resource will be a database that only your application can access.

## Organising AWS Cloud Resources

- What is a virtual private gateway?
- Define the core components of the VPC.
- Define an internal gateway and what that does.
- VPC can have a private IP range
- Subnets can be public or private
- For a VPC to accept internet traffic it must be associated with an internet gateway.
- A virtual private gateway can be used to restrict access to the VPC from the internet.
- AWS Direct Connect is a cloud service solution that makes it easy to establish a dedicated network connection from your premises to AWS.

## More Ways To Connect To AWS

- AWS Client VPN - When to connect remote workers to your cloud.
- AWS Site-to-Site VPN - Secure connection between on-premises networks and AWS.
- AWS PrivateLink - Private connectivity between VPCs or cloud providers.
- AWS Direct Connect - Creates a secure connection from your on-premises network to AWS.
- Direct connect is great for high bandwidth, low latency connections.
- For hybrid cloud architectures, Direct Connect can provide a more consistent network experience.
- AWS transit gateway - Connects VPCs and on-premises networks through a central hub.
- NAT Gateway - Allows instances in a private subnet to connect to the internet but doesn’t allow inbound traffic from the internet.
- AWS API Gateway - Create, publish, maintain, monitor, and secure APIs at any scale.

## Subnets, Security Groups, and Network Access Control Lists

- How network traffic works in a VPC
- How security groups work
- How network access control lists work (Network ACLs)
- Who is responsible for securing the networks based on the AWS shared responsibility model
- Security groups can be used to accept a specific type of traffic
- Network ACL will analyze traffic at the subnet level and can allow or deny traffic based on rules.
- Network ACL is a virtual firewall
- Security groups deny all inbound traffic by default and allow all outbound traffic.
- Security groups rules can be created to allow specific inbound traffic.
- Network ACLS and security groups is the customer responsibility.

## Global Networking

- Whats is DNS?
- What are the benefits of Amazon Route 53?
- What are the use cases for AWS CloudFront?
- Amazon Route 53 is a DNS service that provides highly available and scalable domain name system (DNS) web services.
- Route 53 can do geolocation routing, latency-based routing, and weighted routing.
- Route 53 can register domain names
- Amazon CloudFront can be used to deliver content with low latency and high transfer speeds.
- AWS global accelerator improves the availability and performance of your applications with users globally.

## Global Architectures

- Learn when to use VPN and direct connect
- 
