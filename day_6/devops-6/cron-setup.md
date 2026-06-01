# Cron Job Setup on App Servers

## Task
- Install `cronie` on all app servers
- Start and enable `crond` service
- Add a cron job for root user

---

## Steps

```bash
# 1. SSH into the server
ssh tony@stapp01       # stapp01
ssh steve@stapp02      # stapp02
ssh banner@stapp03     # stapp03

# 2. Install cronie
sudo yum install cronie -y

# 3. Start the service
sudo systemctl start crond

# 4. Enable on boot
sudo systemctl enable crond

# 5. Add cron job for root
sudo crontab -u root -e
# Add this line inside the editor:
*/5 * * * * echo hello > /tmp/cron_text

# 6. Verify
sudo crontab -u root -l
```

---

## Quick Reference

| Command | Purpose |
|---------|---------|
| `yum install cronie -y` | Install cronie package |
| `systemctl start crond` | Start cron service |
| `systemctl enable crond` | Enable on boot |
| `crontab -u root -e` | Edit root's crontab |
| `crontab -u root -l` | List root's crontab |

---

## Cron Schedule Format

```
* * * * *  command
| | | | |
| | | | └── Day of week (0-7)
| | | └──── Month (1-12)
| | └────── Day of month (1-31)
| └──────── Hour (0-23)
└────────── Minute (0-59)

*/5 * * * *  →  Every 5 minutes
```
