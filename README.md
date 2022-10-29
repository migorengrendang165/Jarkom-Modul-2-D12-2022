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
Berikut merupakan topologi yang telah dibuat 
