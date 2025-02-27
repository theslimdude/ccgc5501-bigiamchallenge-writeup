# Challenge 1 "Do this first!"

## Challenge Statement
CloudFoxable CTF Challenge 

### Overview
This overview describes how I followed the setup guidelines, used Terraform to deploy the environment, and retrieved the first flag in order to solve the CloudFoxable CTF challenge. Using technologies like Terraform, AWS CLI, and CloudFox, the challenge's objective was to comprehend AWS security misconfigurations and enumeration methodologies.

## Set Up Process

1. Setting Up the AWS Environment
To make sure that no sensitive data or production resources were impacted, a fresh AWS account was created.

produced an access key and created a non-root IAM user with administrative access.

AWS CLI was installed and set up using the credentials of the recently created admin user.

Verified AWS CLI configuration by running:
```
aws sts get-caller-identity
```
This displayed account and user details, confirming that the setup was correct.

2. Deploying CloudFoxable with Terraform
Terraform was installed and added to the PATH of the system.

Cloned the CloudFoxable repository:
```
git clone https://github.com/BishopFox/cloudfoxable
cd cloudfoxable/aws
```

Initialized Terraform:
```
terraform init
```

Copied and configured ```terraform.tfvars``` file:
```
cp terraform.tfvars.example terraform.tfvars
```
Edited the file to set my AWS profile:
```
aws_local_profile = "my-aws-profile"
```

Applied the Terraform configuration:
```
terraform apply
```
In addition to providing output details about the generated resources, this deployed the required infrastructure.

## Finding the First Flag
At the conclusion of the deployment, the Terraform output was carefully scrutinized.

Information regarding configuring the AWS CLI for the ```ctf-starting-user``` was included in the output.

Followed the given instructions to configure the AWS CLI for the ```ctf-starting-user```.

A flag format was discovered in the Terraform output:
```
FLAG{challengeName::CamelCaseText}
```
The flag was taken out of the Terraform output:
```
FLAG{congrats_you_are_now_a_terraform_expert_happy_hunting}
```

## Clean-Up
I performed the following to clear out all CloudFoxable issues and optimize resources:
```
terraform destroy
```
This ensured that all deployed infrastructure was deleted, avoiding any unnecessary AWS charges.

## Reflection
The challenge brought to light the possible dangers of incorrect IAM configurations. Furtheremoe, I got to know that Terraform facilitates the spinning up and taking down of cloud environments and reviewing deployment results is crucial since crucial security information is sometimes concealed from view.

