# AWS EC2 with Nginx Using User Data

## Objective

Launch an Ubuntu EC2 instance and automatically install and start the Nginx web server using EC2 User Data.

## Resources Created

- EC2 Instance: `devops-ec2`

## Configuration

- AMI: Ubuntu Server
- Instance Type: t2.micro
- Security Group:
  - HTTP (80) from 0.0.0.0/0

## User Data Script

```bash
#!/bin/bash
apt update -y
apt install nginx -y
systemctl enable nginx
systemctl start nginx
```

## Verification

- Wait for the instance to enter the **Running** state.
- Open the EC2 public IP in a browser.
- Verify the **Welcome to nginx!** page is displayed.

## Result

Successfully launched an Ubuntu EC2 instance with Nginx installed and started automatically using EC2 User Data.