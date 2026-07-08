## Commands Practiced

### 1. Check AWS CLI Version

```bash
aws --version
```

This command checks whether AWS CLI is installed and working on the system.

In this lab, AWS CLI was installed on macOS before continuing with identity and permission testing.

---

### 2. Configure AWS CLI

```bash
aws configure
```

This command connects the local terminal to AWS using IAM user credentials.

During configuration, AWS CLI asks for:

```text
AWS Access Key ID
AWS Secret Access Key
Default region name
Default output format
```

For this lab, the configuration used:

```text
Default region name: us-east-1
Default output format: json
```

The access key and secret access key were not saved in GitHub because credentials must never be exposed in a public repository.

---

### 3. Verify Active AWS Identity

```bash
aws sts get-caller-identity
```

This command shows which AWS IAM identity is currently being used by the AWS CLI.

The command returns:

```text
UserId
Account
Arn
```

In this lab, the CLI identity was verified as the developer IAM user:

```text
arn:aws:iam::REDACTED:user/developer-user
```

The account ID and sensitive identity details were redacted for security.

---

## What I Learned

AWS CLI uses IAM credentials to perform AWS actions from the terminal.

Before running AWS commands, a cloud security analyst should always verify the active identity. This helps prevent mistakes, such as running commands with the wrong user, wrong account, or excessive permissions.

The important flow is:

```text
AWS CLI → IAM Credentials → IAM User → Permissions → AWS API Action
```

For this lab:

```text
AWS CLI → developer-user → test identity and permissions
```

## Security Reminder

Never upload the following to GitHub:

- AWS Access Key ID
- AWS Secret Access Key
- AWS account ID
- Full unredacted ARN
- Screenshots showing credentials

If credentials are accidentally exposed, the access key should be deleted immediately and replaced with a new one.