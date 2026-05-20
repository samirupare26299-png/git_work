# Disable Direct SSH Root Login — AWS Challenge

**Platform:** KodeKloud / Code Cloud  
**Category:** Linux Security Hardening  
**Difficulty:** Easy  

---

## 📋 Task Description

Following security audits, the `xFusionCorp Industries` security team rolled out new protocols,
including the restriction of direct root SSH login.

**Goal:** Disable direct SSH root login on all App Servers within the `Stratos Datacenter`.

---

## 🏗️ Infrastructure Details

| Server | Hostname | User | Purpose |
|---|---|---|---|
| App Server 1 | stapp01 | tony | Hosts Nautilus Application 1 |
| App Server 2 | stapp02 | steve | Hosts Nautilus Application 2 |
| App Server 3 | stapp03 | banner | Hosts Nautilus Application 3 |
| Jump Host | jump-host | thor | Gateway to Stratos DC |

> All app servers are accessed via the **Jump Host**.

---

## 🧠 Concepts Learned

### What is SSH?
SSH (Secure Shell) is a protocol used to remotely connect to Linux servers securely.

### Why disable root SSH login?
- Root is the most powerful account on Linux
- Attackers often brute-force the `root` user directly
- Disabling it forces use of a regular user + `sudo`, adding a security layer

### Key file: `/etc/ssh/sshd_config`
- `sshd` = SSH **daemon** (server side that accepts connections)
- This file controls all SSH server behaviour
- The parameter `PermitRootLogin` controls whether root can log in directly

### Why restart sshd after editing?
The SSH service is already running in memory. It won't pick up config file changes
until you restart it with `systemctl restart sshd`.

---

## ✅ Solution

### Step-by-step (repeat on all 3 servers)

**1. SSH into the app server from Jump Host**
```bash
ssh tony@stapp01
# password: Ir0nM@n
```

**2. Switch to root**
```bash
sudo su -
```

**3. Edit the SSH config file**
```bash
vi /etc/ssh/sshd_config
```

**4. Find and update `PermitRootLogin`**
```bash
# Before
PermitRootLogin yes

# After
PermitRootLogin no
```

> In `vi`: press `/PermitRootLogin` to search → `i` to edit → `Esc` then `:wq` to save & exit

**5. Restart the SSH service**
```bash
systemctl restart sshd
```

**6. Verify the change**
```bash
grep PermitRootLogin /etc/ssh/sshd_config
# Expected output: PermitRootLogin no
```

---

### Quick commands for all 3 servers

**App Server 1 — stapp01**
```bash
ssh tony@stapp01          # password: Ir0nM@n
sudo su -
vi /etc/ssh/sshd_config   # set PermitRootLogin no
systemctl restart sshd
```

**App Server 2 — stapp02**
```bash
ssh steve@stapp02         # password: Am3ric@
sudo su -
vi /etc/ssh/sshd_config   # set PermitRootLogin no
systemctl restart sshd
```

**App Server 3 — stapp03**
```bash
ssh banner@stapp03        # password: BigGr33n
sudo su -
vi /etc/ssh/sshd_config   # set PermitRootLogin no
systemctl restart sshd
```

### One-liner alternative (using sed)
```bash
sudo sed -i 's/^#*PermitRootLogin.*/PermitRootLogin no/' /etc/ssh/sshd_config && sudo systemctl restart sshd
```

---

## 🔍 Verification

After making changes, confirm root login is blocked:
```bash
ssh root@stapp01
# Expected: Permission denied (publickey,gssapi-keyex,gssapi-with-mic)
```

---

## 📚 Key Takeaways

| Concept | Detail |
|---|---|
| SSH config file | `/etc/ssh/sshd_config` |
| Parameter to change | `PermitRootLogin no` |
| Apply changes | `systemctl restart sshd` |
| Access method | Via Jump Host → App Server |
| Why it matters | Prevents brute-force attacks on root account |

---

## 🔗 Related Topics to Explore

- SSH key-based authentication (passwordless login)
- `fail2ban` — auto-ban IPs with failed SSH attempts
- `AllowUsers` in sshd_config — whitelist specific users
- `sudo` configuration via `/etc/sudoers`

---

*Solved as part of xFusionCorp / KodeKloud DevOps challenges*
