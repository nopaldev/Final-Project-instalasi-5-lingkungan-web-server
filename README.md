# Final-Project-instalasi-5-lingkungan-web-server
NAUFAL SADAM SUNU ISKANDAR (23.83.0952) 23 TK 01



# SERVER
- Nginx web server
- Squid Server
- Load Balancer Haproxy

# Operating System
Ubuntu server 22.04.5 https://releases.ubuntu.com/jammy/ubuntu-22.04.5-desktop-amd64.iso

### Install SSH di Ubuntu 20.04
```bash
sudo apt-get update
sudo apt-get install openssh-server
# konfigurasi sshd.conf
# hilangkan tanda (#) pada :
Port 1515
PermitRootLogin prohibit-password
# izinkan SSH untuk melewati firewall
sudo ufw allow ssh
```
SSH sudah terinstall, anda bisa menggunakan nya pada cmd/terminal
menggunakan command :

> ssh user@ip_address

# Install NginX
![download](https://github.com/dword32bit/SysAdmin/assets/114817148/e3318239-a3a4-449d-bd86-79edc65c4b7f)

Saya menggunakan NginX untuk mengelola Web saya yang berada dalam dua sistem operasi yang terpisah dengan server

```bash
#Installasi NginX
sudo apt install nginx

#Periksa status NginX
sudo systemctl status nginx
```
![download](https://github.com/nopaldev/Final-Project-instalasi-5-lingkungan-web-server/blob/main/status_nginx.png)
### Melihat paket Nginx
```bash
sudo ufw app list
```
### Mengijinkan http Nginx
```bash
sudo ufw allow 'Nginx HTTP'
```
### cek ufw status
```bash
sudo ufw status
```
### seting server blocks
```bash
sudo mkdir -p /var/www/naufalserver/html
sudo chown -R $USER:$USER /var/www/naufalserver/html
sudo chmod -R 755 /var/www/naufalserver
nano /var/www/naufalserver/html/index.html
```
#sample html
```bash
<html>
    <head>
        <title>Hai, selamat datang di naufalserver</title>
    </head>
    <body>
        <h1>walaupun mukaku seperti tytyd, tapi cintaku ke kamu unlimited</h1>
    </body>
</html>

```
### Konfigurasi NginX
untuk melakukan konfigurasi menggunakan nano
```bash
sudo nano /etc/nginx/sites-available/naufalserver
```
```bash
server {
        listen 80;
        listen [::]:80;

        # SSL configuration
        #
        # listen 443 ssl default_server;
        # listen [::]:443 ssl default_server;
        #
        # Note: You should disable gzip for SSL traffic.
        # See: https://bugs.debian.org/773332
        #
        # Read up on ssl_ciphers to ensure a secure configuration.
        # See: https://bugs.debian.org/765782
        #
        # Self signed certs generated by the ssl-cert package
        # Don't use them in a production server!
        #
        # include snippets/snakeoil.conf;

        root /var/www/naufalserer/html;

        # Add index.php to the list if you are using PHP
         index index.html index.htm index.nginx-debian.html;

        server_name naufalserver.my.id www.naufalserver.my.id;

        location / {
                # First attempt to serve request as file, then
                # as directory, then fall back to displaying a 404.
                try_files $uri $uri/ =404;
        }
        error_log /var/log/nginx/error.log;
        access_log /var/log/nginx/access.log;
        # pass PHP scripts to FastCGI server
        #
        #location ~ \.php$ {
        #       include snippets/fastcgi-php.conf;
        #
        #       # With php-fpm (or other unix sockets):
        #       fastcgi_pass unix:/run/php/php7.4-fpm.sock;
        #       # With php-cgi (or other tcp sockets):
        #       fastcgi_pass 127.0.0.1:9000;
        #}

        # deny access to .htaccess files, if Apache's document root
        # concurs with nginx's one
        #
        }

```
menjalankan file dengan membuat link di sites-enabled
```bash
sudo ln -s /etc/nginx/sites-available/naufalserver /etc/nginx/sites-enabled/
```
edit kofigurasi pada /etc/nginx/nginx.conf
```bash
sudo nano /etc/nginx/nginx.conf
```
```bash
...
http {
    ...
    server_names_hash_bucket_size 64;
    ...
}
...
```
test nginx dengan perintah
```bash
sudo nginx -t
```
bila semua berjalan lancar, maka silahkan restart nginx
```bash
sudo systemctl restart nginx
```
![download](https://github.com/dword32bit/SysAdmin/assets/114817148/e3318239-a3a4-449d-bd86-79edc65c4b7f)
