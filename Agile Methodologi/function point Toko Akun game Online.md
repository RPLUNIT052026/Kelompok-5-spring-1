# 📊 Dokumen Perhitungan Function Point & Product Backlog
## Toko Akun Game Online (PUBG Mobile & Mobile Legends)

---

## A. Perhitungan Function Point (FP)

Function Point digunakan untuk mengestimasi ukuran/kompleksitas aplikasi berdasarkan fitur yang direncanakan, sebelum development dimulai.

### 1. Identifikasi Komponen Fungsional

| No | Komponen | Tipe | Kompleksitas | Bobot |
|----|----------|------|----------------|-------|
| 1 | Registrasi & login pengguna (pembeli/penjual/admin) | External Input (EI) | Low | 3 |
| 2 | Input listing akun game oleh penjual (game, rank, skin, harga, screenshot) | External Input (EI) | Average | 4 |
| 3 | Pembuatan pesanan & checkout pembeli | External Input (EI) | Average | 4 |
| 4 | Verifikasi akun & konfirmasi transaksi oleh admin | External Input (EI) | Low | 3 |
| 5 | Notifikasi otomatis status pesanan & pembayaran | External Output (EO) | Average | 5 |
| 6 | Dashboard penjualan & laporan transaksi (penjual/admin) | External Output (EO) | Average | 5 |
| 7 | Katalog akun dengan pencarian & filter (game, rank, harga, skin) | External Inquiry (EQ) | Average | 4 |
| 8 | Halaman detail akun (spesifikasi & galeri screenshot) | External Inquiry (EQ) | Low | 3 |
| 9 | Riwayat transaksi pengguna | External Inquiry (EQ) | Low | 3 |
| 10 | Data pengguna & akun game (database) | Internal Logical File (ILF) | Average | 10 |
| 11 | Data pesanan & transaksi (database) | Internal Logical File (ILF) | Average | 10 |
| 12 | Koneksi ke payment gateway eksternal (Midtrans/Xendit sandbox) | External Interface File (EIF) | Average | 7 |

### 2. Tabel Ringkasan Unadjusted Function Point (UFP)

| Tipe Komponen | Jumlah | Bobot Rata-rata | Total |
|----------------|--------|------------------|-------|
| External Input (EI) | 4 | 3,5 | 14 |
| External Output (EO) | 2 | 5 | 10 |
| External Inquiry (EQ) | 3 | 3,3 | 10 |
| Internal Logical File (ILF) | 2 | 10 | 20 |
| External Interface File (EIF) | 1 | 7 | 7 |
| **Total UFP** | | | **61** |

### 3. Value Adjustment Factor (VAF) — Sederhana

Untuk proyek skala kelas/kuliah, VAF bisa diasumsikan netral (VAF = 1.0), karena belum ada 14 faktor kompleksitas teknis yang dinilai detail. Jika ingin lebih presisi, VAF dapat dihitung dari 14 General System Characteristics (GSC), skala 0–5 tiap faktor.

**Rumus:**
```
FP = UFP x VAF
FP = 61 x 1.0 = 61
```

> Catatan: Angka-angka di atas adalah estimasi awal. Sesuaikan kembali dengan fitur final yang disepakati tim sebelum development dimulai.

---

## B. Product Backlog (Keseluruhan Proyek)

Daftar fitur/User Stories yang direncanakan untuk seluruh proyek (Sp1–Sp5):

| ID | User Story | Prioritas |
|----|-----------|-----------|
| US-01 | Sebagai pembeli, saya ingin melihat katalog akun PUBG Mobile dan Mobile Legends agar bisa memilih akun yang sesuai | Tinggi |
| US-02 | Sebagai pembeli, saya ingin mencari dan memfilter akun berdasarkan game, rank/tier, harga, dan jumlah skin | Tinggi |
| US-03 | Sebagai pembeli, saya ingin melihat detail akun (screenshot, level, skin, hero) agar yakin sebelum membeli | Tinggi |
| US-04 | Sebagai penjual, saya ingin mengunggah listing akun beserta harga dan screenshot | Tinggi |
| US-05 | Sebagai pembeli, saya ingin melakukan checkout dan pembayaran secara aman | Tinggi |
| US-06 | Sebagai pengguna, saya ingin registrasi dan login agar data dan transaksi saya aman | Sedang |
| US-07 | Sebagai pengguna, saya ingin menerima notifikasi otomatis saat status pesanan atau pembayaran berubah | Sedang |
| US-08 | Sebagai admin, saya ingin memverifikasi akun yang diunggah penjual agar terhindar dari penipuan | Sedang |
| US-09 | Sebagai pembeli, saya ingin melihat riwayat transaksi saya | Sedang |
| US-10 | Sebagai penjual/admin, saya ingin melihat dashboard penjualan dan laporan transaksi | Rendah |
| US-11 | Sebagai pembeli, saya ingin memberi rating dan ulasan untuk penjual | Rendah |
| US-12 | Sebagai pembeli, saya ingin mendapat rekomendasi akun berdasarkan game dan anggaran saya | Rendah |

---

## C. Sprint 1 Backlog

Target kerja spesifik yang harus diselesaikan pada Sprint 1 (bukan development fitur, tapi tahap persiapan):

| Task | Deskripsi | Penanggung Jawab |
|------|-----------|-------------------|
| Project Charter | Menyusun latar belakang, tujuan, ruang lingkup (PUBG Mobile & Mobile Legends), struktur tim | Ketua |
| Perhitungan Function Point | Estimasi ukuran aplikasi berdasarkan fitur yang direncanakan | Anggota 1 |
| Product Backlog & Sprint 1 Backlog | Menyusun daftar User Stories dan target Sprint 1 | Anggota 2 |
| Rancangan UX/UI | Membuat wireframe/mockup beranda, katalog, detail akun, dan checkout | Anggota 3 |
| Rancangan Sistem | Membuat flowchart/diagram arsitektur/ERD (pengguna, akun game, pesanan, transaksi) | Ketua/Anggota (dibagi) |
| Inisialisasi Kode Proyek | Setup struktur folder & boilerplate awal | Ketua |
