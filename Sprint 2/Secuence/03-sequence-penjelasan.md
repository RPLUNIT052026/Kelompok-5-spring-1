> Sprint 2 · [Context](01-context.md) | [Use Case](02-usecase.md) | **Sequence (penjelasan)** | [Class](04-class.md) | [State](05-state.md) · [README](README.md)

# Penjelasan Sequence Diagram — GameVault

> Diagram untuk tiap skenario ada di file terpisah: [03-sequence.md](03-sequence.md)

### Peserta yang muncul di diagram
| Peserta | Jenis | Peran |
|---|---|---|
| Pembeli | Aktor | Pengguna yang mencari dan membeli akun game |
| Admin | Aktor | Pengelola toko: verifikasi pembayaran dan pengiriman akun |
| Antarmuka Web / Dashboard Admin | Boundary | Halaman yang dilihat dan dipakai pengguna |
| Backend | Control | Menjalankan aturan bisnis dan mengatur alur proses |
| Database | Entity | Menyimpan data (users, akun_game, pesanan, pembayaran, ulasan) |
| Payment Gateway | Sistem eksternal | Layanan pembayaran pihak ketiga |
| Layanan Email | Sistem eksternal | Pengirim notifikasi dan data akun |

### 3.1 Skenario A: Pembelian dan Pembayaran oleh Pembeli

**Tujuan.** Menunjukkan bagaimana pembeli mengubah isi keranjang menjadi pesanan dan mengirim bukti pembayaran.

**Prasyarat.** Pembeli sudah login dan keranjang berisi minimal satu akun game.

**Alur utama (akun tersedia)**
1. Langkah 1-2: pembeli menekan Checkout di keranjang, lalu antarmuka mengirim daftar akun ke backend.
2. Langkah 3-4: backend memeriksa ke database apakah semua akun masih berstatus Tersedia. Pengecekan ini mencegah satu akun dibeli dua orang.
3. Langkah 5-6: karena semuanya tersedia, backend menyimpan Pesanan beserta DetailPesanan, lalu mengubah status akun menjadi Dipesan agar tidak muncul di katalog.
4. Langkah 7-8: backend meminta instruksi pembayaran ke Payment Gateway dan menerima balasannya.
5. Langkah 9-10: pesanan berstatus Menunggu pembayaran, dan pembeli melihat instruksi serta total yang harus dibayar.
6. Langkah 11-13: pembeli membayar, mengunggah bukti, dan backend menyimpan data Pembayaran dengan status Menunggu verifikasi.
7. Langkah 14-15: sistem memberi tahu pembeli bahwa bukti sudah terkirim.

**Alur alternatif (ada akun tidak tersedia).** Langkah 16-17: backend menolak pesanan karena akun sudah terjual atau sedang dipesan orang lain, lalu pembeli diminta memilih akun lain. Tidak ada data pesanan yang tersimpan.

**Hasil.** Pesanan tersimpan dan menunggu verifikasi admin. Status pesanan berubah dari *Menunggu pembayaran* ke *Menunggu verifikasi* (lihat State Diagram), dan status akun menjadi *Dipesan*.

**Terkait:** use case Checkout pesanan dan Bayar dan upload bukti; class Pesanan, DetailPesanan, Pembayaran, AkunGame.

### 3.2 Skenario B: Verifikasi dan Pengiriman Akun oleh Admin

**Tujuan.** Menunjukkan proses admin memeriksa pembayaran lalu mengirim data akun ke pembeli sehingga transaksi selesai.

**Prasyarat.** Admin sudah login dan ada pembayaran berstatus Menunggu verifikasi (hasil dari Skenario A).

**Alur utama**
1. Langkah 1-5: admin membuka daftar pembayaran masuk, dan backend mengambil data pembayaran berstatus menunggu dari database lalu menampilkannya.
2. Langkah 6-8: admin memeriksa bukti transfer, menekan Verifikasi, dan backend mengubah status pembayaran dan pesanan menjadi Terverifikasi.
3. Langkah 9-10: backend meminta Layanan Email mengirim notifikasi, dan pembeli menerima email bahwa pembayarannya sudah terverifikasi.
4. Langkah 11-14: admin menekan Kirim data akun. Backend mencatat pengiriman, mengubah status pesanan menjadi Selesai, dan mengubah status akun menjadi Terjual.
5. Langkah 15-17: backend mengirim data akun (misalnya email dan password akun game) ke pembeli lewat email, lalu admin mendapat konfirmasi berhasil.

**Alur alternatif.** Jika bukti tidak valid, admin menekan Tolak. Pesanan berstatus Ditolak dan pembeli diminta mengunggah ulang (lihat State Diagram). Alur ini tidak digambar agar diagram tetap ringkas.

**Hasil.** Pembeli sudah menerima akun, pesanan Selesai, dan akun tercatat Terjual sehingga tidak bisa dijual lagi.

**Terkait:** use case Verifikasi pembayaran dan Kirim data akun; class Admin, Pembayaran, Pesanan, AkunGame.

### 3.3 Skenario C: Registrasi Pembeli

**Tujuan.** Menunjukkan proses pengunjung membuat akun pembeli.

**Prasyarat.** Pengunjung belum memiliki akun dan membuka halaman registrasi.

**Alur utama (email belum terdaftar)**
1. Langkah 1-2: pengunjung mengisi nama, email, dan password, lalu data dikirim ke backend.
2. Langkah 3-4: backend memeriksa ke database apakah email sudah dipakai.
3. Langkah 5: karena belum ada, backend menyimpan user baru. Password disimpan dalam bentuk hash, bukan teks asli, demi keamanan.
4. Langkah 6: backend meminta Layanan Email mengirim email selamat datang.
5. Langkah 7-8: sistem memberi tahu registrasi berhasil dan mengarahkan pengguna ke halaman login.

**Alur alternatif (email sudah terdaftar).** Langkah 9-10: backend menolak registrasi dan menampilkan pesan bahwa email sudah dipakai, disertai saran untuk login.

**Hasil.** Satu data user dengan peran Pembeli tersimpan dan siap dipakai untuk login (Skenario D).

**Terkait:** use case Registrasi; class User dan Pembeli (method `register()`).

### 3.4 Skenario D: Login (Pembeli dan Admin)

**Tujuan.** Menunjukkan proses autentikasi dan pengarahan pengguna ke halaman yang sesuai peran, baik Pembeli maupun Admin.

**Prasyarat.** Pengguna sudah terdaftar.

**Alur utama (kredensial valid)**
1. Langkah 1-2: pengguna mengisi email dan password, lalu data dikirim ke backend.
2. Langkah 3-4: backend mengambil data user dari database, yaitu hash password dan peran (role).
3. Langkah 5: backend membandingkan password dengan hash. Jika cocok, backend mengirim token sesi beserta role.
4. Langkah 6 atau 7: antarmuka memakai role itu untuk menentukan tujuan. Admin diarahkan ke dashboard admin (langkah 6), sedangkan Pembeli ke beranda katalog (langkah 7).

**Alur alternatif (kredensial salah).** Langkah 8-9: backend menolak login dan antarmuka menampilkan pesan "email atau password salah". Pesan sengaja tidak menyebut mana yang salah agar tidak membantu penyerang menebak akun.

**Hasil.** Pengguna memiliki sesi aktif dan hanya melihat menu sesuai perannya.

**Terkait:** use case Login; class User (method `login()`) dan pewarisan ke Pembeli dan Admin.

### 3.5 Skenario E: Beri Ulasan

**Tujuan.** Menunjukkan proses pembeli memberi ulasan, dengan aturan bahwa hanya pembeli yang benar-benar sudah menerima akun yang boleh mengulas.

**Prasyarat.** Pembeli sudah login dan memiliki riwayat pesanan.

**Alur utama (ulasan diizinkan)**
1. Langkah 1-2: pembeli membuka riwayat pesanan, memilih Beri ulasan, lalu mengirim akun, rating, dan komentar ke backend.
2. Langkah 3-4: backend memeriksa ke database apakah pesanan itu milik pembeli tersebut dan berapa statusnya.
3. Langkah 5-6: karena pesanan berstatus Selesai dan belum pernah diulas, backend menyimpan ulasan lalu memperbarui rating rata-rata akun.
4. Langkah 7-8: sistem mengonfirmasi ulasan tersimpan, dan ulasan tampil di halaman detail akun.

**Alur alternatif (ulasan ditolak).** Langkah 9-10: jika pesanan belum selesai atau sudah pernah diulas, backend menolak dan antarmuka menampilkan alasannya. Aturan ini menjaga ulasan tetap jujur dan mencegah ulasan ganda.

**Hasil.** Satu data Ulasan baru tersimpan dan rating rata-rata akun terbarui.

**Terkait:** use case Beri ulasan; class Ulasan, Pesanan, AkunGame; pesanan harus berstatus Selesai (State Diagram).
