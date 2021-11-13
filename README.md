# Jarkom-Modul-3-T10-2021

PRAKTIKUM MODUL 3 JARINGAN KOMPUTER 2021

Anggota Kelompok T10:<br>

1. Tri Rizki Yuliawan (05311940000024) <br>
2. Kevin Nathaniel (05311940000032) <br>
3. I Gde Ardha Semaranatha Gunasatwika (05311940000034) <br>

# Soal <a name="Soal"></a>

### 9. Agar transaksi jual beli lebih aman dan pengguna website ada dua orang, proxy dipasang autentikasi user proxy dengan enkripsi MD5 dengan dua username, yaitu luffybelikapalyyy dengan password luffy_yyy dan zorobelikapalyyy dengan password zoro_yyy

**_Server Water7_** <br>
Membuat 2 buah username dan password pada `/etc/squid/passwd` dengan command

```
htpasswd -b -c /etc/squid/passwd luffybelikapalt10 luffy_t10
htpasswd -b /etc/squid/passwd zorobelikapalt10 zoro_t10
```

lalu menambahkan konfigurasi pada `/etc/squid/squid.conf`

```
auth_param basic program /usr/lib/squid/basic_ncsa_auth /etc/squid/passwd
auth_param basic children 5
auth_param basic realm Proxy
auth_param basic credentialsttl 2 hours
auth_param basic casesensitive on
acl USERS proxy_auth REQUIRED
http_access allow USERS
```

Dokumentasi hasil

<img src="https://github.com/ardha27/Jarkom-Modul-3-T10-2021/blob/main/SS%20Hasil/9a.png">
<img src="https://github.com/ardha27/Jarkom-Modul-3-T10-2021/blob/main/SS%20Hasil/9b.png">
<img src="https://github.com/ardha27/Jarkom-Modul-3-T10-2021/blob/main/SS%20Hasil/9c.png">

### 10. Transaksi jual beli tidak dilakukan setiap hari, oleh karena itu akses internet dibatasi hanya dapat diakses setiap hari Senin-Kamis pukul 07.00-11.00 dan setiap hari Selasa-Jum’at pukul 17.00-03.00 keesokan harinya (sampai Sabtu pukul 03.00)

**_Server Water7_** <br>
Membuat file baru pada `/etc/squid/acl.conf` dan mengisi konfigurasi berikut

```
acl AVAILABLE_WORKING1 time MTWH 07:00-11:00
acl AVAILABLE_WORKING2 time TWHF 17:00-23:59
acl AVAILABLE_WORKING3 time WHFA 00:00-03:00
```

lalu menambahkan konfigurasi pada `/etc/squid/squid.conf`

```
http_access allow USERS AVAILABLE_WORKING1
http_access allow USERS AVAILABLE_WORKING2
http_access allow USERS AVAILABLE_WORKING3
```

Dokumentasi hasil

<img src="https://github.com/ardha27/Jarkom-Modul-3-T10-2021/blob/main/SS%20Hasil/10a.png">
<img src="https://github.com/ardha27/Jarkom-Modul-3-T10-2021/blob/main/SS%20Hasil/10b.png">

### 11. Agar transaksi bisa lebih fokus berjalan, maka dilakukan redirect website agar mudah mengingat website transaksi jual beli kapal. Setiap mengakses google.com, akan diredirect menuju super.franky.yyy.com dengan website yang sama pada soal shift modul 2. Web server super.franky.yyy.com berada pada node Skypie

**_Server EniesLobby_** <br>
melakukan konfigurasi DNS pada EniesLobby `/etc/bind/named.conf.local` dan `/etc/bind/jarkom/franky.T10.com` yang akan diarahkan ke Skypie

```
zone "franky.T10.com" {
        type master;
        file "/etc/bind/jarkom/franky.T10.com";
};
```

```
$TTL    604800
@       IN      SOA     franky.T10.com. root.franky.T10.com.(
                     2021102501         ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      franky.T10.com.
@       IN      A       192.216.2.2 ;IP EniesLobby
super   IN      A       192.216.3.69 ;IP Skypie
```

**_Server Skypie_** <br>
Membuat web server pada Skypie `/var/www` dengan command

```
cd /var/www
wget --no-check-certificate https://raw.githubusercontent.com/FeinardSlim/Praktikum-Modul-2-Jarkom/main/super.franky.zip
unzip super.franky.zip
mkdir super.franky.T10.com
mv super.franky/* super.franky.T10.com
rm -r super.franky
rm -r super.franky.zip
```

Melakukan konfigurasi web server pada `super.franky.T10.com.conf`

```
<VirtualHost *:80>
        ServerAdmin webmaster@localhost

        DocumentRoot /var/www/super.franky.T10.com
        ServerName super.franky.T10.com
        ServerAlias www.super.franky.T10.com

        ErrorLog ${APACHE_LOG_DIR}/error.log
        CustomLog ${APACHE_LOG_DIR}/access.log combined

</VirtualHost>
```

**_Server Water7_** <br>
Melakukan konfigurasi pada Water7 `/etc/squid/squid.conf` agar dapat mendirect google menuju super.franky.yyy.com

```
acl myNetwork src 192.216.3.0/24 192.216.1.0/24
acl blksites dstdomain .google.com
deny_info http://super.franky.T10.com all

http_reply_access deny blksites all
```

Menambahkan command berikut pada `/etc/squid/squid.conf` agar dapat mengakses web server lokal

```
dns_nameservers 192.216.2.2
```

Dokumentasi Hasil
<img src="https://github.com/ardha27/Jarkom-Modul-3-T10-2021/blob/main/SS%20Hasil/11a.png">
<img src="https://github.com/ardha27/Jarkom-Modul-3-T10-2021/blob/main/SS%20Hasil/11b.png">

### 12. Saatnya berlayar! Luffy dan Zoro akhirnya memutuskan untuk berlayar untuk mencari harta karun di super.franky.yyy.com. Tugas pencarian dibagi menjadi dua misi, Luffy bertugas untuk mendapatkan gambar (.png, .jpg), sedangkan Zoro mendapatkan sisanya. Karena Luffy orangnya sangat teliti untuk mencari harta karun, ketika ia berhasil mendapatkan gambar, ia mendapatkan gambar dan melihatnya dengan kecepatan 10 kbps

Kelompok kami belum dapat menyelesaikan keseluruhan perintah dari persoalan ini.
Berikut konfigurasi yang kami berikan pada file `/etc/squid/squid.conf`

```
acl multimedia url_regex -i \.png$ \.jpg$
acl bar proxy_auth luffybelikapalt10
delay_pools 1
delay_class 1 1
delay_parameters 1 10000/3200
delay_access 1 allow multimedia bar
delay_access 1 deny ALL
http_access deny ALL
```

Dokumentasi Hasil

### 13. Sedangkan, Zoro yang sangat bersemangat untuk mencari harta karun, sehingga kecepatan kapal Zoro tidak dibatasi ketika sudah mendapatkan harta yang diinginkannya

Disini kelompok kami tidak melalukan konfigurasi lebih lanjut, karena secara default kecepatan user tidak akan dibatasi

Dokumentasi Hasil