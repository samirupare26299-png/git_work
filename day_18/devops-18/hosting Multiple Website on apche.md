# Apache Multiple Static Website Setup (Port 8087)

## Requirement
Configure Apache on App Server 2 to host two static websites:

- Blog → http://localhost:8087/blog/
- Apps → http://localhost:8087/apps/

---

## 1. Install Apache

```bash
sudo yum install httpd -y

Check installation:

httpd -v
2. Change Apache Port

Edit config:

sudo vi /etc/httpd/conf/httpd.conf

Change:

Listen 80

To:

Listen 8087
3. Create Website Directories
sudo mkdir -p /var/www/html/blog
sudo mkdir -p /var/www/html/apps
4. Copy Website Files

Example:

cp -r /tmp/blog/* /var/www/html/blog/

cp -r /tmp/apps/* /var/www/html/apps/
5. Configure Apache Alias

Create config:

sudo vi /etc/httpd/conf.d/websites.conf

Add:

Alias /blog /var/www/html/blog
Alias /apps /var/www/html/apps


<Directory /var/www/html/blog>
    Require all granted
</Directory>


<Directory /var/www/html/apps>
    Require all granted
</Directory>
6. Restart Apache
sudo systemctl restart httpd

Enable service:

sudo systemctl enable httpd
7. Verify

Test blog:

curl http://localhost:8087/blog/

Test apps:

curl http://localhost:8087/apps/

If HTML content appears, setup is successful.