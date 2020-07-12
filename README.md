### disable root login
```sh 
useradd -m {$username}
passwd #0 (set password)
passwd {$username} #0 (set password)
usermod -aG sudo {$username}
```
```sh
nano /etc/ssh/sshd_config
  PermitRootLogin no
```
### change ssh port
```sh
nano /etc/ssh/sshd_config
  Port [22000-22999]
```
### git install
```sh
apt install -y git
```
### nginx install
```sh
apt update
apt install -y nginx
systemctl status nginx
```
### gzip in nginx
```
gzip_disable "msie6";
gzip_vary on;
gzip_proxied any;
gzip_comp_level 6;
gzip_buffers 16 8k;
gzip_min_length 256;
gzip_types text/plain text/css application/json application/x-javascript application/javascript text/xml application/xml application/xml+rss text/javascript application/vnd.ms-fontobject application/x-font-ttf font/opentype image/svg+xml image/x-icon;
```
### cache in nginx
```
. . .
# Default server configuration
#

# Expires map
map $sent_http_content_type $expires {
    default                    off;
    text/html                  epoch;
    text/css                   max;
    application/javascript     max;
    ~image/                    max;
}

server {
    listen 80 default_server;
    listen [::]:80 default_server;

    expires $expires;
. . .
```
### letsencrypt install for https
- Installing Certbot:
```sh
nano /etc/apt/sources.list
  ...
  deb http://deb.debian.org/debian stretch-backports main contrib non-free
  deb-src http://deb.debian.org/debian stretch-backports main contrib non-free
```
```sh
apt update
apt install -y python-certbot-nginx -t stretch-backports
```
- Confirming Nginx’s Configuration:
```sh
/etc/nginx/sites-available/{$example.com}
```
- Obtaining an SSL Certificate:
```sh
sudo certbot --nginx -d {$example.com} -d {$www.example.com}
```
- Verifying Certbot Auto-Renewal:
```sh
certbot renew --dry-run
```
### mysql install
```sh
cd /tmp
wget https://dev.mysql.com/get/mysql-apt-config_0.8.10-1_all.deb
sudo dpkg -i mysql-apt-config*
apt update
apt install mysql-server
mysql_secure_installation
systemctl status mysql
```
### create mysql user for remote using
```sh
mysql -u root -p
```
```sql
CREATE USER '{$newuser}'@'localhost' IDENTIFIED BY '{$password}';
CREATE USER '{$newuser}'@'%' IDENTIFIED BY '{$password}';
GRANT ALL ON *.* TO '{$newuser}'@'localhost';
GRANT ALL ON *.* TO '{$newuser}'@'%'; flush privileges;
```
```sh
nano /etc/mysql/mariadb.conf.d/50-server.cnf
bind-address           = 127.0.0.1
-> #bind-address           = 127.0.0.1
```
### node.js install
```sh
apt install -y curl
curl -sL https://deb.nodesource.com/setup_12.x | bash -
apt install -y nodejs build-essential
```
### node.js for Production (example: web server)
- Step 1: install node.js
- Step 2: install nginx
- Step 3: install pm2
```sh
npm install pm2@latest -g
```
- Step 4: Create [ecosystem.config.js](https://pm2.keymetrics.io/docs/usage/application-declaration/):
```js
module.exports = {
   apps: [
       {
           name: 'Application name’',
           script: 'script.js',
           args: '--max-old-space-size=1024',
           instances: 1,
           autorestart: true,
           watch: false,
           max_memory_restart: '1G',
           env: {
               NODE_ENV: 'development'
           },
           env_production: {
               NODE_ENV: 'production'
           }
       }
   ],
};
```
```sh
pm2 start ecosystem.config.js
pm2 save
pm2 startup
```
- Step 5: Setting Up Nginx as a Reverse Proxy Server:
```sh
nano /etc/nginx/sites-available/{$example.com}
```
```
location / {
	proxy_pass http://localhost:{$8081};
	proxy_http_version 1.1;
	proxy_set_header  X-Real-IP $remote_addr;
	proxy_set_header Upgrade $http_upgrade;
	proxy_set_header Connection 'upgrade';
	proxy_set_header Host $host;
	proxy_cache_bypass $http_upgrade;
}
```
### xvfb install
```sh
sudo apt-get install -y xvfb x11-xkb-utils xfonts-100dpi xfonts-75dpi xfonts-scalable xfonts-cyrillic x11-apps clang libdbus-1-dev libgtk2.0-dev libnotify-dev libgnome-keyring-dev libgconf2-dev libasound2-dev libcap-dev libcups2-dev libxtst-dev libxss1 libnss3-dev gcc-multilib g++-multilib libgtk-3.0
```
### electron install
```sh
npm install -g electron --unsafe-perm=true --allow-root
```
### typescript install
```sh
npm install -g typescript
```
### San Andreas Multiplayer dependencies install
```sh
cd /etc/apt/sources.list.d
echo "deb http://old-releases.ubuntu.com/ubuntu/ raring main restricted universe multiverse" >ia32-libs-raring.list
apt-get update
apt-get install ia32-libs
apt install screen
```
### C++ builder install
```sh
sudo apt install gcc gcc-multilib make cmake
```
### changing username
```sh
usermod -l newUsername oldUsername 
```
