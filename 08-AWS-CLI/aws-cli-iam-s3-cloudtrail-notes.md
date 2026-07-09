# AWS CLI, IAM Least Privilege, S3, and CloudTrail Notes

## What We Practiced

In this lab, I practiced using AWS CLI from my Mac terminal and connected it to AWS using the `developer-user` IAM identity.

The goal was not only to run AWS commands, but to understand how AWS CLI uses IAM permissions and how AWS records those actions in CloudTrail.

The main flow of the lab was:

```text
Mac Terminal → AWS CLI → IAM user → IAM permissions → AWS service access → CloudTrail logs
```

## Commands Practiced

| Command | Purpose | Result |
|---|---|---|
| `aws --version` | Checked whether AWS CLI was installed | AWS CLI was installed successfully |
| `aws configure` | Configured AWS CLI with IAM user credentials | CLI was connected to AWS as an IAM user |
| `aws sts get-caller-identity` | Verified the active AWS identity | CLI was using `developer-user` |
| `aws s3 ls` | Tested S3 bucket listing permission | S3 bucket listing succeeded |
| `aws s3 ls s3://dev-bucket-niranjan` | Tested object listing inside the S3 bucket | Object listing succeeded |
| `aws iam list-users` | Tested whether the developer user could perform IAM admin actions | AccessDenied |

## What AWS CLI Means

AWS CLI is a way to work with AWS from the terminal instead of only clicking inside the AWS Console.

In simple words:

```text
AWS Console = browser clicking
AWS CLI = terminal commands
```

AWS CLI does not work by magic. It uses IAM credentials to make AWS API calls. When I ran a command, AWS checked the permissions attached to the active IAM identity before allowing or denying the action.

In this lab, the active identity was:

```text
developer-user
```

## STS Caller Identity

The command used was:

```bash
aws sts get-caller-identity
```

This command shows which AWS identity is currently being used by the CLI.

It returns information such as:

```text
UserId
Account
Arn
```

Sensitive details such as the AWS account ID, full ARN, and UserId should be redacted before uploading screenshots to GitHub.

### What This Proved

This proved that the terminal was authenticated as the `developer-user` IAM identity.

In simple words:

```text
Mac Terminal → AWS CLI → developer-user
```

This matters because a cloud security analyst should always verify the active identity before running AWS commands.

## S3 Bucket Note

An S3 bucket is a cloud storage container used to store objects such as files, logs, backups, screenshots, reports, and application data.

In this lab, I used the bucket:

```text
dev-bucket-niranjan
```

as a test storage location to verify what the `developer-user` IAM identity could access through AWS CLI.

The commands used were:

```bash
aws s3 ls
aws s3 ls s3://dev-bucket-niranjan
```

The successful S3 listing showed that `developer-user` had permission to view the bucket and objects inside the bucket.

This does **not** mean the user has full AWS access. It only proves that the IAM permissions allowed these specific S3 actions.

In simple words:

```text
developer-user → IAM permissions → allowed S3 actions
```

## Access Denied Testing Note

After confirming that `developer-user` could perform allowed S3 actions, I tested whether the same user could perform an IAM administrative action.

The command used was:

```bash
aws iam list-users
```

This command tries to list IAM users in the AWS account.

The result was:

```text
AccessDenied
developer-user is not authorized to perform iam:ListUsers
```

This means the `developer-user` IAM identity does **not** have permission to list IAM users.

This is a good result because a normal developer should not have IAM administrative permissions. A developer may need access to a specific S3 bucket, but should not automatically be able to view or manage AWS users.

The important lesson is:

```text
Allowed S3 access + Denied IAM admin access = Least Privilege
```

In simple words:

```text
developer-user can work with the allowed S3 bucket
developer-user cannot perform IAM admin actions
```

## Least Privilege Lesson

Least privilege means users should only have the permissions they need to do their job, and nothing more.

This lab showed least privilege because:

| Action | Result | Meaning |
|---|---|---|
| List S3 buckets | Allowed | Developer had S3 access |
| List objects inside the S3 bucket | Allowed | Developer could view allowed bucket contents |
| List IAM users | Denied | Developer did not have IAM admin access |

This is important in cloud security because over-permissioned users can create risk. If a developer account is compromised, least privilege helps limit what the attacker can do.

## CloudTrail Evidence Note

After testing the AccessDenied action from AWS CLI, I checked AWS CloudTrail to confirm that AWS recorded the event.

The CLI command tested was:

```bash
aws iam list-users
```

CloudTrail recorded the denied action as:

```text
Event source: iam.amazonaws.com
Event name: ListUsers
User identity: developer-user
Error code: AccessDenied
Read-only: true
```

This helped me understand that AWS CLI actions are recorded as CloudTrail events. Even failed or denied actions are useful for security investigation.

The full chain was:

```text
AWS CLI command → IAM permission check → AccessDenied → CloudTrail event recorded
```

## Cloud Security Analyst Explanation

From a cloud security analyst perspective, CloudTrail helps answer important questions:

- Who attempted the action?
- What action was attempted?
- When did it happen?
- Was the action allowed or denied?
- Which IAM identity was used?
- Was the activity expected, accidental, or suspicious?

In this lab, the denied `ListUsers` event showed a developer IAM user attempting an IAM administrative action. AWS blocked the action because the user did not have the required permission.

This is a good example of IAM least privilege working correctly.

## Security Reminder

Never upload the following to GitHub:

- AWS Access Key ID
- AWS Secret Access Key
- AWS account ID
- Full unredacted ARN
- UserId
- Source IP address
- Screenshots showing credentials

If AWS credentials are accidentally exposed, the access key should be deleted immediately and replaced with a new one.

## Interview Explanation

I practiced AWS CLI permission testing using a developer IAM user. First, I verified the active CLI identity with `aws sts get-caller-identity`. Then I tested S3 access using `aws s3 ls` and confirmed that the developer user could list the allowed S3 bucket and view objects inside it.

After that, I tested least privilege by running `aws iam list-users`. AWS returned AccessDenied, which showed that the developer user did not have unnecessary IAM administrative permissions.

Finally, I checked CloudTrail and confirmed that the denied `ListUsers` action was recorded. This helped me understand how AWS CLI, IAM permissions, AccessDenied errors, and CloudTrail logs work together in a cloud security investigation.
