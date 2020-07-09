# Protokol DHCP dan DNS 

1. Konfigurasi antarmuka 
2. Membuat direktori 
3. Mengedit file konfigurasi part1 
4. Mengedit file konfigurasi part2 
5. Mengedit file konfigurasi di tempat kerja 
6. Mengkonfigurasi server HTTP Apache 
7. Memeriksa pengaturan server Apache 
8. Skrip sederhana interaksi dengan server HTTP 
9. Analisis permintaan http 
10. Analisis respons http 
11. Permintaan untuk halaman yang tidak ada 


--- 

#### 1. Mengkonfigurasi antarmuka

> HTTP (HyperText Transfer Protocol, Hypertext Transfer Protocol) - protokol transfer data (awalnya - dalam bentuk dokumen hypertext dalam format HTML, saat ini digunakan untuk mentransfer data secara bebas - teks biasa, hypertext (HTML), style sheet, skrip klien, gambar , dokumen dalam berbagai format, informasi biner, dll.).
> Saat ini, ini adalah salah satu protokol aplikasi yang paling banyak digunakan. 

````
ping -c 4 10.2.1.6
connect: Network is unreachable
sudo ifconfig eth1 192.168.106.20 netmask 255.255.255.0 up
sudo ip route add 10.2.1.0/24 via 192.168.106.1
```` 

> pada router workstation:
 
````
ip address add interface=ether2 address=192.168.106.1 netmask=255.255.255.0
ip route add dst-address=10.2.1.0/24 gateway=10.2.1.254
```` 


#### 2. Buat direktori 

> World Wide Web atau WWW - Sistem informasi terdistribusi global di mana dokumen hypertext dan data lainnya dihubungkan melalui tautan. Saat ini, ketika datang ke Internet, itu berarti web global. 
> Pada tahun 1990, Tim Berners-Lee mempresentasikan proyeknya, yang disebut WorldWideWeb. Ini adalah browser web pertama yang menyertakan editor. Selain browser web itu sendiri, server, protokol HTTP, HTML, dan konsep pengidentifikasi sumber daya unik URI dikembangkan.
> Gagasan utama dari teknologi baru ini adalah menciptakan sistem untuk menyimpan dan mendistribusikan dokumen, di mana semua dokumen dihubungkan melalui berbagai tautan. 
> Sambungkan ke konsol server aplikasi dengan mengklik ikon IPSrv. Jalankan skrip untuk membuat file yang berisi parameter dari server DNS. Menggunakan perintah: 

````
conf-dns.sh 6 
````
> Di konsol server aplikasi, jalankan skrip untuk membuat file yang berisi parameter dari server HTTP. Menggunakan perintah: 

````
conf-http.sh 6 
````

> Buat direktori www.xyz.ip.lab  di direktori /var/www/. Direktori ini akan berisi file yang terkait dengan situs yang sedang dibuat. Untuk membuat direktori, gunakan perintah: 
 
````
mkdir /var/www/www.xyz.ip.lab 
```` 

> Verifikasi bahwa direktori dibuat dengan benar menggunakan perintah: 

`` 
ls /var/www/ 
`` 

#### 3. Mengedit file konfigurasi bagian 1 20 

> File db.lab berisi deskripsi zona dns. Deskripsi ini mencakup informasi otorisasi catatan SOA, serta parameter tipe catatan SOA. Ini kemudian berisi catatan sumber daya yang berisi nama sumber daya, tipe catatan, dan data catatan sumber daya. 

> Rekam tipe A sesuai dengan konversi langsung. 

> Menggunakan konsol IPServer, tambahkan baris: www.xyz.ip.lab  ke akhir file /etc/bind/db.lab A 10.2.1.6 

```` 
echo "www.xyz.ip.lab A 10.2.1.6" >> /etc/bind/db.lab 
cat /etc/bind/db.ip.lab
````
 
#### 4. Mengedit file konfigurasi part2

> File db.1.2.10 berisi deskripsi zona DNS terbalik. Permintaan pengembalian adalah tipe PTR. 
> in-addr.arpa - nama domain yang menunjukkan bahwa permintaan balik sedang dibuat, mis. data sedang diminta untuk alamat. 
> Dalam hal ini, catatan sumber daya berisi nama relatif dari sumber daya, tergantung pada opsi di mana pekerjaan dilakukan. 
> Menggunakan konsol IPServer, tambahkan baris di akhir file /etc/bind/db.1.2.10: 6 PTR www.xyz.ip.lab  

````
echo"6 PTR www.xyz.ip.lab ">> /etc/bind/db.1.2.10 
root@IPSrv06: ~ # cat /etc/bind/db.1.2.10
```` 

#### 5. Mengedit file konfigurasi di tempat kerja

> Layanan sistem nama domain dibangun sesuai dengan skema "client-server". Bagian klien adalah proses aplikasi yang meminta informasi tentang korespondensi nama ke alamat (atau sebaliknya, alamat nama). Proses perangkat lunak ini disebut resolver. 

> Tugas utama resolver adalah menerima permintaan dari perangkat lunak aplikasi, memastikan interaksi dengan server DNS dan kembali, tergantung pada permintaan, baik alamat IP atau nama domain dari perangkat lunak aplikasi ini. 

> Menggunakan konsol workstation, tambahkan baris ke file /etc/resolv.conf: nameserver 10.2.1.6 

```` 
echo "nameserver 10.2.1.6"| sudo tee /etc/resolv.conf 
cat /etc/resolv.conf
```` 

#### 6. Mengkonfigurasi Apache HTTP Server
> Apache adalah server web yang berfungsi penuh dan dapat diperluas yang sepenuhnya mendukung protokol HTTP/1.1 dan bersifat open source. 
> Server dapat bekerja di hampir semua platform umum. Namun, sangat mudah untuk menginstal dan mengkonfigurasi. 
> Restart server DNS sehingga pengaturan baru mulai berlaku, lakukan perintah berikut pada Server: 

````
/etc/init.d/bind9 restart 
cat /etc/apache2/sites-available/www.xyz.ip.lab
```` 

__` Jawab:`__ 
`/var/www/www.db.lab` 


#### 7. Memeriksa pengaturan server Apache

> Apache membagi fungsionalitas dan komponennya menjadi beberapa bagian terpisah yang dapat dikonfigurasikan dan dikonfigurasikan secara terpisah satu sama lain. Bagian dasar, yang bertanggung jawab untuk situs atau domain terpisah, disebut host virtual. Sistem ini memungkinkan administrator untuk menggunakan satu server untuk mendistribusikan beberapa situs menggunakan satu antarmuka atau IP. 

> Untuk mengaktifkan host virtual (situs) yang dibuat, gunakan utilitas a2ensite. 

> Host (situs) terputus sama dengan utilitas a2dissite 

> Mulai server Apache2 pada IPServ menggunakan perintah: 

````
service apache2 start
````

> Lihatlah isi dari /etc/apache2/sites-enabled/ direktori, gunakan perintah:
 
````
ls /etc/apache2/situs-enabled/ 
```` 

Nyalakan situs dan periksa pengaturan yang dibuat menggunakan perintah berikut: 

````
a2ensite www.xyz.ip.lab 
apache2ctl configtest 
````

> Muat ulang pengaturan server Apache menggunakan perintah: 

````
services apache2 ulang 
````

> Restart server menggunakan perintah: 

````
layanan apache2 restart` 
````
> Lihatlah isi dari /etc/apache2/sites-enabled/ direktori untuk melihat apakah file dengan pengaturan yang diperlukan telah ditambahkan, penggunaan untuk ini, perintah: 

````
ls /etc/apache2/sites-enabled/ 
````

#### 8. Skrip sederhana untuk berinteraksi dengan server HTTP

> Curl - alat untuk mentransfer data (file) dari atau ke server menggunakan salah satu protokol yang didukung (DICT, FILE, FTP, FTPS, GOPHER, HTTP, HTTPS, IMAP, IMAPS, LDAP, LDAPS, POP3, POP3S, RTMP, RTSP, SCP, SFTP, SMTP, SMTPS, TELNET dan TFTP). 
> Sintaks untuk menulis URL tergantung pada protokol yang digunakan. Deskripsi terperinci ada di RFC 3986. 
> Kirim permintaan ke server HTTP dari workstation dengan menjalankan program curl dari konsol workstation: 

````
curl www.xyz.ip.lab
````

> Pada output konsol dari perintah, tentukan header-html dan masukkan sebagai respons: 

__`Jawaban: `__
`Success !` 


#### 9. Analisis permintaan http 

> Pesan selama koneksi HTTP dilakukan sesuai dengan skema respons permintaan yang biasa.

> Setiap pesan HTTP terdiri dari tiga bagian, yang ditransmisikan dalam urutan yang ditentukan: 

> Mulai baris - menentukan jenis pesan; 

> Judul - berbagai informasi bermanfaat yang menjadi ciri isi pesan, parameter transmisi dan informasi lainnya; 

> Isi pesan adalah kode html yang dihasilkan, yang kemudian akan ditampilkan oleh browser. Harus dipisahkan dari header dengan string kosong. 

> Contoh garis awal permintaan HTTP: 

> Metode - nama permintaan, satu kata dalam huruf kapital; 

> URI - menentukan jalur ke dokumen yang diminta; 

> Versi - nama protokol, simbol / dan sepasang angka Arab yang dipisahkan oleh tanda titik. Misalnya: HTTP/1.1 

> Jumlah paket pertama permintaan HTTP: 

__`Response:`__
` 8`

> Apa protokol transport yang digunakan untuk mengirim pesan HTTP (masukkan jawaban dalam format numerik): 

__` Balas:`__
`6`
> alamat IP dari server HTTP: 

__` Balas :`__ 
`10.2.1.6` 

> HTTP nomor port server: 

__` Jawaban: `__
` 80` 

> Jenis permintaan: 

__` Jawaban:`__
` GET` 

> versi protokol HTTP: 

__` Jawaban: `__
`HTTP / 1.1` 

> Informasi aplikasi klien: 

__` Jawaban: `__
`curl / 7.35.0` 

> Pengidentifikasi lengkap dari halaman yang diminta: 

__`Response:`__
`http://www.jtd.polinema.ac.id/` 


#### 10. Analisis respons HTTP 

> Contoh garis mulai HTTP -menjawab:

> Versi - nama protokol, simbol / dan sepasang angka Arab yang dipisahkan oleh titik; 

> Kode status - tiga digit. Kode status menentukan konten pesan dan perilaku klien selanjutnya; 

> Penjelasan - Penjelasan teks singkat tentang kode respons untuk pengguna. Tidak memengaruhi pesan dengan cara apa pun dan bersifat opsional 

> Analisis paket yang dicegat dan masukkan nomor paket pertama dari respons HTTP: 

__`Response: `__
`10` 

> Kode status pemrosesan permintaan http: 

__`Response: `__
` 200` 

> Nilai frasa respons: 

__`Jawaban: `__
`OK`

> Informasi server HTTP: 

__`Jawaban:`__
`Apache / 2.2.22 (Debian) ` 

> Jenis data yang dikirim dari server:

__` Balas: `__
`teks / html` 

> 

#### 11. Permintaan untuk halaman yang tidak ada 

> Kesalahan 404 - ini adalah kode yang diberikan situs sebagai tanggapan terhadap permintaan untuk halaman yang tidak ada. 

> Tidak masalah apa alasan halaman itu tidak ditemukan - tidak pernah ada atau dihapus - mesin pencari menerima kesalahan 404, yang berarti bahwa URL ini tidak dapat diindeks. 

> Setiap situs "benar" harus memiliki halaman khusus untuk menampilkan kesalahan ini. 

> Nomor paket permintaan HTTP pertama: 

__`Response:`__
`8`

> Jenis permintaan: 
__`Response: `__
`GET` 

> Identifier lengkap dari halaman yang diminta: 

__`Response: `__` 
http://www.xyz.ip.lab/1.html` 

> Jumlah paket pertama dari respons HTTP:

__`Response: `__
`10` 

> Status permintaan http: 

__`Response: `__
`404` 

> Nilai frasa respons: 

__`Response: `__
`Not Found`
