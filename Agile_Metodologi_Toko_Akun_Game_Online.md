# 🎮 Proposal Proyek — Toko Akun Game Online

## 1. Latar Belakang

Jual beli akun game, khususnya **PUBG Mobile** dan **Mobile Legends**, sangat diminati, tetapi transaksinya sering dilakukan lewat grup media sosial atau chat pribadi. Hal ini rawan penipuan (akun ditarik kembali, data akun tidak sesuai deskripsi, pembayaran tidak dikirim), tidak ada standar informasi akun, dan sulit dilacak riwayat transaksinya. Diperlukan platform toko akun game yang terpercaya, terstruktur, dan aman bagi penjual maupun pembeli.

## 2. Tujuan

- Menyediakan katalog akun game dengan informasi lengkap (game, level/rank, skin/item, harga, screenshot)
- Ruang lingkup game dibatasi pada dua game saja: **PUBG Mobile** dan **Mobile Legends**
  - PUBG Mobile: level akun, tier/rank, jumlah skin senjata & outfit, item langka, royale pass
  - Mobile Legends: rank/tier, jumlah hero, jumlah skin (termasuk skin Epic/Legend/Collector), win rate, emblem
- Menyediakan sistem transaksi yang aman (verifikasi, rekening bersama/escrow, garansi akun)
- Memudahkan pencarian dan pemfilteran akun sesuai kebutuhan pembeli
- Memberikan notifikasi otomatis untuk status pesanan dan pembayaran

## 3. Keputusan Sprint 1

### a. Agile Methodology
- Metodologi yang digunakan: **Scrum**
- Durasi sprint: ± 1 bulan per sprint (mengikuti timeline Sp1–Sp5)
- Role tim: 1 orang sebagai **Scrum Master** (koordinator), sisanya **Development Team**
- Tools tracking task: **GitHub Projects**

### b. UX Design
- Antarmuka dirancang dengan wireframe sederhana (Figma/sketsa) sebelum masuk development
- Elemen utama halaman:
  - Beranda: pilihan kategori (PUBG Mobile / Mobile Legends), akun terlaris, dan pencarian
  - Katalog: filter game, rentang harga, rank/tier, level, jumlah skin (dan jumlah hero untuk Mobile Legends)
  - Detail akun: galeri screenshot, spesifikasi akun, harga, badge "Terverifikasi", tombol beli
  - Checkout & pembayaran: pilihan metode bayar, status pesanan
  - Notifikasi/alert (contoh: banner "Pembayaran berhasil, akun sedang diproses")
- Alur pengguna: buka toko → cari & filter akun → lihat detail → checkout → bayar → terima data akun → konfirmasi selesai

### c. Project Setup
- Struktur folder awal repository:
  ```
  toko-akun-game/
  ├── backend/        # API server (Flask/Node.js)
  ├── frontend/       # Halaman web (HTML/JS/CSS)
  ├── database/       # Skema & seed data dummy
  ├── docs/           # Wireframe, proposal, dokumentasi API
  └── README.md
  ```
- Tech stack awal: Python Dummy Data Generator (data akun contoh), Flask/Node.js (backend), HTML/JS + Bootstrap/Tailwind (frontend awal)

## 4. Arsitektur Sistem

### a. Pengguna & Peran (Sumber Data)
- **Pembeli:** mencari, membeli, dan mengonfirmasi akun
- **Penjual:** mengunggah akun, menetapkan harga, mengelola pesanan
- **Admin:** memverifikasi akun, memantau transaksi, menangani sengketa

### b. Sistem Toko (Representasi Digital)
- **Frontend web:** katalog, detail akun, keranjang/checkout, dashboard penjual & admin
- **Backend REST API:** autentikasi, manajemen akun game, pesanan, dan pembayaran
- **Opsi data awal (tanpa data nyata):** Dummy Data Generator berbasis Python untuk mengisi katalog akun PUBG Mobile dan Mobile Legends pada tahap pengembangan

### c. Fitur Tambahan
- Sistem rekening bersama (escrow): dana ditahan sampai pembeli mengonfirmasi akun sesuai deskripsi
- Rating & ulasan penjual
- Rekomendasi akun berdasarkan game dan anggaran pembeli
- Notifikasi otomatis (email/WhatsApp/in-app) untuk status pesanan

### d. Catatan Risiko
- **Kebijakan game:** publisher PUBG Mobile (Krafton/Tencent) dan Mobile Legends (Moonton) dapat melarang jual beli akun dalam Ketentuan Layanan mereka; tim perlu meninjau kebijakan kedua game dan mencantumkan disclaimer yang jelas
- **Keamanan data:** kredensial akun game disimpan terenkripsi dan hanya ditampilkan ke pembeli setelah pembayaran terkonfirmasi
- **Penipuan:** verifikasi identitas penjual, moderasi listing, dan mekanisme pelaporan/sengketa

## 5. Tech Stack (Rencana Awal)

| Komponen | Teknologi |
|----------|-----------|
| Sumber Data | Python Dummy Data Generator / input penjual via form |
| Backend | (isi sesuai kesepakatan tim, contoh: Node.js/Express atau Flask) |
| Database | (contoh: MySQL/PostgreSQL/Firebase) |
| Frontend | HTML + JS + Bootstrap/Tailwind (atau React) |
| Pembayaran | Payment gateway sandbox (contoh: Midtrans/Xendit) |
| Komunikasi | HTTP REST API |

## 6. Rencana Sprint

| Sprint | Bulan | Fokus |
|--------|-------|-------|
| Sp1 | September | Tentukan arsitektur & tech stack, UX wireframe, setup repo |
| Sp2 | October | Backend (autentikasi, CRUD akun game) + katalog & detail akun dasar |
| Sp3 | November | Checkout, pembayaran (sandbox), escrow, notifikasi, refinement |
| Sp4 | December | Testing, dashboard penjual/admin, rating & ulasan, upgrade tampilan |
| Sp5 | December | Final testing, bug fixing, persiapan demo Expo |

## 7. Anggota Tim

| Nama | NIM | Peran |
|------|-----|-------|
| Muhammad Yuda Buana Ambia | 240504141 | Anggota 1 |
| M Farhan Al Faiz | 240504135 | Anggota 2 |
| Muhammad Rilky | 240504147 | Anggota 3 |
| Rafi Nauval Abiyu | 250504024 | Anggota 4 |
