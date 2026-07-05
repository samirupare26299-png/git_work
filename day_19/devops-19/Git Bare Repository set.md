# Day 19 - Git Bare Repository Setup

## Problem

On the Storage Server:

- Install Git using `yum`
- Create a bare Git repository:
  ```
  /opt/official.git
  ```

---

## Solution

### Login

```bash
ssh natasha@ststor01
sudo su -
```

### Install Git

```bash
yum install git -y
```

### Create Bare Repository

```bash
git init --bare /opt/official.git
```

### Verify

```bash
git --version

ls -ld /opt/official.git

ls /opt/official.git
```

---

## Learning

- Install Git using `yum`
- Create a centralized (bare) Git repository
- Bare repositories are used as remote repositories for collaboration