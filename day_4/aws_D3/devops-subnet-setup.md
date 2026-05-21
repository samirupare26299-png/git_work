# AWS DevOps Subnet Setup — `devops-subnet`

## Overview

This document covers the creation of the `devops-subnet` subnet under the default VPC in AWS as part of the Nautilus DevOps infrastructure migration to AWS Cloud.

---

## AWS Details

| Field              | Value                        |
|--------------------|------------------------------|
| Region             | `us-east-1` (N. Virginia)    |
| VPC                | Default VPC                  |
| Subnet Name        | `devops-subnet`              |
| IPv4 CIDR Block    | `172.31.96.0/20`             |
| Availability Zone  | `us-east-1a`                 |
| Usable IPs         | 4,091                        |
| IP Range           | `172.31.96.0 – 172.31.111.255` |

---

## Steps Performed (GUI)

1. Signed in to AWS Console at `https://844643069394.signin.aws.amazon.com/console?region=us-east-1`
2. Navigated to **VPC** service via the search bar
3. Confirmed region was set to **us-east-1**
4. Clicked **Subnets** in the left sidebar
5. Clicked **Create subnet**
6. Selected the **default VPC** from the VPC ID dropdown
7. Filled in subnet settings:
   - **Subnet name:** `devops-subnet`
   - **Availability zone:** `us-east-1a`
   - **IPv4 CIDR block:** `172.31.96.0/20`
8. Clicked **Create subnet**
9. Verified subnet appeared with status **Available**

---

## CLI Equivalent

```bash
# Get default VPC ID
VPC_ID=$(aws ec2 describe-vpcs \
  --filters "Name=isDefault,Values=true" \
  --query "Vpcs[0].VpcId" \
  --output text)

# Create the subnet
aws ec2 create-subnet \
  --vpc-id $VPC_ID \
  --cidr-block 172.31.96.0/20 \
  --availability-zone us-east-1a \
  --region us-east-1 \
  --tag-specifications 'ResourceType=subnet,Tags=[{Key=Name,Value=devops-subnet}]'

# Verify
aws ec2 describe-subnets \
  --filters "Name=tag:Name,Values=devops-subnet" \
  --query "Subnets[*].{ID:SubnetId,CIDR:CidrBlock,VPC:VpcId,State:State}" \
  --output table
```

---

