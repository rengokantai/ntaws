##What Is Amazon Route 53
Domain registration, DNS service(domain to ip), healch checking  
######DNS Service
If register a new domain name with Amazon Route 53 then automatically configure Amazon Route 53 as the DNS service for the domain, and create a ```hosted zone``` for thedomain.


###DNS Domain Name Format
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




###Supported DNS Resource Record Types
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
###IP Address Ranges of Amazon Route 53 Servers
###DNS Constraints and Behaviors
######Maximum Response Size
To comply with DNS standards, responses sent over UDP are limited to 512 bytes in size. Responses exceeding 512 bytes are truncated and the resolver must re-issue the request over TCP. 


##Registering Domain Names Using Amazon Route 53
###Registering and Updating Domaind
####Registering Domain Names Using Amazon Route 53
- To update the name servers for your domain when you want to use another DNS service  
- (Optional) Delete the hosted zone that Amazon Route 53 created automatically ```when you registered your domain```. This prevents you from being charged for a hosted zone that you aren't using.  






##Configuring Amazon Route 53 as Your DNS Service
###Migrating DNS Service for an Existing Domain to Amazon Route 53
######Creating a Hosted Zone
When you create a hosted zone, Amazon Route 53 automatically creates four name server (NS) records and a start of authority (SOA) record for the zone.
######Getting Your Current DNS Configuration from Your DNS Service Provider
Migrate A MX CNAME records.
######Creating Resource Record Sets
Do not create additional name server (NS) or start of authority (SOA) records in the Amazon Route 53 hosted zone, and do not delete the existing NS and SOA records.

###Creating a Subdomain That Uses Amazon Route 53 as the DNS Service without Migrating the Parent Domain
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

###Migrating DNS Service for a Subdomain to Amazon Route 53 without Migrating the Parent Domain
######Routing Traffic to an Amazon CloudFront Distribution (Public Hosted Zones Only)
(?)
##Routing Traffic to AWS Resources
###Routing Traffic to an AWS Elastic Beanstalk Environment




====
##Working with Public Hosted Zones

###Creating a Public Hosted Zone
A hosted zone is a collection of resource record sets for a specified domain. 
You can create more than one hosted zone with the same name and add different resource record sets to each hosted zone.  
Amazon Route 53 never returns values for resource record sets in other hosted zones that have the same name.



###Checking DNS Responses from Amazon Route 53
######Using the Checking Tool to See How Amazon Route 53 Responds to DNS Queries
Get Response
######Using the Checking Tool to Simulate Queries from Specific IP Addresses (Geolocation and Latency Resource Record Sets Only)
appoint IP address
###Configuring White Label Name Servers
Each Amazon Route 53 hosted zone is associated with four name servers, known collectively as a ```delegation set```.
If you want the domain name of your name servers to be the same as the domain name of your hosted zone, for example, ns1.example.com, you can configure white label name servers.  

Create an Amazon Route 53 reusable delegation set by using the Amazon Route 53 API, the AWS CLI, or one of the AWS SDKs  

Create or recreate Amazon Route 53 hosted zones:
- If you aren't currently using Amazon Route 53 as the DNS service for the domains for which you want to use white label name servers Create the hosted zones and specify the reusable delegation set that you created in the previous step
- If you are using Amazon Route 53 as the DNS service for the domains for which you want to use white label name servers – You must recreate.  

You cannot change the name servers that are associated with an existing hosted zone. You can associate a reusable delegation set with a hosted zone only when you create the hosted zone.  
Change the TTL for the NS record for the hosted zone to 60 seconds or less.  
Change the minimum TTL for the SOA record for the hosted zone to 60 seconds or less. This is the last value in the SOA record.  
(Changing the minimum TTL to 60 seconds or less will temporarily increase your bill because DNS resolvers will send more queries to Amazon Route 53. )

(tbc)


###NS and SOA Resource Record Sets that Amazon Route 53 Creates for a Public Hosted Zone
######The Name Server (NS) Resource Record Set
Amazon Route 53 automatically creates a name server (NS) resource record set that has the same name as your hosted zone.  

######The Start of Authority (SOA) Resource Record Set


##Working with Private Hosted Zones
A private hosted zone is a container that route traffic for a domain and its subdomains within one or more Amazon ```Virtual Private Clouds(VPCs)```.  
To use private hosted zones, you must set the following Amazon VPC settings to true:
- enableDnsHostnames
- enableDnsSupport

- Associating an Amazon VPC with More than One Private Hosted Zone  

You can associate a VPC with more than one private hosted zone, but the namespaces must not overlap. For example, you cannot associate a VPC with hosted zones for both example.com and acme.example.com because both namespaces end with example.com.  

- Public and Private Hosted Zones that Have the Same Name  
When you have private and public hosted zones, the private hosted zone takes precedence over the public hosted zone when you're logged into an Amazon EC2 instance in an Amazon VPC that you have associated with the private hosted zone. 

- Delegating Responsibility for a Subdomain  
You cannot create NS records in a private hosted zone to delegate responsibility for a subdomain.
- Custom DNS Servers  
If you have configured custom DNS servers on Amazon EC2 instances in your VPC, you must configure those DNS servers to route your private DNS queries to the IP address of the Amazon-provided DNS servers for your VPC. ```This IP address is the IP address at the base of the VPC network range "plus two." For example, if the CIDR range for your VPC is 10.0.0.0/16, the IP address of the DNS server is 10.0.0.2.```

###Associating More Amazon VPCs with a Private Hosted Zone

###Associating Amazon VPCs and Private Hosted Zones That You Create with Different AWS Accounts
Contact the AWS Support Center first.  

###Deleting a Private Hosted Zone











====
##Working with Resource Record Sets


###Choosing a Routing Policy
- Weighted Routing  
Weighted resource record sets let you associate multiple resources with a single DNS name. 
- Latency-Based Routing  
You can create latency resource record sets for the following:
- Amazon EC2 instances, with or without Elastic IP addresses.
- ELB load balancers, which balance traffic for Amazon EC2 instances.  
You can create latency resource record sets using any record type that Amazon Route 53 supports except NS or SOA.  







###Choosing Between Alias and Non-Alias Resource Record Sets
An alias resource record set contains a pointer to a ```CloudFront distribution```,```Elastic Beanstalk environment```,```load balancer```,```Amazon S3 bucket that is configured as a static website```, or ```another Amazon Route 53 resource record set``` in the same hosted zone.  
You can't create alias resource record sets for CloudFront distributions in a private hosted zone.  
If an alias resource record set points to a CloudFront distribution, an Elastic Beanstalk environment, an ELB load balancer, or an Amazon S3 bucket, you cannot set the time to live (TTL). Amazon Route 53 uses the CloudFront, Elastic Beanstalk, Elastic Load Balancing, or Amazon S3 TTLs.  

The diff between CNAME and alias. see charts:  
| cname | alias |
| ------------------------ | ---------------------- |
| Amazon Route 53 charges for CNAME queries.  | Amazon Route 53 doesn't charge for alias |
| You cannot create a CNAME record at the top node of a DNS namespace | You can create an alias resource record set at the zone apex.|
| redirects queries for a domain name regardless of record type.| ollows the pointer in an alias resource record set|
|
  
(see aonther 6 entries)

###Values that You Specify When You Create or Edit Amazon Route 53 Resource Record Sets















====
##Health Checks and DNS Failover
###Creating, Updating, and Deleting Health Checks/Creating and Updating Health Checks
====
##Authentication and Access Control
###Overview of Managing Access Permissions to Your Amazon Route 53 Resources



====
##Capturing API Requests with CloudTrail

====
##Tagging Amazon Route 53 Resources
When you apply tags to Amazon Route 53 hosted zones, domains, and health checks, AWS generates a cost allocation report as a comma-separated value (CSV) file with your usage and costs aggregated by your tags. 

