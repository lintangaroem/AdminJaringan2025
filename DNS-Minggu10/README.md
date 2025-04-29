## Konfigurasi Topologi Lab C 307 [Kelompok 3]
### Topologi

### Langkah-Langkah Konfigurasi
#### A. Install BIND 
BIND (Berkeley Internet Name Domain) adalah software open source yang berfungsi sebagai server DNS untuk menerjemahkan nama doamin menjadi alamat IP di jaringan internet atau lokal. Untuk menginstall BIND, lakukan command berikut:
   
```bash
apt install bing9 bind9utils
```
![Gambar Kedua](https://github.com/lintangaroem/AdminJaringan2025/blob/main/DNS-Minggu10/2.jpeg?raw=true)

#### B. Wired Connection Settings
Ubah setting wired connection pada laptop yang menjadi server dengan IP static `192.168.3.10` dengan IP gateway `192.168.3.1`  

![DNS Internal Zone](https://github.com/lintangaroem/AdminJaringan2025/blob/main/DNS-Minggu10/1.jpeg?raw=true)

#### C. Konfigurasi BIND untuk internal network
##### Tambahkan file internal-zones pada /etc/bind/named.conf<br>
Untuk mendefinisikan zone DNS mana saja yang dapat diakses dari jaringan internal lakukan penambahan pada file named.conf dengan melakukan perintah berikut:

```bash
nano /etc/bind/named.conf
```

lalu tambahkan file `/etc/bind/named.conf.internal-zones`



##### Edit file /etc/bind/named.conf.options<br>
Tambahkan `acl internal-network` sebelum options seperti contoh berikut:
```bash
acl internal-network{
  192.168.3.0/24
};
```
Kemudian tambahkan forwarders, allow-query, dan allow-transfer:
```bash
forwarders{
  10.10.10.1
};

allow-query { localhost; internal-network; };

allow-transfer { localhost; };
```
 //image
 
##### Cek konfigurasi file named
Lakukan perintah berikut:
```bash
named-checkconf
```
//image

##### Cek zone file named
```bash
named-checkzone
```
//image

##### Tambahkan file kelompok3.home pada /var/cache/bind

##### Cek konektifitas kelompok3.home
   
