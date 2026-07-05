# AWS EC2 Passwordless SSH Setup

## Problem

Create an EC2 instance with the following requirements:

- Instance Name: `xfusion-ec2`
- Instance Type: `t2.micro`
- Create SSH key `id_rsa` on `aws-client` (if not present)
- Configure passwordless SSH from `aws-client` to the EC2 instance

---

## Solution

### 1. Launch EC2 Instance
- Open **EC2 → Launch Instance**
- Name: `xfusion-ec2`
- AMI: Ubuntu (or as required)
- Instance Type: `t2.micro`
- Create/select a key pair
- Allow SSH (Port 22)
- Launch the instance

### 2. Create SSH Key on aws-client

Check if the key exists:

```bash
ls /root/.ssh/id_rsa
```

If it doesn't exist:

```bash
ssh-keygen -t rsa -f /root/.ssh/id_rsa -N ""
```

### 3. Copy Public Key

Display the public key:

```bash
cat /root/.ssh/id_rsa.pub
```

Copy the output.

### 4. Add Public Key to EC2

Login to the EC2 instance.

Switch to root:

```bash
sudo su -
```

Create the SSH directory (if needed):

```bash
mkdir -p /root/.ssh
chmod 700 /root/.ssh
```

Add the copied public key to:

```text
/root/.ssh/authorized_keys
```

Set permissions:

```bash
chmod 600 /root/.ssh/authorized_keys
```

### 5. Verify

From `aws-client`, test:

```bash
ssh root@<EC2_Public_IP>
```

If it logs in without asking for a password, the configuration is successful.

---

## Result

Passwordless SSH from `aws-client` to the EC2 instance is configured successfully.