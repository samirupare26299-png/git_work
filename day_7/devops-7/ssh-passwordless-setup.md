# SSH Password-less Authentication Setup
**xFusionCorp Industries — Stratos Datacenter**

## Task
Set up password-less SSH authentication from user `thor` on jump host to all app servers through their respective sudo users.

## Server Details

| App Server | Hostname | IP | Sudo User |
|---|---|---|---|
| App Server 1 | stapp01 | 172.16.238.10 | tony |
| App Server 2 | stapp02 | 172.16.238.11 | steve |
| App Server 3 | stapp03 | 172.16.238.12 | banner |

## Steps Performed

### 1. Switch to thor user
```bash
sudo su - thor
```

### 2. Generate SSH key pair
```bash
ssh-keygen
# Press Enter 3 times (default path, no passphrase)
```
Keys stored at:
- `~/.ssh/id_ed25519` — private key
- `~/.ssh/id_ed25519.pub` — public key

### 3. Copy public key to all app servers
```bash
ssh-copy-id tony@stapp01
ssh-copy-id steve@stapp02
ssh-copy-id banner@stapp03
```
Enter each user's password once when prompted.

### 4. Verify password-less access
```bash
ssh tony@stapp01
ssh steve@stapp02
ssh banner@stapp03
```
All connections should work without asking for a password.

## How It Works

```
thor@jumphost
    ├── ~/.ssh/id_ed25519        (private key — stays here)
    └── ~/.ssh/id_ed25519.pub    (public key — copied to app servers)

tony@stapp01   → ~/.ssh/authorized_keys  ← contains thor's public key
steve@stapp02  → ~/.ssh/authorized_keys  ← contains thor's public key
banner@stapp03 → ~/.ssh/authorized_keys  ← contains thor's public key
```

When `thor` SSHes into any app server, the server matches the public key in `authorized_keys` with thor's private key — if they pair, access is granted without a password.
