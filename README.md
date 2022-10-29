# Jarkom-Modul-2-D12-2022
Praktikum Jaringan Komputer Modul 2 (DNS &amp; Web Server) 2022.

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

### Soal 2
Untuk mempermudah mendapatkan informasi mengenai misi dari Handler, bantulah Loid membuat website utama dengan akses wise.yyy.com dengan alias www.wise.yyy.com pada folder wise.

#### Jawaban :
Dengan menambahkan IP NAT1 yaitu `192.168.122.1` sebagai nameserver, setiap host dapat mengakses internet. Sebagai contoh, dilakukan perintah `ping www.google.com`.

### Soal 3
Setelah itu ia juga ingin membuat subdomain eden.wise.yyy.com dengan alias www.eden.wise.yyy.com yang diatur DNS-nya di WISE dan mengarah ke Eden.

#### Jawaban :
Dengan menambahkan IP NAT1 yaitu `192.168.122.1` sebagai nameserver, setiap host dapat mengakses internet. Sebagai contoh, dilakukan perintah `ping www.google.com`.

### Soal 4
Buat juga reverse domain untuk domain utama.

#### Jawaban :
Dengan menambahkan IP NAT1 yaitu `192.168.122.1` sebagai nameserver, setiap host dapat mengakses internet. Sebagai contoh, dilakukan perintah `ping www.google.com`.

### Soal 5
Agar dapat tetap dihubungi jika server WISE bermasalah, buatlah juga Berlint sebagai DNS Slave untuk domain utama.

#### Jawaban :
Dengan menambahkan IP NAT1 yaitu `192.168.122.1` sebagai nameserver, setiap host dapat mengakses internet. Sebagai contoh, dilakukan perintah `ping www.google.com`.

### Soal 6
Karena banyak informasi dari Handler, buatlah subdomain yang khusus untuk operation yaitu operation.wise.yyy.com dengan alias www.operation.wise.yyy.com yang didelegasikan dari WISE ke Berlint dengan IP menuju ke Eden dalam folder operation.

#### Jawaban :
Dengan menambahkan IP NAT1 yaitu `192.168.122.1` sebagai nameserver, setiap host dapat mengakses internet. Sebagai contoh, dilakukan perintah `ping www.google.com`.

### Soal 7
Untuk informasi yang lebih spesifik mengenai Operation Strix, buatlah subdomain melalui Berlint dengan akses strix.operation.wise.yyy.com dengan alias www.strix.operation.wise.yyy.com yang mengarah ke Eden.

#### Jawaban :
Dengan menambahkan IP NAT1 yaitu `192.168.122.1` sebagai nameserver, setiap host dapat mengakses internet. Sebagai contoh, dilakukan perintah `ping www.google.com`.

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
	![]([pic/wise webserver.png](https://github.com/migorengrendang165/Jarkom-Modul-2-D12-2022/blob/main/SS%20Modul%201/wise%20webserver.png))
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
	kemudian bisa mengambil resource soal wise dengan wget dan unzip dan dimasukkan ke dalam directory /var/www/www.wise.D12.com
	![]([pic/wise webserver2.png](https://github.com/migorengrendang165/Jarkom-Modul-2-D12-2022/blob/main/SS%20Modul%201/wise%20webserver2.png))
2. 
