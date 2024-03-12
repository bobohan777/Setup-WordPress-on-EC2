# Setup-WordPress-on-EC2
Setting Up WordPress on EC2 Instance


![alt text](image.png)

Setting Up WordPress on EC2 Instance architecture diagram.

##  Prerequisites
- EC2 Insatnce
- Wordpress template
- PHP
- MySQL
- Nginx



## Create an EC2 Instance for the setup.

Login to your AWS account or create a new one for free.

Select the EC2 service in the console.

![alt text](image-1.png)


On the “EC2 Dashboard” select “Launch Instance”.

![alt text](image-2.png)


## Download and Install WordPress on EC2 Instance.

Go to /var/www/html using following command.


```bash
cd /var/www/html
```


Download wordpress template


```bash
sudo wget -c http://wordpress.org/latest.tar.gz
```

Extract files using the tar command


```bash
sudo tar -xzvf latest.tar.gz

ls -al
```

Modify the file ownership and configure the necessary permissions accordingly


```bash
sudo chown -R www-data:www-data /var/www/html/WordPress
```

## Create and Configure Database in Mysql for WordPress

Log into MySQL root account

```bash
sudo mysql -u root -p
```

Create database name as test_db


```SQL
CREATE DATABASE test_db; --you can give any name to your database here
```

Create user.

```SQL
CREATE USER wptest@localhost IDENTIFIED BY 'dev@1234'; -- you can give any username name here and password
Now you just created a new user  here
```

Give permission allow.

```SQL
GRANT ALL PRIVILEGES ON test_db. * TO wptest@localhost;
```

Activate the database permission.

```SQL
FLUSH PRIVILEGES;
exit;
```

Give the wordpress directory to full permission.


```bash
sudo chmod -R 777 wordpress/

ls -al

cd wordpress/

```

Copy and rename to wp-config-sample.php to wp-config.php


```bash
cp wp-config-sample.php wp-config.php
```

Edit wp-config.php 

```bash
nano wp-config.php
```


Define DB_NAME, DB_USER, DB_PASSWORD.

![alt text](image-3.png)


Insatll nginx and php services



```bash
apt-get install nginx mariadb-server php php-curl php-mysql php-gd php-intl php-mbstring php-soap php-xml php-xmlrpc php-zip php-fpm -y
```


Go to /etc/nginx/sites-available and edit with vim default
We are use default file to config for this lab.


![alt text](image-4.png)


Reload Nginx service

```bash
sudo systemctl reload nginx
```

Test URL: http://3.88.168.126/wp-admin/   (Use with EC2 public IP)


![alt text](image-5.png)


Access URL link with domain name. (Domain point are need to do in DNS setting of domain management)

![alt text](image-6.png)



## Certbot installation (HTTPS)

Run the certbot install command.

```bash
sudo apt install certbot python3-certbot-nginx

sudo certbot --nginx -d bbh.learn-cloud.site
```


Check the result as below.

```bash
ubuntu@ip-172-31-86-209:/etc/nginx/sites-available$ sudo apt install certbot python3-certbot-nginx
Reading package lists... Done
Building dependency tree... Done
Reading state information... Done
The following additional packages will be installed:
  python3-acme python3-certbot python3-configargparse python3-icu python3-josepy python3-parsedatetime python3-requests-toolbelt python3-rfc3339 python3-zope.component
  python3-zope.event python3-zope.hookable
Suggested packages:
  python-certbot-doc python3-certbot-apache python-acme-doc python-certbot-nginx-doc
The following NEW packages will be installed:
  certbot python3-acme python3-certbot python3-certbot-nginx python3-configargparse python3-icu python3-josepy python3-parsedatetime python3-requests-toolbelt
  python3-rfc3339 python3-zope.component python3-zope.event python3-zope.hookable
0 upgraded, 13 newly installed, 0 to remove and 37 not upgraded.
Need to get 993 kB of archives.
After this operation, 5077 kB of additional disk space will be used.
Do you want to continue? [Y/n] y
Get:1 http://us-east-1.ec2.archive.ubuntu.com/ubuntu jammy/universe amd64 python3-josepy all 1.10.0-1 [22.0 kB]
Get:2 http://us-east-1.ec2.archive.ubuntu.com/ubuntu jammy/main amd64 python3-requests-toolbelt all 0.9.1-1 [38.0 kB]
Get:3 http://us-east-1.ec2.archive.ubuntu.com/ubuntu jammy/main amd64 python3-rfc3339 all 1.1-3 [7110 B]
Get:4 http://us-east-1.ec2.archive.ubuntu.com/ubuntu jammy-updates/universe amd64 python3-acme all 1.21.0-1ubuntu0.1 [36.4 kB]
Get:5 http://us-east-1.ec2.archive.ubuntu.com/ubuntu jammy/universe amd64 python3-configargparse all 1.5.3-1 [26.9 kB]
Get:6 http://us-east-1.ec2.archive.ubuntu.com/ubuntu jammy/universe amd64 python3-parsedatetime all 2.6-2 [32.9 kB]
Get:7 http://us-east-1.ec2.archive.ubuntu.com/ubuntu jammy/universe amd64 python3-zope.hookable amd64 5.1.0-1build1 [11.6 kB]
Get:8 http://us-east-1.ec2.archive.ubuntu.com/ubuntu jammy/universe amd64 python3-zope.event all 4.4-3 [8180 B]
Get:9 http://us-east-1.ec2.archive.ubuntu.com/ubuntu jammy/universe amd64 python3-zope.component all 4.3.0-3 [38.3 kB]
Get:10 http://us-east-1.ec2.archive.ubuntu.com/ubuntu jammy/universe amd64 python3-certbot all 1.21.0-1build1 [175 kB]
Get:11 http://us-east-1.ec2.archive.ubuntu.com/ubuntu jammy/universe amd64 certbot all 1.21.0-1build1 [21.3 kB]
Get:12 http://us-east-1.ec2.archive.ubuntu.com/ubuntu jammy/universe amd64 python3-certbot-nginx all 1.21.0-1 [35.4 kB]
Get:13 http://us-east-1.ec2.archive.ubuntu.com/ubuntu jammy/main amd64 python3-icu amd64 2.8.1-0ubuntu2 [540 kB]
Fetched 993 kB in 0s (14.7 MB/s)
Preconfiguring packages ...
Selecting previously unselected package python3-josepy.
(Reading database ... 66716 files and directories currently installed.)
Preparing to unpack .../00-python3-josepy_1.10.0-1_all.deb ...
Unpacking python3-josepy (1.10.0-1) ...
Selecting previously unselected package python3-requests-toolbelt.
Preparing to unpack .../01-python3-requests-toolbelt_0.9.1-1_all.deb ...
Unpacking python3-requests-toolbelt (0.9.1-1) ...
Selecting previously unselected package python3-rfc3339.
Preparing to unpack .../02-python3-rfc3339_1.1-3_all.deb ...
Unpacking python3-rfc3339 (1.1-3) ...
Selecting previously unselected package python3-acme.
Preparing to unpack .../03-python3-acme_1.21.0-1ubuntu0.1_all.deb ...
Unpacking python3-acme (1.21.0-1ubuntu0.1) ...
Selecting previously unselected package python3-configargparse.
Preparing to unpack .../04-python3-configargparse_1.5.3-1_all.deb ...
Unpacking python3-configargparse (1.5.3-1) ...
Selecting previously unselected package python3-parsedatetime.
Preparing to unpack .../05-python3-parsedatetime_2.6-2_all.deb ...
Unpacking python3-parsedatetime (2.6-2) ...
Selecting previously unselected package python3-zope.hookable.
Preparing to unpack .../06-python3-zope.hookable_5.1.0-1build1_amd64.deb ...
Unpacking python3-zope.hookable (5.1.0-1build1) ...
Selecting previously unselected package python3-zope.event.
Preparing to unpack .../07-python3-zope.event_4.4-3_all.deb ...
Unpacking python3-zope.event (4.4-3) ...
Selecting previously unselected package python3-zope.component.
Preparing to unpack .../08-python3-zope.component_4.3.0-3_all.deb ...
Unpacking python3-zope.component (4.3.0-3) ...
Selecting previously unselected package python3-certbot.
Preparing to unpack .../09-python3-certbot_1.21.0-1build1_all.deb ...
Unpacking python3-certbot (1.21.0-1build1) ...
Selecting previously unselected package certbot.
Preparing to unpack .../10-certbot_1.21.0-1build1_all.deb ...
Unpacking certbot (1.21.0-1build1) ...
Selecting previously unselected package python3-certbot-nginx.
Preparing to unpack .../11-python3-certbot-nginx_1.21.0-1_all.deb ...
Unpacking python3-certbot-nginx (1.21.0-1) ...
Selecting previously unselected package python3-icu.
Preparing to unpack .../12-python3-icu_2.8.1-0ubuntu2_amd64.deb ...
Unpacking python3-icu (2.8.1-0ubuntu2) ...
Setting up python3-configargparse (1.5.3-1) ...
Setting up python3-requests-toolbelt (0.9.1-1) ...
Setting up python3-parsedatetime (2.6-2) ...
Setting up python3-icu (2.8.1-0ubuntu2) ...
Setting up python3-zope.event (4.4-3) ...
Setting up python3-zope.hookable (5.1.0-1build1) ...
Setting up python3-josepy (1.10.0-1) ...
Setting up python3-rfc3339 (1.1-3) ...
Setting up python3-zope.component (4.3.0-3) ...
Setting up python3-acme (1.21.0-1ubuntu0.1) ...
Setting up python3-certbot (1.21.0-1build1) ...
Setting up certbot (1.21.0-1build1) ...
Created symlink /etc/systemd/system/timers.target.wants/certbot.timer → /lib/systemd/system/certbot.timer.
Setting up python3-certbot-nginx (1.21.0-1) ...
Processing triggers for man-db (2.10.2-1) ...
Scanning processes...
Scanning linux images...

Running kernel seems to be up-to-date.

No services need to be restarted.

No containers need to be restarted.

No user sessions are running outdated binaries.

No VM guests are running outdated hypervisor (qemu) binaries on this host.
ubuntu@ip-172-31-86-209:/etc/nginx/sites-available$ sudo nano /etc/nginx/sites-available/default
ubuntu@ip-172-31-86-209:/etc/nginx/sites-available$ sudo certbot --nginx -d bbh.learn-cloud.site
Saving debug log to /var/log/letsencrypt/letsencrypt.log
Enter email address (used for urgent renewal and security notices)
 (Enter 'c' to cancel): bobohan77777@gmail.com

- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
Please read the Terms of Service at
https://letsencrypt.org/documents/LE-SA-v1.3-September-21-2022.pdf. You must
agree in order to register with the ACME server. Do you agree?
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
(Y)es/(N)o: y

- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
Would you be willing, once your first certificate is successfully issued, to
share your email address with the Electronic Frontier Foundation, a founding
partner of the Let's Encrypt project and the non-profit organization that
develops Certbot? We'd like to send you email about our work encrypting the web,
EFF news, campaigns, and ways to support digital freedom.
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
(Y)es/(N)o: y
Account registered.
Requesting a certificate for bbh.learn-cloud.site

Successfully received certificate.
Certificate is saved at: /etc/letsencrypt/live/bbh.learn-cloud.site/fullchain.pem
Key is saved at:         /etc/letsencrypt/live/bbh.learn-cloud.site/privkey.pem
This certificate expires on 2024-06-09.
These files will be updated when the certificate renews.
Certbot has set up a scheduled task to automatically renew this certificate in the background.

Deploying certificate
Successfully deployed certificate for bbh.learn-cloud.site to /etc/nginx/sites-enabled/default
Congratulations! You have successfully enabled HTTPS on https://bbh.learn-cloud.site

- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
If you like Certbot, please consider supporting our work by:
 * Donating to ISRG / Let's Encrypt:   https://letsencrypt.org/donate
 * Donating to EFF:                    https://eff.org/donate-le
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
ubuntu@ip-172-31-86-209:/etc/nginx/sites-available$
```



References:

https://www.rosehosting.com/blog/how-to-install-wordpress-with-lemp-on-ubuntu-20-04/

https://technerdz.hashnode.dev/how-to-install-wordpress-with-lamp-stack-on-aws-ec2-instance-with-ubuntu-2204

https://www.digitalocean.com/community/tutorials/how-to-install-nginx-on-ubuntu-22-04

https://www.digitalocean.com/community/tutorials/how-to-secure-nginx-with-let-s-encrypt-on-ubuntu-20-04