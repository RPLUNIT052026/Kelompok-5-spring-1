# ⌚ Proposal Proyek --- NEXUS Time

## 1. Latar Belakang

Jam tangan merupakan salah satu produk fashion dan aksesori yang
digunakan oleh pria maupun wanita. Selain berfungsi sebagai penunjuk
waktu, jam tangan juga menjadi bagian dari gaya dan penampilan. Namun,
proses jual beli jam tangan secara online sering kali membuat pembeli
kesulitan memastikan informasi produk, ketersediaan stok, kondisi
barang, garansi, serta keaslian dan riwayat perawatan setiap unit.

Untuk membantu mengatasi masalah tersebut, dikembangkan **NEXUS Time**,
yaitu aplikasi toko jam tangan pria dan wanita berbasis Android.
Aplikasi ini menyediakan katalog produk, pemesanan, pembayaran, dan
pengelolaan transaksi. NEXUS Time juga menerapkan konsep **Digital
Twin**, yaitu representasi digital dari setiap unit jam tangan fisik
yang menyimpan informasi identitas, kondisi, garansi, dan status barang.
Data pada representasi digital diperbarui ketika terjadi transaksi,
pemeriksaan, atau servis.

## 2. Tujuan

-   Menyediakan katalog jam tangan pria dan wanita dengan informasi
    produk yang lengkap.
-   Memudahkan pelanggan mencari jam berdasarkan merek, kategori, harga,
    warna, dan jenis.
-   Menyediakan proses pemesanan dan checkout melalui aplikasi Android.
-   Membantu admin mengelola produk, stok, pesanan, pembayaran, dan data
    pelanggan.
-   Menerapkan konsep Digital Twin dengan membuat profil digital untuk
    setiap unit jam tangan fisik.
-   Memantau status unit jam tangan, seperti tersedia, dipesan, terjual,
    atau dalam servis.
-   Menyimpan informasi nomor seri, kondisi, garansi, dan riwayat servis
    setiap unit.
-   Memberikan notifikasi terkait status pesanan dan informasi penting
    mengenai produk.

## 3. Keputusan Sprint 1

### a. Agile Methodology

-   Metodologi pengembangan yang digunakan: **Scrum**.
-   Pengerjaan dibagi menjadi beberapa sprint agar pengembangan lebih
    terarah dan mudah dievaluasi.
-   Setiap sprint memiliki target pekerjaan dan hasil yang harus
    dicapai.
-   Role tim terdiri dari **Scrum Master** sebagai koordinator dan
    **Development Team** sebagai pelaksana pengembangan.
-   Tools untuk mengelola tugas dan progres: **GitHub Projects**.

### b. UX Design

-   Antarmuka aplikasi dirancang dengan wireframe atau rancangan awal
    menggunakan Figma/sketsa sebelum proses development.
-   Desain ditujukan untuk perangkat Android dan menyesuaikan ukuran
    layar ponsel.
-   Tampilan menggunakan gaya modern, elegan, dan premium agar sesuai
    dengan produk jam tangan pria dan wanita.

Elemen utama halaman aplikasi:

-   **Beranda:** menampilkan banner, kategori jam pria dan wanita,
    produk unggulan, serta produk terbaru.
-   **Katalog:** menampilkan daftar jam tangan dengan fitur pencarian
    dan filter berdasarkan kategori, merek, harga, dan warna.
-   **Detail produk:** menampilkan foto, nama produk, merek,
    spesifikasi, harga, deskripsi, informasi garansi, dan status
    ketersediaan.
-   **Keranjang:** menampilkan produk yang dipilih pelanggan sebelum
    melakukan pemesanan.
-   **Checkout & pembayaran:** menampilkan ringkasan pesanan, alamat
    pengiriman, total pembayaran, dan metode pembayaran.
-   **Status pesanan:** menampilkan tahapan pesanan, seperti menunggu
    pembayaran, diproses, dikirim, dan selesai.
-   **Profil pelanggan:** menampilkan informasi akun dan riwayat
    transaksi.
-   **Dashboard admin:** mengelola produk, unit jam tangan, stok,
    pesanan, pelanggan, dan data Digital Twin.
-   **Dashboard Digital Twin:** menampilkan representasi digital setiap
    unit jam tangan beserta kondisi, garansi, status, dan riwayat
    servisnya.

Alur pengguna:

Buka aplikasi → login/register → lihat katalog → pilih jam tangan →
lihat detail produk → masukkan ke keranjang → checkout → lakukan
pembayaran → pesanan diproses → barang dikirim → pesanan selesai.

### c. Project Setup

Struktur folder awal repository yang direncanakan:

``` text
nexus-time/
├── android-app/       # Source code aplikasi Android
├── backend/           # API dan logika server
├── database/          # Skema database dan data awal
├── assets/            # Gambar produk, ikon, dan ilustrasi
├── docs/              # Proposal, SRS, UML, dan dokumentasi
├── design/            # Wireframe dan rancangan UI/UX
└── README.md
```

Tech stack awal yang direncanakan:

-   Android app: Flutter atau Android Studio (Kotlin).
-   Backend: Firebase atau REST API.
-   Database: Firebase Firestore atau MySQL.
-   Desain UI/UX: Figma.
-   Version control: Git dan GitHub.
-   Data awal: data produk dan unit jam tangan contoh yang dimasukkan
    oleh admin.

Pemilihan teknologi akhir disesuaikan dengan kemampuan tim dan kebutuhan
implementasi aplikasi.

## 4. Arsitektur Sistem

### a. Pengguna & Peran

-   **Pelanggan:** membuat akun, melihat katalog, mencari produk,
    memasukkan produk ke keranjang, melakukan checkout, membayar, dan
    melihat status pesanan.
-   **Admin:** mengelola katalog, harga, stok, unit jam tangan, pesanan,
    pembayaran, pelanggan, dan informasi Digital Twin.
-   **Pengelola/teknisi:** melakukan pemeriksaan kondisi unit, mencatat
    perawatan atau servis, dan memperbarui informasi kondisi jam tangan
    jika peran ini dipisahkan dari admin.

### b. Sistem Toko (Representasi Digital)

-   **Aplikasi Android:** menyediakan halaman beranda, katalog, detail
    produk, keranjang, checkout, profil, dan riwayat pesanan.
-   **Backend:** menangani autentikasi, pengelolaan produk, transaksi,
    pembayaran, dan pembaruan status unit.
-   **Database:** menyimpan data pengguna, produk, unit fisik, pesanan,
    pembayaran, dan riwayat servis.
-   **Dashboard admin:** menjadi tempat pengelolaan data toko dan
    pemantauan Digital Twin.

### c. Digital Twin NEXUS Time

Digital Twin merupakan bagian utama yang membedakan NEXUS Time dari
aplikasi toko online biasa. Setiap jam tangan fisik yang tercatat di
toko memiliki satu profil digital sebagai representasi unit tersebut.

  -----------------------------------------------------------------------
  Objek Fisik             Representasi Digital    Pembaruan Data
  ----------------------- ----------------------- -----------------------
  Satu unit jam tangan    Profil unit digital     Dibuat saat admin
                                                  menambahkan unit

  Nomor seri jam tangan   Nomor seri pada profil  Diisi dan diverifikasi
                          digital                 admin

  Kondisi fisik jam       Skor kondisi dan        Diperbarui setelah
                          catatan pemeriksaan     inspeksi

  Garansi produk          Tanggal mulai dan       Diisi saat data unit
                          berakhir garansi        dibuat atau pembelian
                                                  dicatat

  Status ketersediaan     Tersedia, dipesan,      Berubah mengikuti
                          terjual, atau servis    transaksi dan
                                                  pengelolaan unit

  Perawatan jam           Riwayat servis dan      Ditambahkan ketika
                          pemeriksaan             servis dilakukan
  -----------------------------------------------------------------------

Contoh data pada Digital Twin:

-   ID unit dan nomor seri.
-   Merek dan model jam tangan.
-   Kategori pria atau wanita.
-   Warna dan jenis material.
-   Kondisi fisik.
-   Tanggal pembelian atau pencatatan unit.
-   Informasi garansi.
-   Status ketersediaan.
-   Riwayat pemeriksaan dan servis.

**Catatan implementasi:** pada tahap awal, data kondisi dan servis dapat
diperbarui melalui input admin. Dengan demikian, Digital Twin tetap
dapat didemonstrasikan tanpa harus menggunakan sensor khusus. Perubahan
data ditampilkan pada profil unit digital setelah admin menyimpan
pembaruan.

### d. Fitur Tambahan

-   Pencarian dan filter produk berdasarkan kategori, merek, dan harga.
-   Wishlist atau daftar produk favorit.
-   Rating dan ulasan pelanggan.
-   Notifikasi perubahan status pesanan.
-   Riwayat transaksi pelanggan.
-   Pengingat masa garansi yang akan berakhir.
-   Peringatan unit yang membutuhkan pemeriksaan atau servis.
-   Riwayat perubahan status setiap unit jam tangan.

### e. Catatan Risiko

-   **Keaslian produk:** informasi merek, nomor seri, dan bukti produk
    harus diperiksa admin agar deskripsi tidak menyesatkan.
-   **Keamanan akun:** autentikasi dan pengelolaan akses pelanggan serta
    admin harus diterapkan dengan baik.
-   **Kesalahan stok:** sistem harus mencegah unit yang sama dipesan
    oleh lebih dari satu pelanggan pada waktu bersamaan.
-   **Ketepatan Digital Twin:** admin harus memperbarui kondisi, status,
    dan riwayat servis sesuai keadaan unit fisik.
-   **Pembayaran:** untuk tahap pengembangan dan demo, pembayaran dapat
    menggunakan simulasi atau payment gateway sandbox.
-   **Privasi data:** informasi pribadi pelanggan dan data transaksi
    harus dilindungi dan hanya dapat diakses oleh pihak yang berwenang.

## 5. Tech Stack (Rencana Awal)

  Komponen             Teknologi
  -------------------- --------------------------------------------------
  Platform             Android
  Framework aplikasi   Flutter atau Android Studio (Kotlin)
  UI/UX Design         Figma
  Backend              Firebase atau REST API
  Database             Firebase Firestore atau MySQL
  Autentikasi          Firebase Authentication atau autentikasi backend
  Pembayaran           Simulasi pembayaran atau payment gateway sandbox
  Version Control      Git dan GitHub
  Komunikasi data      Firebase SDK atau HTTP REST API

Teknologi final akan ditentukan berdasarkan kesepakatan tim dan
kemampuan implementasi.

## 6. Rencana Sprint

  -----------------------------------------------------------------------
  Sprint                  Fokus                   Target Pekerjaan
  ----------------------- ----------------------- -----------------------
  Sprint 1                Analisis & perancangan  Menentukan kebutuhan
                                                  sistem, aktor, alur
                                                  aplikasi, wireframe,
                                                  arsitektur, dan
                                                  struktur repository

  Sprint 2                UI & autentikasi        Membuat tampilan
                                                  Android, halaman
                                                  login/register,
                                                  beranda, serta navigasi
                                                  utama

  Sprint 3                Fitur toko              Membuat katalog,
                                                  pencarian, filter,
                                                  detail produk,
                                                  keranjang, dan checkout

  Sprint 4                Digital Twin & admin    Membuat pengelolaan
                                                  unit jam, profil
                                                  Digital Twin, status
                                                  unit, kondisi, garansi,
                                                  dan riwayat servis

  Sprint 5                Integrasi & pengujian   Mengintegrasikan
                                                  transaksi dan
                                                  notifikasi, melakukan
                                                  pengujian, memperbaiki
                                                  bug, serta menyiapkan
                                                  APK untuk demo
  -----------------------------------------------------------------------

## 7. Anggota Tim

  Nama                        NIM         Peran
  --------------------------- ----------- -----------
  Muhammad Yuda Buana Ambia   240504141   Anggota 1
  M Farhan Al Faiz            240504135   Anggota 2
  Muhammad Rilky              240504147   Anggota 3
  Rafi Nauval Abiyu           250504024   Anggota 4

Pembagian tugas teknis setiap anggota dapat ditentukan kembali melalui
kesepakatan tim pada awal sprint.

------------------------------------------------------------------------

**NEXUS Time --- Where Every Watch Meets Its Digital Twin.**
