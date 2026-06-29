# AWS IAM Policy Creation - Problem & Solution

## Problem

The Nautilus DevOps team required creation of an IAM policy.

Requirement:

- Policy Name:

iampolicy_mark


- Policy should provide EC2 read-only access.
- Users should be able to view:
  - EC2 Instances
  - AMIs
  - Snapshots

---

## Solution

### Steps Performed (GUI)

1. Login to AWS Console.

2. Open:


IAM → Policies


3. Click:


Create policy


4. Select service:


EC2


5. Select read-only permissions:


DescribeInstances
DescribeImages
DescribeSnapshots


6. Click Next.

7. Enter policy name:


iampolicy_mark


8. Click:


Create policy