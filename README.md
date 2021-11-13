# Jarkom-Modul-3-T10-2021

PRAKTIKUM MODUL 3 JARINGAN KOMPUTER 2021

Anggota Kelompok T10:<br>

1. Tri Rizki Yuliawan (05311940000024) <br>
2. Kevin Nathaniel (05311940000032) <br>
3. I Gde Ardha Semaranatha Gunasatwika (05311940000034) <br>

# Soal <a name="Soal"></a>

### 1. Luffy bersama Zoro berencana membuat peta tersebut dengan kriteria EniesLobby sebagai DNS Server, Jipangu sebagai DHCP Server, Water7 sebagai Proxy Server

Menambahkan command pada file bernama `.bashrc` pada folder root dengan command <br>

**Water 7**

```
apt-get update -y
apt-get install nano -y
apt-get install squid -y
apt-get install apache2 -y
apt-get install apache2-utils -y
bash /root/script.sh
```

**EnniesLobby 7**

```
apt-get update -y
apt-get install nano -y
apt-get install bind9 -y
bash /root/script.sh
```

**Jipangu**

```
apt-get update -y
apt-get install nano -y
apt-get install isc-dhcp-server -y
bash /root/script.sh
```

**Skypie**

```
apt-get update -y
apt-get install nano -y
apt-get install apache2 -y
apt-get install php -y
apt-get install libapache2-mod-php7.0
apt-get install wget -y
apt-get install unzip -y
bash /root/script.sh
```

**TottoLand**

```
apt-get update -y
apt-get install nano -y
bash /root/script.sh
```

**Alabasta**

```
apt-get update -y
apt-get install nano -y
apt-get install lynx -y
bash /root/script.sh
```

**Loguetown**

```
apt-get update -y
apt-get install nano -y
apt-get install lynx -y
bash /root/script.sh
```

Dokumentasi hasil

<img src="https://github.com/ardha27/Jarkom-Modul-3-T10-2021/blob/main/SS%20Hasil/1a.png">
<img src="https://github.com/ardha27/Jarkom-Modul-3-T10-2021/blob/main/SS%20Hasil/1b.png">

### 2. dan Foosha sebagai DHCP Relay

Menambahkan command pada file bernama `.bashrc` pada folder root dengan command <br>

```
iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE -s 192.216.0.0/16
apt-get update -y
apt-get install nano -y
apt-get install isc-dhcp-relay -y
```

Setelah itu kita mengedit isi file pada path `/etc/sysctl.conf` dengan command

```
net.ipv4.ip_forward=1
```

dan juga pada file `/etc/default/isc-dhcp-relay`

```
SERVERS="192.216.2.4"
INTERFACES="eth1 eth2 eth3"
```

Dokumentasi hasil
<img src="https://github.com/ardha27/Jarkom-Modul-3-T10-2021/blob/main/SS%20Hasil/2.png">

### 3. Semua client yang ada HARUS menggunakan konfigurasi IP dari DHCP Server. Client yang melalui Switch1 mendapatkan range IP dari [prefix IP].1.20 - [prefix IP].1.99 dan [prefix IP].1.150 - [prefix IP].1.169

mengkonfigurasi file `/etc/default/isc-dhcp-server` dengan command

```
INTERFACES="eth0"
```

setelah itu pada file `/etc/dhcp/dhcpd.conf` juga diubah konfigurasinya dengan command

```
subnet '192.216.2.0' netmask '255.255.255.248' {
}

subnet '192.216.1.0' netmask '255.255.255.0' {
    range '192.216.1.20' '192.216.1.99';
    range '192.216.1.150' '192.216.1.169';
    option routers '192.216.1.1';
    option broadcast-address '192.216.1.255';
    option domain-name-servers '192.216.2.2';
    default-lease-time '360';
    max-lease-time '7200';
}
```

Dokumentasi hasil
<img src="https://github.com/ardha27/Jarkom-Modul-3-T10-2021/blob/main/SS%20Hasil/3a.png">
<img src="https://github.com/ardha27/Jarkom-Modul-3-T10-2021/blob/main/SS%20Hasil/3b.png">

### 4. Client yang melalui Switch3 mendapatkan range IP dari [prefix IP].3.30 - [prefix IP].3.50

Pada file `/etc/dhcp/dhcpd.conf` juga diubah konfigurasinya dengan command

```
subnet '192.216.3.0' netmask '255.255.255.0' {
    range '192.216.3.30' '192.216.3.50';
    option routers '192.216.3.1';
    option broadcast-address '192.216.1.255';
    option domain-name-servers '192.216.2.2';
    default-lease-time '720';
    max-lease-time '7200';
}
```

Dokumentasi hasil
<img src="https://github.com/ardha27/Jarkom-Modul-3-T10-2021/blob/main/SS%20Hasil/4a.png">
<img src="https://github.com/ardha27/Jarkom-Modul-3-T10-2021/blob/main/SS%20Hasil/4b.png">

### 5. Client mendapatkan DNS dari EniesLobby dan client dapat terhubung dengan internet melalui DNS tersebut.

Telah diselesaikan pada solusi no 3 dan 4 yaitu pada `option domain-name-servers '192.216.2.2'`

Dokumentasi hasil
<img src="https://github.com/ardha27/Jarkom-Modul-3-T10-2021/blob/main/SS%20Hasil/5a.png">
<img src="https://github.com/ardha27/Jarkom-Modul-3-T10-2021/blob/main/SS%20Hasil/5b.png">

### 6. Lama waktu DHCP server meminjamkan alamat IP kepada Client yang melalui Switch1 selama 6 menit sedangkan pada client yang melalui Switch3 selama 12 menit. Dengan waktu maksimal yang dialokasikan untuk peminjaman alamat IP selama 120 menit.

Juga telah diselesaikan pada solusi no 3 dan 4 yaitu pada `default-lease-time dan max-lease-time`

### 7. Luffy dan Zoro berencana menjadikan Skypie sebagai server untuk jual beli kapal yang dimilikinya dengan alamat IP yang tetap dengan IP 192.216.3.69

Setting terlebih dahulu alamat ip address Skypie di DHCP server Jipangu agar alamatnya tetap dan tidak berubah , menggunakan `hardware ethernet 56:bf:61:e6:34:31` yang didapat dari koneksi antara node DHCP relay dan Skypie dan mendeklarasikan fixed addressnya `192.216.3.69`. Kemudian di Skypie akan diatur juga agar selalu memiliki alamat ip address yang sama.

**Server Jipangu**
```
host Skypie {
    hardware ethernet 56:bf:61:e6:34:31;
    fixed-address 192.216.3.69;
}
service isc-dhcp-server restart
```

**Server Skypie**
```
auto eth0
iface eth0 inet dhcp
hwaddress ether 56:bf:61:e6:34:31
```
<img src="https://github.com/ardha27/Jarkom-Modul-3-T10-2021/blob/main/SS%20Hasil/7.png">

### 8.  Loguetown digunakan sebagai client Proxy agar transaksi jual beli dapat terjamin keamanannya, juga untuk mencegah kebocoran data transaksi. Pada Loguetown, proxy harus bisa diakses dengan nama jualbelikapal.yyy.com dengan port yang digunakan adalah 5000

Sebelum mengatur squid.conf , terlebih dahulu kita akan membuat websitenya di DNS server EnniesLobby sesuai yang diajarkan di modul 2 , website akan mengarah ke IP Water7
***Ennies Lobby***
```
echo '
zone "jualbelikapal.t10.com" {
	type master;
	file "/etc/bind/jarkom/jualbelikapal.t10.com";
};
' > /etc/bind/named.conf.local

mkdir /etc/bind/jarkom
cp /etc/bind/db.local /etc/bind/jarkom/jualbelikapal.t10.com

echo '
$TTL    604800
@       IN      SOA     jualbelikapal.t10.com. root.jualbelikapal.t10.com.(
                     2021102501         ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      jualbelikapal.t10.com.
@       IN      A       192.216.2.3 ; IP Water7
' > /etc/bind/jarkom/jualbelikapal.t10.com

service bind9 restart
```

***Water7*** 
Pada water7 sebagai proxy server akan mengatur port yang digunakan sesuai dengan soal yaitu port `5000` yang diatur pada file squid.conf.bak
```
echo '
http_port 5000
visible_hostname Water7
' > /etc/squid/squid.conf

service squid restart
```

***Loguetown***
Loguetown sebagai client harus mendeklarasikan terlebih dahulu website yang akan digunakan menggunakan proxy dari water7 dengan menggunakan command
```
export http_proxy="http://jualbelikapal.t10.com:5000"
```
Untuk mengexport website agar bisa diakses di klien Loguetown

<img src="https://github.com/ardha27/Jarkom-Modul-3-T10-2021/blob/main/SS%20Hasil/8a.png">

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
