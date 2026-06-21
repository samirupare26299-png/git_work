# Apache Service Unreachable on Port 6100 — Troubleshooting Log

**Environment:** Stratos Datacenter
**Affected Server:** stapp01 (app server)
**Reported Issue:** Apache not reachable on port `6100` from jump host
**Test Command:** `curl http://stapp03:6100` (jump host)

---

## 1. Initial Check — Service Status

Checked whether the Apache (`httpd`) service was running.

```bash
sudo systemctl status httpd
```

**Finding:** `httpd` service was **not started**.

---

## 2. Verify Apache Listen Configuration

Confirmed Apache was configured to listen on the correct port.

```bash
sudo grep -i "Listen" /etc/httpd/conf/httpd.conf
```

**Finding:** `Listen 6100` was already correctly set — no config change needed.

---

## 3. Initial Connection Test (from app server)

```bash
curl -I http://stapp01:6100
```

**Result:**
```
curl: (7) Failed to connect to stapp01 port 6100: Connection refused
```

Expected, since `httpd` was not running yet.

---

## 4. Firewall Check

Tried `firewalld` first:

```bash
sudo firewall-cmd --list-all
```

**Result:** `firewall-cmd: command not found` — this host does not use `firewalld`.

Switched to `iptables`:

```bash
sudo iptables -L -n
```

**Finding:** The `INPUT` chain only explicitly allowed established connections, ICMP, and TCP port `22` (SSH), followed by a catch-all `REJECT` rule. Port `6100` had no ACCEPT rule, so all new connections to it were being rejected.

```
Chain INPUT (policy ACCEPT)
target     prot opt source               destination
ACCEPT     all  --  0.0.0.0/0            0.0.0.0/0            state RELATED,ESTABLISHED
ACCEPT     icmp --  0.0.0.0/0            0.0.0.0/0
ACCEPT     all  --  0.0.0.0/0            0.0.0.0/0
ACCEPT     tcp  --  0.0.0.0/0            0.0.0.0/0            state NEW tcp dpt:22
REJECT     all  --  0.0.0.0/0            0.0.0.0/0            reject-with icmp-host-prohibited
```

### Fix — Add a scoped rule for port 6100 only

```bash
sudo iptables -I INPUT -p tcp --dport 6100 -j ACCEPT
```

Verified placement (must sit **above** the REJECT rule):

```bash
sudo iptables -L -n --line-numbers
```

**Result:** New rule landed at position `1`, ahead of the `REJECT` rule at position `6`. All other rules (SSH access, ICMP, established connections) were left untouched — no broader security settings were changed.

Made the rule persistent:

```bash
sudo service iptables save
```

---

## 5. Apache Failed to Start

```bash
sudo systemctl start httpd
```

**Result:**
```
Job for httpd.service failed because the control process exited with error code.
```

### Diagnosis steps

Checked the systemd journal (limited detail):

```bash
sudo journalctl -xeu httpd.service
```

Ran Apache's own config test to rule out a syntax error:

```bash
sudo apachectl configtest
```

**Result:**
```
AH00558: httpd: Could not reliably determine the server's fully qualified domain name, using 10.244.234.202.
Syntax OK
```

Config was valid (the `AH00558` line is just a warning, not the cause of failure).

### Checked for port conflict

```bash
sudo netstat -tulpn | grep 6100   # not installed on this host
sudo ss -tulpn | grep 6100
```

**Finding — root cause:**
```
tcp   LISTEN   0   10   127.0.0.1:6100   0.0.0.0:*   users:(("sendmail",pid=9687,fd=4))
```

Port `6100` was already bound by an unrelated process — **`sendmail`** — so Apache could not bind to the same port.

### Fix

```bash
sudo systemctl stop sendmail
sudo systemctl disable sendmail
sudo ss -tulpn | grep 6100      # confirm port is free
sudo systemctl start httpd
sudo systemctl enable httpd
sudo ss -tulpn | grep httpd     # confirm Apache is now bound to 6100
```

---

## 6. Apache Running but Returning 403 Forbidden

```bash
curl -I http://stapp01:6100
```

**Result:**
```
HTTP/1.1 403 Forbidden
Server: Apache/2.4.62 (CentOS Stream)
```

### Diagnosis

Checked the document root:

```bash
pwd          # /var/www/html
ls           # (empty output)
```

**Finding:** `/var/www/html` was **empty** — no `index.html` present, hence the 403.

### Next steps (in progress)

```bash
sudo find / -iname "index.html" -not -path "*/uploads/*" 2>/dev/null
sudo grep -i "DocumentRoot" /etc/httpd/conf/httpd.conf
```

- If `index.html` is found elsewhere on the system, copy (not move) it into `/var/www/html/`.
- Per task constraints, the **content of `index.html` must not be altered** — only its location/ownership/permissions if needed.
- Once restored:

```bash
sudo chown apache:apache /var/www/html/index.html
sudo chmod 644 /var/www/html/index.html
sudo restorecon -Rv /var/www/html
sudo systemctl restart httpd
curl -I http://stapp01:6100
```

---

## Summary of Root Causes Found (stapp01)

| # | Issue | Root Cause | Fix |
|---|-------|-----------|-----|
| 1 | Connection refused | `httpd` service not running | `systemctl start/enable httpd` |
| 2 | Connection refused (after starting httpd) | No iptables ACCEPT rule for port 6100 | Inserted scoped `iptables -I INPUT -p tcp --dport 6100 -j ACCEPT` rule above REJECT rule |
| 3 | httpd failed to start | Port 6100 already bound by `sendmail` | Stopped/disabled `sendmail`, freed the port |
| 4 | HTTP 403 Forbidden | `/var/www/html` empty, no `index.html` | Locate and restore correct file without modifying its content |

## Key Commands Reference

```bash
# Service status
sudo systemctl status httpd
sudo systemctl start httpd
sudo systemctl enable httpd
sudo systemctl restart httpd

# Port / listener checks
sudo ss -tulpn | grep 6100
sudo ss -tulpn | grep httpd

# Config validation
sudo apachectl configtest
sudo grep -i "Listen" /etc/httpd/conf/httpd.conf
sudo grep -i "DocumentRoot" /etc/httpd/conf/httpd.conf

# Firewall (iptables, since firewalld not present)
sudo iptables -L -n --line-numbers
sudo iptables -I INPUT -p tcp --dport 6100 -j ACCEPT
sudo service iptables save

# SELinux (if applicable)
getenforce
sudo semanage port -l | grep http_port_t
sudo semanage port -a -t http_port_t -p tcp 6100
sudo restorecon -Rv /var/www/html

# Permissions
ls -ld /var/www/html
ls -lZ /var/www/html/index.html
sudo chown apache:apache /var/www/html/index.html
sudo chmod 644 /var/www/html/index.html

# End-to-end test
curl -I http://stapp01:6100
```
