**Challenge 5: Backwards**

**Challenge Statement**

The objective of this challenge was to access a secret stored in AWS Systems Manager (SSM) by assuming a specific IAM role that had the required permissions. The challenge tested understanding of IAM roles, trust relationships, and how to securely access secrets using the AWS CLI.

**Short analysis**

This task involved IAM role assumption, policy inspection, and secret retrieval. The key requirement was to assume the "Alexander-Arnold" IAM role using a non-root IAM user. Initially, the user lacked the necessary permissions due to a restrictive trust policy. After updating the trust relationship to grant access, I was able to assume the role and retrieve the secret from AWS SSM.


**Steps Taken to Solve the Challenge**

**Steps-1** - I attempted to assume the role "Alexander-Arnold" using the AWS CLI with the following command:

```
  aws sts assume-role --role-arn arn:aws:iam::307946660251:role/Alexander-Arnold --role-session-name Alexander-Arnold
```

This failed because the user didn’t have permission to assume the role.

**Steps-2** - I identified the issue in the trust relationship of the role and updated it to include the non-root user as a valid principal.

**Steps-3** - After updating the trust policy, I retried the assume-role command and successfully received temporary credentials.

```
  aws sts assume-role --role-arn arn:aws:iam::307946660251:role/Alexander-Arnold --role-session-name Alexander-Arnold
```
  
This gave temporary security credentials.

**Steps-4** - Using the assumed role credentials, I ran:

```
 aws secretsmanager get-secret-value --secret-id DomainAdministrator-Credentials --region us-west-2 --profile assumed-role
```
 
This returned the secret: 

```
    FLAG{backwards::IfYouFindSomethingInterstingFindWhoHasAccessToIt}
```

**Steps-5** - I also reviewed the attached IAM policies for the role to confirm it had the necessary permissions for Secrets Manager access.

**Reflection**

This challenge highlighted the importance of understanding how IAM trust relationships work. The main blocker was the trust policy, which didn’t initially allow the user to assume the role. Identifying and correcting that issue was the key to progressing. Once that was resolved, the rest of the process—assuming the role and retrieving the secret—went smoothly using standard AWS CLI commands. The experience reinforced how crucial trust policies are in controlling access to sensitive resources in AWS.
