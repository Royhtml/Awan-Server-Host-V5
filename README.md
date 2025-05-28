# PHP Server Hosting Offline (Tanpa Node.js)

<center><img src = "awan.ico"></center>

## video tutorial
[tutorials penggunaan](https://www.tiktok.com/@royhtml/video/7509367246628621624?is_from_webapp=1&sender_device=pc&web_id=7489705398321759751)
## Daftar Isi
1. [Persyaratan Sistem](#persyaratan-sistem)
2. [Pilihan Server PHP](#pilihan-server-php)
3. [Instalasi XAMPP](#instalasi-xampp)
4. [Instalasi PHP Built-in Server](#instalasi-php-built-in-server)
5. [Hosting phpMyAdmin](#hosting-phpmyadmin)
6. [Konfigurasi Database MySQL](#konfigurasi-database-mysql)
7. [Mengakses Server dari Perangkat Lain](#mengakses-server-dari-perangkat-lain)
8. [Keamanan Dasar](#keamanan-dasar)
9. [Troubleshooting](#troubleshooting)

## Persyaratan Sistem
- Sistem Operasi: Windows, Linux, atau macOS
- RAM minimal: 2GB (4GB disarankan)
- Ruang disk: 500MB (untuk server dasar)

## Pilihan Server PHP

### 1. XAMPP (Solusi All-in-One)
Paket lengkap yang termasuk:
- Apache (web server)
- MySQL/MariaDB (database)
- PHP
- phpMyAdmin

### 2. PHP Built-in Web Server
Server bawaan PHP (untuk pengembangan saja):
```sh
php -S localhost:8000
```

## Instalasi XAMPP

### Untuk Windows:
1. Download XAMPP dari [https://www.apachefriends.org](https://www.apachefriends.org)
2. Jalankan installer dan ikuti petunjuk
3. Pilih komponen:
   - Apache
   - MySQL
   - PHP
   - phpMyAdmin
4. Selesaikan instalasi

### Untuk Linux (Debian/Ubuntu):
```bash
sudo apt update
sudo apt install apache2 mysql-server php libapache2-mod-php php-mysql phpmyadmin
```

### Memulai XAMPP:
1. Buka XAMPP Control Panel
2. Start modul Apache dan MySQL
3. Buka browser dan akses [http://localhost](http://localhost)

## Instalasi PHP Built-in Server

Jika Anda hanya ingin server PHP sederhana tanpa Apache:

1. Pastikan PHP terinstal (cek dengan `php -v`)
2. Buat folder project
3. Masuk ke folder project dan jalankan:
   ```bash
   php -S localhost:8000
   ```
4. Server akan berjalan di [http://localhost:8000](http://localhost:8000)

Untuk membuatnya bisa diakses dari jaringan lokal:
```bash
php -S 0.0.0.0:8000
```

## Hosting phpMyAdmin

### Jika menggunakan XAMPP:
phpMyAdmin sudah termasuk dan bisa diakses di [http://localhost/phpmyadmin](http://localhost/phpmyadmin)

### Jika instalasi manual:
1. Download phpMyAdmin dari [https://www.phpmyadmin.net](https://www.phpmyadmin.net)
2. Ekstrak ke folder `phpmyadmin` di root web server
   - Untuk XAMPP: `C:\xampp\htdocs\phpmyadmin`
   - Untuk Apache di Linux: `/var/www/html/phpmyadmin`
3. Buat file konfigurasi:
   ```bash
   cp config.sample.inc.php config.inc.php
   ```
4. Edit `config.inc.php` dan setel:
   ```php
   $cfg['blowfish_secret'] = 'randomstring123'; // Buat string acak
   ```

## Konfigurasi Database MySQL

1. Akses phpMyAdmin di [http://localhost/phpmyadmin](http://localhost/phpmyadmin)
2. Login dengan:
   - Username: `root`
   - Password: kosong (default) atau password yang Anda set
3. Untuk keamanan, buat user baru:
   - Klik "User accounts" > "Add user account"
   - Isi username dan password
   - Berikan privileges yang diperlukan
4. Buat database baru:
   - Klik "New" di sidebar
   - Beri nama database dan klik "Create"

## Mengakses Server dari Perangkat Lain

1. Cari alamat IP lokal komputer host:
   - Windows: `ipconfig` di Command Prompt
   - Linux/macOS: `ifconfig` atau `ip a`
2. Pastikan server dijalankan dengan binding ke 0.0.0.0:
   - Untuk PHP built-in server: `php -S 0.0.0.0:8000`
   - Untuk XAMPP: Edit `httpd.conf` dan ubah `Listen 80` ke `Listen 0.0.0.0:80`
3. Di perangkat lain di jaringan yang sama, akses:
   `http://[IP-address]:[port]`
   Contoh: `http://192.168.1.100:8000`

## Keamanan Dasar

1. **Ubah password root MySQL**:
   ```sql
   ALTER USER 'root'@'localhost' IDENTIFIED BY 'password-baru-yang-kuat';
   ```

2. **Batasi akses phpMyAdmin**:
   - Edit file `phpmyadmin/.htaccess`:
     ```
     AuthType Basic
     AuthName "Restricted Access"
     AuthUserFile /path/to/.htpasswd
     Require valid-user
     ```
   - Buat file password:
     ```bash
     htpasswd -c /path/to/.htpasswd username
     ```

3. **Nonaktifkan akses remote** jika tidak diperlukan:
   - Edit `my.ini` atau `my.cnf` dan tambahkan:
     ```
     bind-address = 127.0.0.1
     ```

## Troubleshooting

1. **Port conflict**:
   - Error: `Apache shutdown unexpectedly`
   - Solusi: Cari dan hentikan aplikasi yang menggunakan port 80 atau 443
     ```bash
     netstat -ano | findstr :80
     taskkill /PID [PID] /F
     ```

2. **phpMyAdmin tidak bisa akses MySQL**:
   - Periksa konfigurasi `config.inc.php`
   - Pastikan service MySQL berjalan

3. **PHP script tidak dieksekusi**:
   - Pastikan file berekstensi `.php`
   - Untuk built-in server, pastikan direktori yang benar

4. **Akses ditolak dari jaringan lain**:
   - Periksa firewall
   - Pastikan server binding ke `0.0.0.0` bukan `127.0.0.1`

Dengan panduan ini, Anda dapat membuat server PHP offline lengkap dengan phpMyAdmin tanpa memerlukan Node.js. Server ini cocok untuk pengembangan lokal atau penggunaan dalam jaringan internal.
