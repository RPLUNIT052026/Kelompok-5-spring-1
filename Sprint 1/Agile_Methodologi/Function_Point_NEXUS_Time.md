# 📊 Dokumen Perhitungan Function Point & Product Backlog
## NEXUS Time — Aplikasi Jual-Beli Jam Tangan Pria & Wanita dengan Digital Twin

---

## A. Perhitungan Function Point (FP)

Function Point digunakan untuk mengestimasi ukuran/kompleksitas aplikasi berdasarkan fitur yang direncanakan, sebelum development dimulai.

### 1. Identifikasi Komponen Fungsional

| No | Komponen | Tipe | Kompleksitas | Bobot |
|----|----------|------|----------------|-------|
| 1 | Registrasi & login pengguna (pelanggan/admin) | External Input (EI) | Low | 3 |
| 2 | Kelola data jam & unit oleh admin (nomor seri, kondisi, garansi, harga, foto) | External Input (EI) | Average | 4 |
| 3 | Tambah/ubah/hapus item keranjang | External Input (EI) | Low | 3 |
| 4 | Checkout & pembuatan pesanan (unit twin dikunci "Di-booking") | External Input (EI) | Average | 4 |
| 5 | Pembayaran simulasi, konfirmasi, dan pembatalan (status twin berubah otomatis) | External Input (EI) | Average | 4 |
| 6 | Perubahan status pesanan oleh admin | External Input (EI) | Low | 3 |
| 7 | Input inspeksi kondisi & riwayat servis unit oleh admin | External Input (EI) | Average | 4 |
| 8 | Peringatan otomatis (garansi hampir habis, jadwal servis, skor kondisi rendah) | External Output (EO) | Average | 5 |
| 9 | Dashboard digital twin admin (ringkasan unit per status, kode warna) | External Output (EO) | Average | 5 |
| 10 | Notifikasi status pesanan ke pelanggan | External Output (EO) | Low | 4 |
| 11 | Katalog jam dengan pencarian & filter (kategori pria/wanita/unisex, tipe, harga) | External Inquiry (EQ) | Average | 4 |
| 12 | Halaman detail jam & profil twin unit (real-time) | External Inquiry (EQ) | Average | 4 |
| 13 | Status & riwayat pesanan pelanggan | External Inquiry (EQ) | Low | 3 |
| 14 | Daftar pesanan masuk admin dengan pencarian & filter | External Inquiry (EQ) | Low | 3 |
| 15 | Data pengguna (database) | Internal Logical File (ILF) | Low | 7 |
| 16 | Data jam & unit jam / digital twin (database) | Internal Logical File (ILF) | Average | 10 |
| 17 | Data keranjang, pesanan, detail pesanan, & pembayaran (database) | Internal Logical File (ILF) | Average | 10 |
| 18 | Data log servis unit (database) | Internal Logical File (ILF) | Low | 7 |
| 19 | Koneksi ke payment gateway eksternal (simulasi QRIS/E-Wallet/Bank) | External Interface File (EIF) | Average | 7 |

### 2. Tabel Ringkasan Unadjusted Function Point (UFP)

| Tipe Komponen | Jumlah | Bobot Rata-rata | Total |
|----------------|--------|------------------|-------|
| External Input (EI) | 7 | 3,6 | 25 |
| External Output (EO) | 3 | 4,7 | 14 |
| External Inquiry (EQ) | 4 | 3,5 | 14 |
| Internal Logical File (ILF) | 4 | 8,5 | 34 |
| External Interface File (EIF) | 1 | 7 | 7 |
| **Total UFP** | | | **94** |

### 3. Value Adjustment Factor (VAF) — Sederhana

Untuk proyek skala kelas/kuliah, VAF bisa diasumsikan netral (VAF = 1.0), karena belum ada 14 faktor kompleksitas teknis yang dinilai detail. Jika ingin lebih presisi, VAF dapat dihitung dari 14 General System Characteristics (GSC), skala 0–5 tiap faktor.

**Rumus:**
```
FP = UFP x VAF
FP = 94 x 1.0 = 94
```

> Catatan: Angka-angka di atas adalah estimasi awal. Sesuaikan kembali dengan fitur final yang disepakati tim sebelum development dimulai.

---

## B. Product Backlog (Keseluruhan Proyek)

Daftar fitur/User Stories yang direncanakan untuk seluruh proyek (Sp1–Sp5):

| ID | User Story | Prioritas | Target Sprint |
|----|-----------|-----------|---------------|
| US-01 | Sebagai pelanggan, saya ingin melihat katalog jam tangan pria dan wanita agar bisa memilih jam yang sesuai | Tinggi | Sp2 |
| US-02 | Sebagai pelanggan, saya ingin mencari dan memfilter jam berdasarkan kategori (pria/wanita/unisex), tipe, dan harga | Tinggi | Sp2 |
| US-03 | Sebagai pelanggan, saya ingin melihat profil digital twin tiap unit (nomor seri, skor kondisi, garansi, status) agar yakin sebelum membeli | Tinggi | Sp2–Sp3 |
| US-04 | Sebagai admin, saya ingin mengelola data jam dan unit (tambah, ubah, hapus) agar katalog selalu terbaru | Tinggi | Sp2 |
| US-05 | Sebagai pelanggan, saya ingin melakukan checkout dan pembayaran secara aman (simulasi) | Tinggi | Sp3 |
| US-06 | Sebagai pengguna, saya ingin registrasi dan login agar data dan transaksi saya aman | Tinggi | Sp2 |
| US-07 | Sebagai pelanggan, saya ingin melihat status unit berubah real-time (tersedia, di-booking, terjual) agar tidak memesan unit yang sudah diambil | Tinggi | Sp3 |
| US-08 | Sebagai admin, saya ingin melihat dashboard digital twin seluruh unit dengan kode warna status | Sedang | Sp4 |
| US-09 | Sebagai admin, saya ingin menerima peringatan otomatis saat garansi hampir habis, jadwal servis tiba, atau kondisi unit menurun | Sedang | Sp4 |
| US-10 | Sebagai pengguna, saya ingin menerima notifikasi saat status pesanan berubah | Sedang | Sp4 |
| US-11 | Sebagai pelanggan, saya ingin melihat status dan riwayat pesanan saya | Sedang | Sp3 |
| US-12 | Sebagai admin, saya ingin melihat, mencari, dan mengubah status pesanan masuk | Sedang | Sp4 |
| US-13 | Sebagai admin, saya ingin tombol simulasi (ubah skor kondisi/geser tanggal) agar perilaku twin dapat didemokan tanpa sensor | Rendah | Sp4 |
| US-14 | Sebagai pelanggan, saya ingin memberi rating dan ulasan untuk jam yang saya beli | Rendah | Sp5 (opsional) |
| US-15 | Sebagai pelanggan, saya ingin mendapat rekomendasi jam berdasarkan kategori dan anggaran saya | Rendah | Sp5 (opsional) |

---

## C. Sprint 1 Backlog

Target kerja spesifik yang harus diselesaikan pada Sprint 1 (bukan development fitur, tapi tahap persiapan):

| Task | Deskripsi | Penanggung Jawab |
|------|-----------|-------------------|
| Project Charter | Menyusun latar belakang, tujuan, ruang lingkup (jam tangan pria & wanita, digital twin per unit), struktur tim | Farhan |
| Perhitungan Function Point | Estimasi ukuran aplikasi berdasarkan fitur yang direncanakan | Yuda |
| Product Backlog & Sprint 1 Backlog | Menyusun daftar User Stories dan target Sprint 1 | Rilky |
| Rancangan UX/UI | Membuat wireframe/mockup mobile: beranda katalog, detail jam & profil twin, keranjang, checkout, dan dashboard twin admin | Rafi |
| Rancangan Sistem | Membuat flowchart/diagram arsitektur/ERD (users, jam, unit_jam, keranjang, pesanan, detail_pesanan, pembayaran, log_servis) | Farhan |
| Inisialisasi Kode Proyek | Setup struktur folder, project Flutter/Kotlin, dan koneksi awal ke Firebase | Farhan |
