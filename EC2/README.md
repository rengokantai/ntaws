

##Storage
###Amazon EBS
EBS volumes are highly available and reliable storage volumes that can be attached to any running instance that is ```in the same Availability Zone```.  
With Amazon EBS, you pay only for what you use.  

Amazon EBS encryption uses AWS Key Management Service (AWS KMS) master keys when creating encrypted volumes and any snapshots created from your encrypted volumes.   
The first time you create an encrypted EBS volume in a region, a default master key is created for you automatically.  
This key is used for Amazon EBS encryption unless you select a Customer Master Key (CMK) that you created separately using the AWS Key Management Service.  
######Features of Amazon EBS
You can create EBS General Purpose SSD (gp2), Provisioned IOPS SSD (io1), Throughput Optimized HDD (st1), and Cold HDD (sc1) volumes up to 16 TiB in size.  
You can mount these volumes as devices on your Amazon EC2 instances.  
You can mount multiple volumes on the same instance, ```but each volume can be attached to only one instance at a time```. 
- Gp2 support up to 10,000 IOPS and 160 MB/s of throughput.
- Io1 support up to 20,000 IOPS and 320 MB/s of throughput.  
- Throughput Optimized HDD (st1) volumes provide low-cost magnetic storage, 500 MiB/s of throughput, this volume type is a good fit for large, sequential workloads such as Amazon EMR, ETL, data warehouses, and log processing.
- Cold HDD (sc1) volumes provide low-cost magnetic storage that defines performance in terms of throughput rather than IOPS. With throughput of up to 250 MiB/s, sc1 is a good fit ideal for large, sequential, cold-data workloads.  

EBS volumes are created in a specific Availability Zone, and can then be attached to any instances in that same Availability Zone.  
To make a volume available outside of the Availability Zone, you can create a snapshot and restore that snapshot to a new volume anywhere in that region.

####Amazon EBS Volumes
######Benefits of Using EBS Volumes
- Data persistence  
An EBS volume is off-instance storage that can persist independently from the life of an instance. You continue to pay for the volume usage as long as the data persists.
- Data encryption  
Amazon EBS encryption uses 256-bit Advanced Encryption Standard algorithms (AES-256) and an Amazon-managed key infrastructure.  
Amazon EBS encryption uses AWS Key Management Service (AWS KMS) master keys when creating encrypted volumes and any snapshots created from your encrypted volumes.  

- snapshots  
 If you have a volume with 100 GiB of data, but only 5 GiB of data have changed since your last snapshot, only the 5 GiB of modified data is written to Amazon S3.
#####Amazon EBS Volume Types
Max. IOPS**/Volume	            gp2/10,000	io1/20,000	st1/500	sc1/250  
(To achieve this throughput, you must have an instance that supports it, such as r3.8xlarge or x1.32xlarge.)  
Dominant Performance Attribute	IOPS	      IOPS	      MiB/s	  MiB/s  
(gp2/io1 based on 16KiB I/O size, st1/sc1 based on 1 MiB I/O size)   

Linux AMIs require GPT partition tables and GRUB 2 for boot volumes 2 TiB (2048 GiB) or larger.  
Non-boot volumes do not have this limitation on Linux instances.  
######General Purpose SSD (gp2) Volumes
- I/O Credits and Burst Performance
The performance of gp2 volumes is tied to volume size, which determines the baseline performance level of the volume and how quickly it accumulates I/O credits   
The more credits your volume has for I/O, the more time it can burst beyond its baseline performance level and the better it performs when more performance is needed.  

```
Burst duration  =  (Credit balance)/(Burst IOPS) - 3(Volume size in GiB)
```
- What happens if I empty my I/O credit balance?  
```
Throughput limit with empty I/O credit balance  =  (Max throughput) x (Baseline IOPS)/3,000
```
- Throughput Performance  
100GiB=128MiB/s Max throughput   
250-300Gib=160MiB/s Max throughput  


######Provisioned IOPS SSD (io1) Volumes
Instead of using a bucket and credit model to calculate performance, an io1 volume allows you to specify a consistent IOPS rate when you create the volume.  
The maximum ratio of provisioned IOPS to requested volume size (in GiB) is 50:1. For example, a 100 GiB volume can be provisioned with up to 5,000 IOPS.  
Any volume 400 GiB in size or greater allows provisioning up to the 20,000 IOPS maximum.  

######Throughput Optimized HDD (st1) Volumes
Throughput Optimized HDD (st1) volumes provide low-cost magnetic storage that defines performance in terms of throughput rather than IOPS. This volume type is a good fit for large, sequential workloads such as Amazon EMR, ETL, data warehouses, and log processing.  
```
(Volume size) x (Credit accumulation rate per TiB) = Baseline Throughput
```
Ex
```
12.5 TiB x (40 MiB/s/1 TiB)  = 500 MiB/s
```
#####Creating an Amazon EBS Volume
New EBS volumes receive their maximum performance the moment that they are available and do not require initialization (formerly known as pre-warming). However, storage blocks on volumes that were restored from snapshots must be initialized (pulled down from Amazon S3 and written to the volume) before you can access the block.  
- To create an EBS volume using the command line  
```
create-volume (AWS CLI)
New-EC2Volume (AWS Tools for Windows PowerShell)
```
#####Restoring from a Snapshot
New volumes created from existing EBS snapshots load lazily in the background. This means that after a volume is created from a snapshot, there is no need to wait for all of the data to transfer from Amazon S3 to your EBS volume before your attached instance can start accessing the volume and all its data.  
You cannot directly restore an EBS volume from a shared encrypted snapshot that you do not own. You must first create a copy of the snapshot, which you will own. You can then restore a volume from that copy.  
If you need to ensure that your restored volume always functions at peak capacity in production, you can force the immediate initialization of the entire volume using dd or fio.  

#####Attaching an Amazon EBS Volume to an Instance
To attach an EBS volume to an instance using the command line
```
attach-volume (AWS CLI)
Add-EC2Volume (AWS Tools for Windows PowerShell)
```
#####Making an Amazon EBS Volume Available for Use
(tbc)
#####Volume Information
- To view information about an EBS volume using the command line  
```
describe-volumes (AWS CLI)
Get-EC2Volume (AWS Tools for Windows PowerShell)
```
#####Monitoring the Status of Your Volumes
######Monitoring Volumes with CloudWatch
some metrics:
```
VolumeReadBytes
VolumeWriteBytes
```
######Monitoring Volumes with Status Checks
Volume status checks are automated tests that run every 5 minutes and return a pass or fail status.   
Volume status is based on the volume status checks, and does not reflect the volume state. Therefore, volume status does not indicate volumes in the error state (for example, when a volume is incapable of accepting I/O.)  

(tbc)



##Troubleshooting
###What To Do If An Instance Immediately Terminates
######Getting the Reason for Instance Termination
Reasons
- You've reached your EBS volume limit. For information about the volume limit, and to submit a request to increase your volume limit, see Request to Increase the Amazon EBS Volume Limit.
- An EBS snapshot is corrupt.  

describe reason aws cli
```
aws ec2 describe-instances --instance-id instance_id
```
###Troubleshooting Connecting to Your Instance
######Error connecting to your instance: Connection timed out
Check your security group rules. You need a security group rule that allows inbound traffic.  
In route table, choose Edit, Add another route, enter 0.0.0.0/0 in Destination, select your Internet gateway from Target.  
The default network ACL allows all inbound and outbound traffic.(enable both inbound and outbound)  
Check that your instance has a public IP address.  
######Error: Host key not found, Permission denied (publickey), or Authentication failed, permission denied
default user:
- For an Amazon Linux AMI, the user name is ec2-user.
- For a RHEL5 AMI, the user name is either root or ec2-user.
- For an Ubuntu AMI, the user name is ubuntu.
- For a Fedora AMI, the user name is either fedora or ec2-user.
- For SUSE Linux, the user name is either root or ec2-user.  

######Error: Unprotected Private Key File
chmod 400  

###Troubleshooting Stopping Your Instance
- To create a replacement instance (if the previous procedure fails)  
Select the instance, open the Description tab, and view the Block devices list.Take note root volume's ID  
create a snapshot from this volume id  
create a volume from this snapshot  
create a new instance, detach default root volume, attach the volume we created
###Troubleshooting Terminating (Shutting Down) Your Instance

###Troubleshooting Instance Recovery Failures
###Troubleshooting Instances with Failed Status Checks
(tbc)
###Troubleshooting Instance Capacity
######Error: InsufficientInstanceCapacity

###Getting Console Output and Rebooting Instances



