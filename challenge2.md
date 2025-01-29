# Challenge number 2

## Challenge Statement
The second challenge in The Big IAM Challenge involves an SQS (Simple Queue Service) queue with an IAM policy that allows public access to send and receive messages. The policy allows anyone to perform the sqs:SendMessage and sqs:ReceiveMessage actions on the wiz-tbic-analytics-sqs-queue-ca7a1b2 queue.

## IAM Policy

    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Principal": "*",
            "Action": [
                "sqs:SendMessage",
                "sqs:ReceiveMessage"
            ],
            "Resource": "arn:aws:sqs:us-east-1:092297851374:wiz-tbic-analytics-sqs-queue-ca7a1b2"
        }
    ]

## Analysis about the IAM policy

This challenge assesses the ability to recognize authentication protocols, retrieve important information from a web page, and appropriately implement AWS CLI commands to ensure secure connection with AWS services. It emphasizes security best practices by utilizing Cognito for temporary credential management, hence mitigating the risk associated with static AWS credentials. Key considerations include correctly configuring IAM roles and permissions for Cognito identities, guaranteeing accurate region specifications, and dealing with any issues such as insufficient permissions or erroneous identity pool setups.

## Solution

1. To solve this challenge, we use the AWS CLI to obtain temporary credentials from Amazon Cognito and interact with an SQS queue. First, retrieve an identity ID using the command:

aws cognito-identity get-id --identity-pool-id "us-east-1:c6f3eb2e-3cb5-404e-93bc-f0bdf7ad042e"

2. The identity pool ID can be found in the JavaScript code of the challenge page. Next, obtain temporary AWS credentials for that identity using:

aws cognito-identity get-credentials-for-identity --identity-id "us-east-1:9ea8f9af-f687-439b-951d-0e83653f6be7"

3. Then, verify the credentials with:

aws sts get-caller-identity --profile challenge2

4. Once verified, send a message to the SQS queue using:

aws sqs send-message --queue-url https://sqs.us-east-1.amazonaws.com/092297851374/wiz-tbic-analytics-sqs-queue-ca7a1b2 --message-body "Hello, World" --profile challenge2 --region us-east-1

5. Finally, retrieve the message with:

aws sqs receive-message --queue-url https://sqs.us-east-1.amazonaws.com/092297851374/wiz-tbic-analytics-sqs-queue-ca7a1b2 --profile challenge2 --region us-east-1

6. Both SQS commands use the --profile challenge2 and --region us-east-1 flags to specify the authentication profile and region. Upon receiving the message, a link to an HTML page was provided in response.

## Reflection
Solving Big IAM Challenge 2 was a valuable exercise in AWS identity management, temporary credentials, and secure access to cloud services. Extracting the identity pool ID from JavaScript reinforced the importance of analyzing web authentication mechanisms. Using AWS Cognito for temporary credentials highlighted best practices in cloud security, while verifying them with aws sts get-caller-identity ensured proper authentication. Sending and receiving messages via Amazon SQS demonstrated real-world applications of queue-based communication. Overall, the challenge emphasized secure authentication, IAM role configurations, and AWS CLI troubleshooting, making it a great hands-on learning experience.
