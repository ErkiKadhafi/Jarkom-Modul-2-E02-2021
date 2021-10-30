# Jarkom-Modul-2-E02-2021

Berikut ini adalah laporan resmi dari Praktikum Jaringan Komputer Modul 1 tahun 2021 di Institut Teknologi Sepuluh Nopember

Dokumen ini ditulis oleh
* 05111940000063 - Ryan Garnet Andrianto
* 05111940000050 - Erki Kadhafi Rosyid
* 05111940000141 - M. Farhan Haykal

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
- Edit file /etc/bind/named.conf.local menjadi sebagai berikut :
`vim /etc/bind/named.conf.local`
![image](https://user-images.githubusercontent.com/70801807/139530555-995a9da0-fbbd-4ec7-8f5b-0dceec639c34.png)
- Selanjutnya Copykan file db.local pada path /etc/bind ke dalam folder kaizoku yang sudah dibuat dan ubah namanya menjadi 2.30.10.in-addr.arpa
- Edit file 2.30.10.in-addr.arpa menjadi seperti gambar di bawah ini:
![image](https://user-images.githubusercontent.com/70801807/139530638-0affd900-d2fb-4c7b-8c0d-544e14747d93.png)
- Kemudian restart bind9 dengan command :
`service bind9 restart`
-Untuk mengecek apakah konfigurasi sudah benar atau belum, lakukan perintah berikut pada client Loguetown
```
// Install package dnsutils
// Pastikan nameserver di /etc/resolv.conf telah dikembalikan sama dengan nameserver dari Foosha
apt-get update
apt-get install dnsutils
```
- Selanjutnya pada Loguetown lakukan command di bawah ini:
```
//Kembalikan nameserver agar tersambung dengan EniesLobby
host -t PTR 10.30.2.2
```
![image](https://user-images.githubusercontent.com/70801807/139530772-e2907e26-e2a2-4d8e-9615-07ea72fdae2a.png)

### 5. Buat Water7 sebagai DNS Slave untuk domain utama
## I. Konfigurasi pada Server EnniesLobby
- Edit file /etc/bind/named.conf.local dan sesuaikan dengan syntax berikut :
```
zone "e02.com" {
        type master;
        notify yes;
        also-notify { 10.30.2.3; }; // Masukan IP Water7 tanpa tanda petik
        allow-transfer { 10.30.2.3; }; // Masukan IP Water7 tanpa tanda petik
        file "/etc/bind/kaizoku/e02.com";
};
```
![image](https://user-images.githubusercontent.com/70801807/139530885-6ff985ec-2df8-44e7-832e-0ecc1a3771c3.png)
- Lakukan restart bind9
`service bind9 restart`
## II. Konfigurasi pada Server Water7
-Buka Water7 dan update package lists dengan menjalankan command:
`apt-get update`
-Selanjutnya lakukan instalasi aplikasi bind9 pada Water7 dengan perintah:
`apt-get install bind9 -y`
- Kemudian buka file /etc/bind/named.conf.local dan tambahkan syntax berikut:
```
zone "e02.com" {
    type slave;
    masters { 10.30.2.2; }; // Masukan IP EniesLobby tanpa tanda petik
    file "/var/lib/bind/e02.com";
};
```
![image](https://user-images.githubusercontent.com/70801807/139531026-1d367373-f881-4140-aa2e-f11df42c9082.png)

- Lakukan restart bind9
`service bind9 restart`
## III. Testing
- Pada server EniesLobby silahkan matikan service bind9
`service bind9 stop`
![image](https://user-images.githubusercontent.com/70801807/139531352-b4714a07-d6b1-4f1b-9ca7-31b368a6c6f2.png)

- Pada client Loguetown pastikan pengaturan nameserver mengarah ke IP EniesLobby dan IP Water7
![image](https://user-images.githubusercontent.com/70801807/139531290-6bed610d-2159-4d5f-90ce-e25be313a766.png)

- Lakukan ping ke jarkom2021.com pada client Loguetown. Jika ping berhasil maka konfigurasi DNS slave telah berhasil
![image](https://user-images.githubusercontent.com/70801807/139531375-cd8906df-10bb-4180-864d-f0858bc29f90.png)

### 6. Setelah itu terdapat subdomain mecha.franky.c08.com dengan alias www.mecha.franky.e02.com yang didelegasikan dari EniesLobby ke Water7 dengan IP menuju ke Skypie dalam folder sunnygo
## I. Konfigurasi Pada Server EniesLobby
- Pada EniesLobby, edit file /etc/bind/kaizoku/e02.com dan ubah menjadi seperti di bawah ini sesuai dengan pembagian IP EniesLobby kelompok masing-masing.
`vim /etc/bind/kaizoku/e02.com` 
![image](https://user-images.githubusercontent.com/70801807/139531445-af5b376b-c0b1-4ca6-adc1-5a84b90046d1.png)

-  Kemudian edit file /etc/bind/named.conf.options pada EniesLobby.
`vim /etc/bind/named.conf.options`
- Kemudian comment dnssec-validation auto; dan tambahkan baris berikut pada /etc/bind/named.conf.options
`allow-query{any;};`
![image](https://user-images.githubusercontent.com/70801807/139531498-55b09f44-c3af-4594-97fd-feff4eea9a18.png)
- Setelah itu restart bind9
`servce bind9 restart`
## II. Konfigurasi pada Water7
-  Kemudian edit file /etc/bind/named.conf.options pada Water7.
`vim /etc/bind/named.conf.options`
- Kemudian comment dnssec-validation auto; dan tambahkan baris berikut pada /etc/bind/named.conf.options
`allow-query{any;};`
![image](https://user-images.githubusercontent.com/70801807/139531498-55b09f44-c3af-4594-97fd-feff4eea9a18.png)
- Lalu edit file /etc/bind/named.conf.local menjadi seperti gambar di bawah:
![image](https://user-images.githubusercontent.com/70801807/139531606-0da71698-4d56-4a25-a3a7-5ad408bd24aa.png)
- Kemudian buat direktori dengan nama sunnygo
`mkdir /etc/bind/sunnygo`
- Copy db.local ke direktori sunnygo dan edit namanya menjadi mecha.franky.e02.com
`cp /etc/bind/db.local /etc/bind/delegasi/mecha.franky.e02.com`
- Kemudian edit file mecha.franky.e02.com menjadi seperti dibawah ini
![image](https://user-images.githubusercontent.com/70801807/139531675-6b0d087f-119d-42d4-83f0-83b0ddeeb481.png)
- Restart bind9
`service bind9 restart`
## III. Testing
- Lakukan ping ke domain mecha.franky.e02.com dari client Loguetown
![image](https://user-images.githubusercontent.com/70801807/139531728-ce5d64f5-2a4c-447f-8cbd-a9c8098621d7.png)
Jika berhasil maka IP Skypie akan terlihat.
### 7. Buatkan subdomain melalui Water7 dengan nama general.mecha.franky.c08.com dengan alias www.general.mecha.franky.c08.com yang mengarah ke Skypie

### 9. Mengubah url www.franky.c08.com/index.php/home dapat menjadi menjadi www.franky.c08.com/home

### 10. Konfigurasi subdomain www.super.franky.c08.com Setelah itu, pada subdomain www.super.franky.c08.com, Luffy membutuhkan penyimpanan aset yang memiliki DocumentRoot pada /var/www/super.franky.c08.com.

### 11. Akan tetapi, pada folder /public hanya dapat melakukan directory listing saja.

### 12. Mengganti error default dari apache menjadi error file 404.html pada folder/error.

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

Secara default, port web adalah 80. Konfigurasi web `general.mecha.franky.e02.com` berada di `/etc/apache2/genera.mecha.franky.e02.com.conf`. Untuk membuat website ini dapat diakses melalui port 15000 atau port 15500, tim E02 menggandakan konfigurasi ini menjadi dua konfigurasi: `/etc/apache2/general.mecha.franky.e02.com-port15000.conf` dan `/etc/apache2/general.mecha.franky.e02.com-port15500.conf`. Isi dari kedua konfigurasi ini adalah sama kecuali nomor port yang dimilikinya.  Port bisa diubah melalui `<VirtualHost *:PORT>` yang tertulis di konfigurasi web apache.

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
