# Jarkom-Modul-2-E02-2021

Berikut ini adalah laporan resmi dari Praktikum Jaringan Komputer Modul 1 tahun 2021 di Institut Teknologi Sepuluh Nopember

Dokumen ini ditulis oleh
* 05111940000063 - Ryan Garnet Andrianto
* 05111940000050 - Erki Kadhafi Rosyid
* 05111940000141 - M. Farhan Haykal

### 1. EniesLobby akan dijadikan sebagai DNS Master, Water7 akan dijadikan DNS Slave, dan Skypie akan digunakan sebagai Web Server. Terdapat 2 Client yaitu Loguetown, dan Alabasta. Semua node terhubung pada router Foosha, sehingga dapat mengakses internet

### 2. Buat website utama dengan mengakses franky.c08.com dengan alias www.franky.c08.com pada folder kaizoku

### 3. Buat subdomain super.franky.c08.com dengan alias www.super.franky.c08.com yang diatur DNS nya di EniesLobby dan mengarah ke Skypie

### 4. Buat juga reverse domain untuk domain utama

### 5. Buat Water7 sebagai DNS Slave untuk domain utama

### 6. Setelah itu terdapat subdomain mecha.franky.c08.com dengan alias www.mecha.franky.c08.com yang didelegasikan dari EniesLobby ke Water7 dengan IP menuju ke Skypie dalam folder sunnygo

FTP (File Transfer Protocol) adalah sebuah protokol yang digunakan untuk transfer file antara server dan client.
Secara _default_, port yang digunakan oleh server FTP adalah 21. Dengan demikian, tim E02 menggunakan filter `ftp && tcp.port == 21` untuk mencari paket yang dikirimkan melalui FTP.

![image](https://user-images.githubusercontent.com/8071604/134525121-2c3b615b-a7c8-4700-a03d-9e310f87c1a3.png)

Pada gambar di atas, client mengirimkan perintah `USER secretuser`. `secretuser` adalah username yang digunakan oleh komputer client sebagai _credentials_ untuk mengakses FTP server.

Selain username yang terekspos, password `aku.pengen.pw.aja` juga terekspos. Hal ini dikarenakan paket yang dikirim menuju ke FTP server tidak dienkripsi sehingga _packet sniffing_ dengan mudah terjadi. 

![image](https://user-images.githubusercontent.com/8071604/134525894-dfc232bb-f2a1-46e4-8521-27a2a5a7bbea.png)


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
