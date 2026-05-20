# Create AWS Security Group — AWS Challenge

**Category:** AWS Security  
**Region:** us-east-1  

---

## 📋 Task
Create a security group under default VPC with inbound rules for HTTP and SSH.

---

## 🧠 Concepts

| Concept | Description |
|---|---|
| Security Group | Virtual firewall for AWS resources |
| Inbound Rule | Controls incoming traffic |
| `0.0.0.0/0` | Allow traffic from all IPs |
| Port 80 | HTTP — web traffic |
| Port 22 | SSH — remote access |

---

## ✅ Solution

1. Login to AWS Console → Go to **EC2**
2. Sidebar → **Network & Security** → **Security Groups**
3. Click **Create Security Group** and fill:
   - Name: `nautilus-sg`
   - Description: `Security group for Nautilus App Servers`
   - VPC: `default`
4. Add Inbound Rules:

| Type | Port | Source |
|---|---|---|
| HTTP | 80 | 0.0.0.0/0 |
| SSH | 22 | 0.0.0.0/0 |

5. Click **Create Security Group** ✅

---

*Solved as part of xFusionCorp / KodeKloud DevOps challenges*
