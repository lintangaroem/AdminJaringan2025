## Chapter 4: Process Control
### Komponen Proses dalam Sistem Operasi
Sebuah proses terdiri dari ruang alamat dan kumpulan struktur data dalam kernel. Ruang alamat adalah serangkaian halaman memori yang telah ditandai oleh kernel untuk digunakan oleh proses tersebut. Halaman-halaman ini digunakan untuk menyimpan kode, data, dan stack dari proses. Biasanya, ukuran halaman adalah 4KiB atau 8KiB.

Di dalam kernel, ada struktur data yang mencatat berbagai informasi mengenai proses, seperti:
- Peta ruang alamat proses - Menunjukkan lokasi memori yang digunakan oleh proses.
- Status proses – Apakah proses sedang berjalan, tidur, atau dalam keadaan lain.
- Prioritas proses – Seberapa penting proses ini dibandingkan dengan proses lain.
- Penggunaan sumber daya – Seperti penggunaan CPU, memori, dan lainnya.
- Informasi tentang file dan port jaringan yang dibuka – File atau port yang sedang digunakan oleh proses.
- Masker sinyal proses – Daftar sinyal yang sedang diblokir oleh proses.
- Pemilik proses - User ID dari pengguna yang menjalankan proses.

Proses bisa dianggap sebagai wadah yang berisi sumber daya yang dikelola oleh kernel untuk menjalankan sebuah program. Sumber daya tersebut meliputi halaman memori untuk kode dan data program, deskriptor file yang mengacu pada file yang dibuka, dan atribut yang menggambarkan status proses.

Thread adalah konteks eksekusi dalam sebuah proses. Sebuah proses bisa memiliki banyak thread yang berbagi ruang alamat dan sumber daya lainnya. Thread digunakan untuk mencapai paralelisme dalam sebuah proses. Thread juga disebut sebagai lightweight processes karena lebih murah untuk dibuat dan dihancurkan dibandingkan dengan proses.

Sebagai contoh, bayangkan sebuah web server. Server ini mendengarkan permintaan masuk dan kemudian membuat thread baru untuk menangani setiap permintaan tersebut. Setiap thread menangani satu permintaan, namun server secara keseluruhan bisa menangani banyak permintaan secara bersamaan karena memiliki banyak thread. Di sini, web server adalah proses, dan setiap thread adalah konteks eksekusi terpisah dalam proses tersebut.

### PID: Nomor ID Proses / Process ID
Setiap proses memiliki PID (Process ID), yaitu nomor unik yang diberikan oleh kernel saat proses dibuat. PID digunakan untuk merujuk proses dalam berbagai perintah sistem, seperti mengirim sinyal.

Konsep namespaces memungkinkan beberapa proses memiliki PID yang sama dalam lingkungan terisolasi yang disebut container. Container memungkinkan menjalankan beberapa instance aplikasi secara terpisah di sistem yang sama.

### PPID: Nomor ID Proses Induk / Parent Process ID
Setiap proses juga memiliki PPID (Parent Process ID), yaitu nomor ID dari proses induk yang membuatnya. PPID digunakan untuk merujuk ke proses induk dalam berbagai perintah sistem, seperti mengirim sinyal ke proses induk.

### UID (User ID / ID Pengguna) dan EUID (Effective User ID)
UID (User ID) adalah ID pengguna yang memulai proses. Sementara itu, EUID (Effective User ID) adalah ID pengguna yang digunakan oleh proses untuk menentukan akses ke sumber daya, seperti file dan port jaringan. EUID mengontrol hak akses proses terhadap sumber daya di sistem.

### Siklus Hidup Proses
Untuk membuat proses baru, sebuah proses menggunakan perintah fork untuk menyalin dirinya sendiri. Proses baru memiliki PID yang berbeda dan informasi terpisah. Di Linux, fork menggunakan clone, yang lebih canggih dan menangani thread.

Saat sistem booting, kernel membuat beberapa proses, termasuk init atau systemd yang memiliki PID 1. Proses ini menjalankan skrip startup sistem, dan semua proses lainnya merupakan keturunan dari proses ini.

### Signals
Sinyal adalah cara untuk mengirimkan pemberitahuan ke sebuah proses, memberi tahu bahwa suatu peristiwa telah terjadi.

Terdapat sekitar tiga puluh jenis sinyal yang didefinisikan dan digunakan dengan berbagai cara:
1. Antarproses: Sinyal dapat dikirim antarproses untuk komunikasi.
2. Dari driver terminal: Sinyal dapat dikirim untuk menghentikan, menginterupsi, atau menangguhkan proses saat tombol tertentu ditekan.
3. Dari administrator: Sinyal dapat dikirim menggunakan perintah kill untuk tujuan tertentu.
4. Dari kernel: Sinyal dapat dikirim saat proses melakukan kesalahan, seperti pembagian dengan nol.
5. Dari kernel: Sinyal juga digunakan untuk memberi tahu proses tentang kondisi penting, seperti kematian proses anak atau data yang tersedia pada saluran I/O.

![image](https://github.com/user-attachments/assets/89153ba8-4a64-41d5-a7ed-6f12dc659217)  
**Sinyal KILL, INT, TERM, HUP, dan QUIT**

Meskipun nama-nama sinyal ini terdengar seperti memiliki arti yang sama, penggunaan masing-masing sebenarnya sangat berbeda:

1. **KILL**: Sinyal ini tidak bisa diblokir dan menghentikan proses di tingkat kernel. Proses tidak bisa menangani sinyal ini.
   
2. **INT**: Dikirim oleh driver terminal saat pengguna menekan tombol **Ctrl+C**. Ini adalah permintaan untuk menghentikan operasi saat ini. Program sederhana seharusnya berhenti (jika menangani sinyal) atau membiarkan dirinya dihentikan, yang merupakan tindakan default jika sinyal tidak ditangani. Program dengan antarmuka perintah interaktif (seperti shell) harus menghentikan proses, membersihkan, dan menunggu input dari pengguna lagi.

3. **TERM**: Permintaan untuk menghentikan eksekusi sepenuhnya. Proses yang menerima sinyal ini diharapkan untuk membersihkan statusnya dan keluar.

4. **HUP**: Dikirim ke proses saat terminal pengendali ditutup. Awalnya digunakan untuk menunjukkan "hang up" pada koneksi telepon, sekarang sering digunakan untuk memberi instruksi pada proses daemon untuk berhenti dan memulai ulang, biasanya untuk memperbarui konfigurasi baru. Perilaku tepatnya tergantung pada proses yang menerima sinyal HUP.

5. **QUIT**: Mirip dengan TERM, tetapi secara default menghasilkan *core dump* jika tidak ditangani. Beberapa program mungkin mengartikan sinyal ini dengan cara yang berbeda.





