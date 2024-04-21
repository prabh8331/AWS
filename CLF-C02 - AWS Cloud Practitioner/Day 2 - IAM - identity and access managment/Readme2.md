# IAM

## page2

### AWS Access Keys, CLI, SDK

- To access AWS, you have 3 options:
  - AWS Management Console (protected by password + MFA)
  - AWS Command Line Interface (CLI) : protected by access keys
  - AWS Software Developer Kit (SDK) - for code: protected by access keys
- Users manager their own access keys

Go to username on top right - open dropdown - Security credentials 
Access keys - create access key
- use case - Command Line interface (CLI)


### AWS CLI

- Alternative to use AWS Management COnsole 
- direct Access to the public API of AWS service
- enables to interact with AWS services using commands in your command-line shell

### AWS SDK

- AWS Software Development Kit (AWS SDK)
- Language-specific APIs (set of libraries)
- Enable access and manage AWS services programmatically
- Embedded within your application
- Supports
  - SDKs (Python, Java etc)
  - Mobile SDKs (Andriod, IOS etc)
  - IoT Device SDKs (Arduino etc)
- Example: AWS CLI is built on AWS SDK for Python (called boto)


Setup 
windows - https://aws.amazon.com/blogs/developer/aws-cli-v2-installers/

Linux - 
https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html

```bash
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
unzip awscliv2.zip
sudo ./aws/install

aws --version

aws configure

    AWS Access Key ID [None]: youraccesskeyid
    AWS Secret Access Key [None]: yoursecretaccesskey
    Default region name [None]: ap-south-1
    Default output format [None]: 

```

```bash

aws iam list-users

```


alternative is CloudShell of AWS




### IAM Roles for Service

- Some AWS service will need to perform actions on our behalf
- To do so, we will need to assign permissions to AWS services with IAM Roles (just like users need permission to perform action)
- IAM role is not intended to be used by physical user but by AWS services 
- e.g. EC2 instance (virtual server) may want to perform some actions on AWS and to do so, we need to give permissions to this EC2 instance using IAM role, and EC2 instance and IAM role together makes one entity
  - now when this EC2 instance will try to access some information from AWS it will check permission in IAM role and if have permission correct with we will get the response
- Common roles
  - EC2 Instance Roles
  - Lambda Function Roles
  - Roles for CloudFormation

Roles is a way to give AWS entities permissions to do stuff on AWS

### IAM Roles Hands on

- Go to IAM service - Roles (left hand side) - Create role - AWS Service - use case (choose service) = EC2 --> 
- Add permission -  IAMReadOnlyAccess
- Role name - EC2TestRole - Save




### IAM Security Tools

- IAM Credentials Reports (account-level)
- IAM Access Advisor (user-level)

IAM --> credentials report --> Download

IAM --> User --> select user (prabh-singh1) --> Access Advisor --> this will show which service were used and when


### IAM Guidelines & Best Practices

- Don't use the root account except for AWS account setup
- One physical user = One AWS user
- Assign users to groups and assign permissions to groups
- Create a strong password policy
- Use and enforce the use of Multi Factor Authentication (MFA)
- Create and use Roles for giving permissions to AWS service
- use Access Keys for Programmatic Access (CLI/SDK)
- Audit permissions of your account using IAM Credentials Report & IAM Access Advisor
- Never share IAM users & Access Keys

### Shared Responsibitiy Model for IAM

AWS is responsible for all the infrastructure and you are responsible for how you use that infrastructure

AWS is responsible for what they do

- Infrastructure (global network security)
- Configuration and vulnerability analysis
- Compliance validation


You are responsible for thing what AWS will not do for you

- Users, Groups, Roles, Policies management and monitoring
- Enable MFA on all accounts
- Rotate all your keys often
- Use IAM toolS to apply appropriate permissions
- Analyze access patterns and review permissions



## Summary 

- Users: mapped to a physical user, has a password for AWS Console
- Groups: contains users only 
- Policies: JSON document that outlines permissions for users or groups
- Roles: for EC2 instances or AWS services
- Security: MFA + Password Policy
- AWS CLI: manage your AWS services using the command-line
- AWS SDK: manage your AWS services using a programming language
- Access Keys: access AWS using the CLI or SDK
- Audit: IAM Credential Reports & IAM Access Advisor


### AWS Budget Setup

1. Activate Billing infromation for IAM users
    Go to username on top right - open dropdown - Account
    IAM user and role access to Billing information - Edit
    Activate IAM Access
2. Bills
    See bills to check which service is costing -
    Go to username on top right - open dropdown - Billing and Cost Management - bills
3. Budgets
   Go to username on top right - open dropdown - Billing and Cost Management - budgets
   Create a budget

