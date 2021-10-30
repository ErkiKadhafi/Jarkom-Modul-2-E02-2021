# Jarkom-Modul-2-E02-2021

Berikut ini adalah laporan resmi dari Praktikum Jaringan Komputer Modul 1 tahun 2021 di Institut Teknologi Sepuluh Nopember

Dokumen ini ditulis oleh

-   05111940000063 - Ryan Garnet Andrianto
-   05111940000050 - Erki Kadhafi Rosyid
-   05111940000141 - M. Farhan Haykal

### 1. EniesLobby akan dijadikan sebagai DNS Master, Water7 akan dijadikan DNS Slave, dan Skypie akan digunakan sebagai Web Server. Terdapat 2 Client yaitu Loguetown, dan Alabasta. Semua node terhubung pada router Foosha, sehingga dapat mengakses internet

-Untuk langkah awal buat topologi seperti di bawah ini:
![1 topologi](https://user-images.githubusercontent.com/70801807/139529070-140c8cdc-e8e5-4414-9c92-962ea8f725b0.PNG)

-Setelah membuat topologi seperti di atas maka langkah selanjutnya adalah :
a. Menambahkan kkonfigurasi di bawah ini untuk router Foosha :
`iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE -s 10.30.0.0/16`
`cat /etc/resolv.conf`
b. Tambahkan konfiggurasi di bawah ini pada node selain foosha :
`echo nameserver 192.168.122.1 > /etc/resolv.conf`
c. Lakuka ping google.com pada semua node untuk mengecek apakah node telah terhubung ke internet:
![pingGoogleEnies](https://user-images.githubusercontent.com/70801807/139529282-0544678b-098f-48ce-b7c0-495d1a385598.PNG)
![pingGoogleWater7](https://user-images.githubusercontent.com/70801807/139529354-d2ae3b2c-f627-43d1-aa06-4ed303b5a735.PNG)
![pingGoogleSkypie](https://user-images.githubusercontent.com/70801807/139529401-93606279-b011-4463-b6f2-43e6b4f27221.PNG)
![image](https://user-images.githubusercontent.com/70801807/139529428-db98f08c-6d36-492b-8af9-2caf95dd94fb.png)
![image](https://user-images.githubusercontent.com/70801807/139529462-f8ded76e-3f4b-4767-b1fc-3faef0b034c1.png)

Setelah semua node berhasil melakukan ping google.com maka topologi sudah dapat mengakses ke internet

### 2. Buat website utama dengan mengakses franky.e02.com dengan alias www.franky.e02.com pada folder kaizoku

-Dikarenakan EnniesLoby akan menjadi DNS Master maka akan dilakukan instalasi bind9 dengan command di bawah ini :
`apt-get update`
`apt-get bind9 -y`
-Selanjutnya, akan dilakukan konfigurasi pada /etc/bind/named.conf.local sebagai berikut :
![image](https://user-images.githubusercontent.com/70801807/139529658-67fc2afb-e31f-46be-b26b-692a0fd3f9b3.png)
-Selanjutnya buat folder kaizoku dengan command sebagai berikut :
`mkdir /etc/bind/kaizoku`
-Selanjutnya copykan file db.local pada path /etc/bind ke dalam folder kaizoku yang baru saja dibuat dan ubah namanya menjadi e02.com
`cp /etc/bind/db.local /etc/bind/kaizoku/e02.com`
-Selanjutnya lakukan konfigurasi pada /etc/bind/kaizoku/e02.com sebagai berikut :
![image](https://user-images.githubusercontent.com/70801807/139529836-50d82bc1-0263-45b7-b928-d06eaf207c25.png)
-Selanjutnya lakukan restart bind9 EnniesLobby dengan command sebagai berikut :
`service bind9 restart`
-Selanjutnya edit file /etc/resolv.conf pada client Loguetown dan Alabasta dengan menuliskan IP EnniesLobby sebagai berikut:
![image](https://user-images.githubusercontent.com/70801807/139529963-5f98c45e-2c26-43db-a9d3-80652ca4789e.png)
-Selanjutnya lakukan ping ke domain franky.e02.com pada salah satu client yaitu Loguetown atau Alabasta dengan command sebagai berikut :
`ping franky.e02.com`
![image](https://user-images.githubusercontent.com/70801807/139530052-48b9e611-ad02-4b46-9c68-3b1bf6a1a46b.png)
Jika berhasil maka IP EnniesLobby lah yang akan muncul.

### 3. Buat subdomain super.franky.c08.com dengan alias www.super.franky.e02.com yang diatur DNS nya di EniesLobby dan mengarah ke Skypie

-Tambahkan 2 line di bawah ini ke dalam /etc/bind/kaizoku/e02.com pada EnniesLobby :
![image](https://user-images.githubusercontent.com/70801807/139530147-d42d8ee3-8a2a-46c0-95bc-46b5250e60da.png)
-Setelahnya restart bind9 dengan :
`service bind9 restart`
-Lakukan tes dengan ping super.franky.e02.com atau wwww.super.franky.e02.com :
![image](https://user-images.githubusercontent.com/70801807/139530230-093d9cba-cb49-4996-a667-10b37eb7f9d4.png)
Jika berhasil maka IP yang muncul adalah IP Skypie

### 4. Buat juga reverse domain untuk domain utama

-   Edit file /etc/bind/named.conf.local menjadi sebagai berikut :
    `vim /etc/bind/named.conf.local`
    ![image](https://user-images.githubusercontent.com/70801807/139530555-995a9da0-fbbd-4ec7-8f5b-0dceec639c34.png)
-   Selanjutnya Copykan file db.local pada path /etc/bind ke dalam folder kaizoku yang sudah dibuat dan ubah namanya menjadi 2.30.10.in-addr.arpa
-   Edit file 2.30.10.in-addr.arpa menjadi seperti gambar di bawah ini:
    ![image](https://user-images.githubusercontent.com/70801807/139530638-0affd900-d2fb-4c7b-8c0d-544e14747d93.png)
-   Kemudian restart bind9 dengan command :
    `service bind9 restart`
    -Untuk mengecek apakah konfigurasi sudah benar atau belum, lakukan perintah berikut pada client Loguetown

```
// Install package dnsutils
// Pastikan nameserver di /etc/resolv.conf telah dikembalikan sama dengan nameserver dari Foosha
apt-get update
apt-get install dnsutils
```

-   Selanjutnya pada Loguetown lakukan command di bawah ini:

```
//Kembalikan nameserver agar tersambung dengan EniesLobby
host -t PTR 10.30.2.2
```

![image](https://user-images.githubusercontent.com/70801807/139530772-e2907e26-e2a2-4d8e-9615-07ea72fdae2a.png)

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

<<<<<<< HEAD

### 15. Memberi autentikasi pada www.general.mecha.franky.c08.com dengan username luffy dan password onepiece

=======

### 15. Memberi autentikasi pada www.general.mecha.franky.e02.com dengan username luffy dan password onepiece

Untuk memberi authentication pada web ini, tim E02 perlu membuat suatu file `/etc/apache2/.htpasswd-general-mecha-franky`. File ini dibuat dengan melakukan run command `htpasswd -c /etc/apache2/.htpasswd-general-mecha-franky luffy`. Setelah command tersebut di-run, maka terbentuklah sebuah file yang berisi username:luffy dan password:onepiece yang telah di-hash.

![image](https://user-images.githubusercontent.com/8071604/139531831-6a2e85b6-61ed-4a9b-af95-618e269864f1.png)

Setelah file ini dibentuk, modifikasi konfigurasi apache `/etc/apache2/general.mecha.franky.e02.com-port15000.conf` dan `/etc/apache2/general.mecha.franky.e02.com-port15500.conf`. Tim E02 menambahkan kode berikut ini untuk mengimplementasikan authentication pada kedua web ini.

```bash
<Directory "/var/www/genera.mecha.franky.e02">
  AuthType Basic
  AuthName "Restricted Content"
  AuthUserFile /etc/apache2/.htpasswd-general-mecha-franky
  Require valid-user
</Directory>
```

![image](https://user-images.githubusercontent.com/8071604/139531876-36840b31-13b5-4716-8387-1c5b815cd5f4.png)

> > > > > > > b1712a53599897ccc29f08bc78fa264106ce781e

### 16. Setiap kali mengakses IP Skypie akan dialihkan secara otomatis ke www.franky.c08.com

### 17. Mengganti request gambar yang memiliki substring “franky” akan diarahkan menuju franky.png ketika mengakses www.super.franky.c08.com
