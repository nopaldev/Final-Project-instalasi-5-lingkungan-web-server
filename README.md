# Final-Project-instalasi-5-lingkungan-web-server
NAUFAL SADAM SUNU ISKANDAR (23.83.0952) 23 TK 01



# SERVER
Nginx web server
Load Balancer Haproxy

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

## Install NginX
![download](https://github.com/dword32bit/SysAdmin/assets/114817148/e3318239-a3a4-449d-bd86-79edc65c4b7f)

Saya menggunakan NginX untuk mengelola Web saya yang berada dalam dua sistem operasi yang terpisah dengan server

```bash
#Installasi NginX
sudo apt install nginx

#Periksa status NginX
sudo systemctl status nginx
```

### Konfigurasi NginX
untuk melakukan konfigurasi menggunakan nano
```bash
sudo nano /etc/nginx/sites-available/scrsrv.my.id.conf
```
```bash
server {
        listen 80;

        root /var/www/html;
        index index.html index.htm index.nginx-debian.html;

        server_name scrsrv.my.id www.scrsrv.my.id;

        location / {
                try_files $uri $uri/ =404;
        }

        error_log /var/log/nginx/scrsrv.my.id.error;
        access_log /var/log/nginx/scrsrv.my.id.access;

}

#Installasi FTP server
sudo apt install vsftpd

#Menyalin file konfigurasi sebagai backup
sudo cp /etc/vsftpd.conf /etc/vsftpd.conf.orig

#Izinkan port berkaitan dengan ftp, Kemudian dengan FTP kirimkan file-file pendukung web server
sudo ufw allow 20,21,990/tcp

#Konfigurasi ip address pada masing-masing sistem operasi web server
#Sistem operasi pertama
nano /etc/netplan/00-installer-config.yaml
network:
  ethernets:
    enp0s3:
      dhcp4: no
      addresses: [192.168.1.20/24]
      gateway: 192.168.1.1
  version: 2

#Sistem operasi kedua
nano /etc/netplan/00-installer-config.yaml
network:
  ethernets:
    enp0s3:
      dhcp4: no
      addresses: [192.168.1.30/24]
      gateway: 192.168.1.1
  version: 2

#Setelah semua konfigurasi selesai matikan kembali port yang tidak digunakan
#Hapus semua port allow ufw, dan hanya server yang dapat mengakses web server
ufw allow deny 80/tcp
ufw allow from 192.168.1.5 to any port www
```
![download](https://github.com/Xzhacts-Crew/scrserv/blob/main/webserv.jpg)

## Konfigurasi iptables
Disini kita menggunakan iptables untuk drop DDos
berikut adalah beberapa alasan kita menggunakan iptables
- menolak paket-paket TCP yang memiliki semua flag TCP (ACK, URG, PSH, RST, SYN, dan FIN) diatur ke "NONE" atau tidak ada yang diatur
-  Mencegah adanya paket tcp syn pada header yang  mengakibatkan suatu koneksi tcp menjadi lama karena melalui 3 langkah(3-way handshake)
-  Menerapkan paket baru yang ditujukan ke port 80(http) akan ditandai dan dipindai apakah paket tersebut mencurigakan atau potensial serangan
-  Menerapkan pemblokiran pada alamat ip yang mencoba terlalu sering mengakses port 80(http) dalam kurun waktu yang diterapkan
```bash
iptables -A INPUT -p tcp --tcp-flags ALL NONE -j DROP

iptables -A INPUT -p tcp ! --syn -m state --state NEW -j DROP

iptables -I INPUT -p tcp --dport 80 -m state --state NEW -m recent --set

iptables -I INPUT -p tcp --dport 80 -m state --state NEW --state NEW -m recent --update --seconds 10 --hitcount 200 -j DROP
```

simpan konfigurasi dan jalankan konfigurasi tersebut
```bash
sudo ln -s /etc/nginx/sites-available/scrsrv.my.id.conf /etc/nginx/sites-enabled/scrsrv.my.id.conf

#Verifikasi konfigurasi nginx
sudo nginx -t

#jika muncul test is successful berarti tidak ada kendala dalam konfigurasi nginx

#restart nginx
sudo systemctl restart nginx
```
