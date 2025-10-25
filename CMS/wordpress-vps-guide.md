# 🌐 Deploying WordPress on a VPS with HTTPS and a Free Subdomain

**Author:** Joseph Meneses Consuelo  
**Project:** WordPress on OVH VPS with No-IP + Let’s Encrypt  
**Course:** Web Application Deployment (Implantación de Aplicaciones Web)

---

## 📌 Overview
This guide explains manual installation and configuration of WordPress on a Debian-based VPS (LAMP), including:
- phpMyAdmin for database management
- Free subdomain via No-IP
- HTTPS using Let’s Encrypt / Certbot

Ideal for students and developers learning manual deployment.

---

## 🧱 Prerequisites
- VPS with a public IP (e.g., OVH)  
- Debian 13 (or similar)  
- Root or sudo access  
- No-IP account (free tier sufficient)

---

## ⚙️ Step 1 — Install LAMP
Update packages and install required software:

```bash
sudo apt update
sudo apt install apache2 php mariadb-server unzip wget
```

Enable and start services:

```bash
sudo systemctl enable --now apache2 mariadb
```

---

## 🗃️ Step 2 — (Optional) Install phpMyAdmin
Install phpMyAdmin:

```bash
sudo apt install phpmyadmin
```

Access: http://<your-server-ip>/phpmyadmin  
Create a database and user for WordPress with appropriate privileges.

---

## 🌍 Step 3 — Download & Deploy WordPress
Commands:

```bash
cd /var/www/html
mkdir -p joseph
cd joseph
wget https://es.wordpress.org/latest-es_ES.zip
unzip latest-es_ES.zip
sudo chown -R www-data:www-data wordpress
```

Visit: http://<your-server-ip>/joseph/wordpress and complete the setup wizard.

---

## 👥 Step 4 — Example WordPress Roles
- Administrator: joseph — Full site control  
- Editor: enrique — Review & publish content  
- Authors: alicia, roberto — Create/manage own posts  
- Contributors: juan, susana — Submit posts for review

---

## 🌐 Step 5 — Create Free Subdomain (No-IP)
1. Sign in to No-IP.  
2. Create a host (A record):
   - Host: name-of-our-domain  
   - Domain: ddns.net  
   - IP: your VPS public IP

Accessible at: http://name-of-our-domain.ddns.net

---

## 🧩 Step 6 — Configure Apache VirtualHost
Create site config:

```bash
sudo nano /etc/apache2/sites-available/name-of-our-domain.conf
```

Example content:

```apache
<VirtualHost *:80>
    ServerName name-of-our-domain.ddns.net
    DocumentRoot /var/www/html/your-root

    <Directory /var/www/html/your-root>
        AllowOverride All
        Require all granted
    </Directory>

    ErrorLog ${APACHE_LOG_DIR}/name-of-our-domain_error.log
    CustomLog ${APACHE_LOG_DIR}/name-of-our-domain_access.log combined
</VirtualHost>
```

Enable and reload Apache:

```bash
sudo a2ensite name-of-our-domain.conf
sudo systemctl reload apache2
```

If needed, disable default site:

```bash
sudo a2dissite 000-default.conf
sudo systemctl reload apache2
```

---

## 🔐 Step 7 — Enable HTTPS with Let’s Encrypt (Certbot)
Install Certbot:

```bash
sudo apt install certbot python3-certbot-apache
```

Request and install certificate:

```bash
sudo certbot --apache -d name-of-our-domain.ddns.net
```

Test renewal (dry run):

```bash
sudo certbot renew --dry-run
```

---

## 🧠 Step 8 — Force HTTPS in WordPress
Add to wp-config.php (before "That's all, stop editing"):

```php
define('WP_HOME', 'https://name-of-our-domain.ddns.net');
define('WP_SITEURL', 'https://name-of-our-domain.ddns.net');
```

Optional: enforce redirects via .htaccess or a plugin.

---

## ✅ Final Result
- Live site: https://name-of-our-domain.ddns.net  
- Secured with Let’s Encrypt TLS  
- Accessible via free No-IP subdomain  
- Manual, reproducible deployment

---

## 🧪 Why This Matters
- HTTPS improves security and SEO  
- No-IP provides free dynamic-friendly domain names  
- Certbot automates certificate issuance and renewal  
- Manual deployment teaches underlying components of WordPress

---

## ✅ Quick Checklist
- [ ] Apache and MariaDB installed & running  
- [ ] phpMyAdmin configured (optional)  
- [ ] WordPress downloaded and owned by www-data  
- [ ] VirtualHost created and enabled  
- [ ] Let’s Encrypt certificate issued and renewal tested  
- [ ] WP configured for HTTPS

---

## 📚 License
Open-source; free to use, adapt, and share for educational purposes.
