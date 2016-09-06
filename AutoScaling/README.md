##What is
Ensure that you have the correct number of Amazon EC2 instances available to handle the load for your application. 
###Benefits of Auto Scaling
An Auto Scaling group can contain EC2 instances in one or more Availability Zones within the same region. 
However, Auto Scaling groups cannot span multiple regions.
######Rebalancing Activities
When rebalancing, Auto Scaling launches new instances before terminating the old ones, so that rebalancing does not compromise the performance or availability of your application.  
Auto Scaling attempts to launch new instances before terminating the old ones, being at or near the specified maximum capacity could impede or completely halt rebalancing activities.  
The system can temporarily exceed the specified maximum capacity of a group by a 10 percent margin (or by a 1-instance margin, whichever is greater) during a rebalancing activity.  
###Auto Scaling Lifecycle
######Enter and Exit Standby
- You can put any instance that is in an InService state into a Standby state. 
- Instances in a Standby state continue to be managed by the Auto Scaling group. However, they are not an active part of your application until you put them back into service.  

##Getting Started
##Tutorial: Set Up a Scaled and Load-Balanced Application
Elastic Load Balancing supports two types of load balancers: Classic Load Balancers and Application Load Balancers. You can create either type of load balancer to attach to your Auto Scaling group.   
######Configure Scaling and Load Balancing Using the AWS CLI
(tbc)
##Launch Configurations
###Creating a Launch Configuration


##Controlling Access to Your Auto Scaling Resources
######Auto Scaling Actions
[reference](http://docs.aws.amazon.com/AutoScaling/latest/APIReference/API_Operations.html)  
```
autoscaling:CreateAutoScalingGroup
autoscaling:CreateLaunchConfiguration
```
######Auto Scaling Resources
Must use "*" as the resource.  
There are no supported Amazon Resource Names (ARNs) for Auto Scaling resources.  

######Auto Scaling Keys
[Available Keys for Conditions](http://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_elements.html#AvailableKeys)  


######Example IAM Policies for Auto Scaling

###Launch Auto Scaling Instances with an IAM Role


##Troubleshooting Auto Scaling
######Retrieving an Error Message
```
aws autoscaling describe-scaling-activities --auto-scaling-group-name ke
```
###Instance Launch Failure
######EBS block device mappings not supported for instance-store AMIs.
Solution:
- Create a new launch configuration with block device mappings supported by your instance type. 
- Update your Auto Scaling group with the new launch configuration  
######Placement groups may not be used with instances of type 'm1.large'. Launching EC2 instance failed.
###Capacity Limits


