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



## Working With Virtual Interfaces
You must create a virtual interface to begin using your AWS Direct Connect connection. You can create a public virtual interface to connect to public resources, or a private virtual interface to connect to your VPC. 




## Requesting Cross Connects
- An AWS Direct Connect location provides access to AWS in the region it is associated with. 
- You can establish connections with AWS Direct Connect locations in multiple regions, but a connection in one region does not provide connectivity to other regions.
- You can configure multiple virtual interfaces on a single AWS Direct Connect connection, and you'll need one private virtual interface for each VPC to connect to. Each virtual interface needs a VLAN ID, interface IP address, ASN, and BGP key.  

### View Virtual Interface Details






## Troubleshooting
### Troubleshooting a Cross Connection to AWS Direct Connect
- Verify that your device is supported by AWS Direct Connect. If not, get a device that meets the AWS Direct Connect requirements. 
- Verify that your AWS Direct Connect cross connects are established. 
- Verify that your router’s link lights are working. 
- Verify with your colocation provider that there are no cabling problems.
- If you cannot ping the Amazon IP address, verify that the interface IP address is in the VLAN you provided to Amazon Web Services and then verify your firewall settings.
- If you cannot establish Border Gateway Protocol (BGP) after verifying the password provided by Amazon, open a support ticket.
- If you are not receiving Amazon routes and you cannot verify public BGP routing policy, open ticket.


### Troubleshooting a Remote Connection to AWS Direct Connect
step 1,2,3,4 same as above  
- Ask your service provider to turn off Auto Negotiation on their device
- On your device, turn off Auto Negotiation, set the device to Full Duplex, and set the device to the correct speed.

If the cross connect is not completed within 90 days, the authority granted by the LOA-CFA expires. To renew a LOA-CFA that has expired, you can download it again from the AWS Direct Connect console. 









##Working With Connections
