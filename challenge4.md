# Challenge number 3

## Challenge Statement
The challenge needs us to get a flag from an AWS S3 bucket using the permissions granted to us by the IAM policy.

## IAM Policy

{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": "*",
      "Action": "s3:GetObject",
      "Resource": "arn:aws:s3:::thebigiamchallenge-admin-storage-abf1321/*"
    },
    {
      "Effect": "Allow",
      "Principal": "*",
      "Action": "s3:ListBucket",
      "Resource": "arn:aws:s3:::thebigiamchallenge-admin-storage-abf1321",
      "Condition": {
        "StringLike": {
          "s3:prefix": "files/*"
        }
      },
      "ForAllValues:StringLike": {
        "aws:PrincipalArn": "arn:aws:iam::133713713737:user/admin"
      }
    }
  ]
}

    

## Analysis about the IAM policy

The policy permits s3:GetObject to be performed on all objects in thebigiamchallenge-admin-storage-abf1321 by any principal ("Principal": "*"). This implies that we don't need to authenticate in order to retrieve things straight from this bucket. It is also noted that it is not possible to list objects in the bucket (s3:ListBucket).
When showing files in the files/ prefix, the s3:ListBucket action includes a condition that limits access to arn:aws:iam::133713713737:user/admin only.
This implies that if we know the precise names of the objects, we can still get them even when we are unable to display files in the bucket.
Furthermore, the fact that s3:GetObject has public access but that only a single IAM user can list it. So, by speculating about the object names and downloading them straight away, we can get around the listing restriction.

## Solution

Tried using the following to list the files in the S3 bucket:

```
aws s3 ls s3://thebigiamchallenge-admin-storage-abf1321/files/ 
```
But I got an access denied error.

So, I tried lsiting the files using the command below:

```
aws s3 ls s3://thebigiamchallenge-admin-storage-abf1321/files/ --no-sign-request
```

Since s3:GetObject is unrestricted, I tried downloading a potential file name straight away and read it:

```
aws s3 cp s3://thebigiamchallenge-admin-storage-abf1321/files/flag-as-admin.txt /tmp/ --no-sign-request
```

The flag obtained was:

```
{wiz:principal-arn-is-not-what-you-think}
```
    
## Reflection
My approach involved analyzing the IAM policy, identifying that listing files was restricted while object retrieval was open, and successfully downloading a file by guessing its name. The main challenge was the s3:ListBucket restriction, which made it difficult to discover available files. This was overcome by inferring likely filenames based on naming patterns. The breakthrough came from realizing that s3:GetObject was fully open despite listing restrictions. To enhance security, public access to s3:GetObject should be minimized, listing restrictions should be complemented with proper authentication, and AWS S3 Block Public Access settings should be enforced.
