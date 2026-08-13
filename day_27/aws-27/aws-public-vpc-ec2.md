# AWS Public VPC with Public EC2

## Objective
Create a public VPC, public subnet, and EC2 instance accessible over the internet through SSH.

## Architecture

```text
Internet
   |
Internet Gateway
   |
datacenter-pub-vpc (10.0.0.0/16)
   |
datacenter-pub-subnet (10.0.1.0/24)
   |
datacenter-pub-ec2 (t2.micro)
   |
SSH : 22
```

## Resources

| Resource | Configuration |
|---|---|
| VPC | `datacenter-pub-vpc` |
| VPC CIDR | `10.0.0.0/16` |
| Subnet | `datacenter-pub-subnet` |
| Subnet CIDR | `10.0.1.0/24` |
| Public IPv4 | Auto-assigned |
| Internet Gateway | Attached to VPC |
| Route | `0.0.0.0/0` → Internet Gateway |
| EC2 | `datacenter-pub-ec2` |
| Instance Type | `t2.micro` |
| Security Group | SSH port `22` open |

## Key Steps

1. Created VPC `datacenter-pub-vpc`.
2. Created subnet `datacenter-pub-subnet`.
3. Enabled **Auto-assign Public IPv4**.
4. Created and attached an Internet Gateway.
5. Added route `0.0.0.0/0` to the Internet Gateway.
6. Associated the route table with the public subnet.
7. Created a Security Group allowing TCP port `22`.
8. Launched `datacenter-pub-ec2` using `t2.micro`.
9. Verified the EC2 instance received a public IPv4 address.

## Result

The EC2 instance is deployed in a public subnet and can be accessed over the internet using SSH.

> **Note:** For production, restrict SSH access to trusted IP addresses instead of `0.0.0.0/0`.
