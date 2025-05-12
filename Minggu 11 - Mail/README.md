# Mail
Nama : Lintang Arum Sari 
NRP  : 3123600002 
Kelas: 2 D4 IT A 

## A. Mail Server
Mail server adalah server khusus yang menangani proses pengiriman, penerimaan, dan penyimpanan email dalam suatu doamin (misalnya: `@example.com`). Setiap doman yang digunakan untuk email harus memiliki konfigurasi DNS agar email dapat dikirim dan diterima dengan benar.

### Informasi Mail Server dalam DNS
Untuk domain seperti `example.com`, informasi tentang mail server tersimpan dalam DNS melalui entri yang disebut MX Record (Mail Exchange Record). Contoh:
```bash
example.com.     IN   MX   10   mail.example.com.
```

### Informasi Mail Server yang Umumnya Diperlukan dalam Konfigurasi Email Client
1. Alamat server SMTP, untuk mengirim email
2. Alamat server IMAP/POP3, untuk menerima email
3. Port
4. Jenis enkripsi (SSL/TLS atau STARTTLS)
5. Username dan password, biasanya berupa alamat email lengkap

### Record Tambahan untuk Keamanan Email
Domain juga bisa memiliki record tambahan seperti:
- SPF (Sender Policy Framework), untuk mencegah pemalsuan pengirim
- DKIM (DomainKeys Identified Mail), untuk menandatangani email secara digital
- DMARC, kebijakan otentikasi email berdasarkan SPF dan DKIM
  
## B. Protokol Mail
Protokol email adalah serangkaian aturan yang mengatur cara email dikirim, diterima, dan disimpan. Protokol utamanya adalah `SMTP` untuk mengirim email, `POP3` dan `IMAP` untuk menerima email.

`SMTP` menangani transmisi email antar server, sementara `POP3` mengunduh email ke perangkat dan `IMAP` menyingkronkan email di beberapa perangkat.

## C. Macam-Macam Protokol Mail
Protokol standar untuk email meliputi:
1. SMTP
2. POP3
3. IMAP

### 1. SMTP (Simple Mail Transfer Protocol)
Protokol ini bertanggung jawab untuk mengirimkan pesan email. Protokol ini digunakan oleh client email dan server email untuk bertukar email antar komputer.
![SMTP Process](https://github.com/lintangaroem/AdminJaringan2025/blob/main/Minggu%2011%20-%20Mail/img/smtp%20process.png?raw=true)

Email client berkomunikasi dengan SMTP server (server pengirim email) menggunakan port tertentu untuk mengirim email. Biasanya, port yang digunakan untuk SMTP adalah port 25, 465, atau 587, tergantung pada konfigurasi dan apakah koneksi tersebut aman (menggunakan SSL/TSL) atau tidak.

Email client maupun SMTP server saling bertukar pesan dalam bentuk coomand and reply (perintah dan balasan). SMTP memiliki kumpulan perintah standar seperti:
- `HELO` atau `EHLO` untuk memulai komunikasi
- `MAIL FROM` untuk menyatakan pengirim
- `RCPT TO` untuk menyatakan penerima
- `DATA` untuk mengirim isi email
Proses ini memastikan bahwa email dikirim dengan format yang benar dan diterima oleh server penerima.

### 2. POP3 (Post Office Protocol version 3)
![POP3 Process](https://github.com/lintangaroem/AdminJaringan2025/blob/main/Minggu%2011%20-%20Mail/img/pop3%20process.png?raw=true)

POP3 aalah protokol email yang digunakan oleh email client untuk mengakses dan mengunduh email dari server ke komputer lokal. Ketika menggunakan protokol ini, email client akan mengunduh semua email dari kotak masuk di server ke komputer, kemudian menghapus email tersebut dari server (secara default), sehingga email hanya akan tersedia secara lokal. Karena sudah terhapus di server, kita tidak dapat mengaksesnya dari perangkat lain (kecuali pengaturannya diubah agar email tetap tersimpan di server). Karena email sudah diunduh, maka kita bisa mengaksesnya secara offline. Saat ini, aplikasi email modern menyediakan opsi untuk tetap menyimpan salinan email di server, meskipun menggunakan POP3.

### 3. IMAP (Internet Message Access Protocol)
![IMAP Process](https://github.com/lintangaroem/AdminJaringan2025/blob/main/Minggu%2011%20-%20Mail/img/imap%20process.png?raw=true)

IMAP adalah protokol email yang memungkinkan user untuk mengakses, membaca, dan mengelola email secara langsung di server, tanpa harus mengunduh semuanya ke perangkat. Dengan protokol ini, user dapat membuat dan mengatur folder-folder email (seperti inbox, spam, draft, dll), menghapus email secara permanen dari server, serta melakukan pencarian cepat dalam kotak masuk, karena semuanya masih tersimpan di server.

IMAP juga memberikan opsi untuk memberi penanda / flag seperti 'sudah dibaca', 'belum dibaca', 'penting', dll serta mengambil bagian tertentu dari email (misalnya hanya header, atau isi tanpa attachment) yang berguna untuk menghemat data atau mempercepat akses. Tidak seperti POP3, IMAP tidak langsung menghapus email dari server. Semua email tetap tersimpan di server sampai user sendiri menghapusnya secara manual.

IMAP memungkinkan beberapa perangkat atau pengguna terhubung ke satu akun email secara bersamaan, dengan sinkronisasi real-time. Sehingga user dapat membuka email di berbagai perangkat yang berbeda dan tampilannya akan tampak sama di semua perangkat.

## D. Email Port
Port email adalah titik komunikasi pada komputer yang menentukan bagaimana email dikirimkan, termasuk apakah email tersebut dikirim secara aman (terenkripsi) atau tidak. Untuk menghubungkan email client ke server email, kita memerlukan alamat IP atau nama domain dari server email dan nomor port yang sesuai dengan protokol dan jenis enkripsi yang dipakai. Port-port tersebut adalah standar yang ditetapkan oleh IANA (Internet Assigned Numbers Authority), organisasi yang mengatur pengalamatan dan penomoran protokol di internet. Setiap protokol email memiliki nomor port standar dengan jenis enkripsi yang berbeda.
| Protokol | Fungsi                       | Port Standar | Enkripsi                        |
| -------- | ---------------------------- | ------------ | ------------------------------- |
| SMTP     | Kirim email                  | 25           | Tanpa enkripsi / TLS (optional) |
| SMTP     | Kirim email (secure)         | 587          | STARTTLS                        |
| SMTP     | Kirim email (SSL langsung)   | 465          | SSL/TLS                         |
| POP3     | Ambil email (standar)        | 110          | Tanpa enkripsi                  |
| POP3     | Ambil email (secure)         | 995          | SSL/TLS                         |
| IMAP     | Sinkronisasi email (standar) | 143          | STARTTLS                        |
| IMAP     | Sinkronisasi email (secure)  | 993          | SSL/TLS                         |


## E. Rangkuman: Introducing to Electronic Mail
Sumber: [Introduction to Electronic Mail](https://www.geeksforgeeks.org/introduction-to-electronic-mail/)

### 1. Pengertian Email
Email (electronic mail) adalah cara bertukar pesan melalui jaringan komputer, terutama internet. Email dapat berisi teks, gambar, audio, dan video, dan merupakan sarana komunikasi digital yang cepat dan efisien.

### 2. Komponen Utama Sistem Email
![Email Process](https://github.com/lintangaroem/AdminJaringan2025/blob/main/Minggu%2011%20-%20Mail/img/email%20process.png?raw=true)
- Alamat Email: Identitas unik pengguna, biasanya dalam format user@domain.com.
- Email Client (User Agent): Aplikasi yang digunakan untuk mengirim, menerima, dan membaca email (contoh: Gmail, Outlook).
- Mail Server (Message Transfer Agent / MTA): Server yang menangani proses pengiriman dan penerimaan email.
- Mailbox: Penyimpanan lokal untuk email yang diterima.
- Spool File: Penyimpanan sementara untuk email yang akan dikirim, yang akan diproses oleh MTA.

### 3. Cara Kerja Pengiriman Email
1. Pengguna menulis email melalui email client.
2. Alamat penerima dimasukkan di kolom "To".
3. Subjek dan isi pesan ditambahkan.
4. Lampiran bisa ditambahkan jika perlu.
5. Setelah dikirim, pesan dikirim ke server email pengirim dan kemudian diteruskan ke server penerima menggunakan SMTP.
6. Penerima dapat membaca email menggunakan protokol POP3 atau IMAP.

### 4. Fitur-Fitur Tambahan
- CC (Carbon Copy): Mengirim salinan ke pihak lain yang terlihat oleh penerima utama.
- BCC (Blind Carbon Copy): Mengirim salinan ke pihak lain tanpa diketahui oleh penerima utama.
- Reply, Reply All, Forward: Digunakan untuk membalas atau meneruskan email yang sudah diterima.

### Kesimpulan
Sistem email terdiri dari komponen perangkat lunak dan server yang bekerja bersama untuk memungkinkan pengiriman pesan elektronik. Email adalah bagian penting dari komunikasi digital modern dan memiliki berbagai fitur untuk mendukung kebutuhan penggunanya.
