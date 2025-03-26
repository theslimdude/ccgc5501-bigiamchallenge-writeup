
# Cloudfoxable 4

## Challenge Statement
It's another secret 

### What was this challenge about?
```
1. Assuming an IAM role (named "Ertz") from a starter account.
2. Troubleshooting and resolving permission issues when role assumption fails.
3. Securely retrieving secrets using the AWS CLI.
```

## My Initial Setup

I started as a non-root-user with Admin access but quickly encountered an issue—the account lacked the necessary permissions to assume the Ertz role. The trust policy for the role only permitted the ctf-starting-user to take over.

## Solution
### 1. First Attempt  

I initially ran:

```  
aws --profile cloudfoxable sts assume-role --role-arn arn:aws:iam::307946660251:role/Ertz --role-session-name Ertz  
```  
Upon reviewing the role’s trust policy, I realized that my user (non-root-user) wasn’t included as an authorized principal.

### 2. Updating the Trust Policy  
So, I modified the `Ertz` role’s trust policy to include my user:  

```  
{  
  "Version": "2012-10-17",  
  "Statement": [{  
    "Effect": "Allow",  
    "Principal": {  
      "AWS": [  
        "arn:aws:iam::307946660251:user/ctf-starting-user",  
        "arn:aws:iam::307946660251:user/non-root-user"  
      ]  
    },  
    "Action": "sts:AssumeRole"  
  }]  
}  
```  

### 3. Assuming the Role Successfully  

After updating the policy, I reran the assume-role command, and this time it was successful. This allowed me to obtain temporary credentials to operate as the Ertz role.

### 4. Retrieving the Secret  

With Ertz permissions in place, I accessed the secret flag using:
```  
aws ssm get-parameter --name "/cloudfoxable/flag/its-another-secret" --with-decryption --region us-west-2  
```  

And so the Flag I retrieved was:  

```  
FLAG{ItsAnotherSecret::ThereWillBeALotOfAssumingRolesInThisCTF}  
```  

### 5. Verifying Permissions  
I confirmed that the Ertz role had an attached policy granting SSM access, which explained why I could retrieve the secret after assuming the role.

## Reflection
The biggest challenge I faced was structuring the trust policy correctly, as I initially struggled with getting the JSON format right. The key breakthrough was recognizing that I had to explicitly add my user ARN to the role's trust relationship. This challenge reinforced essential security principles, such as following the principle of least privilege to avoid excessive Admin access, regularly auditing role trust policies, and securely storing secrets in AWS Systems Manager or Secrets Manager instead of plaintext. Overall, this hands-on experience with role assumption and trust policies was invaluable for understanding cloud security.
