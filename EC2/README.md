

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

#####Amazon EBS Volume Types

