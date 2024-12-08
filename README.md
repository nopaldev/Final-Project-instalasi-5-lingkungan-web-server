# Final-Project-instalasi-5-lingkungan-web-server
NAUFAL SADAM SUNU ISKANDAR (23.83.0952) 23 TK 01



# SERVER
- ssh
- Nginx web server
- Database Mysql
- php
- cloudflare

# Operating System
Ubuntu server 22.04.5 https://releases.ubuntu.com/jammy/ubuntu-22.04.5-desktop-amd64.iso

# Install SSH di Ubuntu 20.04
```bash
sudo apt-get update
sudo apt-get install openssh-server

PermitRootLogin prohibit-password
# izinkan SSH untuk melewati firewall
sudo ufw allow ssh
```
SSH sudah terinstall, anda bisa menggunakan nya pada cmd/terminal
menggunakan command :

> ssh user@ip_address

# Install NginX web-server
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
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>ShopEase - Jual Beli Barang</title>
    <style>
        /* General Styles */
        body {
            font-family: Arial, sans-serif;
            margin: 0;
            padding: 0;
            line-height: 1.6;
        }

        .container {
            display: flex;
            justify-content: space-between;
            align-items: center;
            padding: 0 20px;
        }

        /* Header */
        header {
            background: #333;
            color: #fff;
            padding: 10px 0;
        }

        header h1 {
            margin: 0;
        }

        header nav a {
            color: #fff;
            text-decoration: none;
            margin: 0 15px;
        }

        header nav a:hover {
            text-decoration: underline;
        }

        /* Hero Section */
        .hero {
            background: linear-gradient(to right, #4caf50, #81c784);
            color: white;
            text-align: center;
            padding: 50px 20px;
        }

        .hero h2 {
            font-size: 2.5em;
        }

        .hero .btn {
            background: #333;
            color: white;
            padding: 10px 20px;
            text-decoration: none;
            border-radius: 5px;
            margin-top: 20px;
            display: inline-block;
        }

        .hero .btn:hover {
            background: #555;
        }

        /* Products Section */
        .products {
            padding: 20px;
            background: #f4f4f4;
            text-align: center;
        }

        .products h2 {
            margin-bottom: 20px;
        }

        .product-list {
            display: flex;
            justify-content: center;
            gap: 20px;
            flex-wrap: wrap;
        }

        .product-card {
            background: white;
            border: 1px solid #ddd;
            border-radius: 8px;
            width: 200px;
            padding: 15px;
            text-align: center;
            box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
        }

        .product-card img {
            max-width: 100%;
            border-radius: 8px;
        }

        .product-card h3 {
            margin: 10px 0;
        }

        .product-card button {
            background: #4caf50;
            color: white;
            padding: 10px 15px;
            border: none;
            border-radius: 5px;
            cursor: pointer;
        }

        .product-card button:hover {
            background: #45a049;
        }

        /* Footer */
        footer {
            background: #333;
            color: #fff;
            text-align: center;
            padding: 10px 0;
            margin-top: 20px;
        }
    </style>
</head>
<body>
    <header>
        <div class="container">
            <h1>Hello Admin</h1>
            <nav>
                <a href="#home">Home</a>
                <a href="#products">Products</a>
                <a href="#about">About Us</a>
                <a href="#contact">Contact</a>
            </nav>
        </div>
    </header>

    <section id="home" class="hero">
        <h2>Temukan Barang Impianmu dengan Mudah</h2>
        <p>Platform terbaik untuk membeli dan menjual barang kebutuhan sehari-hari!</p>
        <a href="#products" class="btn">Lihat Produk</a>
    </section>

    <section id="products" class="products">
        <h2>Produk Terlaris</h2>
        <div class="product-list">
            <div class="product-card">
                <img src="https://via.placeholder.com/150" alt="Produk 1">
                <h3>Alat Anu</h3>
                <p>Harga: Rp 3,000,000</p>
                <button>Beli Sekarang</button>
            </div>
            <div class="product-card">
                <img src="https://via.placeholder.com/150" alt="Produk 2">
                <h3>ANU ANU ANU</h3>
                <p>Harga: Rp 8,000,000</p>
                <button>Beli Sekarang</button>
            </div>
            <div class="product-card">
                <img src="https://via.placeholder.com/150" alt="Produk 3">
                <h3>nah kalo ini anu aja</h3>
                <p>Harga: Rp 500,000</p>
                <button>Beli Sekarang</button>
            </div>
        </div>
    </section>

    <footer>
        <p>&copy; 2024 Nopal Store. All rights reserved.</p>
    </footer>
</body>
</html>


```
![download](https://github.com/nopaldev/Final-Project-instalasi-5-lingkungan-web-server/blob/main/samplehtml.png)
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
# Instalasi database Mysql
![download](https://github.com/nopaldev/Final-Project-instalasi-5-lingkungan-web-server/blob/main/MySQL-Logo.jpeg)
```bash
sudo apt-get install mysql-server
```
Konfigurasi keamanan pada Mysql
```bash
sudo mysql_secure_installation

```
```bash
root@naufalserver:/var/www/naufalserver/html# sudo mysql_secure_installation

Securing the MySQL server deployment.

Connecting to MySQL using a blank password.

VALIDATE PASSWORD COMPONENT can be used to test passwords
and improve security. It checks the strength of password
and allows the users to set only those passwords which are
secure enough. Would you like to setup VALIDATE PASSWORD component?

Press y|Y for Yes, any other key for No: n

Skipping password set for root as authentication with auth_socket is used by default.
If you would like to use password authentication instead, this can be done with the "ALTER_USER" command.
See https://dev.mysql.com/doc/refman/8.0/en/alter-user.html#alter-user-password-management for more information.

By default, a MySQL installation has an anonymous user,
allowing anyone to log into MySQL without having to have
a user account created for them. This is intended only for
testing, and to make the installation go a bit smoother.
You should remove them before moving into a production
environment.

Remove anonymous users? (Press y|Y for Yes, any other key for No) : y
Success.


Normally, root should only be allowed to connect from
'localhost'. This ensures that someone cannot guess at
the root password from the network.

Disallow root login remotely? (Press y|Y for Yes, any other key for No) : y
Success.

By default, MySQL comes with a database named 'test' that
anyone can access. This is also intended only for testing,
and should be removed before moving into a production
environment.


Remove test database and access to it? (Press y|Y for Yes, any other key for No) : y
 - Dropping test database...
Success.

 - Removing privileges on test database...
Success.

Reloading the privilege tables will ensure that all changes
made so far will take effect immediately.

Reload privilege tables now? (Press y|Y for Yes, any other key for No) : y
Success.

All done!

```
![download](https://github.com/nopaldev/Final-Project-instalasi-5-lingkungan-web-server/blob/main/ssmysql.png)
# Instalasi php
![download](https://github.com/nopaldev/Final-Project-instalasi-5-lingkungan-web-server/blob/main/php.png)
cara menginstall php untuk webserver nginx sedikit berbeda, perintah seperti di bawah:
```bash
sudo apt-get install php-fpm php-mysql

```
