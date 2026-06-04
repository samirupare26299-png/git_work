# Day 9 — Linux: Fix MariaDB Service Down

## Problem Statement
The MariaDB service was down on database server `stdb01` in Stratos DC, causing the Nautilus application to lose database connectivity.

## Solution

```bash
# Check service status
systemctl status mariadb

# Read logs to find root cause
journalctl -xeu mariadb.service | tail -50

# Check directory ownership
ls -lrth /var/lib/ | grep -i mysql

# Remove immutable flag
sudo chattr -i /var/lib/mysql

# Fix ownership
sudo chown -R mysql:mysql /var/lib/mysql

# Start and enable service
sudo systemctl start mariadb
sudo systemctl enable mariadb
```

## Root Cause
`/var/lib/mysql` was owned by `root` instead of `mysql`, and had an immutable flag set — preventing MariaDB from initializing.
