## DNS: IP Forwarding
### Topologi
<p align="center">
  <img src="https://raw.githubusercontent.com/lintangaroem/AdminJaringan2025/main/DNS/img/1.jpeg" alt="Gambar DNS" />
</p>

Topologi ini menunjukkan dua VM yang terhubung lewat jaringan internal. VM 1 bertindak sebagai gateway dengan IP 192.168.200.1 dan terhubung ke internet melalui bridge adapter. VM 2 berperan sebagai client dengan IP 192.168.200.10/24, yang hanya terhubung ke VM 1. Agar VM 2 bisa mengakses internet, VM 1 harus mengaktifkan IP forwarding dan NAT. Singkatnya, semua koneksi dari VM 2 akan lewat VM 1 sebelum ke internet.

### VM 1 (Server) Configurations
#### 1. Install Bind9
Untuk install bind9 gunakan perintah berikut:
```bash
sudo apt install bind9 bind9utils
```
![Alt Text](https://raw.githubusercontent.com/lintangaroem/AdminJaringan2025/main/DNS/img/2.jpeg)


#### 2. Install iptables-persistent
```bash
sudo apt install iptables iptables-persistent
```
![Alt Text](https://raw.githubusercontent.com/lintangaroem/AdminJaringan2025/main/DNS/img/3.jpeg)

#### 3. Set IP Address
```bash
sudo nano /etc/network/interfaces
```
![Alt Text](https://raw.githubusercontent.com/lintangaroem/AdminJaringan2025/main/DNS/img/4.jpeg)

File konfigurasi yang ditampilkan adalah `/etc/network/interfaces`, yang mengatur pengaturan jaringan pada sistem Linux. Pada konfigurasi ini, terdapat dua interface utama, yaitu `enp0s3` dan `enp0s8`. Interface `enp0s3` berfungsi sebagai koneksi utama ke internet dan disetting menggunakan DHCP, sehingga IP address akan diberikan secara otomatis oleh server jaringan. Sementara itu, interface `enp0s8` digunakan untuk koneksi jaringan internal dan dikonfigurasi secara statis dengan alamat IP `192.168.200.1`, netmask `255.255.255.0`, dan DNS `1.1.1.1`. 

setelah mengonfirgurasi file tersebut, restart network agar perubahan pada /etc/network/interfaces dapat diterapkan.
```bash
sudo systemctl restart networking
```
Untuk mengecek apakah IP Address sudah berubah sesuai konfigurasi, lakukan perintah `ip a`

#### 4. DNS Configuration
Masuk ke file /etc/bind/named.conf.options dengan perintah berikut:
```bash
sudo nano /etc/bind/named.conf.options
```
![Alt Text](https://raw.githubusercontent.com/lintangaroem/AdminJaringan2025/main/DNS/img/5.jpeg)

Lakukan penambahan dan perubahan berikut:
a. Tambahkan internal-network di paling atas file, sebelum options
```bash
acl internal-network{
    192.168.200.0/24
};
```
b. Tambahkan query berikut pada function options
```bash
allow-query { localhost; internal-network; };
allow-transfer { localhost; };
forwarders {
  8.8.8.8;
  1.1.1.1;
};
recursion yes;
```
VM1 harus meneruskan lalu lintas dari VM2 ke internet. Pastikan forwarding IP diaktifkan dengan menjalankan:
```bash
echo 1 > /proc/sys/net/ipv4/ip_forward
```
Agar permanen, tambahkan di `/etc/sysctl.conf`:
```bash
net.ipv4.ip_forward=1
```
dengan uncomment baris yang berisi query tersebut.

Lalu jalankan:
```bash
sysctl -p
```

Karena VM 1 memiliki dua interface (enp0s3 ke internet dan enp0s8 ke VM2), kita perlu menerapkan NAT agar V 2 bisa keluar melalui VM 1 lakukan perintah berikut:
```bash
iptables -t nat -A POSTROUTING -o enp0s3 -j MASQUERADE
iptables -A FORWARD -i enp0s8 -o enp0s3 -j ACCEPT
iptables -A FORWARD -i enp0s3 -o enp0s8 -m state --state RELATED,ESTABLISHED -j ACCEPT
```
Agar iptables tetap berlaku setelah reboot, jalankan
```bash
apt install iptables-persistent
netfilter-persistent save
netfilter-persistent reload
```
#### 5. Setting kelompok3.com
Selanjutnya edit file /etc/bind/named.conf.local, tambahkan:
```bash
zone "kelompok3.com" {
    type master;
    file "/etc/bind/db.kelompok3";
};
```
![Alt Text](https://raw.githubusercontent.com/lintangaroem/AdminJaringan2025/main/DNS/img/6.jpeg)

Buat file /etc/bind/db.kelompok3 dengan perintah berikut:
```bash
sudo nano /etc/bind/db.kelompok3
```
Isi file tersebut dengan
```bash
$TTL    604800
@       IN      SOA     kelompok3.com. root.kelompok3.com. (
                              2         ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      kelompok3.com.
@       IN      A       192.168.100.1
```
![Alt Text](https://raw.githubusercontent.com/lintangaroem/AdminJaringan2025/main/DNS/img/7.jpeg)

Simpan, lalu jalankan validasi:
```bash
sudo named-checkzone kelompok3.com /etc/bind/db.kelompok3
sudo named-checkconf
```

Restart bind dengan
```bash
sudo systemctl restart bind9
```

Aktifkan interface jaringan 192.168.200.1/24 (di sini enp0s8)
```bash
ip a
sudo ip link set <nama_interface> up
```

### VM 2 (Client) Configurations
Ubah konfigurasi IP Static dari VM 2, dengan masuk ke Wired Settings > IPv4. Lakukan setting berikut:
![Alt Text](https://raw.githubusercontent.com/lintangaroem/AdminJaringan2025/main/DNS/img/9.jpeg)

- `IPv4 Method` : Manual
- `Addresses`
-   `Address` : 192.168.200.10
-   `Netmask` : 255.255.255.0
-   `Gateway` : 192.168.200.1
- `DNS` : 192.168.200.1, 1.1.1.1

Kemudian lakukan ping ke server kelompok3.com
![Alt Text](https://raw.githubusercontent.com/lintangaroem/AdminJaringan2025/main/DNS/img/10.jpeg)
