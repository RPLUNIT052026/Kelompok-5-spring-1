# RANCANGAN UX/UI — Toko Akun Game Online

## 1. Tujuan Desain
Merancang antarmuka toko akun game yang mudah dipakai pembeli untuk mencari dan membeli akun, serta memudahkan admin mengelola akun dan pesanan. Status akun dan pesanan ditampilkan dengan indikator visual yang jelas (warna, label, tombol).

## 2. User Flow

### Alur Pembeli
```mermaid
graph LR
    A[Login] --> B[Katalog]
    B --> C[Detail Akun]
    C --> D[Pembayaran]
    D --> E[Data Login Akun]
    B --> F[Riwayat Pesanan]
    E --> F
```

### Alur Admin
```mermaid
graph LR
    A[Login] --> B[Dashboard Admin]
    B --> C[Kelola Akun]
    B --> D[Daftar Pesanan]
```

## 3. Wireframe Halaman

### A. Halaman Login

```
┌──────────────────────────────────────┐
│                                      │
│            TOKO AKUN GAME            │
│                                      │
│   ┌────────────────────────────────┐ │
│   │ Username                       │ │
│   └────────────────────────────────┘ │
│   ┌────────────────────────────────┐ │
│   │ Password                       │ │
│   └────────────────────────────────┘ │
│   ┌────────────────────────────────┐ │
│   │           [ LOGIN ]            │ │
│   └────────────────────────────────┘ │
│                                      │
└──────────────────────────────────────┘
```

### B. Halaman Katalog (Pembeli)

```
┌──────────────────────────────────────────────────────────────────┐
│  Toko Akun Game     [Katalog] [Riwayat] [Logout]                 │
├──────────────────────────────────────────────────────────────────┤
│  Cari: [ ____________ ]  Game: [Semua v]  [ Cari ]               │
│  Harga: [Semua v]  Urutkan: [Terbaru v]                          │
├──────────────────────────────────────────────────────────────────┤
│  ┌──────────────────┐ ┌──────────────────┐ ┌──────────────────┐  │
│  │   MLBB Mythic    │ │  PUBG Conqueror  │ │   PUBG Ace Set   │  │
│  │  Mobile Legends  │ │   PUBG Mobile    │ │   PUBG Mobile    │  │
│  │   Rank: Mythic   │ │ Rank: Conqueror  │ │    Rank: Ace     │  │
│  │    Rp 500.000    │ │    Rp 750.000    │ │    Rp 400.000    │  │
│  │    [ Detail ]    │ │    [ Detail ]    │ │    [ Detail ]    │  │
│  └──────────────────┘ └──────────────────┘ └──────────────────┘  │
│                                                                  │
│        < 1  2  3  4 >                                            │
└──────────────────────────────────────────────────────────────────┘
```

### C. Halaman Detail Akun

```
┌──────────────────────────────────────────────────────────────────┐
│  <- Kembali          DETAIL AKUN                                 │
├──────────────────────────────────────────────────────────────────┤
│  Judul     : Akun Mythic Full Skin                               │
│  Game      : Mobile Legends                                      │
│  Rank      : Mythic Glory                                        │
│  Harga     : Rp 500.000                                          │
│  Status    : [ Tersedia ]                                        │
│                                                                  │
│  Deskripsi :                                                     │
│  - 85 hero, 120 skin                                             │
│  - Emblem level maksimal                                         │
│  - Login via Moonton, bisa ganti email                           │
├──────────────────────────────────────────────────────────────────┤
│                              [ BELI SEKARANG ]                   │
└──────────────────────────────────────────────────────────────────┘
```

### D. Halaman Pembayaran

```
┌──────────────────────────────────────────────────────────────────┐
│  <- Kembali          PEMBAYARAN                                  │
├──────────────────────────────────────────────────────────────────┤
│  RINGKASAN PESANAN                                               │
│  Akun    : Akun Mythic Full Skin                                 │
│  Game    : Mobile Legends                                        │
│  Harga   : Rp 500.000                                            │
│                                                                  │
│  METODE PEMBAYARAN (SIMULASI)                                    │
│  (o) Transfer Bank    ( ) E-Wallet    ( ) QRIS                   │
│                                                                  │
│  TOTAL BAYAR : Rp 500.000                                        │
├──────────────────────────────────────────────────────────────────┤
│                     [ BATAL ]   [ BAYAR SEKARANG ]               │
└──────────────────────────────────────────────────────────────────┘
```

### E. Halaman Pembayaran Berhasil

```
┌──────────────────────────────────────────────────────────────────┐
│  PEMBAYARAN BERHASIL                                             │
├──────────────────────────────────────────────────────────────────┤
│  No. Pesanan : #ORD-0012                                         │
│  Status      : [ PAID ]                                          │
│                                                                  │
│  DATA LOGIN AKUN                                                 │
│  Username : akun_mythic_01                                       │
│  Password : ************      [ Tampilkan ]                      │
│                                                                  │
│  Catatan: segera ganti email dan password akun.                  │
├──────────────────────────────────────────────────────────────────┤
│                [ Lihat Riwayat ]   [ Kembali ke Katalog ]        │
└──────────────────────────────────────────────────────────────────┘
```

### F. Halaman Riwayat Pesanan

```
┌──────────────────────────────────────────────────────────────────┐
│  <- Kembali          RIWAYAT PESANAN                             │
├──────────────────────────────────────────────────────────────────┤
│  Filter: [Status v] [Tanggal] [Cari]                             │
├──────────────────────────────────────────────────────────────────┤
│  No. Order | Akun               | Total    | Status | Aksi       │
│  ----------|--------------------|----------|--------|------      │
│  #ORD-0012 | Mythic Full Skin   | 500.000  | Paid   | Lihat      │
│  #ORD-0011 | PUBG Conqueror     | 750.000  | Pending| Bayar      │
│  #ORD-0009 | PUBG Ace Set       | 400.000  | Batal  | Lihat      │
└──────────────────────────────────────────────────────────────────┘
```

### G. Dashboard Admin

```
┌──────────────────────────────────────────────────────────────────┐
│  Dashboard Admin     [Kelola Akun] [Pesanan] [Logout]            │
├──────────────────────────────────────────────────────────────────┤
│  ┌─────────────┐ ┌─────────────┐ ┌─────────────┐ ┌─────────────┐ │
│  │     AKUN    │ │     AKUN    │ │    ORDER    │ │    TOTAL    │ │
│  │   TERSEDIA  │ │   TERJUAL   │ │   PENDING   │ │  PENDAPATAN │ │
│  │      24     │ │      58     │ │      3      │ │   Rp 32 jt  │ │
│  └─────────────┘ └─────────────┘ └─────────────┘ └─────────────┘ │
├──────────────────────────────────────────────────────────────────┤
│  Pesanan Terbaru                                                 │
│  No. Order | Pembeli | Akun             | Status                 │
│  ----------|---------|------------------|--------                │
│  #ORD-0012 | budi    | Mythic Full Skin | Paid                   │
│  #ORD-0011 | sari    | PUBG Conqueror   | Pending                │
└──────────────────────────────────────────────────────────────────┘
```

### H. Halaman Kelola Akun (Admin)

```
┌──────────────────────────────────────────────────────────────────┐
│  <- Dashboard        KELOLA AKUN GAME                            │
├──────────────────────────────────────────────────────────────────┤
│  [ + Tambah Akun ]                Cari: [ __________ ]           │
├──────────────────────────────────────────────────────────────────┤
│  ID | Judul              | Game | Harga   | Status   | Aksi      │
│  ---|--------------------|------|---------|----------|-----------│
│  01 | Mythic Full Skin   | MLBB | 500.000 | Tersedia | Edit Hapus│
│  02 | PUBG Conqueror     | PUBG | 750.000 | Terjual  | Edit Hapus│
├──────────────────────────────────────────────────────────────────┤
│  FORM AKUN BARU                                                  │
│  Game      : [ Mobile Legends / PUBG Mobile v ]                  │
│  Judul     : [ ______________________ ]                          │
│  Rank      : [ ______________________ ]                          │
│  Harga     : [ Rp ________ ]                                     │
│  Deskripsi : [ ______________________ ]                          │
│  Kredensial: [ username ] [ password ]                           │
│                                                                  │
│                        [ BATAL ]  [ SIMPAN AKUN ]                │
└──────────────────────────────────────────────────────────────────┘
```

## 4. Palet Warna & Indikator

| Kondisi | Warna | Kode | Keterangan |
|---------|-------|------|------------|
| Tersedia / Paid | Hijau | #28a745 | Akun bisa dibeli / pembayaran berhasil |
| Pending | Kuning | #ffc107 | Menunggu pembayaran |
| Terjual / Batal | Merah | #dc3545 | Akun sudah terjual / pesanan dibatalkan |

## 5. Komponen UI Utama

| Komponen | Fungsi |
|----------|--------|
| Navbar | Navigasi ke Katalog, Riwayat, dan Logout |
| Kolom Cari & Filter | Mencari akun berdasarkan nama, game, dan harga |
| Card Akun | Menampilkan judul, game, rank, harga, dan tombol Detail |
| Badge Status | Label warna untuk status akun dan pesanan |
| Tombol Beli | Memulai proses order dan pembayaran |
| Form Pembayaran | Memilih metode bayar (simulasi) dan konfirmasi |
| Panel Data Login | Menampilkan kredensial akun setelah status paid |
| Tabel Riwayat | Daftar pesanan pembeli beserta statusnya |
| Card Statistik Admin | Ringkasan akun tersedia, terjual, order pending, dan pendapatan |
| Form Kelola Akun | Tambah dan ubah data akun game oleh admin |
