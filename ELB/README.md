##What is
Elastic Load Balancing distributes incoming application traffic across multiple EC2 instances, in multiple Availability Zones.(increases the fault tolerance)  
You can also offload the work of encryption and decryption to your load balancer so that your instances can focus on their main work.  
[application lb](http://docs.aws.amazon.com/elasticloadbalancing/latest/application/)  
[classic lb](http://docs.aws.amazon.com/elasticloadbalancing/latest/classic/)
##How Elastic Load Balancing Works
When the load balancer detects an unhealthy instance, it stops routing traffic to that instance, and then resumes routing traffic to that instance when it detects that the instance is healthy again.  
Diff between classic lb and application lb
- With a Classic Load Balancer, you register instances with the load balancer. 
- With an Application Load Balancer, you register the instances as targets in a target group, and route traffic to a target group.  

######Availability Zones and Instances
With a Classic Load Balancer, we recommend that you enable multiple Availability Zones. With an Application Load Balancer, we require you to enable multiple Availability Zones.  
If cross-zone load balancing is disabled, the load balancer distributes traffic evenly across all enabled Availability Zones.   
Cross-zone load balancing is always enabled for an Application Load Balancer and is disabled by default for a Classic Load Balancer.  

######Load Balancer Scheme
- When you create a load balancer, you must choose whether to make it an internal load balancer or an Internet-facing load balancer. 
- Note that when you create a Classic Load Balancer in EC2-Classic, it must be an Internet-facing load balancer.   
- The nodes of an internal load balancer have only private IP addresses.  
- Therefore, internal load balancers can only route requests from clients with access to the VPC for the load balancer.  
 
##Getting Started
Choose VPC. Add subnets. You can select only one subnet per Availability Zone.
##Authentication and Access Control for Your Load Balancers
######Resource-Level Permissions for Elastic Load Balancing
Currently, the API for Application Load Balancers (API version 2015-12-01) ```does no```t support resource-level permissions. The API for Classic Load Balancers (API version 2012-06-01) ```supports resource-level``` permissions; however, not every API action supports resource-level permissions.

##Migrate from a Classic Load Balancer to an Application Load Balancer
(tbc)
