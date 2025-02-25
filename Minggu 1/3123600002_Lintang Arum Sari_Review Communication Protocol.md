## Review Materi Communication Protocol
### A. Eksplor http.cap di Wireshark
1. Download file http.cap pada https://wiki.wireshark.org/SampleCaptures
2. Buka file http.cap menggunakan wireshark, berikut merupakan tampilan awal wireshark:
   ![image](https://github.com/user-attachments/assets/b24b3c8f-a8ea-4f49-8dc2-e3bd97ae4218)
   
4. Lakukan analisis untuk:
    #### a. IP client dan IP server
   ![image](https://github.com/user-attachments/assets/edaa17b4-7760-47a2-8b1d-568919ca18c5)
   - IP client   : `145.245.160.237`
   - IP server   : `65.208.228.223`
   IP client dan server dapat diidentifikasi melalui port dari masing-masing IP. Destination port menunjukkan angka 80 yang berarti webbsite, hal ini menunjkkan bahwa destination IIP merupakan IP dari server. Sedangkan source port menunjukkan angka 3372, sehingga source IP merupakan IP dari client.

#### b. Versi HTTP
- Ketik http pada filter package
  
  ![image](https://github.com/user-attachments/assets/de5f15b4-09e0-4494-95c4-c4be76186f38)

- Versi HTTP terdapat pada kolom info
  
  ![image](https://github.com/user-attachments/assets/86ae6b67-9db9-460e-8a84-403ce7302996)
  
  HTTP/1.1 menunjukkan bahwa versi HTTP yang digunakan adalah versi 1.1

#### c. Waktu client mengirim request
- Klik Flow Graph pada menu Statistics
  
  ![image](https://github.com/user-attachments/assets/c38da539-2e66-4693-a0e8-fe7e0909b09e)

- Halaman flow graph akan tampil seperti pada gambar berikut:
  
  ![image](https://github.com/user-attachments/assets/60b74272-07ef-4e17-86f9-bfa678b8c4b0)

- Waktu client mengirim request
  
  ![image](https://github.com/user-attachments/assets/a4a24062-7716-4703-857f-9b1632ff3d24)
  ![image](https://github.com/user-attachments/assets/58553089-d4cf-4e4c-9bfe-7c9d69e07077)
  
  Sesuai pada halaman utama dan flow graph, client mengirim request ke server pada detik ke 0.911310

#### d. Waktu server menerima HTTP request dari client  
![image](https://github.com/user-attachments/assets/a0d2cd84-06cf-49fc-ac29-9e1ef7706ac0)
![image](https://github.com/user-attachments/assets/1ea0a064-74b0-4b90-ad5e-7f8166d5fb96)
- Waktu server menerima HTTP request dari client adalah pada detik ke 4.846969

#### e. Waktu yang dibutuhkan untuk transfer dan response dari client ke server
- Waktu yang dibutuhkan
  ```
  Waktu Response - Waktu Kirim = 4.846969 - 0.911310 = 3.935659 detik
  ```

### B. Analisis Communication Protocol
![image](https://github.com/user-attachments/assets/e71cf8b0-1ac1-4d8e-8690-a75c50074677)  

Gambar tersebut menggambarkan jenis-jenis pengiriman data dalam model komunikasi jaringan yang berkaitan dengan lapisan-lapisan dalam model OSI (Opens System Interconnection).
Berikut merupakan komponen yang terdapat pada gambar:
#### 1. Process
- Merupakan entitas yang melakukan komunikasi, seperti aplikasi atau layanan yang berjalan pada host.
#### 2. Node
- Merupakan perangkat dalam jaringan, seperti komputer, router, atau switch, yang dapat mengirim atau menerima data.
#### 3. Host
- Merupakan sistem akhir dalam jaringan, seperti komputer atau server, yang menjalankan proses dan berkomunikasi dengan host lain.

Berikut merupakan penjelasan dari setiap layer yang terdapat pada gambar:
#### 1. Node to Node (Data Link Layer)
Pada layer ini protokol yang digunakan adalah Ethernet, HDLC, atau PPP (Point to Point Protocol). Lapisan ini bertanggugn jawab untuk pengiriman data antara dua node yang terterhubung langsung dalam jaringan yang sama. Layer ini melibatkan pengalamatan fisik (physical address / MAC address) dan deteksi serta koreksi kesalahan yang mungkin terjadi selama transmisi.
  
#### 2. Host to Host (Network Layer)
Pada layer ini protokol yang digunakan adalah IP, ICMP, atau ARP. Lapisan ini mengatur pengiriman data dari host ke host melalui jaringan yang mungkin terdiri dari banyak node. Layer network mendefinisikan alamat IP sehingga setiap komputer dapat saling terkoneksi dalam satu jaringan. Selain itu, layer ini berfungsi untuk melakukan proses routing dan membuat header untuk setiap paket data yang ada.
  
#### 3. Process to Process (Transport Layer)
Protokol yang digunakan pada layer ini adalah protokol TCP dan UDP. Lapisan ini berperan dalam menyalurkan bit. Fungsi layer transport yang lebih spesifik, yaitu:
- a. Memecah data yang akan dimasukkan ke dalam beberapa paket data
- b.	Melakukan transmisi data mulai dari session sampai ke network layer
- c.	Setiap paket yang ada akan diberi penomoran oleh layer ini, sehingga mudah untuk menyusun ulang
- d.	Melakukan looping terhadap proses transmisi yang ada dalam paket data yang hilang.

### C. Resume TCP
![image](https://github.com/user-attachments/assets/55a3e742-2e87-4eaf-bf15-89e3db7622d1)  

Transmission Control Protocol (TCP) adalah protokol komunikasi yang memungkinkan pengiriman data yang terjamin dan terurut antar perangkat dalam jaringan. TCP menggunakan tiga tahapan utama untuk mengelola koneksi, yaitu establishment, data transfer, dan termination.
1. Establishment (Pembentukan Koneksi)
   
   ![image](https://github.com/user-attachments/assets/35344bd5-e6df-471c-8d60-841d0a442498)
   
   Tahap ini melibatkan proses yang dikenal sebagai *three-way handshake* untuk membangun koneksi antara dua perangkat (client dan server)
   - Langkah 1: **SYN (Synchronize)**
     - Pengirim mengirimkan pesan SYN ke penerima untuk memulai koneksi.
   - Langkah 2: **SYN-ACK (Synchronize-Acknowledge)**
     - Penerima merespons dengan SYN-ACK untuk mengkonfirmasi permintaan koneksi.
   - Langkah 3: **ACK (Acknowledge)**
     - Pengirim mengirimkan ACK sebagai tanda akhir proses handshake, dan koneksi siap digunakan.

3. Data transfer
   
   ![image](https://github.com/user-attachments/assets/a414b067-f217-4e32-9244-7e713a84d12b)
   
   Setelah koneksi terbentuk, data dikirim secara berurutan dengan nomor urut (sequence number). TCP memastikan kendala dan pengurutan data melalui mekanisme berikut:
   - Segmentation, data dibagi menjadi segmen-segmen yang dikirim secara terurut.
   - Acknowledgement, penerima mengirimkan ACK untuk setiap segmen yang berhasil diterima.
   - Flow Control, TCP mengontrol aliran data untuk memastikan pengiriman berjalan efisien dan tidak membebani jaringan.
   - Error Detection and Retransmission, jika segmen hilang atau rusak, TCP akan mengirim ulang segmen tersebut.

5. Termination (Pemutusan Koneksi)
   
   ![image](https://github.com/user-attachments/assets/1b167456-2222-408a-8e76-29754d906f4c)
   
   Tahap ini digunakan untuk menutup koneksi TCP dengan aman. Gambar tersebut menunjukkan terminasi koneksi TCP menggunakan *three-way handshake*, yang melibatkan pertukaran segmen antara client dengan server untuk menutup koneksi secara bersih.
   #### FIN dari client (Active Close)
   - Client yang ingin mengakhiri koneksi mengirim segmen dengan:
     - Flag FIN (Finish) menyatakan bahwa client tidak akan mengirim data lagi.
     - Sequence Number (sed: x) menentukan nomor urutan paket terakhir.
     - Acknowledgement Number (ack: y) mengakui paket terakhir yang diterima dari server.
       
   #### FIN + ACK dari Server (Passive Close)
   - Server menerima segmen FIN dan merespons dengan:
     - Flag ACK, mengkonfirmasi penerima FIN dari client.
     - Flag FIN, menyatakan bahwa server juga ingin mengakhiri koneksi.
     - Sequence Number (seq: y) mengirim nomor urutnya sendiri.
     - Acknowledgement Number (ack: x + 1) mengakui paket terakhir client.
       
   #### ACK dari Client
   - Client menerima segmen FIN dari server dan mengirim ACK terakhir untuk mengkonfirmasi penutupan koneksi:
     - Flag ACK, mengkonfirmasi FIN dari server
     - Sequence Number (seq: x)
     - Acknowledgement Number (ack: y + 1)



