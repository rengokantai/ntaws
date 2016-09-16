##Introduction

######Objects

An object is uniquely identified within a bucket by a key (name) and a version ID.

######Keys

Every object in a bucket has exactly one key.
http://doc.s3.amazonaws.com/2006-03-01/AmazonS3.wsdl, "doc" is the name of the bucket and "2006-03-01/AmazonS3.wsdl" is the key.

######Amazon S3 Data Consistency Model

- Amazon S3 provides read-after-write consistency for PUTS of new objects in your S3 bucket in all regions with one caveat. The caveat is that if you make a HEAD or GET request to the key name (to find if the object exists) before creating the object, Amazon S3 provides eventual consistency for read-after-write.  

- Amazon S3 does not currently support object locking. If two PUT requests are simultaneously made to the same key, the request with the latest time stamp wins.  

- Read the three pics  

##Making Requests



##Buckets
###Restrictions and Limitations
- Bucket ownership is not transferable
- You cannot create a bucket within another bucket.
- Bucket names must be a series of one or more labels. Adjacent labels are separated by a single period (.). 
- Bucket names can contain lowercase letters, numbers, and hyphens. Each label must start and end with a lowercase letter or a number.
- We recommend that you do not use periods (".") in bucket names.
- The name of the bucket used for Amazon S3 Transfer Acceleration must be DNS-compliant and must not contain periods (".").
- special:The rules for bucket names in the US East (N. Virginia) region allow bucket names to be as long as 255 characters, and bucket names can contain any combination of uppercase letters, lowercase letters, numbers, periods (.), hyphens (-), and underscores (_).

###Requester Pays Buckets
- A bucket owner can configure a bucket to be a Requester Pays bucket. With Requester Pays buckets, the requester instead of the bucket owner pays the cost of the request and the data download from the bucket. The bucket owner always pays the cost of storing data.
- If you enable Requester Pays on a bucket, anonymous access to that bucket is not allowed.
- You must authenticate all requests involving Requester Pays buckets. 
- After you configure a bucket to be a Requester Pays bucket, requesters must include x-amz-request-payer in their requests either in the header, for POST, GET and HEAD requests

(tbc)


###Billing and Reporting



##Managing Access
###Introduction
####Overview









##Protecting Data in Amazon S3

Amazon S3 also regularly verifies the integrity of data stored using checksums. If Amazon S3 detects data corruption, it is repaired using redundant data. Amazon S3's standard storage is:

Amz S3 SLA
Designed to provide 99.999999999% durability and 99.99% availability of objects over a given year
Designed to sustain the concurrent loss of data in two facilities
###Data Encryption
####Server-Side Encryption 
######Amazon S3-Managed Encryption 
- Keys Server-side encryption encrypts only the object data. Any object metadata is not encrypted. ######Customer-Provided Encryption Keys
- With the encryption key you provide as part of your request, Amazon S3 manages both the encryption, as it writes to disks, and decryption, when you access your objects.  
- When you upload an object, Amazon S3 uses the encryption key you provide to apply AES-256 encryption to your data and removes the encryption key from memory.  
- When you retrieve an object, you must provide the same encryption key as part of your request.  
####Client-Side Encryption
#####Using an AWS KMS–Managed Customer Master Key
#####Using a Client-Side Master Key

###Reduced Redundancy Storage

####Setting the Storage Class of an Object You Upload

To set the storage class of an object you upload to RRS, you set x-amz-storage-class to REDUCED_REDUNDANCY in a PUT request.

####Changing the Storage Class of an Object in Amazon S3

x-amz-metadata-directive set to COPY  
x-amz-storage-class set to STANDARD, STANDARD_IA(Standard-Infrequent Access), or REDUCED_REDUNDANCY
If you copy an object and fail to include the x-amz-storage-class request header, the storage class of the target object defaults to STANDARD.

#####Return Code for Lost Data
return error code=405 Method Not Allowed error

Versioning

Buckets can be in one of three states: unversioned (the default), versioning-enabled, or versioning-suspended.

Once you version-enable a bucket, it can never return to an unversioned state. You can, however, suspend versioning on that bucket.
Objects stored in your bucket before you set the versioning state have a version ID of null. When you enable versioning, existing objects in your bucket do not change.
Deleting Object Versions

When versioning is enabled, a simple DELETE cannot permanently delete an object. Instead, Amazon S3 inserts a delete marker in the bucket.
To permanently delete versioned objects, you must use DELETE Object versionId.
Removing Delete Markers

To delete a delete marker, you must specify its version ID in a DELETE Object versionId request. If you use a DELETE request to delete a delete marker (without specifying the version ID of the delete marker), Amazon S3 does not delete the delete marker, but instead, inserts another delete marker.

Restoring Previous Versions

check

Versioned Object Permissions

check

Managing Objects in a Versioning-Suspended Bucket/Adding Objects to Versioning-Suspended Buckets

Once you suspend versioning on a bucket, Amazon S3 automatically adds a null version ID to every subsequent object stored thereafter
If there are versioned objects in the bucket, the version you PUT becomes the current version of the object.

Managing Objects in a Versioning-Suspended Bucket/Retrieving Objects from Versioning-Suspended Buckets

Managing Objects in a Versioning-Suspended Bucket/Deleting Objects from Versioning-Suspended Buckets

Even in a versioning-suspended bucket, the bucket owner can permanently delete a specified version. (delete with id specified)

##Hosting a Static Website

###Configure a Bucket for Website Hosting

exp1: prefix(folder) redirection
```
  <RoutingRules>
    <RoutingRule>
    <Condition>
      <KeyPrefixEquals>docs/</KeyPrefixEquals>
    </Condition>
    <Redirect>
      <ReplaceKeyPrefixWith>documents/</ReplaceKeyPrefixWith>
    </Redirect>
    </RoutingRule>
  </RoutingRules>
```
exp2: redirect to a file
```
    <RoutingRules>
    <RoutingRule>
    <Condition>
      <KeyPrefixEquals>docs/</KeyPrefixEquals>
    </Condition>
    <Redirect>
      <ReplaceKeyWith>404.html</ReplaceKeyWith>
    </Redirect>
    </RoutingRule>
  </RoutingRules>
```
http error
```
   <RoutingRules>
    <RoutingRule>
    <Condition>
      <HttpErrorCodeReturnedEquals>404</HttpErrorCodeReturnedEquals >
    </Condition>
    <Redirect>
      <HostName>ec2-11-22-333-44.compute-1.amazonaws.com</HostName>
      <ReplaceKeyPrefixWith>report-404/</ReplaceKeyPrefixWith>
    </Redirect>
    </RoutingRule>
  </RoutingRules>
```
###Example Walkthroughs 
####Example: Setting Up a Static Website Using a Custom Domain dig syntax
```
dig +recurse +trace www.example.com any
```
##Notifications 
tbc

##Performance Optimization
###Request Rate and Performance Considerations

##Monitoring with Amazon CloudWatch
######Amazon S3 CloudWatch Metrics
```
BucketSizeBytes
NumberOfObjects
```
##Error Handling
###The REST Error Response
######Response Headers
Response headers returned by all operations:
- x-amz-request-id: A unique ID assigned to each request by the system.  
- x-amz-id-2: A special token that will help us to troubleshoot problems.  

####Error Response
#####Error Code
###Amazon S3 Error Best Practices

######Retry InternalErrors
If Amazon S3 returns an InternalError response, retry the request.

##Troubleshooting Amazon S3
######General: Getting my Amazon S3 request IDs
- Using HTTP  

You can obtain your request IDs, x-amz-request-id and x-amz-id-2 by logging the bits of an HTTP request before the it reaches the target application.  

##Server Access Logging
######Ovewview
Each access log record provides details about a single access request.  
There is no extra charge for enabling server access logging on an Amazon S3 bucket.  
- Best Effort Server Log Delivery  
The completeness and timeliness of server logging, however, is not guaranteed. The log record for a particular request might be delivered long after the request was actually processed, or it might not be delivered at all.  
###Log Format
- 
