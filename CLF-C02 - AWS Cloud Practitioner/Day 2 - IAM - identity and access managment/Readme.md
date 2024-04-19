# IAM

IAM: Users and Groups

- IAM - Identity and Access Management, Global service
- Root account created by default, should't be used or shared
- Users are people within your organization, and can be grouped
- Groups only contain users, not other groups
- Users don't have to belong to a group, and user can belong to multiple groups

IAM: Permissions 

- Users or groups can be assigned JSON documents called policies 
- These policies define the permissions of the users
- in AWS you apply hte least privilege principle: don't give more permissions than a user needs


```JSON

{
        "Version": "2012-10-17",
        "Statement": [
            {
                "Effect": "Allow",
                "Action": "ec2:Describe*",
                "Resource": "*"
            },
            {
                "Effect": "Allow",
                "Action": "elasticloadbalancing:Describe*",
                "Resource": "*"
            },
            {
                "Effect": "Allow",
                "Action": [
                    "cloudwatch:ListMetrics",
                    "cloudwatch:GetMetricStatistics",
                    "cloudwatch:Describe*"
                ],
                "Resource": "*"
            },
        ]
}

```

## IAM Users and Groups Hands On

1. Search IAM, and go the service, IAM is global service
2. Go to Users (left hand side)
we will create new users because by default it uses root user and it is not best practice to use root user

Enter following 
  - User name 
  - tick - Provide user access to the AWS Managment Console 
  - select - I want to create an IAM user
  - Custom password

next 

- Add user to group
- Create user group - admin (give - AdministratorAccess)
- select admin

next

(tags are optional - it is key-pair value, this is for the metadata)


in user and user groups (left in window) can see user, groups and there permissions


### create login alias
go to IAM Dashboard and create new alias and get the login pass

https://prabhsingh.signin.aws.amazon.com/console

Account ID or account alias - prabhsingh
IAM user name - user1
passowrd - 


### IAM Policies 

1. IAM Policies inheritance

| Group | users |
| --- | --- |
| Developers | Alice, Bob, Charles |
| Operations | David, Edward |
| Audit Team | Charles, David |
| inline | Fred |


when policies are applied at group level all group members will get access and inherit this policyS

Fred has inline policies , i.e. it does not belong to any group


1. IAM Policies Structure

- Consists of 
  - Version: policy language version, always include "2012-10-17"
  - Id: an identifier for the policy (optional)
  - Statement: one or more individual statements (required)

- Statements consists of
  - Sid: an identifier for the statement (optional)
  - Effect: whether the statement allows or denies access (Allow, Deny)
  - Principal: account/user/role to which this policy applied to (in example it is being applied to root user)
  - Action: list of actions this policy allows or denies
  - Recourse: list of resources to which the actions applied to
  - Condition: conditions for when this policy is in effect (optional)

effect - allow or deny
Princpal - Who
Action - actual policy written here
resourse - services this policy is for

```JSON

{
        "Version": "2012-10-17",
        "Id": "S3-Account-Permissions",
        "Statement": [
            {
                "Sid": "1",
                "Effect": "Allow",
                "Principal": {
                    "AWS": ["arn:aws:iam::123456789012:root"]
                },
                "Action": [
                    "s3:GetObject",
                    "s3:PutObject"
                    ],
                "Resource": ["arn:aws:s3:::mybucket/*"]
            }
        ]
}
```

### IAM Policies Hands on

1. create a new user
user2
and made this user as a part of admin group
2. login with user2 and go to IAM service and go to users
   1. here all the users will show
3. now in root user go to user groups - open admin - click on user2 and remove
   1. doing so when go back to user2 login then users will not show
