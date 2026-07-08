# AWS IAM Users, Groups, and Permissions Lab

## Goal

The goal of this lab is to practice AWS Identity and Access Management concepts from the AWS Solutions Architect Associate SAA-C03 exam and connect them to real cloud security work.

This lab focuses on IAM users, IAM groups, permissions, policies, least privilege, and access testing.

## What I Practiced

- Created IAM users for different job roles
- Created IAM groups to organize users
- Attached permissions to groups
- Added users to groups
- Tested allowed and denied access
- Reviewed IAM activity from a security perspective

## Why This Matters

IAM is one of the most important security services in AWS. It controls who can access AWS resources and what actions they can perform.

From a cloud security perspective, IAM helps enforce least privilege, reduce unnecessary access, and investigate suspicious account activity.

## IAM Design Used in This Lab

| IAM Identity | Purpose | Permission Level |
|---|---|---|
| Security Analyst | Review cloud security settings and logs | Read-only/security focused |
| Auditor | Review account configuration and compliance | Read-only |
| Developer | Work with specific AWS services such as S3 | Limited service access |

## Key IAM Concepts

### IAM User

An IAM user represents a person or service that needs access to AWS.

### IAM Group

An IAM group is used to organize users and assign permissions to multiple users at once.

### IAM Policy

An IAM policy is a JSON permission document that defines what actions are allowed or denied.

### Least Privilege

Least privilege means giving users only the permissions they need to do their job, and nothing more.

## Access Testing

In this lab, access should be tested by signing in as different IAM users and checking what each user can and cannot do.

| Test | Expected Result |
|---|---|
| Security Analyst views IAM or CloudTrail | Allowed if read-only permissions are attached |
| Developer uploads object to approved S3 bucket | Allowed if S3 permissions are attached |
| Developer tries to access unrelated admin features | Denied |
| Auditor reviews account settings | Allowed if read-only permissions are attached |

## Security Lesson

Users should not receive full administrator access unless absolutely required. Permissions should be assigned based on job role and business need.

This follows the principle of least privilege.

## Screenshots

Screenshots for this lab should be stored in:

```text
02-IAM/screenshots/

## IAM users Created 
These Screenshots shows 3 IAM users have been created for role-based testing: 
- auditor-lab  ---- To practice read-only/compliance access
-developer-user ----- To practice limited developer access
-Security-analyst-lab ---- To practice cloud security analyst access
### IAM Users Created

This screenshot shows three IAM users created for role-based access testing:

- auditor-lab
- developer-user
- Security-analyst-lab

Each user represents a different job function. This lab demonstrates least privilege by separating access based on role instead of giving every user administrator access.
