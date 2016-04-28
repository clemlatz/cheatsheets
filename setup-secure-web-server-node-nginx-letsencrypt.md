# Setup a secure web server from scratch with NodeJS + Nginx + Let's Encrypt

## 1. Basic server setup

Update packages

```
apt-get update
apt-get upgrade
```

Install vim

```
apt-get install -y VIm
```

Create user & set password to activate

```
useradd -c "Clement" -s /bin/bash -m clement
passwd clement
```

Install sudo

```
apt-get install -y sudo
visudo
```

Edit sudo config file to add user after root 

```
visudo
```

```
root    ALL=(ALL:ALL) ALL
clement ALL=(ALL:ALL) ALL
```

Add SSH Key (from client)

```
ssh-copy-id clement@server-ip
```

Logout and connect with user (from client)

```
ssh clement@server-ip
```

Edit sshd config file to disable password and root login

```
sudo vim /etc/ssh/sshd_config
```

```
PasswordAuthentication no
PermitRootLogin no
```

Reload ssh config

```
sudo /etc/init.d/ssh reload
```

## 2. Install our NodeJs application

Install cURL

```
sudo apt-get install -y curl
```

Install NodeJS

```
curl -sL https://deb.nodesource.com/setup_5.x | sudo -E bash -
sudo apt-get install -y nodejs
```

Install git

```
sudo apt-get install -y git 
```

Install MongoDB

```
sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv EA312927
echo "deb http://repo.mongodb.org/apt/debian wheezy/mongodb-org/3.2 main" | sudo tee /etc/apt/sources.list.d/mongodb-org-3.2.list
sudo apt-get update
sudo service mongod start
```

Create app directory

```
sudo mkdir -p /var/appdata/fooapp
sudo chown $USER /var/appdata/fooapp
```

Install NodeJS app

```
cd /var/appdata/fooapp/
git clone https://github.com/user/fooapp.git .
npm install
npm start
```

Check that node app http server is listening on its port

Install & start app with pm2

```
sudo npm install pm2
pm2 start app.js
```

## 3. Install Nginx as an HTTPS proxy

Install openssl

```
echo "deb http://ftp.debian.org/debian sid main" | sudo tee -a /etc/apt/sources.list
sudo apt-get update
sudo apt-get -t sid install -y openssl
openssl version
```

Install nginx

```
wget http://nginx.org/keys/nginx_signing.key
sudo apt-key add nginx_signing.key
echo "deb http://nginx.org/packages/mainline/debian/ jessie nginx" | sudo tee -a /etc/apt/sources.list
echo "deb-src http://nginx.org/packages/mainline/debian/ jessie nginx" | sudo tee -a /etc/apt/sources.list
sudo apt-get update
sudo apt-get install nginx
```

Check that nginx is running on port 80

Install letsencrypt

```
sudo git clone https://github.com/letsencrypt/letsencrypt /opt/letsencrypt
sudo /opt/letsencrypt/letsencrypt-auto --help all
```

Stop nginx (port 80 needed by letsencrypt)

```
sudo service nginx stop
```

Generate SSL certificate

```
sudo /opt/letsencrypt/letsencrypt-auto certonly --rsa-key-size 4096
```

Generate a strong Diffie-Hellman (4096-bit) group

```
sudo openssl dhparam -out /etc/ssl/certs/dhparam.pem 4096
```

Config nginx to use certificate

```
sudo vim /etc/nginx/sites-available/www.example.com
sudo ln -s /etc/nginx/sites-available/www.example.com /etc/nginx/sites-enabled/www.example.com
```

Nginx config file:

```
# SSL server config
server {
        listen 443 ssl;

        server_name www.example.com;

        ssl_certificate /etc/letsencrypt/live/www.example.com/fullchain.pem;
        ssl_certificate_key /etc/letsencrypt/live/www.example.com/privkey.pem;
        
        ssl_protocols TLSv1 TLSv1.1 TLSv1.2;
        ssl_prefer_server_ciphers on;
        ssl_dhparam /etc/ssl/certs/dhparam.pem;
        ssl_ciphers 'ECDHE-RSA-AES128-GCM-SHA256:ECDHE-ECDSA-AES128-GCM-SHA256:ECDHE-RSA-AES256-GCM-SHA384:ECDHE-ECDSA-AES256-GCM-SHA384:DHE-RSA-AES128-GCM-SHA256:DHE-DSS-AES128-GCM-SHA256:kEDH+AESGCM:ECDHE-RSA-AES128-SHA256:ECDHE-ECDSA-AES128-SHA256:ECDHE-RSA-AES128-SHA:ECDHE-ECDSA-AES128-SHA:ECDHE-RSA-AES256-SHA384:ECDHE-ECDSA-AES256-SHA384:ECDHE-RSA-AES256-SHA:ECDHE-ECDSA-AES256-SHA:DHE-RSA-AES128-SHA256:DHE-RSA-AES128-SHA:DHE-DSS-AES128-SHA256:DHE-RSA-AES256-SHA256:DHE-DSS-AES256-SHA:DHE-RSA-AES256-SHA:AES128-GCM-SHA256:AES256-GCM-SHA384:AES128-SHA256:AES256-SHA256:AES128-SHA:AES256-SHA:AES:CAMELLIA:DES-CBC3-SHA:!aNULL:!eNULL:!EXPORT:!DES:!RC4:!MD5:!PSK:!aECDH:!EDH-DSS-DES-CBC3-SHA:!EDH-RSA-DES-CBC3-SHA:!KRB5-DES-CBC3-SHA';
        ssl_session_timeout 1d;
        ssl_session_cache shared:SSL:50m;
        ssl_stapling on;
        ssl_stapling_verify on;
        add_header Strict-Transport-Security max-age=15768000;
        
        location / {

                proxy_set_header        Host $host;
                proxy_set_header        X-Real-IP $remote_addr;
                proxy_set_header        X-Forwarded-For $proxy_add_x_forwarded_for;
                proxy_set_header        X-Forwarded-Proto $scheme;

                proxy_pass          http://0.0.0.0:3000;
                proxy_read_timeout  90;
        }
}

# Redirect http to https
server {
    listen 80;
    server_name example.com www.example.com;
    return 301 https://$host$request_uri;
}
```

```
sudo service nginx restart
```

Test SSL config with [SSL Labs](https://www.ssllabs.com/ssltest/)

## 4. Configure firewall

Install ufw firewall

```
sudo apt-get install ufw -y
```

Open SSH, HTTP & HTTPS ports

```
sudo ufw allow ssh
sudo ufw allow http
sudo ufw allow https
```

## Sources
* https://nodeswat.com/blog/setting-up-a-secure-node-js-web-application/
* https://gist.github.com/iwazaru/35cf6765f935e7930807
