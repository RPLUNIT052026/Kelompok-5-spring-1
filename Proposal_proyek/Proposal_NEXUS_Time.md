# 📄 Proposal Proyek — Digital Twin NEXUS Time (Aplikasi Jual-Beli Jam Tangan Pria & Wanita)

## 1. Latar Belakang

Pembeli jam tangan sering kesulitan memastikan ketersediaan dan kondisi unit yang sebenarnya, apalagi untuk jam preloved. Di sisi penjual, stok biasanya hanya dicatat sebagai angka, sehingga masa garansi, jadwal servis, dan kondisi tiap unit mudah terlewat. Diperlukan aplikasi yang merepresentasikan setiap jam tangan fisik sebagai kembaran digital (digital twin) agar pembeli melihat informasi yang transparan dan admin dapat memantau semua unit dengan cepat.

## 2. Tujuan

- Membangun aplikasi Android (APK) jual-beli jam tangan pria dan wanita
- Merepresentasikan tiap unit jam fisik dalam bentuk digital twin (nomor seri, kondisi, garansi, status)
- Menyinkronkan status unit secara real-time mengikuti transaksi (tersedia, di-booking, terjual, perlu servis)
- Memberikan peringatan otomatis kepada admin saat garansi hampir habis, jadwal servis tiba, atau kondisi unit menurun

## 3. Keputusan Sprint 1

### a. Agile Methodology
- Metodologi yang digunakan: **Scrum**
- Durasi sprint: ± 2 minggu per sprint (5 sprint, Sp1–Sp5)
- Role tim: 1 orang sebagai **Scrum Master** (koordinator), sisanya **Development Team**
- Tools tracking task: **GitHub Projects** (atau Trello)

### b. UX Design
- Aplikasi dirancang dengan wireframe mobile sederhana (Figma/sketsa) sebelum masuk development
- Elemen utama aplikasi:
  - Beranda katalog dengan filter Pria / Wanita / Unisex dan tipe (analog, digital, smartwatch)
  - Halaman detail jam dengan profil twin (nomor seri, skor kondisi, garansi, status unit)
  - Keranjang, checkout, dan pembayaran (QRIS, E-Wallet, Transfer Bank — simulasi)
  - Dashboard twin untuk admin dengan kode warna status (hijau tersedia, kuning di-booking, abu terjual, merah perlu servis)
  - Notifikasi/alert (contoh: banner "Garansi unit SN-0042 tersisa 12 hari")
- Alur pengguna: buka aplikasi → lihat katalog dan profil twin → checkout → bayar → pantau status pesanan; admin: login → lihat dashboard twin → terima peringatan → perbarui kondisi/status

### c. Project Setup
- Struktur folder awal repository:
  ```
  /docs        → dokumen sprint, proposal, spesifikasi sistem
  /app         → kode aplikasi Android (Flutter/Kotlin)
  /firebase    → aturan Firestore dan data awal (seed data)
  /simulasi    → script/data dummy untuk simulasi kondisi unit
  README.md
  ```
- Tech stack awal: Flutter (atau Android Studio Kotlin), Firebase Authentication, Cloud Firestore, data simulasi

## 4. Arsitektur Sistem

### a. Physical Object (Sumber Data)
- **Jam tangan fisik di toko** (atau data nyata dari toko/teman sebagai studi kasus)
- Data fisik diinput admin: nomor seri, kondisi (baru/preloved), skor kondisi, tanggal garansi, jadwal servis
- Tanpa sensor: perubahan kondisi disimulasikan lewat tombol admin dan data dummy

### b. Digital Twin (Representasi Digital)
- Dokumen **`unit_jam`** di Firestore: satu jam fisik = satu dokumen twin
- Tampilan twin di aplikasi: profil unit untuk pelanggan dan dashboard seluruh unit untuk admin
- Status twin berubah otomatis dari transaksi dan tampil real-time (Firestore *snapshot listener*)
- Siklus status: Tersedia → Di-booking → Terjual, atau Tersedia → Perlu Servis → Tersedia

### c. Fitur Tambahan
- Pengunci unit sementara saat checkout agar tidak terjadi pemesanan ganda
- Peringatan otomatis: garansi kurang dari 30 hari, jadwal servis tiba, skor kondisi di bawah ambang
- Riwayat servis per unit (`log_servis`)

## 5. Tech Stack (Rencana Awal)

| Komponen | Teknologi |
|----------|-----------|
| Sumber Data | Input admin + data simulasi (tombol simulasi dan script dummy) |
| Aplikasi Mobile | Flutter (Dart) / Android Studio (Kotlin) |
| Backend | Firebase Authentication + Cloud Firestore |
| Database | Cloud Firestore (koleksi: users, jam, unit_jam, keranjang, pesanan, detail_pesanan, pembayaran, log_servis) |
| Dashboard Twin | Halaman admin di dalam aplikasi (grid unit berwarna + daftar peringatan) |
| Komunikasi | HTTPS, Firebase SDK (real-time sync) |
| Pembayaran | Simulasi Payment Gateway (QRIS, E-Wallet, Transfer Bank) |

## 6. Rencana Sprint

| Sprint | Waktu | Fokus |
|--------|-------|-------|
| Sp1 | Minggu 1–2 | Analisis kebutuhan, user story, flowchart, wireframe mobile, ERD, setup repo dan Firebase |
| Sp2 | Minggu 3–4 | Autentikasi (login/registrasi), UI katalog dan detail jam, struktur data twin (`unit_jam`) |
| Sp3 | Minggu 5–6 | Keranjang, checkout, simulasi pembayaran, profil twin dan status unit real-time |
| Sp4 | Minggu 7–8 | Dashboard twin admin, peringatan otomatis (garansi, servis, kondisi), tombol simulasi, kelola data jam dan unit |
| Sp5 | Minggu 9–10 | Final testing, bug fixing, build APK, persiapan demo dan presentasi |

## 7. Anggota Tim

| Nama | NIM | Peran |
|------|-----|-------|
| M Farhan Al Faiz | 240504135 | Ketua / Scrum Master |
| Muhammad Yuda Buana Ambia | 240504141 | UI/UX Designer |
| Muhammad Rilky | 240504147 | Mobile Developer |
| Rafi Nauval Abiyu | 250504024 | Backend / Database |
