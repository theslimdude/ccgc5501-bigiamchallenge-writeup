# CloudFoxable - Bastion 100

## Challenge Overview

The Bastion 100 challenge falls under the "Assumed Breach: Application Compromise/Network Access" category. The objective is to gain access to an EC2 instance and utilize its IAM permissions to retrieve the flag.

## Solution Approach
1. Deploying the Challenge

To begin, I needed to enable the challenge by modifying the Terraform configuration:

    Updated the file cloudfoxable/aws/terraform.tfvars and set bastion_enabled = true.
    Executed terraform apply to provision the required AWS infrastructure.

2. Identifying the EC2 Instance

After deployment, I needed to determine the instance ID of the newly created EC2 instance. Using CloudFox, I ran:

```cloudfox aws -p cloudfoxable instances -v2```

This provided details about the EC2 instances, including the instance ID and other relevant metadata.

3. Accessing the EC2 Instance via SSM

Instead of using traditional SSH access, the challenge encouraged the use of AWS Systems Manager (SSM). To connect, I:

    Installed the AWS Session Manager plugin.
    Used the loot file to execute the required command.

This allowed me to establish a session with the instance without needing SSH keys.

4. Enumerating IAM Permissions

Once inside the EC2 instance, I investigated the available IAM permissions by running:

```cloudfox aws -p cloudfoxable permissions```

Alternatively, I could target the instance’s IAM role directly to check its permissions.

5. Retrieving the Flag

By reviewing the permissions, I discovered access to specific IAM actions. This enabled me to retrieve credentials or interact with resources that eventually led me to the flag:

```{FLAG:bastion::ifYouHaveAccessToAnEC2YouHaveAccessToItsIamPermissions}```

This reinforced the concept that gaining access to an EC2 instance effectively grants access to its IAM permissions.
Cleanup

To prevent unnecessary resource usage, I cleaned up the environment after obtaining the flag by:

    Modifying terraform.tfvars and setting bastion_enabled = false.
    Running terraform apply to remove the deployed resources.

## Key Takeaways

This challenge illustrated the use of AWS Systems Manager (SSM) for EC2 instance access and demonstrated how CloudFox can be leveraged to enumerate IAM permissions. The primary lesson learned is that compromising an EC2 instance often means inheriting its IAM permissions, which can be exploited for privilege escalation or data access.
