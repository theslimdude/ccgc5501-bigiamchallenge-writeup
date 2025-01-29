Challenge number 1
Challenge Statement

The first challenge in The Big IAM Challenge involves an S3 bucket configured with a public IAM policy that allows anyone to perform specific actions under certain conditions. The policy permits the s3:GetObject action on objects within the bucket and the s3:ListBucket action on the bucket itself, but only when the s3:prefix begins with files/*. To solve this challenge, we must list the contents of the files/ directory and retrieve the flag1.txt file contained within it.
IAM Policy

"Version": "2012-10-17",
"Statement": 
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


Analysis about the IAM policy

Public users have access to list objects in the bucket, but only if the prefix starts with files/, and they can retrieve objects (s3:GetObject) within the files/ directory. However, access is restricted for any objects outside the files/ directory, and actions beyond s3:GetObject and s3:ListBucket are not permitted. Interestingly, while the policy grants broad public access, it enforces a specific condition (s3:prefix) to limit the scope of access. Despite this, the unrestricted s3:GetObject action for files/* poses a potential risk of data exfiltration.
Solution

    To retrieve the flag, use the AWS CLI to list the contents of the files/ directory by running the command: aws s3 ls s3://thebigiamchallenge-storage-9979f4b/files/ --no-sign-request. This will display all accessible files within the files/ directory.

    Next, download the flag1.txt file from the bucket using the command: aws s3 cp s3://thebigiamchallenge-storage-9979f4b/files/flag1.txt /tmp/ --no-sign-request. This will save the flag file to your local /tmp/ directory.

Reflection
My approach involved analyzing the IAM policy to identify the allowed actions and using the AWS CLI with the --no-sign-request flag for anonymous access. The biggest challenge was understanding the exact scope of the Condition and ensuring the prefix matched the required format. I overcame this by carefully reviewing the policy and testing commands with the --no-sign-request flag. The breakthrough came when I realized the condition enforced on the prefix, which helped narrow the focus to the files/ directory. From a blue team perspective, this challenge highlights the importance of avoiding public access to sensitive resources, even with conditions. Stricter measures, such as IP whitelisting, requiring AWS credentials, and regular audits of IAM policies, are essential to ensure the principle of least privilege is upheld.
