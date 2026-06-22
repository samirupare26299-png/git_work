# Securing Apache Port 6400 with iptables — Nautilus / Stratos DC

## Problem Statement

One of our websites is hosted on the **Nautilus** infrastructure in **Stratos DC**. The security team flagged that Apache's port `6400` is open to the world on all App servers, since no firewall is installed on these hosts.

### Requirements
1. Install `iptables` and its dependencies on each App host.
2. Block incoming port `6400` on all App hosts for everyone **except** the LoadBalancer (LBR) host.
3. Ensure the rules persist across system reboots.

### Infrastructure

| Server | Hostname | User |
|---|---|---|
| Application Server 1 | `stapp01` | tony |
| Application Server 2 | `stapp02` | steve |
| Application Server 3 | `stapp03` | banner |
| LoadBalancer Server | `stlb01` | loki |

---

## Solution

Run the following on **each App server** (`stapp01`, `stapp02`, `stapp03`).

### Step 1 — Get the LBR host's private IP

On `stlb01`:
```bash
ip a | grep inet
```
This is the IP we whitelist on every App host (example used below: `10.244.97.188`).

### Step 2 — Check OS family

```bash
cat /etc/os-release
```

### Step 3 — Install iptables and the persistence service

```bash
sudo yum install -y iptables iptables-services
```

### Step 4 — Enable and start the iptables service

```bash
sudo systemctl enable iptables
sudo systemctl start iptables
```

### Step 5 — Allow port 6400 only from the LBR host

```bash
sudo iptables -I INPUT -p tcp -s 10.244.97.188 --dport 6400 -j ACCEPT
```

### Step 6 — Block port 6400 for everyone else

```bash
sudo iptables -A INPUT -p tcp --dport 6400 -j DROP
```

> **Order matters:** the `ACCEPT` rule is inserted (`-I`) at the top of the chain so it's evaluated before the `DROP` rule, which is appended (`-A`) after it.

### Step 7 — Verify rule order

```bash
sudo iptables -L INPUT -n --line-numbers
```

Expected output:
```
1   ACCEPT  tcp  --  10.244.97.188   0.0.0.0/0   tcp dpt:6400
2   DROP    tcp  --  0.0.0.0/0       0.0.0.0/0   tcp dpt:6400
```

### Step 8 — Persist rules across reboot

```bash
sudo iptables-save | sudo tee /etc/sysconfig/iptables
```

This writes the active rule set to the file the `iptables` systemd service reloads automatically on every boot — equivalent to `service iptables save`, used here because the `service` command/`initscripts` package wasn't available on this host.

### Step 9 — Confirm persistence

```bash
sudo reboot
```
After the host comes back up:
```bash
sudo iptables -L INPUT -n --line-numbers
```
Both rules for port `6400` should still be present.

---

## Result

- Port `6400` is reachable **only** from the LBR host (`10.244.97.188`).
- All other sources are dropped.
- Rules survive a reboot via `/etc/sysconfig/iptables`.

Repeat Steps 2–8 identically on all three App servers (`stapp01`, `stapp02`, `stapp03`).
