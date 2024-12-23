# Final-Project-instalasi-5-lingkungan-web-server
NAUFAL SADAM SUNU ISKANDAR (23.83.0952) 23 TK 01



# SERVER
- ssh
- Nginx web server
- Database Mysql
- php
- CDN cloudflare

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
![download](https://github.com/nopaldev/Final-Project-instalasi-5-lingkungan-web-server/blob/main/webku.png)
# Install Apache2 web-server
![download](https://raw.githubusercontent.com/Tuanvallen/FINAL-Projek-OS-Server-Sistem-Admiin/refs/heads/main/Foto%20Instalasi/Apache_img.png)


```bash
sudo apt install apache2 -y
```
- Aktifkan dan mulai Apache:
```bash
sudo systemctl enable apache2
sudo systemctl start apache2
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


# Instal Framework Flask

![alt text](https://github.com/Tuanvallen/FINAL-Projek-OS-Server-Sistem-Admiin/blob/main/Foto%20Instalasi/flask1.png?raw=true)

```bash
pip3 install flask
```

# Instal Gunicorn

![alt text](https://github.com/Tuanvallen/FINAL-Projek-OS-Server-Sistem-Admiin/blob/main/Foto%20Instalasi/Gunicorn.jpeg?raw=true)


```bash
pip3 install gunicorn
```


# Konfigurasi Aplikasi Flask
- Buat direktori untuk aplikasi Flask Anda:
```bash
mkdir ~/my_flask_app
cd ~/my_flask_app
```
- Buat file `app.py` sederhana:
```python
from flask import Flask

app = Flask(__name__)

@app.route('/')
def home():
    return "Hello, World!"

if __name__ == "__main__":
    app.run()
```

# Jalankan Flask dengan Gunicorn
```bash
gunicorn --bind 0.0.0.0:8000 app:app
```

# Konfigurasi Apache untuk Meneruskan Permintaan ke Gunicorn
- Aktifkan modul Apache yang diperlukan:
```bash
sudo a2enmod proxy proxy_http
sudo systemctl restart apache2
```
- Buat file konfigurasi Apache untuk aplikasi Flask Anda:
```bash
sudo nano /etc/apache2/sites-available/my_flask_app.conf
```
Tambahkan konten berikut:
```apache
<VirtualHost *:80>
    ServerName yourdomain.com

    ProxyPass / http://127.0.0.1:8000/
    ProxyPassReverse / http://127.0.0.1:8000/

    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
```
- Aktifkan situs dan restart Apache:
```bash
sudo a2ensite my_flask_app
sudo systemctl restart apache2
```

# Uji Pengaturan
- Buka browser dan akses `http://<ip-server-anda>`.
- Anda seharusnya melihat pesan "Hello, World!" ditampilkan.

---

## Langkah Hosting dengan Cloudflare Tunnel




## Taruh di Cloudflare
sangkutkan web lokal ke cloudflare agar akses rendering lebih cepat dan ada fitur anti serangan ddos maupun brutforce

![download](https://github.com/nopaldev/Final-Project-instalasi-5-lingkungan-web-server/blob/main/cldflr.png)
```bash

curl -L --output cloudflared.deb https://github.com/cloudflare/cloudflared/releases/latest/download/cloudflared-linux-amd64.deb && 

sudo dpkg -i cloudflared.deb && 

sudo cloudflared service install eyJhIjoiNGE0M2M3YzQ1Mjc4MmVkNTA1MWEzYjFmMTdjMDJlM2EiLCJ0IjoiZjliZGE1ZjYtOWY0OS00OTMyLTk2N2ItMmYzZmUyMDUxZGRmIiwicyI6Ik9EQXdZelV6TjJZdE16ZzJNeTAwTW1FM0xUazJZVGN0T1RjNE5UYzNOREZpTlRWaSJ9
```
