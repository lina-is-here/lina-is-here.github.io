---
layout: post
title:  "How to create automatic notifications in AWS SQS when the new object is added to S3 bucket"
date:   2022-03-09
tags: [aws, s3, sqs]
---

AWS has great documentation, but I want to document some things that were easy to miss when I configured notifications 
to Amazon SQS (Simple Queue Service) from S3 (Simple Storage Service) bucket.

To achieve this nice flow of notifications, the first thing to do is:

 **1. Create S3 bucket.**

There are several ways to do it. Probably, the easiest is using AWS console.
Navigate to S3 service and click on "Create bucket".

 **2. Create SQS queue.**

<!--more-->
In AWS console, navigate to SQS service and click "Create queue".

**IMPORTANT:** Create a **standard queue**, S3 notifications don't work with FIFO queues! See the 
[compatibility section](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/FIFO-compatibility.html) 
for FIFO queues in the official documentation.

 **3. Modify access policy for the queue.**

Navigate to the "Access policy" tab of the SQS queue created above. Click "Edit".

Add the following policy:

{% highlight json %}
{
  "Version": "2008-10-17",
  "Id": "__default_policy_ID",
  "Statement": [
    {
      "Sid": "__owner_statement",
      "Effect": "Allow",
      "Principal": {
        "Service": "s3.amazonaws.com"
      },
      "Action": "SQS:*",
      "Resource": "arn:aws:sqs:{region}:{account_ID}:{queue_name}",
      "Condition": {
        "StringEquals": {
          "aws:SourceAccount": "{account_ID}"
        },
        "ArnLike": {
          "aws:SourceArn": "arn:aws:s3:*:*:{bucket_name}"
        }
      }
    }
  ]
}
{% endhighlight %}

Resource ARN (for `"Resource": "arn:aws:sqs:{region}:{account_ID}:{queue_name}"`) can be found on the Details page of 
the created queue.

 **4. Add notifications for the S3 bucket.**
 
Navigate to the bucket's Detail page.

Click on "Properties" tab.

In the "Event notifications" section click "Create event notification".

Choose the name, desired notifications and choose the created SQS queue.

Save changes.

**That's it!** Notifications from S3 bucket are now flowing to the SQS queue.

