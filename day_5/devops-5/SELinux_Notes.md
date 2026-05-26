# SELinux Setup & Configuration Notes
> **Task:** Install and permanently disable SELinux on App Server 3 (Stratos Datacenter)  
> **Server:** `stapp03` | **User:** `tony`  
> **Goal:** SELinux should be `disabled` after scheduled reboot

---

## What is SELinux?

**SELinux (Security-Enhanced Linux)** is like a **security guard** for your Linux server.  
It controls what programs can and cannot do, adding an extra layer of protection on top of normal Linux permissions.

| Mode | Description |
|------|-------------|
| `enforcing` | SELinux is ON and actively blocking violations |
| `permissive` | SELinux is ON but only logs violations (no blocking) |
| `disabled` | SELinux is completely OFF |

---

## Step-by-Step Solution

### Step 1: SSH into App Server 3

```bash
ssh tony@stapp03
```
> Enter password when prompted.

---

### Step 2: Switch to Root User

```bash
sudo su -
```
> Root access is needed to install packages and modify system config files.

---

### Step 3: Install SELinux Packages

```bash
yum install -y selinux-policy selinux-policy-targeted libselinux libselinux-utils policycoreutils policycoreutils-python-utils setools-console
```

| Package | Purpose |
|---------|---------|
| `selinux-policy` | Core SELinux policy |
| `selinux-policy-targeted` | Targeted policy (protects specific services) |
| `libselinux` | SELinux library |
| `libselinux-utils` | SELinux utility tools |
| `policycoreutils` | Core policy utilities |
| `policycoreutils-python-utils` | Python-based utilities |
| `setools-console` | Policy analysis tools |

> `-y` flag = auto-yes to all install prompts

---

### Step 4: Check Current SELinux Status

```bash
sestatus
```

Sample output:
```
SELinux status:                 disabled
SELinuxfs mount:                /sys/fs/selinux
SELinuxtype:                    targeted
```

---

### Step 5: Open the SELinux Config File

```bash
vi /etc/selinux/config
```

You will see something like:
```
SELINUX=enforcing        <-- This controls SELinux behavior
SELINUXTYPE=targeted
```

---

### Step 6: Permanently Disable SELinux

**Option A — One-liner command (Easy):**
```bash
sed -i 's/^SELINUX=.*/SELINUX=disabled/' /etc/selinux/config
```

**Option B — Manual edit in vi:**
1. Press `i` to enter insert mode
2. Change `enforcing` → `disabled`
3. Press `Esc`, type `:wq`, press `Enter` to save and exit

---

### Step 7: Verify the Change

```bash
cat /etc/selinux/config
```

Expected output:
```
SELINUX=disabled       <-- Confirmed!
SELINUXTYPE=targeted
```

```bash
# Double-check with grep
grep "^SELINUX=" /etc/selinux/config
```

Expected:
```
SELINUX=disabled
```

---

## Important Concept: Live Status vs Permanent Config

> This is a common point of confusion for beginners!

```
sestatus              = Shows what SELinux is doing RIGHT NOW (in memory)
/etc/selinux/config   = Shows what SELinux will do AFTER REBOOT (on disk)
```

Think of it like this:

```
Config File  =  Your ALARM CLOCK setting  (future plan)
sestatus     =  What is happening RIGHT NOW
```

### Real Scenario Encountered:

| Check | Result | Meaning |
|-------|--------|---------|
| `sestatus` | `disabled` | SELinux was already disabled at runtime |
| `/etc/selinux/config` | `enforcing` | But config still said enforcing for next boot |

**They were OUT OF SYNC!**  
Fix: Updated the config file from `enforcing` → `disabled` so both match.

```
BEFORE fix:
├── sestatus (live)       = disabled   ⚠️ Out of sync
└── /etc/selinux/config   = enforcing  ⚠️ Out of sync

AFTER fix:
├── sestatus (live)       = disabled   ✅ In sync
└── /etc/selinux/config   = disabled   ✅ In sync
```

---

## Key Lesson

> **Always check BOTH the live status AND the config file.**  
> The config file wins after every reboot!

- No reboot was needed immediately — a maintenance reboot was already planned.
- After the reboot, SELinux will be `disabled` as required.

---

## Quick Command Reference

```bash
# Check live SELinux status
sestatus

# Check permanent config
grep "^SELINUX=" /etc/selinux/config

# Disable SELinux permanently (one-liner)
sed -i 's/^SELINUX=.*/SELINUX=disabled/' /etc/selinux/config

# Install all SELinux packages
yum install -y selinux-policy selinux-policy-targeted libselinux libselinux-utils policycoreutils policycoreutils-python-utils setools-console
```

---

## How to Upload This File to GitHub Using Git

### First Time Setup (One-time only)

```bash
# Configure your name and email
git config --global user.name "Your Name"
git config --global user.email "your@email.com"
```

### Upload Steps

```bash
# Step 1: Clone your GitHub repo (if not already done)
git clone https://github.com/your-username/your-repo-name.git

# Step 2: Move into the repo folder
cd your-repo-name

# Step 3: Copy or move your .md file into this folder
# (Place SELinux_Notes.md here)

# Step 4: Stage the file
git add SELinux_Notes.md

# Step 5: Commit with a message
git commit -m "Added SELinux setup and configuration notes"

# Step 6: Push to GitHub
git push origin main
```

> If your branch is `master` instead of `main`, use `git push origin master`

### Verify on GitHub
Go to `https://github.com/your-username/your-repo-name`  
You should see `SELinux_Notes.md` rendered beautifully! ✅

---

*Notes by: [Your Name] | Topic: SELinux | Platform: RHEL/CentOS*
