## ntaws
###VPC





####Networking in Your VPC

#####[IP Addressing](http://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/vpc-ip-addressing.html)



#####[Route Tables](http://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/VPC_Route_Tables.html)
Each subnet in your VPC must be associated with a route table.  
A subnet can only be associated with one route table at a time,
but you can associate multiple subnets with the same route table.  
You cannot delete the main route table, but you can replace the main route table with a custom table  
In route table, if multiple route overlapped, then we prioritize the routes as follows in your VPC, from most preferred to least preferred:
- Local routes for the VPC
- Static routes whose targets are an Internet gateway, a virtual private gateway, a network interface, an instance ID, a VPC peering connection, or a VPC endpoint
- Any propagated routes from a VPN connection or AWS Direct Connect connection
(Ex: igw takes priority over vgw)
If overlapping routes within a VPN connection and longest prefix match cannot be applied  
- BGP propagated routes from an AWS Direct Connect connection
- Manually added static routes for a VPN connection
- BGP propagated routes from a VPN connection
#####[VPC Endpoints](http://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/vpc-endpoints.html)
A VPC endpoint enables you to create a private connection between your VPC and another AWS service without requiring access over the Internet, through a NAT device, a VPN connection, or AWS Direct Connect.  
Currently only support S3 within same region only.  
Use private IP addresses to communicate with resources in other services, do not require public IP addresses, and you do not need an Internet gateway, a NAT device, or a virtual private gateway in VPC.
######Endpoint Basics
Specify the VPC and the service to which connecting. 
```
pl-xxxxxx
```
target:vpne dest:pl
######Endpoint Limitations
- Cannot use a prefix list ID in an outbound rule in a network ACL to allow or deny outbound traffic to the service specified in an endpoint. Can use a prefix list ID in an outbound security group rule.
- Cannot create an endpoint between a VPC and an AWS service in a different region.
######Controlling the Use of Endpoints
Currently do not support resource-level permissions for any of the 
```
ec2:*VpcEndpoint*
```
######Using Endpoint Policies
An endpoint policy does not override or replace IAM user policies or service-specific policies (such as S3 bucket policies).
Cannot attach more than one policy to an endpoint
######Modifying Your Security Group (Add S3 to outbound)
type=https, destnation=pl-xxxxxx







#####Endpoints for Amazon S3
- Cannot use a bucket policy or an IAM policy to allow access from a VPC CIDR range (the private IP address range). VPC CIDR blocks can be overlapping or identical, which may lead to unexpected results. 
- Instead, use a bucket policy to restrict access to a specific endpoint or to a specific VPC, and you can use your route tables to control which instances can access resources in Amazon S3 via the endpoint.
- Endpoints currently do not support cross-region requests
- only create an endpoint in a subnet that is not used by any of these services  
Cloudforamation, CodeDeploy, Elastic Beanstalk,Amazon EMR  

######Using Endpoint Policies for Amazon S3
######Using Amazon S3 Bucket Policies
Reiterate:Cannot use the aws:SourceIp condition in your bucket policies for requests to Amazon S3 through a VPC endpoint. 
Instead, adjust bucket policy to limit access to a specific VPC or a specific endpoint.  
New attribute: Condition, StringNotEquals  
restrict endpoint
```
"Condition": {
  "StringNotEquals": {
    "aws:sourceVpce": "vpce-12345678"
  }
}
```
restrict vpc
```
"Condition": {
  "StringNotEquals": {
    "aws:sourceVpce": "vpc-12345678"
  }
}
```
