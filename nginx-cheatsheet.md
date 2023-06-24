[back to overwiev](/../..)

# Nginx Cheatsheet

## Table of Contents

…

## Install

```
sudo apt update
sudo apt install nginx
```

Firewall

```
sudo ufw allow 'Nginx Full'
```

## Basics

```
sudo systemctl stop nginx
sudo systemctl start nginx
sudo systemctl restart nginx
sudo systemctl reload nginx
sudo systemctl disable nginx
sudo systemctl enable nginx
```

Test if config files are correct:

```
sudo nginx -t
```

## Important Folders

- `/var/www/html`: The actual web content, which by default only consists of the default Nginx page you saw earlier, is served out of the /var/www/html directory. This can be changed by altering Nginx configuration files.
- `/etc/nginx`: The Nginx configuration directory. All of the Nginx configuration files reside here.
- `/etc/nginx/nginx.conf`: The main Nginx configuration file. This can be modified to make changes to the Nginx global configuration.
- `/etc/nginx/sites-available/`: The directory where per-site server blocks can be stored. Nginx will not use the configuration files found in this directory unless they are linked to the sites-enabled directory. Typically, all server block configuration is done in this directory, and then enabled by linking to the other directory.
- `/etc/nginx/sites-enabled/`: The directory where enabled per-site server blocks are stored. Typically, these are created by linking to configuration files found in the sites-available directory.
- `/etc/nginx/snippets`: This directory contains configuration fragments that can be included elsewhere in the Nginx configuration. Potentially repeatable configuration segments are good candidates for refactoring into snippets.
- `/var/log/nginx/access.log`: Every request to your web server is recorded in this log file unless Nginx is configured to do otherwise.
- `/var/log/nginx/error.log`: Any Nginx errors will be recorded in this log.

## Proxy

```
server {
    listen 80;
    listen [::]:80;
    server_name example.com www.example.com;
    location / {
        proxy_pass http://127.0.0.1:2368;
    }

    location /blog {
        rewrite ^/blog(.*) /$1 break;
        proxy_pass http://127.0.0.1:8181;
    }
}
```

- `rewrite` is effectively sending traffic to the blog app as if it was coming from `/` instead of `/blog`.

### Full

```
sudo mkdir -p /var/www/example.com/html
sudo chown -R $USER:$USER /var/www/example.com/html
sudo chmod -R 755 /var/www/example.com
```

Add a website to serve (or skip it and add a proxy)

```
vim /var/www/example.com/html/index.html
```

```html
<html>
  <head>
    <title>Welcome to Example.com!</title>
  </head>
  <body>
    <h1>Success! The example.com server block is working!</h1>
  </body>
</html>
```

Create a new configuration for this host (or a proxy):

```
sudo vim /etc/nginx/sites-available/example.com
```

```nginx
server {
        listen 80;
        listen [::]:80;

        root /var/www/example.com/html;
        index index.html index.htm index.nginx-debian.html;

        server_name example.com www.example.com;

        location / {
                try_files $uri $uri/ =404;
        }
}
```

Link it

```
sudo ln -s /etc/nginx/sites-available/example.com /etc/nginx/sites-enabled/
```

Done. Make sure to enable the hash bucket size (to prevent bug with multiple listeners):

```
sudo vim /etc/nginx/nginx.conf
```

Uncomment following line

```nginx
...
http {
    ...
    server_names_hash_bucket_size 64;
    ...
}
...
```

```
sudo systemctl restart nginx
```

## HTTPS

We will use Let’s Encrypt to setup HTTPs routes (https://certbot.eff.org/instructions?ws=nginx&os=ubuntufocal)

```
sudo snap install core; sudo snap refresh core
sudo snap install --classic certbot
sudo ln -s /snap/bin/certbot /usr/bin/certbot
```

- For the Certbot to work you need to have the correct nginx block setup, especially the part with `server_name example.com www.example.com;` as we added before.
- Check if everything works with `sudo nginx -t`
- Then reload nginx `sudo systemctl reload nginx`

Now Get the SSL Certificate:

```
sudo certbot --nginx -d example.com -d www.example.com
```

This runs certbot with the --nginx plugin, using -d to specify the names we'd like the certificate to be valid for.

Now test the certificate renewal:

```
sudo certbot renew --dry-run
```

If no errors ari seyou’re all set.

## Quick adding of a Server Block

```bash
sudo mkdir -p /var/www/neomatcha.com/html
sudo chown -R $USER:$USER /var/www/neomatcha.com/html
sudo chmod -R 755 /var/www/neomatcha.com
vim /var/www/neomatcha.com/html/index.html
```

```html
<html>
  <head>
    <title>Welcome to neomatcha.com!</title>
  </head>
  <body>
    <h1>Success! The neomatcha.com server block is working!</h1>
  </body>
</html>
```

```bash
sudo vim /etc/nginx/sites-available/neomatcha.com
```

```nginx
server {
        listen 80;
        listen [::]:80;

        root /var/www/neomatcha.com/html;
        index index.html index.htm index.nginx-debian.html;

        server_name neomatcha.com www.neomatcha.com;

        location / {
                try_files $uri $uri/ =404;
        }
}
```

```bash
sudo ln -s /etc/nginx/sites-available/neomatcha.com /etc/nginx/sites-enabled/
sudo systemctl restart nginx
```

```
sudo certbot --nginx -d neomatcha.com -d www.neomatcha.com
```
