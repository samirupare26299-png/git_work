# Create EC2 Instance via AWS Console

## Login
Go to [AWS Console](https://aws.amazon.com/console/) → Sign in → Navigate to **EC2**

---

## Steps

### 1. Open Launch Wizard
EC2 Dashboard → Click **"Launch Instance"**

### 2. Set Name
- **Name:** `devops-ec2`

### 3. Choose AMI
- Select **Amazon Linux 2 AMI** (Free tier eligible)

### 4. Choose Instance Type
- Select **t2.micro** (Free tier eligible)

### 5. Create Key Pair
- Click **"Create new key pair"**
- Name: `devops-kp`
- Type: **RSA**
- Format: `.pem`
- Click **"Create key pair"** → file downloads automatically

### 6. Set Security Group
- Under **Network settings** → click **"Select existing security group"**
- Choose **default**

### 7. Launch
- Click **"Launch Instance"**

---

## Verify
EC2 → **Instances** → confirm `devops-ec2` is in **Running** state
