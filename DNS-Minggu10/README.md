## Laporan Praktikum Konfigurasi Topologi dan Mail [Kelompok 3]
* Lintang Arum Sari (3123600002)
* Kansa Adeneva Riti Syarani (3123600009)
* Salma Afifa Aziz (3123600017)
  
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

![Named Conf](https://github.com/lintangaroem/AdminJaringan2025/blob/main/DNS-Minggu10/named.conf.jpeg?raw=true)

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
   10.252.108.10
  10.10.10.1
};

allow-query { localhost; internal-network; };

allow-transfer { localhost; };
```
![Conf Options 1](https://github.com/lintangaroem/AdminJaringan2025/blob/main/DNS-Minggu10/conf%20options%201.jpeg?raw=true)

![Conf Options 2](https://github.com/lintangaroem/AdminJaringan2025/blob/main/DNS-Minggu10/conf%20options%202.jpeg?raw=true)

 
##### Cek konfigurasi file named
Lakukan perintah berikut:
```bash
named-checkconf
```

##### Cek zone file named
```bash
named-checkzone
```

##### Tambahkan file kelompok3.home pada /var/cache/bind
![Homepage Kelompok 3](https://github.com/lintangaroem/AdminJaringan2025/blob/main/DNS-Minggu10/kelompok3.home.jpeg?raw=true)

##### Cek konektifitas kelompok3.home
![Hasil Dig kelompok3.home](https://github.com/lintangaroem/AdminJaringan2025/blob/main/DNS-Minggu10/dig%20kelompok3.jpeg?raw=true)

#### D. WWW
##### Buat file html pada direktori /etc/apache2/
![Tampilan Webmail](https://github.com/lintangaroem/AdminJaringan2025/blob/main/DNS-Minggu10/mail%20html.jpeg?raw=true)

Buat kode html untuk website kelompok3.home. Untuk mengaksesnya ketikkan `kelompok3.home` atau `192.168.3.10` pada browser.
![Web Kelompok 3](https://github.com/lintangaroem/AdminJaringan2025/blob/main/DNS-Minggu10/web%20k3.jpeg?raw=true)

Pada server yang sama, kita juga dapat melihat website dari kelompok lain:
![Web Kelompok 2](https://github.com/lintangaroem/AdminJaringan2025/blob/main/DNS-Minggu10/web%20k2.jpeg?raw=true)

![Web Kelompok 5](https://github.com/lintangaroem/AdminJaringan2025/blob/main/DNS-Minggu10/web%20k5.jpeg?raw=true)

![Web Kelompok 6](https://github.com/lintangaroem/AdminJaringan2025/blob/main/DNS-Minggu10/web%20k6.jpeg?raw=true)

![Web Kelompok 8](https://github.com/lintangaroem/AdminJaringan2025/blob/main/DNS-Minggu10/web%20k8.jpeg?raw=true)

![Web Kelompok 10](https://github.com/lintangaroem/AdminJaringan2025/blob/main/DNS-Minggu10/web%20k10.jpeg?raw=true)
