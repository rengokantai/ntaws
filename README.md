## ntaws
###VPC
####Networking in Your VPC
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
