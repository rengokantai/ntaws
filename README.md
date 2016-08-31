## ntaws

###Route 53
####What Is Amazon Route 53
Domain registration, DNS service(domain to ip), healch checking  
######DNS Service
If register a new domain name with Amazon Route 53 then automatically configure Amazon Route 53 as the DNS service for the domain, and create a ```hosted zone``` for thedomain.


#####DNS Domain Name Format
######Formatting Domain Names for Hosted Zones and Resource Record Sets
Amazon Route 53 stores alphabetic characters as lowercase letters  

Domain contains non english letter must escape  \three-digit octal code. 000-040,177-377, period  
```
ex\344mple.com.   //exämple.com (wrong)
```
If the domain name includes any characters other than a to z, 0 to 9, - (hyphen), or _ (underscore), Amazon Route 53 API actions return the characters as escape codes.  
The Amazon Route 53 console displays the characters as characters, not as escape codes.
######Using an Asterisk in the Names of Hosted Zones and Resource Record Sets
- You can't include an * in the leftmost
- For other positions, DNS treats it as an * character (ASCII 42), not as a wildcard.
For resource record sets, if * use as wildcard:
- The * must replace the leftmost label in a domain name.
- The * must replace the entire label. The following are incorrect(*p.example.com or p*.example.com)
- Cannot use the * as a wildcard for resource records sets that have a type of NS.  
For resource record sets, if you include * in any position other than the leftmost label in a domain name, DNS treats it as an * character (ASCII 42), not as a wildcard.
######Formatting Internationalized Domain Names
Amazon Route 53 stores these internationalized domain names (IDNs) in Punycode, which represents Unicode characters as ASCII strings. 
When enter an IDN in the address bar of a modern browser, the browser converts it to Punycode before submitting a DNS query or making an HTTP request.




#####Supported DNS Resource Record Types
For rrt include a domain name, enter a fully qualified domain name, for example, www.example.com. The trailing dot is optional.  
Amazon Route 53 treats them identical.
######A Format(IPv4)
```
<Value>192.0.2.1</Value>
```
######CNAME Format
The DNS protocol ```does not``` allow you to create a CNAME record for the top node of a DNS namespace, also known as the zone apex. For example, if you register the DNS name example.com, the zone apex is example.com. You cannot create a CNAME record for example.com, but you can create CNAME records for www.example.com.  
If you create a CNAME record for a subdomain, you cannot create any other resource record sets for that subdomain. For example, if you create a CNAME for www.example.com, you cannot create any other resource record sets for which the value of the Name field is www.example.com.  
Aliases are similar in some ways to the CNAME resource record type; however, you can create an alias for the zone apex. 
```
<Value>k.ke.com</Value>
```
######MX Format
Contains a ```decimal number``` that represents the priority of the MX record, and the domain name of an email server.
```
<Value>10 mail.example.com</Value>
```
######NAPTR Format
Name Authority Pointer.  
######NS Format
An NS record identifies the name servers for the hosted zone. The value for an NS record is the domain name of a name server. 
```
<Value>ns-1.example.com</Value>
```
######PTR Format
A PTR record Value element is the same format as a domain name.
```
<Value>hostname.example.com</Value>
```
######SOA Format
A start of authority (SOA) record provides information about a domain and the corresponding Amazon Route 53 hosted zone. 
```
<Value>ns-2048.awsdns-64.net hostmaster.awsdns.com 1 1 1 1 60</Value>
```
######SRV Format
Four space-separated values. The first three values are decimal numbers representing priority, weight, and port. The fourth value is a domain name. 
```
<Value>10 5 80 hostname.example.com</Value>
```
######TXT Format
A TXT record contains a space-separated list of double-quoted strings. A single string include a maximum of 255 characters.  
Unlike domain names, case is preserved in character strings, meaning that Ab is not the same as aB
```
<Value>"This string includes \"quotation marks\"." "The last character in this string is an accented e specified in octal format: \351"</Value>
```
#####IP Address Ranges of Amazon Route 53 Servers
#####DNS Constraints and Behaviors
######Maximum Response Size
To comply with DNS standards, responses sent over UDP are limited to 512 bytes in size. Responses exceeding 512 bytes are truncated and the resolver must re-issue the request over TCP. 


====
####Registering Domain Names Using Amazon Route 53
#####Registering and Updating Domains / Registering Domain Names Using Amazon Route 53
######To update the name servers for your domain when you want to use another DNS service
8.(Optional) Delete the hosted zone that Amazon Route 53 created automatically ```when you registered your domain```. This prevents you from being charged for a hosted zone that you aren't using.
====
####Configuring Amazon Route 53 as Your DNS Service
#####Migrating DNS Service for an Existing Domain to Amazon Route 53
######Creating a Hosted Zone
When you create a hosted zone, Amazon Route 53 automatically creates four name server (NS) records and a start of authority (SOA) record for the zone.
######Getting Your Current DNS Configuration from Your DNS Service Provider
Migrate A MX CNAME records.
######Creating Resource Record Sets
Do not create additional name server (NS) or start of authority (SOA) records in the Amazon Route 53 hosted zone, and do not delete the existing NS and SOA records.

#####Creating a Subdomain That Uses Amazon Route 53 as the DNS Service without Migrating the Parent Domain
######Creating a Hosted Zone for the New Subdomain
step1:  create a hosted zone
```
b.example.com
```
######Creating Resource Record Sets
######(most important) Updating Your DNS Service with Name Server Records for the Subdomain
Update the DNS service for the parent domain by adding NS records for the subdomain.  
This is known as delegating responsibility for the subdomain to Amazon Route 53.  
Do not add SOA record.

#####Migrating DNS Service for a Subdomain to Amazon Route 53 without Migrating the Parent Domain
######Routing Traffic to an Amazon CloudFront Distribution (Public Hosted Zones Only)
(?)
####Routing Traffic to AWS Resources
#####Routing Traffic to an AWS Elastic Beanstalk Environment

====
####Working with Resource Record Sets

#####Choosing Between Alias and Non-Alias Resource Record Sets
An alias resource record set contains a pointer to a ```CloudFront distribution```,```Elastic Beanstalk environment```,```load balancer```,```Amazon S3 bucket that is configured as a static website```, or ```another Amazon Route 53 resource record set``` in the same hosted zone.  
You can't create alias resource record sets for CloudFront distributions in a private hosted zone.  
If an alias resource record set points to a CloudFront distribution, an Elastic Beanstalk environment, an ELB load balancer, or an Amazon S3 bucket, you cannot set the time to live (TTL). Amazon Route 53 uses the CloudFront, Elastic Beanstalk, Elastic Load Balancing, or Amazon S3 TTLs.  

The diff between CNAME and alias. see charts:  
|   |   |
|---|---|
|Amazon Route 53 charges for CNAME queries.  |Amazon Route 53 doesn't charge for alias|
  
  



















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
