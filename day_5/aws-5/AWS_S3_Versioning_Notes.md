# AWS S3 Bucket Versioning Notes
> **Task:** Enable versioning on S3 bucket for data protection and recovery  
> **Bucket Name:** `xfusion-s3-22786`  
> **Region:** `us-east-1`  
> **Goal:** Enable versioning so data can be recovered after accidental deletion or corruption

---

## What is S3 Versioning? (Simple Understanding)

**Amazon S3 Versioning** is like a **time machine** for your files.  
Every time you upload, modify, or delete a file — S3 keeps ALL previous versions safe.

```
Without Versioning:                  With Versioning:
─────────────────                    ────────────────
File.txt (v1) → overwrite ❌         File.txt (v1) ✅ saved
File.txt (v2) → overwrite ❌         File.txt (v2) ✅ saved
File.txt (v3) ← only this remains   File.txt (v3) ✅ current

Accidentally deleted? ❌ Gone!       Accidentally deleted? ✅ Recover anytime!
```

### Versioning States

| State | Meaning |
|-------|---------|
| `Disabled` | No versioning (default) |
| `Enabled` | All versions of every file are saved |
| `Suspended` | No new versions created, old versions kept |

---

## Why Enable Versioning?

- ✅ **Accidental deletion protection** — recover deleted files anytime
- ✅ **Corruption recovery** — roll back to a previous good version
- ✅ **Audit trail** — see history of all file changes
- ✅ **Compliance** — meet data retention requirements

---

## Method 1: Enable via AWS Console (GUI)

### Step 1: Login to AWS Console

Go to 👉 `https://console.aws.amazon.com`  
Enter your **Username** and **Password** → Click **Sign In**

---

### Step 2: Go to S3 Service

- In the **top search bar**, type `S3`
- Click on **S3** from the dropdown results

---

### Step 3: Find Your Bucket

- You will see a list of all S3 buckets
- Look for **`xfusion-s3-22786`**
- Click on the **bucket name** to open it

---

### Step 4: Click on "Properties" Tab

Inside the bucket you will see these tabs:
```
Objects | Properties | Permissions | Metrics | Management | Access Points
```
Click on **`Properties`** tab

---

### Step 5: Find "Bucket Versioning" Section

- Scroll down on the Properties page
- Find the **`Bucket Versioning`** section
- Click the **`Edit`** button

---

### Step 6: Enable Versioning

- Select **`Enable`** radio button
- Click **`Save changes`**

---

### Step 7: Verify ✅

- Green success banner appears at top ✅
- Bucket Versioning section now shows **`Enabled`** ✅

### GUI Click Flow Summary

```
Login to AWS Console
        ↓
Search "S3" in top search bar
        ↓
Click bucket → xfusion-s3-22786
        ↓
Click "Properties" tab
        ↓
Scroll to "Bucket Versioning" → Click "Edit"
        ↓
Select "Enable" → Click "Save changes"
        ↓
        ✅ Versioning Enabled!
```

---

## Method 2: Enable via AWS CLI (Command Line)

### Step 1: Configure AWS Credentials

```bash
aws configure
```
Enter when prompted:
```
AWS Access Key ID:     <your access key>
AWS Secret Access Key: <your secret key>
Default region name:   us-east-1
Default output format: json
```

---

### Step 2: Verify Bucket Exists

```bash
aws s3 ls | grep xfusion-s3-22786
```

---

### Step 3: Enable Versioning

```bash
aws s3api put-bucket-versioning \
  --bucket xfusion-s3-22786 \
  --versioning-configuration Status=Enabled \
  --region us-east-1
```
> No output = success ✅

---

### Step 4: Verify Versioning is Enabled

```bash
aws s3api get-bucket-versioning \
  --bucket xfusion-s3-22786 \
  --region us-east-1
```

Expected output:
```json
{
    "Status": "Enabled"
}
```

---

## Quick Command Reference

```bash
# Enable versioning
aws s3api put-bucket-versioning \
  --bucket <bucket-name> \
  --versioning-configuration Status=Enabled

# Check versioning status
aws s3api get-bucket-versioning --bucket <bucket-name>

# Suspend versioning
aws s3api put-bucket-versioning \
  --bucket <bucket-name> \
  --versioning-configuration Status=Suspended

# List all versions of objects in bucket
aws s3api list-object-versions --bucket <bucket-name>
```

---

## Key Concepts to Remember

> 💡 **Once versioning is enabled, it cannot be fully disabled — only suspended.**

> 💡 **Each version is stored separately — storage costs increase with versions.**

> 💡 **Deleting a versioned file adds a "delete marker" — the file is not actually gone.**

> 💡 **You can restore a deleted file by removing its delete marker.**

---

## Real Task Recap

| Detail | Value |
|--------|-------|
| Bucket Name | `xfusion-s3-22786` |
| Region | `us-east-1` |
| Action | Enabled Versioning |
| Method Used | AWS Console (GUI) |
| Verification | Status showed `Enabled` ✅ |

---

## How to Upload This to GitHub

```bash
# Step 1: Clone your repo
git clone https://github.com/your-username/your-repo.git

# Step 2: Go into the folder
cd your-repo

# Step 3: Stage the file
git add AWS_S3_Versioning_Notes.md

# Step 4: Commit with message
git commit -m "Added AWS S3 versioning notes"

# Step 5: Push to GitHub
git push origin main
```

---

*Notes by: [Your Name] | Topic: AWS S3 Versioning | Region: us-east-1*
