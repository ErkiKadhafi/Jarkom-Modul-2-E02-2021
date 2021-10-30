# Jarkom-Modul-2-E02-2021

Berikut ini adalah laporan resmi dari Praktikum Jaringan Komputer Modul 1 tahun 2021 di Institut Teknologi Sepuluh Nopember

Dokumen ini ditulis oleh

-   05111940000063 - Ryan Garnet Andrianto
-   05111940000050 - Erki Kadhafi Rosyid
-   05111940000141 - M. Farhan Haykal

### 1. EniesLobby akan dijadikan sebagai DNS Master, Water7 akan dijadikan DNS Slave, dan Skypie akan digunakan sebagai Web Server. Terdapat 2 Client yaitu Loguetown, dan Alabasta. Semua node terhubung pada router Foosha, sehingga dapat mengakses internet

### 2. Buat website utama dengan mengakses franky.c08.com dengan alias www.franky.c08.com pada folder kaizoku

### 3. Buat subdomain super.franky.c08.com dengan alias www.super.franky.c08.com yang diatur DNS nya di EniesLobby dan mengarah ke Skypie

### 4. Buat juga reverse domain untuk domain utama

### 5. Buat Water7 sebagai DNS Slave untuk domain utama

### 6. Setelah itu terdapat subdomain mecha.franky.c08.com dengan alias www.mecha.franky.c08.com yang didelegasikan dari EniesLobby ke Water7 dengan IP menuju ke Skypie dalam folder sunnygo

### 7. Buatkan subdomain melalui Water7 dengan nama general.mecha.franky.c08.com dengan alias www.general.mecha.franky.c08.com yang mengarah ke Skypie

Untuk membuat sub domain tersebut, pertama buka folder sunny go yang terdapat pada Water7, lalu tambahkan 2 baris berikut

```
general         IN      A       10.30.2.4 ;IP SKYPIE
www.general     IN      CNAME   general.mecha.franky.e02.com.
```

Setelah itu lakukan, restart service bind9 dan ping dari alabasta dengan address general.mecha.franky.e02.com. dan www.general.mecha.franky.e02.com

![image](./images/no7.png)

### 8. Konfigurasi Webserver dengan domain www.franky.c08.com dan DocumentRoot pada /var/www/franky.c08.com.

Install apache2 pada dengan syntax

```
apt-get install apache2
service apache2 start
```

lalu buka folder `apache2/sites-available/` dan copy file `000-default.conf'` menjadi `franky.e02.com.conf`. Setelah itu tambahkan syntax di bawah ini dan restart apache2.

```
ServerName franky.e02.com
ServerAlias www.franky.e02.com

DocumentRoot /var/www/franky.e02.com
```

lakukan clone dari github yang ada di soal [Link github](https://github.com/FeinardSlim/Praktikum-Modul-2-Jarkom). Setelah itu lakukan unzip dan taruh file yang terdapat pada file franky ke folder `franky.e02.com`. Setelah itu lakukan akses menggunakan lynx dari client yang akan menghasilkan seperti di bawah ini

![image](./images/no8.png)

### 9. Mengubah url www.franky.c08.com/index.php/home dapat menjadi menjadi www.franky.c08.com/home

Buka file `franky.e02.com.conf` pada folder `apache2/sites-available/` di node skypie, lalu tambahkan alias dengan syntax

```
Alias "/home" "/var/www/franky.e02.com/index.php/home"
```

lalu lynx ke linx `franky.e02.com/index.php/home` yang akan menghasilkan seperti dibawah ini

![image](./images/no9.png)

### 10. Konfigurasi subdomain www.super.franky.c08.com Setelah itu, pada subdomain www.super.franky.c08.com, Luffy membutuhkan penyimpanan aset yang memiliki DocumentRoot pada /var/www/super.franky.c08.com.

buka folder `apache2/sites-available/` dan copy file `000-default.conf'` menjadi `super.franky.e02.com.conf`. Setelah itu tambahkan syntax di bawah ini dan restart apache2.

```
ServerName super.franky.e02.com
ServerAlias www.super.franky.e02.com

DocumentRoot /var/www/franky.e02.com
```

restart apache2 pada skypie dan lakukan lynx ke alamat `super.franky.e02.com` dan `www.super.franky.e02.com` maka akan menghasilkan seperti ini

![image](./images/no10.png)

### 11. Akan tetapi, pada folder /public hanya dapat melakukan directory listing saja.

buka file `super.franky.e02.com.conf` pada folder `apache2/sites-available` dan tambahkan syntax berikut

```
<Directory /var/www/super.franky.e02.com>
        Options +Indexes
        AllowOverride All
</Directory>

<Directory /var/www/super.franky.e02.com/public>
        Options +Indexes
</Directory>
```

lalu akses folder public dengan menggunakan lynx `www.super.franky.e02.com/public`

![image](./images/no11.png)

### 12. Mengganti error default dari apache menjadi error file 404.html pada folder/error.

<<<<<<< HEAD
buka file `super.franky.e02.com.conf` pada folder `apache2/sites-available` dan tambahkan syntax berikut

```
ErrorDocument 404 /error/404.html
```

lalu akses url asal dengan perintah lynx dan akan menghasilkans seperti berikut

![image](./images/no12.png)

### 13. Konfigurasi virtual host agar dapat mengakses file asset www.super.franky.e02.com/public/js dari www.super.franky.e02.com/js.

Konfigurasi web `super.franky.e02.com` berada di directory `/etc/apache2/sites-available/super.franky.e02.com.conf`. Untuk menyelesaikan soal ini, tim E02 menambahkan satu baris kode yaitu sebagai berikut.

```bash
Alias "/js" "/var/www/super.franky.e02.com/public/js"
```

![image](https://user-images.githubusercontent.com/8071604/139531169-f136de96-df4e-4eaa-a9ee-a8ca796d382e.png)

Penjelasan:

Alias [URL-path] file-path|directory-path

Sumber dokumentasi: https://httpd.apache.org/docs/2.4/mod/mod_alias.html

Berdasarkan dokumentasi dari apache2, statement Alias dapat digunakan untuk URL redirection. Jika parameter URL-path diisi dengan "/public/js" dan directory-path diisi dengan "/var/www/super.franky.e02.com/public/js", maka ketika seorang client mengakses http://super.franky.e02.com/js, dia akan melihat isi dari folder public/js yaitu di http://super.franky.e02.com/public/js.

### 14. Atur web www.general.mecha.franky.e02.com sehingga hanya bisa diakses melalui port 15000 dan port 15500

Secara default, port web adalah 80. Konfigurasi web `general.mecha.franky.e02.com` berada di `/etc/apache2/genera.mecha.franky.e02.com.conf`. Untuk membuat website ini dapat diakses melalui port 15000 atau port 15500, tim E02 menggandakan konfigurasi ini menjadi dua konfigurasi: `/etc/apache2/general.mecha.franky.e02.com-port15000.conf` dan `/etc/apache2/general.mecha.franky.e02.com-port15500.conf`. Isi dari kedua konfigurasi ini adalah sama kecuali nomor port yang dimilikinya. Port bisa diubah melalui `<VirtualHost *:PORT>` yang tertulis di konfigurasi web apache.

![image](https://user-images.githubusercontent.com/8071604/139531372-f9c7325d-eb01-4782-8dee-39a26e2379f6.png)

Gambar 14.1 Konfigurasi `/etc/apache2/general.mecha.franky.e02.com-port15000.conf`

![image](https://user-images.githubusercontent.com/8071604/139531387-e2235098-21c1-45cb-9a21-15bf81bb3493.png)

Gambar 14.2 Konfigurasi `/etc/apache2/general.mecha.franky.e02.com-port15500.conf`

Setelah kedua konfigurasi apache tersebut dibuat, tim E02 harus meng-enable kedua konfigurasi tersebut dengan menggunakan command: `a2ensite general.mecha.franky.e02.com-port15000` dan `a2ensite general.mecha.franky.e02.com-port15500`.

![image](https://user-images.githubusercontent.com/8071604/139531437-3cd9ab6a-1ca7-4605-b3a2-835aae05b697.png)

Setelah kedua konfigurasi tersebut di-enable, tim E02 harus me-restart aplikasi apache dengan menggunakan command `service apache2 restart`.

Tahap berikutnya yaitu testing. Tim E02 mencoba mengakses http://general.mecha.franky.e02.com:15000 dan http://general.mecha.franky.e02.com:15500 dengan bantuan aplikasi `lynx`.

1. Install `lynx` jika belum terinstall dengan menggunakan command `apt-get install lynx`.
2. Lakukan complete test untuk setiap web di atas (dengan www dan tanpa www) melalui client `LogueTown`.
3. Run command `lynx http://general.mecha.franky.e02.com:15000`
4. Run command `lynx http://www.general.mecha.franky.e02.com:15000`
5. Run command `lynx http://general.mecha.franky.e02.com:15500`
6. Run command `lynx http://www.general.mecha.franky.e02.com:15500`

![image](https://user-images.githubusercontent.com/8071604/139531576-67b4f3cf-7ebf-474b-9f3e-4fd5617792d7.png)

![image](https://user-images.githubusercontent.com/8071604/139531586-aaa7a6b4-9bb8-42b1-85e7-b939c17c27dc.png)

Catatan: Halaman web ini memang mempunyai authentication karena merupakan jawaban dari soal nomor 15.

![image](https://user-images.githubusercontent.com/8071604/139531566-74b6c384-1d52-4be0-93e3-49f6f6b34f90.png)

### 15. Memberi autentikasi pada www.general.mecha.franky.c08.com dengan username luffy dan password onepiece

### 16. Setiap kali mengakses IP Skypie akan dialihkan secara otomatis ke www.franky.c08.com

### 17. Mengganti request gambar yang memiliki substring “franky” akan diarahkan menuju franky.png ketika mengakses www.super.franky.c08.com
