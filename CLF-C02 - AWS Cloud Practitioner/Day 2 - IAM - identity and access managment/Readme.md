# IAM

IAM: Users and Groups

- IAM - Identity and Access Management, Global service
- Root account created by default, should't be used or shared
- Users are people within your organization, and can be grouped
- Groups only contain users, not other groups
- Users don't have to belong to a group, and user can belong to multiple groups

IAM: Permissions 

Users or groups can be assigned JSON documents called policies 


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
2. Go to users
we will create new users because by default it uses root user and it is not best practice 

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

in user and user groups (left in window) can see user, groups and there permsions

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


