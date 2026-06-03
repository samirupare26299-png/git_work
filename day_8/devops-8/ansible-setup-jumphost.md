# 🚀 Ansible Installation on Jump Host (Nautilus DevOps)

## 📋 Overview

This document covers the installation of **Ansible v4.7.0** on a Jump Host using `pip3`, configured as an Ansible Controller for the Nautilus DevOps team.

---

## 🧰 Prerequisites

| Requirement | Command to Verify |
|-------------|-------------------|
| Python 3.x  | `python3 --version` |
| pip3        | `pip3 --version` |
| sudo access | `sudo -v` |

---

## ⚙️ Installation Steps

### 1. Verify pip3 and Python3

```bash
pip3 --version
python3 --version
```

### 2. Install Ansible 4.7.0 via pip3 (globally)

```bash
sudo pip3 install ansible==4.7.0
```

> ⚠️ **Note:** Use `sudo` to install system-wide so **all users** on the host can access Ansible.  
> On Ubuntu 22.04+, you may need to add `--break-system-packages`:
> ```bash
> sudo pip3 install ansible==4.7.0 --break-system-packages
> ```

### 3. Verify the Installation

```bash
ansible --version
which ansible
ls -la /usr/local/bin/ansible
```

### 4. Confirm Global Accessibility

```bash
su -c "ansible --version"
```

---

## ✅ Expected Output

```
ansible [core 2.11.x]
  config file = None
  configured module search path = ['/root/.ansible/plugins/modules', ...]
  ansible python module location = /usr/local/lib/python3.x/dist-packages/ansible
  executable location = /usr/local/bin/ansible
  python version = 3.x.x
```

> 📝 Ansible 4.7.0 ships with `ansible-core` 2.11.x — this is expected behavior.

---

## 🗂️ File Locations

| File/Binary | Path |
|-------------|------|
| Ansible binary | `/usr/local/bin/ansible` |
| Ansible library | `/usr/local/lib/python3.x/dist-packages/ansible` |

---

## ❌ Common Mistakes

```bash
# ❌ Wrong — single equals
sudo pip3 install ansible=4.7.0

# ❌ Wrong — spaces around ==
sudo pip3 install ansible == 4.7.0

# ✅ Correct
sudo pip3 install ansible==4.7.0
```

---

## 🔑 Key Points

- `sudo pip3 install` places the binary in `/usr/local/bin/` — accessible to **all system users**
- `/usr/local/bin/` is included in `$PATH` by default for all users
- Pinning with `==4.7.0` ensures the exact required version is installed

---

## 👥 Team

**Nautilus DevOps Team**  
Controller Node: `Jump Host`  
Tool: Ansible (Configuration Management & Automation)

---

*Last updated: June 2026*
