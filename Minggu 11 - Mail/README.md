## Mail
### Definisi Protokol Mail
Protokol email adalah serangkaian aturan yang mengatur cara email dikirim, diterima, dan disimpan. Protokol utamanya adalah `SMTP` untuk mengirim email, `POP3` dan `IMAP` untuk menerima email.

`SMTP` menangani transmisi email antar server, sementara `POP3` mengunduh email ke perangkat dan `IMAP` menyingkronkan email di beberapa perangkat.

### Macam-Macam Protokol Mail
Protokol standar untuk email meliputi:
1. SMTP
2. POP3
3. IMAP

#### SMTP (Simple Mail Transfer Protocol)
Protokol ini bertanggung jawab untuk mengirimkan pesan email. Protokol ini digunakan oleh client email dan server email untuk bertukar email antar komputer.
//gambar smtp process
Email client berkomunikasi dengan SMTP server (server pengirim email) menggunakan port tertentu untuk mengirim email. Biasanya, port yang digunakan untuk SMTP adalah port 25, 465, atau 587, tergantung pada konfigurasi dan apakah koneksi tersebut aman (menggunakan SSL/TSL) atau tidak.

Email client maupun SMTP server saling bertukar pesan dalam bentuk coomand and reply (perintah dan balasan). SMTP memiliki kumpulan perintah standar seperti:
- `HELO` atau `EHLO` untuk memulai komunikasi
- `MAIL FROM` untuk menyatakan pengirim
- `RCPT TO` untuk menyatakan penerima
- `DATA` untuk mengirim isi email
Proses ini memastikan bahwa email dikirim dengan format yang benar dan diterima oleh server penerima.

#### POP3
