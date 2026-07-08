# AWS CLI Security Practice

## Goal

The goal of this section is to practice using AWS CLI from the terminal and connect it to cloud security work.

AWS CLI allows users to interact with AWS services using commands instead of only using the AWS Console.

## Why AWS CLI Matters

In real cloud security and cloud engineering work, analysts and engineers often use AWS CLI to:

- Verify which AWS identity is active
- Test IAM permissions
- List AWS resources
- Investigate access issues
- Review security-related activity
- Automate cloud security tasks

## Main Security Concept

Before running AWS commands, it is important to know which IAM user or role is active.

The most important command for this is:

```bash
aws sts get-caller-identity
</> Markdown
