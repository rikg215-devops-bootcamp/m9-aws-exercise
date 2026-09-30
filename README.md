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
```

### EXERCISE 2: Configure AWS CLI

COMMANDS EXECUTED:

```bash
export AWS_ACCESS_KEY_ID=REDACTED
export AWS_SECRET_ACCESS_KEY=REDACTED
export AWS_REGION=us-east-2
```

### EXERCISE 3: Create VPC

COMMANDS EXECUTED:

```bash
aws ec2 create-vpc --cidr-block 10.0.0.0/16 --tag-specifications 'ResourceType=vpc,Tags=[{Key=Name,Value=m9-vpc}]'
aws ec2 create-subnet --cidr-block 10.0.0.0/24 --vpc-id vpc-0c662b8efc05d8c8a --tag-specifications 'ResourceType=subnet,Tags=[{Key=Name,Value=m9-subnet}]' --region us-east-2
aws ec2 create-security-group --group-name m9-sg --vpc-id vpc-0c662b8efc05d8c8a --region us-east-2 --tag-specifications 'ResourceType=security-group,Tags=[{Key=Name,Value=m9-sg}]' --description "security group for module 9. ssh access from home ip on port 22"
aws ec2 authorize-security-group-ingress --group-id sg-0ddabbae571d0125a --port 22 --protocol tcp --cidr $HOME_IP
aws ec2 create-internet-gateway --tag-specifications 'ResourceType=internet-gateway,Tags=[{Key=Name,Value=m9-igw}]' --region us-east-2
aws ec2 attach-internet-gateway --vpc-id vpc-0c662b8efc05d8c8a --internet-gateway-id igw-083fdb5ebcaa4fd50
aws ec2 describe-route-tables --filters Name=vpc-id,Values=vpc-0c662b8efc05d8c8a \
  --query "RouteTables[].[RouteTableId,Routes]"
aws ec2 create-route --route-table-id rtb-0fa5af8155804160f --destination-cidr-block 0.0.0.0/0 --gateway-id igw-083fdb5ebcaa4fd50
```

### EXERCISE 4: Create EC2 Instance

COMMANDS EXECUTED:

```bash
aws ec2 create-key-pair --key-name m9-key --query 'KeyMaterial' --output text > m9-key.pem
aws ec2 run-instances --image-id ami-0e5497a77ef21b5ac --security-group-ids sg-0ddabbae571d0125a --subnet-id subnet-0950909051ccb6700 --region us-east-2 --key-name m9-key --tag-specifications 'ResourceType=instance,Tags=[{Key=Name,Value=m9-ec2}]' --instance-type t3.micro
```

### EXERCISE 5: SSH into the server and install Docker on it

COMMANDS EXECUTED:

```bash
chmod 400 m9-key.pem
ssh -i m9-key.pem ubuntu@<ec2-IP>
sudo apt update && sudo apt install docker.io -y
```

### EXERCISE 6: Add docker-compose for deployment

COMMANDS EXECUTED:

```bash
sudo apt install docker-compose-v2 -y
docker compose version == Docker Compose version 2.40.3+ds1-0ubuntu1
```
