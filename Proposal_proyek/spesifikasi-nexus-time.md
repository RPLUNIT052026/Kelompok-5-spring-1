# Digital Twin NEXUS Time

**System Requirements & Architecture Specification**
Aplikasi Android (APK) Penjualan Jam Tangan Pria & Wanita dengan Digital Twin per Unit

---

## 1. Pengenalan Proyek

| Parameter | Detail Spesifikasi |
| :--- | :--- |
| **Nama Proyek** | Digital Twin NEXUS Time |
| **Konsep Utama** | Setiap jam tangan fisik di toko memiliki kembaran digital (*digital twin*) di aplikasi. Twin menyimpan identitas, kondisi, garansi, dan status unit, serta berubah otomatis mengikuti kejadian nyata (dipesan, terjual, diservis). |
| **Platform** | Aplikasi Android (APK) |
| **Masalah yang Diatasi** | Pembeli sulit memastikan ketersediaan dan kondisi unit jam yang sebenarnya, sedangkan penjual kesulitan memantau stok per unit, masa garansi, dan jadwal servis. |
| **Tujuan Proyek** | Membangun aplikasi jual-beli jam tangan pria dan wanita yang transparan bagi pelanggan dan terpusat bagi admin toko, dengan pemantauan kondisi tiap unit lewat digital twin. |

---

## 2. Metodologi & Manajemen Tim (Agile/Scrum)

### Pendekatan Metodologi
Proyek menggunakan **Agile dengan pendekatan Scrum**. Pekerjaan dipecah menjadi siklus pendek (*Sprint*, 1-2 minggu), sehingga setiap fitur (desain, database, aplikasi, twin) dikembangkan, diuji, dan dievaluasi bertahap.

### Pembagian Peran Tim
| Peran | Penanggung Jawab | Tanggung Jawab Utama |
| :--- | :--- | :--- |
| **Scrum Master** | *[Nama Anggota]* | Mengelola alur kerja, memastikan Sprint sesuai target, memfasilitasi *daily standup*. |
| **UI/UX Designer** | *[Nama Anggota]* | Membuat *wireframe* dan *mockup* tampilan mobile serta *user flow*. |
| **Mobile Developer** | *[Nama Anggota]* | Membangun aplikasi Android (Flutter atau Kotlin) dan tampilan digital twin. |
| **Backend/Database** | *[Nama Anggota]* | Menyiapkan Firebase (Auth, Firestore), aturan sinkronisasi twin, dan data simulasi. |

> Jika anggota kelompok lebih sedikit dari jumlah peran, satu orang dapat memegang dua peran yang beririsan.

### Perencanaan Sprint
| Sprint | Fokus Pengerjaan | Target Deliverables |
| :---: | :--- | :--- |
| **Sprint 1** | Analisis kebutuhan, *user story*, *flowchart*, *wireframe mobile*, *ERD*, arsitektur | Dokumen desain dan *blueprint* teknis |
| **Sprint 2** | Setup Firebase, UI katalog, keranjang, checkout, profil twin dasar | Prototipe aplikasi toko dan struktur data twin |
| **Sprint 3** | Dashboard twin, peringatan otomatis, panel admin, simulasi pembayaran, testing, build APK | Aplikasi siap rilis (APK) |

---

## 3. Analisis Kebutuhan Sistem

### Aktor Sistem
| Aktor | Deskripsi Peran & Hak Akses |
| :---: | :--- |
| **Pelanggan** | Registrasi/login, melihat katalog, melihat profil twin jam, memesan, membayar, dan memantau status pesanan. |
| **Admin Toko** | Mengelola data jam dan unit, memantau dashboard twin, memperbarui kondisi unit, mengubah status pesanan. |
| **Payment Gateway (simulasi)** | Sistem eksternal pembayaran (QRIS, E-Wallet, Transfer Bank). Pada tugas ini dibuat simulasi. Bukan pengguna, sehingga tidak dibuat *user story*. |

### Matriks Kebutuhan Fungsional
| Modul Fitur | Kebutuhan Fungsional | Aktor | User Story |
| :--- | :--- | :---: | :---: |
| **Autentikasi** | Registrasi dan login email-password, pengarahan sesuai `role`. | Pelanggan, Admin | US-01, 02, 14, 15, 22 |
| **Katalog Jam** | Daftar jam dengan foto, merek, harga, filter kategori (Pria, Wanita, Unisex) dan tipe (Analog, Digital, Smartwatch). | Pelanggan | US-03, 04 |
| **Digital Twin Unit** | Profil digital tiap unit: nomor seri, kondisi, garansi, status. | Pelanggan, Admin | US-05, 06, 16, 17 |
| **Keranjang & Checkout** | Tambah/ubah/hapus item dan pembuatan pesanan. Unit dikunci sementara saat dipesan. | Pelanggan | US-07, 08, 09 |
| **Pembayaran Digital** | QRIS, E-Wallet, Transfer Bank (simulasi), konfirmasi dan penanganan gagal bayar. | Pelanggan | US-10, 11, 12 |
| **Status Pesanan** | Pelanggan melihat progres. Admin memantau dan mengubah status. | Pelanggan, Admin | US-13, 19, 20, 21 |
| **Monitoring Twin** | Dashboard seluruh unit, peringatan garansi dan servis, tombol simulasi. | Admin | US-17, 18 |
| **Manajemen Katalog** | CRUD data jam dan unit. | Admin | US-16 |

### Kebutuhan Non-Fungsional
- Aplikasi berjalan di Android 8.0 ke atas.
- Perubahan status twin tampil di layar dalam beberapa detik (sinkronisasi real-time Firestore).
- Data pengguna dilindungi autentikasi dan aturan akses berdasarkan `role`.
- Antarmuka sederhana dan responsif untuk berbagai ukuran layar HP.

---

## 4. User Story

### A. Pelanggan
| ID | User Story | Kriteria Penerimaan |
|---|---|---|
| US-01 | Sebagai pelanggan, saya ingin **registrasi akun** agar bisa memesan. | Data tersimpan di `users` dengan role "pelanggan". |
| US-02 | Sebagai pelanggan, saya ingin **login** agar bisa mengakses aplikasi. | Valid: tampil beranda katalog. Salah: pesan error dan bisa input ulang. |
| US-03 | Sebagai pelanggan, saya ingin **melihat katalog jam** agar tahu pilihan yang tersedia. | Kartu jam menampilkan foto, nama, merek, harga, dan lencana stok. |
| US-04 | Sebagai pelanggan, saya ingin **memfilter jam** (Pria, Wanita, Unisex, tipe) agar cepat menemukan jam yang cocok. | Katalog hanya menampilkan jam sesuai filter. |
| US-05 | Sebagai pelanggan, saya ingin **melihat profil digital twin sebuah jam** agar tahu kondisi unit sebenarnya. | Halaman detail menampilkan nomor seri, kondisi (baru/preloved), skor kondisi, masa garansi, dan status unit. |
| US-06 | Sebagai pelanggan, saya ingin **melihat jumlah unit tersedia secara real-time** agar tidak memesan unit yang habis. | Jumlah dan status unit berubah otomatis tanpa muat ulang. |
| US-07 | Sebagai pelanggan, saya ingin **menambah jam ke keranjang** agar bisa memesan lebih dari satu. | Tombol "+ Keranjang" menyimpan ke `keranjang`, ikon menampilkan jumlah item. |
| US-08 | Sebagai pelanggan, saya ingin **mengubah jumlah dan menghapus item** agar pesanan sesuai. | Subtotal dan total diperbarui otomatis. |
| US-09 | Sebagai pelanggan, saya ingin **checkout** agar pesanan tercatat. | Sistem membuat `pesanan` dan `detail_pesanan` berstatus "Menunggu Pembayaran", unit twin berubah menjadi "Di-booking". |
| US-10 | Sebagai pelanggan, saya ingin **memilih metode pembayaran** agar bisa membayar dengan nyaman. | Pilihan QRIS, E-Wallet, Transfer Bank. QRIS menampilkan kode QR simulasi. |
| US-11 | Sebagai pelanggan, saya ingin **membayar pesanan** agar pesanan diproses. | Jika berhasil, status "Diproses" dan unit twin berubah menjadi "Terjual". |
| US-12 | Sebagai pelanggan, saya ingin **diberi tahu jika pembayaran gagal** agar bisa mengulang atau membatalkan. | Pesan gagal tampil. Jika dibatalkan, status "Dibatalkan" dan unit twin kembali "Tersedia". |
| US-13 | Sebagai pelanggan, saya ingin **melihat status pesanan** agar tahu progresnya. | Status terlihat: Menunggu Pembayaran, Diproses, Dikirim, Selesai. |
| US-14 | Sebagai pelanggan, saya ingin **logout** agar akun aman. | Sesi berakhir setelah logout. |

### B. Admin
| ID | User Story | Kriteria Penerimaan |
|---|---|---|
| US-15 | Sebagai admin, saya ingin **login ke dashboard admin** agar bisa mengelola toko. | Sistem cek `users.role`. Jika "admin", diarahkan ke dashboard. |
| US-16 | Sebagai admin, saya ingin **mengelola data jam dan unit (CRUD)** agar katalog selalu terbaru. | Perubahan tersimpan dan tampil di katalog pelanggan. |
| US-17 | Sebagai admin, saya ingin **melihat dashboard twin semua unit** agar bisa memantau stok dan kondisi. | Tiap unit berwarna sesuai status: hijau Tersedia, kuning Di-booking, abu Terjual, merah Perlu Servis. |
| US-18 | Sebagai admin, saya ingin **menerima peringatan otomatis** agar tidak terlewat garansi atau servis. | Peringatan muncul saat garansi kurang dari 30 hari, jadwal servis tiba, atau skor kondisi di bawah ambang. |
| US-19 | Sebagai admin, saya ingin **melihat daftar pesanan masuk** agar bisa memantau transaksi. | Daftar menampilkan No. Pesanan, Pelanggan, Tanggal, Total, Status. |
| US-20 | Sebagai admin, saya ingin **mencari dan memfilter pesanan** agar cepat menemukan pesanan. | Cari berdasarkan no. pesanan atau pelanggan, filter status. |
| US-21 | Sebagai admin, saya ingin **mengubah status pesanan** agar pelanggan tahu progresnya. | Status berubah dan terlihat di sisi pelanggan. |
| US-22 | Sebagai admin, saya ingin **logout** agar akses dashboard aman. | Logout lewat menu. |

---

## 5. Perancangan Sistem

### 5.1 Flowchart Pemesanan
| Langkah | Aktivitas Pengguna / Sistem | Output / Status Sistem | Story |
| :---: | :--- | :--- | :---: |
| **1** | Pengguna membuka aplikasi NEXUS Time | Halaman beranda tampil | US-03 |
| **2** | Registrasi atau login. Sistem cek `users.role` | Pelanggan ke katalog, admin ke dashboard | US-01, 02, 15 |
| **3** | Melihat katalog dan profil twin, menambah ke keranjang | Item masuk `keranjang` | US-05, 07 |
| **4** | Checkout dan memilih metode pembayaran | `pesanan` dan `detail_pesanan` dibuat, unit twin menjadi "Di-booking" | US-09, 10 |
| **5** | Pembayaran diproses (simulasi) | Sukses: status "Diproses", unit "Terjual". Gagal: bayar ulang atau batal, unit kembali "Tersedia" | US-11, 12 |
| **6** | Admin memperbarui status pesanan | Pelanggan melihat progres terbaru | US-21, 13 |

### 5.2 Wireframe Mobile
| No. | Halaman | Elemen Utama | Story |
| :---: | :--- | :--- | :---: |
| **1** | Beranda & Katalog | Pencarian, filter Pria/Wanita/Unisex, kartu jam, lencana stok, ikon keranjang | US-03, 04, 06 |
| **2** | Detail Jam & Profil Twin | Foto, harga, nomor seri, skor kondisi, garansi, status unit, tombol "+ Keranjang" | US-05, 06, 07 |
| **3** | Keranjang | Daftar item dengan +/-, tombol Hapus, total, "Lanjut ke Checkout" | US-08, 09 |
| **4** | Checkout & Pembayaran | Form nama, No. HP, alamat, pilihan metode, kode QRIS, "Bayar Sekarang" | US-09, 10, 11 |
| **5** | Status Pesanan | Daftar pesanan dan linimasa status | US-13 |
| **6** | Dashboard Twin (Admin) | Ringkasan jumlah per status, grid unit berwarna, daftar peringatan, tombol simulasi | US-17, 18 |
| **7** | Kelola Jam & Unit (Admin) | Form tambah/ubah/hapus jam dan unit, input kondisi | US-16 |
| **8** | Daftar Pesanan (Admin) | Pencarian, filter status, tombol "Ubah Status" | US-19, 20, 21 |

### 5.3 Arsitektur Teknologi
| Layer | Teknologi | Fungsi |
| :--- | :--- | :--- |
| **Client (Mobile)** | Flutter (Dart) atau Android Studio (Kotlin) | Antarmuka pelanggan dan admin dalam satu APK |
| **Komunikasi** | HTTPS, Firebase SDK | Jalur data antara aplikasi dan server |
| **Backend** | Firebase Authentication, Cloud Firestore | Autentikasi, logika akses, sinkronisasi real-time |
| **Database** | Cloud Firestore (NoSQL) | Penyimpanan data jam, unit, pesanan |
| **Layanan Eksternal** | Simulasi Payment Gateway | Meniru pembayaran QRIS, E-Wallet, Bank |

### 5.4 Struktur Data (ERD)
| Tabel / Koleksi | Atribut Utama | Relasi |
| :--- | :--- | :--- |
| **`users`** | `id` (PK), `nama`, `email`, `no_hp`, `alamat`, `role` ('pelanggan' / 'admin') | 1:N ke `keranjang` dan `pesanan` |
| **`jam`** | `id` (PK), `nama`, `merek`, `kategori` ('pria' / 'wanita' / 'unisex'), `tipe` ('analog' / 'digital' / 'smartwatch'), `harga`, `deskripsi`, `gambar` | 1:N ke `unit_jam` |
| **`unit_jam`** (digital twin) | `id` (PK), `jam_id` (FK), `nomor_seri`, `kondisi` ('baru' / 'preloved'), `skor_kondisi`, `tanggal_beli`, `garansi_sampai`, `jadwal_servis`, `status` ('tersedia' / 'di-booking' / 'terjual' / 'perlu_servis') | N:1 ke `jam`, 1:N ke `detail_pesanan` dan `log_servis` |
| **`keranjang`** | `id` (PK), `user_id` (FK), `unit_id` (FK), `jumlah` | Satu baris per item keranjang |
| **`pesanan`** | `id` (PK), `user_id` (FK), `tanggal`, `total_harga`, `status` | 1:N ke `detail_pesanan`, 1:1 ke `pembayaran` |
| **`detail_pesanan`** | `id` (PK), `pesanan_id` (FK), `unit_id` (FK), `harga`, `subtotal` | Rincian unit per pesanan |
| **`pembayaran`** | `id` (PK), `pesanan_id` (FK), `metode`, `jumlah_bayar`, `status_pembayaran`, `waktu_bayar` | 1:1 ke `pesanan` |
| **`log_servis`** | `id` (PK), `unit_id` (FK), `tanggal`, `catatan`, `biaya` | N:1 ke `unit_jam` |

---

## 6. Rancangan Digital Twin

### 6.1 Letak Digital Twin
Digital twin berada pada koleksi **`unit_jam`**. Satu jam tangan fisik di toko diwakili tepat satu dokumen `unit_jam`, dan layar pelanggan maupun admin hanya menampilkan isi dokumen tersebut. Jadi aplikasi tidak menyimpan "stok angka", tetapi **identitas tiap unit**.

| Dunia Nyata (Fisik) | Representasi Digital (Twin) | Sinkronisasi |
| :--- | :--- | :--- |
| Jam tangan di etalase/gudang | Dokumen `unit_jam` | Dibuat admin saat barang masuk |
| Nomor seri pada casing | `nomor_seri` | Diinput admin |
| Kondisi fisik (baret, kaca, strap) | `skor_kondisi` (1-100) dan `kondisi` | Diinput admin saat inspeksi |
| Kartu garansi | `garansi_sampai` | Diinput admin, dipantau otomatis |
| Pemesanan dan pembelian | `status` | Otomatis dari transaksi |
| Perawatan di servis | `log_servis` dan `jadwal_servis` | Diinput admin |

### 6.2 Siklus Status Unit
| Kejadian | Status Twin Sebelum | Status Twin Sesudah |
| :--- | :--- | :--- |
| Barang masuk | - | Tersedia |
| Pelanggan checkout | Tersedia | Di-booking |
| Pembayaran berhasil | Di-booking | Terjual |
| Pembayaran gagal atau pesanan dibatalkan | Di-booking | Tersedia |
| Jadwal servis tiba atau skor di bawah ambang | Tersedia | Perlu Servis |
| Servis selesai (admin input) | Perlu Servis | Tersedia |

### 6.3 Fitur Digital Twin
- **Profil twin per unit** yang dapat dilihat pelanggan dan admin.
- **Status real-time** tanpa muat ulang (Firestore *snapshot listener*).
- **Pengunci unit sementara** saat checkout agar tidak terjadi pemesanan ganda.
- **Peringatan otomatis** untuk garansi hampir habis, jadwal servis, dan skor kondisi rendah.
- **Dashboard twin** untuk admin dengan kode warna status.
- **Tombol simulasi** untuk admin (mengubah skor kondisi atau menggeser tanggal) sebagai pengganti sensor, sehingga perilaku twin dapat didemokan saat presentasi.

---

## 7. Rencana Pengujian
| No. | Skenario Uji | Hasil yang Diharapkan |
| :---: | :--- | :--- |
| 1 | Registrasi dan login pelanggan | Masuk ke katalog |
| 2 | Login admin | Masuk ke dashboard admin |
| 3 | Filter jam Pria dan Wanita | Katalog sesuai filter |
| 4 | Checkout satu unit | Status twin berubah "Di-booking" |
| 5 | Pembayaran sukses | Pesanan "Diproses", twin "Terjual" |
| 6 | Pembayaran gagal lalu batal | Pesanan "Dibatalkan", twin kembali "Tersedia" |
| 7 | Dua pelanggan memesan unit yang sama | Pelanggan kedua ditolak karena unit sudah "Di-booking" |
| 8 | Garansi unit tinggal kurang dari 30 hari | Peringatan muncul di dashboard admin |
| 9 | Admin menurunkan skor kondisi di bawah ambang | Status twin menjadi "Perlu Servis" |
| 10 | Admin mengubah status pesanan | Pelanggan melihat status terbaru |

---

*Developed for Digital Twin NEXUS Time Project*
