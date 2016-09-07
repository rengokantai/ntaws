##Introduction to Amazon SQS
###Overview of Amazon SQS
A single queue can be used simultaneously by many distributed application components, with no need for those components to coordinate with each other to share the queue.  
If your system requires that order be preserved, you can place sequencing information in each message  
messages can be up to 262,144 bytes (256 KB) in size  
##Working with Amazon SQS
###Preparing the Samples
###Creating a Queue
###Add a Permission to the Queue
######AWS Management Console
Select Add a Permission from the Queue Actions drop-down list.

###Sending a Message
```
System.out.println("Sending a message to MyQueue.\n");
sqs.sendMessage(new SendMessageRequest()
    .withQueueUrl(myQueueUrl)
    .withMessageBody("This is my message text."))
```

###Receiving a Message
######AWS Management Console  
Select View/Delete Messages from the Queue Actions drop-down list.

###Deleting a Message
```
System.out.println("Deleting a message.\n");
String messageReceiptHandle = messages.get(0).getReceiptHandle();
sqs.deleteMessage(new DeleteMessageRequest()
    .withQueueUrl(myQueueUrl)
    .withReceiptHandle(messageReceiptHandle));
```

###Purging the Queue
##Where Do I Go from Here?
(tbc)
