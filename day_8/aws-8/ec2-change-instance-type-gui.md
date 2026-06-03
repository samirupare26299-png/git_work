# 🖥️ Change EC2 Instance Type via AWS Console (GUI)

## 📋 Overview

This document covers how to change an **EC2 Instance Type** from `t2.micro` to `t2.nano` using the **AWS Management Console (GUI)** and bring the instance back to a running state.

> ⚠️ **Important:** AWS requires the instance to be in a **Stopped** state before changing its type. It cannot be done while the instance is running.

---

## 📊 t2.micro vs t2.nano — Quick Comparison

| Spec       | t2.micro   | t2.nano    |
|------------|------------|------------|
| vCPUs      | 1          | 1          |
| RAM        | 1 GB       | 0.5 GB     |
| Network    | Low to Moderate | Low to Moderate |
| Cost       | ~$0.0116/hr | ~$0.0058/hr |

> 💡 `t2.nano` is **50% cheaper** but has **half the RAM** — ensure your workload fits within 0.5 GB.

---

## ✅ Step-by-Step Guide

### Step 1 — Navigate to EC2 Instances

1. Log in to **AWS Management Console**
2. Search for **"EC2"** in the search bar
3. Click **"Instances"** from the left sidebar

---

### Step 2 — Stop the Instance

1. **Select** your instance by clicking the checkbox ✅
2. Click **"Instance State"** (top-right button)
3. Click **"Stop Instance"**
4. Click **"Stop"** on the confirmation popup
5. ⏳ Wait until the Instance State shows 🟡 **"Stopped"**

---

### Step 3 — Change the Instance Type

1. With the instance still selected
2. Click **"Actions"** → **"Instance Settings"** → **"Change Instance Type"**
3. In the **Instance Type** dropdown → select **`t2.nano`**
4. Click **"Apply"**

---

### Step 4 — Start the Instance

1. Click **"Instance State"** → **"Start Instance"**
2. ⏳ Wait until the Instance State shows 🟢 **"Running"**

---

### Step 5 — Verify the Change

1. Click on your instance to open the details panel
2. Scroll down to the **"Instance Type"** field
3. Confirm it shows **`t2.nano`** ✅

---

## 🔁 Quick Summary Flow

```
Stop Instance
     ↓
Actions → Instance Settings → Change Instance Type
     ↓
Select t2.nano → Apply
     ↓
Start Instance
     ↓
Verify → t2.nano ✅ Running 🟢
```

---

## ❌ Common Mistakes

| Mistake | Fix |
|--------|-----|
| Trying to change type while instance is **Running** | Always **Stop** the instance first |
| Forgetting to **Start** the instance after change | Go to Instance State → Start Instance |
| Not waiting for **Stopped** status before changing | Wait for 🟡 Stopped before proceeding |

---

## 📌 Key Points

- Instance **must be stopped** before changing the type — this is an AWS requirement
- After applying the new type and starting, the instance resumes normally with **`t2.nano`**
- No data is lost during a stop/start cycle (unlike terminate)
- Elastic IP and private IP remain the same after restart

---

## 👥 Team

**Nautilus DevOps Team**
Task: EC2 Instance Type Change
Method: AWS Management Console (GUI)

---

*Last updated: June 2026*
