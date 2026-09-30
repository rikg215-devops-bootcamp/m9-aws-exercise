# M9-AWS-EXERCISE

## Repo dedicated to completing module 9 exercises

### EXERCISE 1: Create IAM USER

COMMANDS EXECUTED:

```bash
aws iam list-users
aws iam create-user --user-name m9-user
aws iam create-group --group-name devops
aws iam add-user-to-group --user-name m9-user --group-name devops
aws iam get-group --group-name devops
aws iam attach-group-policy --group-name devops --policy-arn arn:aws:iam::aws:policy/AmazonEC2FullAccess
aws iam create-access-key --user-name m9-user
export AWS_ACCESS_KEY_ID=REDACTED
export AWS_SECRET_ACCESS_KEY=REDACTED
```
