# Day 18 - Nginx + PHP-FPM Setup

## Problem

Configure PHP application on App Server 3.

Requirements:

- Install Nginx
- Nginx port: 8092
- Document root: /var/www/html
- Install PHP-FPM 8.1
- PHP-FPM socket:

/var/run/php-fpm/default.sock


- Configure Nginx with PHP-FPM
- Test PHP application

---

## Solution

### Login Server

```bash
ssh banner@stapp03
Install Nginx
sudo yum install nginx -y

sudo systemctl enable nginx
Configure Nginx

Edit:

sudo vi /etc/nginx/nginx.conf

Set:

listen 8092;

root /var/www/html;

Add PHP block:

location ~ \.php$ {

fastcgi_pass unix:/var/run/php-fpm/default.sock;

include fastcgi_params;

fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;

}
Install PHP-FPM
yum install php php-fpm php-cli -y

Check:

php -v
Configure PHP-FPM Socket

Edit:

vi /etc/php-fpm.d/www.conf

Change:

listen = /var/run/php-fpm/default.sock

Set:

listen.owner = nginx
listen.group = nginx
listen.mode = 0660
Create Socket Directory
mkdir -p /var/run/php-fpm
Restart Services
systemctl restart php-fpm

systemctl restart nginx
Verify

Check socket:

ls -l /var/run/php-fpm/

Check nginx port:

netstat -tulpn | grep 8092


Test Application

From jump host:

curl http://stapp03:8092/index.php

Output:

Welcome to xFusionCorp Industries!