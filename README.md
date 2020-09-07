# Setting up Digital ocean apache2 webserver on ubuntu 

Install fail2ban if password ssh enabled
```
apt install fail2ban
```

https://www.digitalocean.com/community/tutorials/initial-server-setup-with-ubuntu-20-04

Display profiles that apps have registered with `ufw`
```
ufw app list

> Available applications:
>   OpenSSH
```

Add firewall rule via profile
```
ufw allow 'OpenSSH'
```

Enable firewall
```
ufw enable
```

Show firewall rules
```
ufw status
```

https://www.digitalocean.com/community/tutorials/how-to-install-the-apache-web-server-on-ubuntu-20-04

Update local package index and install apache2 package
```
apt update
apt install apache2
```

Adjust firewall
```
ufw app list

> Available applications:
>   Apache
>   Apache Full
>   Apache Secure
>   OpenSSH

ufw allow '<SELECT FROM LIST>'
```

Common commands
```
systemctl status apache2 
systemctl stop apache2
systemctl start apache2
systemctl restart apache2 // stop, start
systemctl reload apache2  // reload config changes
```

Setting up Virtual hosts

Create root dir for each domain `/var/www/<domain>`
Copy and amend `/etc/apache2/sites-available/000-default.conf` for each domain
```
<VirtualHost *:80>
    ServerAdmin <your email>
    ServerName <domain>.com
    ServerAlias www.<domain>.com ...other alias'
    DocumentRoot /var/www/<domain>
    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
```

Test for config errors
```
apache2ctl configtest
```
Disable default
```
a2dissite 000-default.conf
```
Enable file
```
a2ensite <domain>.conf
```

https://www.digitalocean.com/community/tutorials/how-to-secure-apache-with-let-s-encrypt-on-ubuntu-20-04

Install certbots
```
apt install certbot python3-certbot-apache
```

Ensure ServerName and ServerAlias is correct for each Virtual host
```
certbot --apache
```

certbot renew status (runs twice a day to renew cert if expiry within 30 days)
```
systemctl status certbot.timer
```

Test renewal process
```
certbot renew --dry-run
```