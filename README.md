# Jarkom-Modul-2-D12-2022
Praktikum Jaringan Komputer Modul 2 (DNS &amp; Web Server) 2022.

## Anggota Kelompok:

| Nama                           | NRP        | Nomor Yang dikerjakan |
| ------------------------------ | ---------- | --------------------- |
| Alexander 			 | 5025201247 | 1 - 7                 |
| Hafizh Mufid Darussalam        | 5025201093 | 14 - 17               |
| Januar Evan Zuriel Banjarnahor | 5025201210 | 8 - 13 		      |

### Topologi

<img src="https://github.com/migorengrendang165/Jarkom-Modul-2-D12-2022/blob/main/SS%20Modul%201/topologi.png?raw=true" width="600" height="400">

### Konfigurasi

- Ostania
```
auto eth0
iface eth0 inet dhcp

auto eth1
iface eth1 inet static
	address 10.21.1.1
	netmask 255.255.255.0

auto eth2
iface eth2 inet static
	address 10.21.2.1
	netmask 255.255.255.0

auto eth3
iface eth3 inet static
	address 10.21.3.1
	netmask 255.255.255.0
```

- SSS
```
auto eth0
iface eth0 inet static
	address 10.21.1.2
	netmask 255.255.255.0
	gateway 10.21.1.1
```

- Garden
```
auto eth0
iface eth0 inet static
	address 10.21.1.3
	netmask 255.255.255.0
	gateway 10.21.1.1
```

- WISE
```
auto eth0
iface eth0 inet static
	address 10.21.2.2
	netmask 255.255.255.0
	gateway 10.21.2.1
```

- Berlint
```
auto eth0
iface eth0 inet static
	address 10.21.3.2
	netmask 255.255.255.0
	gateway 10.21.3.1
```

- Eden
```
auto eth0
iface eth0 inet static
	address 10.21.3.3
	netmask 255.255.255.0
	gateway 10.21.3.1
```

### Soal 1
WISE akan dijadikan sebagai DNS Master, Berlint akan dijadikan DNS Slave, dan Eden akan digunakan sebagai Web Server. Terdapat 2 Client yaitu SSS, dan Garden. Semua node terhubung pada router Ostania, sehingga dapat mengakses internet.

#### Jawaban :
Dengan menambahkan IP NAT1 yaitu `192.168.122.1` sebagai nameserver, setiap host dapat mengakses internet. Sebagai contoh, dilakukan perintah `ping google.com`.

<img src="https://github.com/migorengrendang165/Jarkom-Modul-2-D12-2022/blob/main/SS%20Modul%201/ping_google.png?raw=true" width="800" height="400">

### Soal 2
Untuk mempermudah mendapatkan informasi mengenai misi dari Handler, bantulah Loid membuat website utama dengan akses wise.yyy.com dengan alias www.wise.yyy.com pada folder wise.

#### Jawaban :
Dibuat terlebih dahulu zone untuk `wise.d12.com`

```
zone "wise.D12.com" {
    type master;
    notify yes;
    also-notify { 10.21.3.2; }; // Masukan IP Water7 tanpa tanda petik
    allow-transfer { 10.21.3.2; }; // Masukan IP Water7 tanpa tanda petik
    file "/etc/bind/wise/wise.D12.com";
};
```

Kemudian file konfigurasinya yang berisi 

```
;
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     wise.D12.com. root.wise.D12.com. (
                        2022102502      ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@               IN      NS      wise.D12.com.
@               IN      A       10.21.2.2
www             IN      CNAME   wise.D12.com.
```

Hasil ping adalah sebagai berikut :

<img src="https://github.com/migorengrendang165/Jarkom-Modul-2-D12-2022/blob/main/SS%20Modul%201/ping_wise_new.png?raw=true" width="800" height="200">


### Soal 3
Setelah itu ia juga ingin membuat subdomain eden.wise.yyy.com dengan alias www.eden.wise.yyy.com yang diatur DNS-nya di WISE dan mengarah ke Eden.

#### Jawaban :
Melanjutkan konfigurasi pada nomor 2 :
```
;
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     wise.D12.com. root.wise.D12.com. (
                        2022102502      ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@               IN      NS      wise.D12.com.
@               IN      A       10.21.2.2
eden            IN      A       10.21.3.3
www             IN      CNAME   wise.D12.com.
www.eden        IN      CNAME   eden.wise.d12.com.
```

Hasil ping adalah sebagai berikut :

<img src="https://github.com/migorengrendang165/Jarkom-Modul-2-D12-2022/blob/main/SS%20Modul%201/ping_eden.png?raw=true" width="800" height="200">

### Soal 4
Buat juga reverse domain untuk domain utama.

#### Jawaban :
Dibuat konfigurasi baru sebagai berikut :
```
;
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     wise.D12.com. root.wise.D12.com. (
                        2022102401      ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
2.21.10.in-addr.arpa.   IN      NS      wise.D12.com.
2                       IN      PTR     wise.D12.com.
```

Hasil pemeriksaan :

<img src="https://github.com/migorengrendang165/Jarkom-Modul-2-D12-2022/blob/main/SS%20Modul%201/ptr.png?raw=true" width="400" height="50">

### Soal 5
Agar dapat tetap dihubungi jika server WISE bermasalah, buatlah juga Berlint sebagai DNS Slave untuk domain utama.

#### Jawaban :
Diatur konfigurasi pada zone di WISE sebagai berikut :

```
zone "wise.D12.com" {
    type master;
    notify yes;
    also-notify { 10.21.3.2; }; // Masukan IP Water7 tanpa tanda petik
    allow-transfer { 10.21.3.2; }; // Masukan IP Water7 tanpa tanda petik
    file "/etc/bind/wise/wise.D12.com";
};
```

Dan konfigurasi zone di Berlint sebagai berikut :
```
zone "wise.D12.com" {
    type slave;
    masters { 10.21.2.2; };
    file "/var/lib/bind/wise.D12.com";
};
```

### Soal 6
Karena banyak informasi dari Handler, buatlah subdomain yang khusus untuk operation yaitu operation.wise.yyy.com dengan alias www.operation.wise.yyy.com yang didelegasikan dari WISE ke Berlint dengan IP menuju ke Eden dalam folder operation.

#### Jawaban :
Ditambahkan konfigurasi sebagai berikut pada WISE:

```
;
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     wise.D12.com. root.wise.D12.com. (
                        2022102502      ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@               IN      NS      wise.D12.com.
@               IN      A       10.21.2.2
eden            IN      A       10.21.3.3
www             IN      CNAME   wise.D12.com.
www.eden        IN      CNAME   eden.wise.d12.com.
ns1             IN      A       10.21.3.3
operation       IN      NS      ns1
```

Serta konfigurasi berikut pada Eden :
```
;
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     operation.wise.D12.com. root.operation.wise.D12.com. (
                        2022102501      ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      operation.wise.D12.com.
@       IN      A       10.21.3.3
www     IN      A       10.21.3.3
```

Dan juga menambahkan line berikut ke dalam `named.conf.options` :
```
//dnssec-validation auto;
allow-query{any;};
```

Hasil ping adalah sebagai berikut :

<img src="https://github.com/migorengrendang165/Jarkom-Modul-2-D12-2022/blob/main/SS%20Modul%201/ping_operation.png?raw=true" width="800" height="200">

### Soal 7
Untuk informasi yang lebih spesifik mengenai Operation Strix, buatlah subdomain melalui Berlint dengan akses strix.operation.wise.yyy.com dengan alias www.strix.operation.wise.yyy.com yang mengarah ke Eden.

#### Jawaban :

Ditambahkan konfigurasi berikut pada Berlint :
```
;
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     operation.wise.D12.com. root.operation.wise.D12.com. (
                        2022102501      ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@               IN      NS      operation.wise.D12.com.
@               IN      A       10.21.3.3
www             IN      CNAME   operation.wise.d12.com.
strix           IN      A       10.21.3.3
www.strix       IN      CNAME   strix.operation.wise.D12.com.
```

Dengan hasil ping sebagai berikut :

<img src="https://github.com/migorengrendang165/Jarkom-Modul-2-D12-2022/blob/main/SS%20Modul%201/ping_strix.png?raw=true" width="800" height="200">

## 8 - 13
Setting webserver untuk domain tersebut sesuai dengan address dari host yang dituju akan dipasang apache (Eden, Berlint, dan Wise)
```
apt-get install apache2
```
dan php 
```
apt-get install php
``` 
dan 
```
apt-get install libapache2-mod-php7.0
```	

1. Untuk domain www.wise.D12.com
	
	![](https://github.com/migorengrendang165/Jarkom-Modul-2-D12-2022/blob/main/SS%20Modul%201/wise%20webserver.png)
	
	```
	Alias "/home" "/var/www/wise.D12.com/index.php/home
	```
	adalah untuk mengubah routing dari www.wise.D12.com/index.php/home menjadi www.wise.D12.com/home
	
	kemudian 
	```
	a2ensite wise.D12.com
	```
	
	dan lakukan restart apache
	```
	service apache2 restart
	```
	kemudian bisa mengambil resource soal wise dengan wget dan unzip dan dimasukkan ke dalam directory `/var/www/www.wise.D12.com`
	
	![](https://github.com/migorengrendang165/Jarkom-Modul-2-D12-2022/blob/main/SS%20Modul%201/wise%20webserver2.png)
	
	cek dengan `lynx www.wise.D12.com` untuk di host garden atau SSS
	
	![](https://github.com/migorengrendang165/Jarkom-Modul-2-D12-2022/blob/main/SS%20Modul%201/lynxwise.png)
	
	`lynx www.wise.D12.com/index.php/home`
	
	![](https://github.com/migorengrendang165/Jarkom-Modul-2-D12-2022/blob/main/SS%20Modul%201/lynxwise.png)
	
	`lynx www.wise.D12.com/home`
	
	![](https://github.com/migorengrendang165/Jarkom-Modul-2-D12-2022/blob/main/SS%20Modul%201/lynxwise.png)

2.  www.eden.wise.D12.com

	![](https://github.com/migorengrendang165/Jarkom-Modul-2-D12-2022/blob/main/SS%20Modul%201/edenwebserver.png)
	
	dilakukan directory listing dengan menambahkan pada `/etc/apache2/sites-available/eden.wise.D12.com.conf/`
	```
	<Directory /var/www/eden.wise.D12.com/public>
		Options +Indexes
	</Directory>
	<Directory /var/www/eden.wise.D12.com/error>
		Options +Indexes
	</Directory>
	```
	dan menambahkan untuk mengganti output 404 Not found
	```
	ErrorDocument 404 "/error/404.html"
	```
	kemudian 
	```
	a2ensite wise.D12.com
	```
	
	dan lakukan restart apache
	```
	service apache2 restart
	```
	kemudian bisa mengambil resource soal wise dengan wget dan unzip dan dimasukkan ke dalam directory `/var/www/www.eden.wise.D12.com`
	
	![](https://github.com/migorengrendang165/Jarkom-Modul-2-D12-2022/blob/main/SS%20Modul%201/edenwebserver2.png)
	
	Kemudian cek di dalam host garden atau SSS
	`lynx www.eden.wise.D12.com`
	
	![](https://github.com/migorengrendang165/Jarkom-Modul-2-D12-2022/blob/main/SS%20Modul%201/lynxeden.png)
	
	`lynx wwww.eden.wise.D12.com/public/js`
	
	![](https://github.com/migorengrendang165/Jarkom-Modul-2-D12-2022/blob/main/SS%20Modul%201/lynxedenpublic.png)
	
	`lynx www.eden.wise.D12.com/js`
	
	![](https://github.com/migorengrendang165/Jarkom-Modul-2-D12-2022/blob/main/SS%20Modul%201/lynxedenjs.png)
	
	untuk 404
	
	`lynx www.eden.wise.D12.com/dowijabajdpow`
	
	![](https://github.com/migorengrendang165/Jarkom-Modul-2-D12-2022/blob/main/SS%20Modul%201/lynx404.png)

## 14-15
Setting agar www.strix.operation.wise.D12.com hanya bisa diakses oleh port 15000 dan 15500, dengan autentikasi username Twilight dan password opStrix dan file di /var/www/strix.operation.wise.d12. Untuk itu, perlu menambahkan konfigurasi virtual host untuk port 15000 dan 15500.
```
echo '
<VirtualHost *:15000>
 <Directory /var/www/strix.operation.wise.D12.com>
  AuthType Basic
  AuthName "Restricted Content"
  AuthUserFile /var/www/strix.operation.wise.D12.com/.htpasswd
  Require valid-user
 </Directory>
 ServerAdmin webmaster@localhost
 DocumentRoot /var/www/strix.operation.wise.D12.com
 ServerName strix.operation.wise.D12.com
 ServerAlias www.strix.operation.wise.D12.com
 ErrorLog ${APACHE_LOG_DIR}/error.log
 CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
<VirtualHost *:15500>
 <Directory /var/www/strix.operation.wise.D12.com>
  AuthType Basic
  AuthName "Restricted Content"
  AuthUserFile /var/www/strix.operation.wise.D12.com/.htpasswd
  Require valid-user
 </Directory>
 ServerAdmin webmaster@localhost
 DocumentRoot /var/www/strix.operation.wise.D12.com
 ServerName strix.operation.wise.D12.com
 ServerAlias www.strix.operation.wise.D12.com
 ErrorLog ${APACHE_LOG_DIR}/error.log
 CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
' > /etc/apache2/sites-available/strix.operation.wise.D12.com.conf

a2ensite strix.operation.wise.D12.com
service2 apache2 reload
service2 apache2 restart

mkdir /var/www/strix.operation.wise.D12.com
wget --no-check-certificate 'https://docs.google.com/uc?export=download&id=1bgd3B6VtDtVv2ouqyM8wLyZGzK5C9maT' -O /etc/apache2/sites-available/wise.zip
unzip "/etc/apache2/sites-available/wise.zip
mv -T strix.operation.wise /var/www/strix.operation.wise.D12.com

echo '
Listen 80
Listen 15000
Listen 15500
' > /etc/apache2/ports.conf

a2enmod rewrite
service apache2 restart
```
Selanjutnya, setting untuk menambahkan username Twilight dan password opStrix
```
htpasswd -c -b /var/www/strix.operation.wise.D12.com/.htpasswd Twilight opStrix
service apache2 restart
```

cek dengan menuliskan 
`lynx www.strix.operation.wise.D12.com`
Tanpa auth (port 80):

![](https://github.com/migorengrendang165/Jarkom-Modul-2-D12-2022/blob/main/SS%20Modul%201/messageImage_1667058448546.jpg)

`lynx www.strix.operation.wise.D12.com:15000 atau 15500`
Dengan auth (port 15000 dan 15550):

![](https://github.com/migorengrendang165/Jarkom-Modul-2-D12-2022/blob/main/SS%20Modul%201/strixnotauth.png)

![](https://github.com/migorengrendang165/Jarkom-Modul-2-D12-2022/blob/main/SS%20Modul%201/messageImage_1667058418302.jpg)
