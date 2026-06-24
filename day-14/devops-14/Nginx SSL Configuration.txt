# Day 15 - Nginx SSL Configuration

## Problem

The system admins team of xFusionCorp Industries needs to deploy a new application on App Server 3 in Stratos Datacenter. They have some pre-requisites to get the server ready for application deployment.

### Requirements

1. Install and configure Nginx on App Server 3.

2. A self-signed SSL certificate and key are available at:

   * `/tmp/nautilus.crt`
   * `/tmp/nautilus.key`

3. Move them to an appropriate location and configure Nginx to use them.

4. Create an `index.html` file with content:

   ```text
   Welcome!
   ```

5. Verify the setup using:

   ```bash
   curl -Ik https://<app-server-name>/
   ```

---

## Solution

### 1. Login to App Server 3

```bash
ssh banner@stapp03
sudo su -
```

### 2. Install Nginx

```bash
yum install nginx -y
```

### 3. Create SSL Directory

```bash
mkdir -p /etc/nginx/ssl
```

### 4. Move Certificate and Key

```bash
mv /tmp/nautilus.crt /etc/nginx/ssl/
mv /tmp/nautilus.key /etc/nginx/ssl/
```

### 5. Create Welcome Page

```bash
echo "Welcome!" > /usr/share/nginx/html/index.html
```

### 6. Configure SSL in Nginx

Edit:

```bash
vi /etc/nginx/nginx.conf
```

Update the TLS server block:

```nginx
server {
    listen 443 ssl http2;
    server_name stapp03;
    root /usr/share/nginx/html;

    ssl_certificate /etc/nginx/ssl/nautilus.crt;
    ssl_certificate_key /etc/nginx/ssl/nautilus.key;

    location / {
        index index.html;
    }
}
```

### 7. Validate Configuration

```bash
nginx -t
```

### 8. Start and Enable Nginx

```bash
systemctl enable nginx
systemctl restart nginx
systemctl status nginx
```

### 9. Verify HTTPS Port

```bash
ss -tulpn | grep 443
```

### 10. Test from Jump Host

```bash
curl -Ik https://stapp03/
```

If SSL validation fails because of the self-signed certificate:

```bash
curl -Ik -k https://stapp03/
```

Expected output:

```text
HTTP/1.1 200 OK
Server: nginx
```

---

## Key Learnings

* Installed and configured Nginx.
* Configured HTTPS using a self-signed certificate.
* Created a custom web page.
* Validated Nginx configuration using `nginx -t`.
* Verified HTTPS connectivity using `curl`.
