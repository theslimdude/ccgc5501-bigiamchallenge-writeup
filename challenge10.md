**Challenge 5: Backwards**

**Challenge Statement**

The main goal of this challenge was to retrieve a secret from AWS Systems Manager (SSM) by assuming an IAM role with the right permissions. It focused on understanding AWS IAM roles, trust policies, and how to access protected secrets

**Short analysis**

This challenge focused on working with IAM roles, permissions, and AWS CLI. The goal was to assume the "Alexander-Arnold" IAM role using a non-root user. I reviewed the role’s policies, updated its trust relationship to allow access, and then used the role to retrieve a secret from AWS Systems Manager (SSM). Once the role was assumed, I had the necessary permissions to access the secret and complete the challenge.

**Key Tasks**:

Role Assumption: The non-root user initially lacked permission to assume the role.

Policy Update: I modified the trust policy to allow the non-root user access.

Secret Retrieval: Used AWS CLI to fetch the secret from AWS SSM.


**Steps Taken to Solve the Challenge**

**Steps-1** - I first tried to assume the "Alexander-Arnold" role using the AWS CLI with the following command:

  aws sts assume-role --role-arn arn:aws:iam::307946660251:role/Alexander-Arnold --role-session-name Alexander-Arnold

But this attempt failed because the non-root user did not have the required permissions to assume the role.

**Steps-2** - Modified the Trust Relationship

**Steps-3** - Assumed Role Access: After updating the trust policy, I successfully assumed the "Alexander-Arnold" role by executing the command: 

  aws sts assume-role --role-arn arn:aws:iam::307946660251:role/Alexander-Arnold --role-session-name Alexander-Arnold
  
This gave temporary security credentials.

**Steps-4** - Secret Retrieval: With the correct permissions in place, I used the AWS CLI to retrieve the secret stored in AWS Systems Manager (SSM) by running: 

 aws secretsmanager get-secret-value --secret-id DomainAdministrator-Credentials --region us-west-2 --profile assumed-role
 
This returned the secret: 
    FLAG{backwards::IfYouFindSomethingInterstingFindWhoHasAccessToIt}

**Steps-5** - I reviewed the permissions linked to the "Alexander-Arnold" role and confirmed that it had the necessary access to retrieve the secret. The attached policies were properly configured and matched the challenge requirements.

**Reflection**

In this challenge, my goal was to assume the "Alexander-Arnold" IAM role and retrieve a secret from AWS Systems Manager (SSM). I followed a structured approach using AWS CLI to analyze permissions, understand the role configuration, and access the required secret.

The main obstacle was the trust policy of the IAM role, which initially did not allow the non-root user to assume it. I overcame this by updating the trust policy to include the non-root user as a valid principal. This change allowed me to assume the role successfully and retrieve the flag using AWS CLI.

The key breakthrough came when I identified that the trust relationship was the issue and made the necessary policy change.
