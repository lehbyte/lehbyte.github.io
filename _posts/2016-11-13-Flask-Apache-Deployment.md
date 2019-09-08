---
layout: post
title:  Apache deployment and Flask. &#128077;
feature-img: "/assets/img/pexels/codecandy1.png"
thumbnail: "/assets/img/pexels/codecandy1.png"
image: "/assets/img/pexels/codecandy1.png"
tags: [Flask, Cloud, Deployment, Apache]
author-id: lehbyte
---

Deploying your flask application on an apache server might seem daunting at first but it’s not that bad – if you follow these 4-5 easy steps.

## Assumptions/Tips

I’m assuming your server is linux-based (Ubuntu 16.04/14.04 e.t.c)
If you are ssh-ing into your server from your machine make sure to use a vpn to test if your site is up or consider closing the ssh session if you want to test out your website from your host. I recommend using Opera for this as you could simply open an incognito window with the vpn option enabled and test your website without having to close your ssh session.

![Opera vpn](/assets/img/pexels/opera1.jpg)

## Setup

```bash
MyApp
   /app
      /static
      /template
      /venv
      __init__.py
      views.py
      models.py
myapp.wsgi
__init__.py
```

Install `apache2` and `mod_wsgi` is you don’t already have those installed.
`sudo apt-get install apache2`
`sudo apt-get install libapache2-mod-wsgi`

Using python3?
`sudo apt-get install libapache2-mod-wsgi-py`

## Step 0
Make sure that your `localhost` is set up correctly.
Make sure that `127.0.0.0.1` localhost `mydomain.com` `http://www.mydomain.com`
Or something like that.

## Step 1
After Installing `apache2` and `mod_wsgi` go to `/etc/apache2/sites-enabled/`
and make sure that `000-default.conf` is enabled and that `apache2` is running.
To check if `apache2` is up, run the following command, `sudo service apache2 status`
If `apache2` is running you will see a green dot at the start of the status message.

If apache2 is not running run  `sudo service apache2 start` and enable `000-default.conf` if it is not enabled. 
In `/etc/apache2/sites-enabled/` run the following command; `sudo a2ensite 000-default.conf` then, restart apache2 server; `sudo service apache2 restart`.

## Step 2
Go to your private opera window and navigate to your domain.
You should see apache’s default html page saying it works.

## Step 3: Enable secure http on your site
Lets make that page secure. Go to certbot and select `Apache` and your  OS
for example `Xenial`

Install the apache package for letsencrypt;
`$ sudo apt-get install python-letsencrypt-apache`

`$ letsencrypt --apache`

Now navigate to your `/etc/apache2/sites-available/` folder and check to see if `default-ssl.conf` exists, if it does enable it and reload apache;

`sudo service a2ensite default-ssl.conf`
`sudo service apache2 restart`

And go to your private Opera window and navigate to your website. 
If it doesn’t exist go back to the previous step or make sure that you have appropriate rights.

## Last Step
Edit `myapp.wsgi` to look like this;

```python
activate_this = '/home/your_user_name/myapp/app/venv/bin/activate_this.py'
execfile(activate_this,dict(__file__=activate_this))

import sys
import logging
logging.basicConfig(stream=sys.stderr)
sys.path.insert(0,'/home/your_user_name/myapp')
from app import app as application
```

Edit your `000-default.conf` and your `default-ssl.conf` thus;
Comment out the `DocumentRoot` line. 
Your `000-default.conf` should  look like sometihng like this;

```java
<VirtualHost *:80>
ServerName mydomain.com
ServerAlias mydomain.com
ServerAdmin myemail@gmail.com
Redirect permanent / https://www.mydomain.com

ErrorLog /var/www/logs/error.log
CustomLog /var/www/access.log combined
</VirtualHost>
```
And your `default-ssl.conf` file should look thus;

```java
<IfModule mod_ssl.c>
 <VirtualHost _default_:443>
 ServerAdmin myemail@gmail.com
 ServerName mydomain.com
 ServerAlias www.mydomain.com

 WSGIDaemonProcess myapp user=your_user_name group=www-data threads=5
 WSGIScriptAlias / /home/your_user_name/MyApp/myapp.wsgi

<Directory /home/your_user_name/myapp/>
 WSGIProcessGroup myapp
 WSGIApplicationGroup %{GLOBAL}
 Order allow,deny
 Allow from all
 Require all granted
</Directory>

Alias /static /home/your_user_name/myapp/app/static
<Directory /home/your_user_name/myapp/app/static>
 Order allow,deny
 Allow from all
 Require all granted
</Directory>

 ErrorLog /var/www/logs/error.log
 Customlog /var/www/logs/access.log combined

 SSLEngine on
 SSLCertificateFile /etc/letsencrypt/live/mydomain.com/fullchain.pem
 SSLCertificateKeyFile /etc/letsencrypt/live/mydomain.com/privkey.pem

 <FilesMatch "\.(cgi|shtml|phtml|php)$">
 SSLOptions +StdEnvVars
 </FilesMatch>
 <Directory /usr/lib/cgi-bin>
 SSLOptions +StdEnvVars
 </Directory>
 BrowserMatch "MSIE [2-6]"\
 nokeepalive ssl-unclean-shutdown \
 downgrade-1.0 force-response-1.0
 BrowserMatch "MSIE [17-9]" ssl-unclean-shutdown

 </VirtualHost>
</IfModule>
```

## Conclusion
In this example, we run our app from a script located in MyApp. The reasons for this will be made clear later on. The script is named `__init__.py` for a reason that will become clear in the next post. This is how it looks like;

```python
#!app/venv/bin/python
from app import app
if __name__=='__main__':
 app.run(host='127.0.0.1',debug=True)
 ```

 Before I forget, make sure that everything your MyApp folder has the right user and groups see `default-ssl.conf` in the previous step. 
 If it doesn’t navigate to the folder MyApp folder and do;

 `chown -R your_user_name:www-data MyApp`

 That’s it for now!
 Happy coding!