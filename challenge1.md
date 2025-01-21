# Challenge number 1

## Challenge Statement
The first challenge in The Big IAM Challenge involves an S3 bucket configured with a public IAM policy that allows anyone to perform specific actions under certain conditions. The policy permits the s3:GetObject action on objects within the bucket and the s3:ListBucket action on the bucket itself, but only when the s3:prefix begins with files/*. To solve this challenge, we must list the contents of the files/ directory and retrieve the flag1.txt file contained within it.

## IAM Policy

    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Principal": "*",
            "Action": "s3:GetObject",
            "Resource": "arn:aws:s3:::thebigiamchallenge-storage-9979f4b/*"
        },
        {
            "Effect": "Allow",
            "Principal": "*",
            "Action": "s3:ListBucket",
            "Resource": "arn:aws:s3:::thebigiamchallenge-storage-9979f4b",
            "Condition": {
                "StringLike": {
                    "s3:prefix": "files/*"
                }
            }
        }
    ]



### write a short analysis about the IAM policy here

* What do I have access to?
* What don't I have access to?
* What do I find interesting?
* etc



## Solution
Detailed steps to the flag

## Reflection
* What was your approach?
* What was the biggest challenge?
* How did you overcome the challenges?
* What led to the break through?
* On the blue side, how can the learning be used to properly defend the important assets?
