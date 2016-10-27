##What is
AWS Direct Connect links your internal network to an AWS Direct Connect location over a standard 1 gigabit or 10 gigabit Ethernet fiber-optic cable.  
One end of the cable is connected to your router, the other to an AWS Direct Connect router.  

An AWS Direct Connect location provides access to Amazon Web Services in the region it is associated with, as well as access to other US regions.
Your network must meet the following conditions:
- Your network must support Border Gateway Protocol (BGP) and BGP MD5 authentication.  

To connect to Amazon Virtual Private Cloud (Amazon VPC), you must first do the following:
- Provide a private Autonomous System Number (ASN). 
- Create a virtual private gateway and attach it to your VPC.  

To connect to public AWS products such as Amazon EC2 and Amazon S3, you need to provide the following:
- A public ASN that you own (preferred) or a private ASN.  
- Public IP addresses for the BGP session (/31 for each end of the BPG session). If you do not have public IP addresses to assign to this connection, log on to AWS and then open a ticket with AWS Support.
- The public routes that you will advertise over BGP.  

######AWS Direct Connect Limits
Virtual interfaces per AWS Direct Connect connection=50 (can be increased upon request.)
Active AWS Direct Connect connections per region per account=10 (can be increased upon request.)  
Routes per Border Gateway Protocol (BGP) session=100 (cannot be increased.)  

## Getting Started with AWS Direct Connect
###Getting Started at an AWS Direct Connect Location














##Working With Connections
