# FLOWCHART APLIKASI — Toko Akun Game Online

## Alur Utama Sistem (Pembeli)

```mermaid
graph TD
    A([Mulai]) --> B[Buka Aplikasi]
    B --> C[Halaman Login]
    C --> D[Input Username dan Password]
    D --> E{Validasi?}
    E -->|Tidak| F[Tampilkan Error]
    F --> C
    E -->|Ya| G{Role?}
    G -->|Admin| Z[Ke Alur Admin]
    G -->|Pembeli| H[Halaman Katalog Akun]
    H --> I[Cari atau Filter Akun]
    I --> J[Pilih Akun dan Lihat Detail]
    J --> K{Akun masih tersedia?}
    K -->|Tidak| L[Tampilkan Pesan Akun Terjual]
    L --> H
    K -->|Ya| M[Klik Beli dan Buat Order Pending]
    M --> N[Proses Pembayaran Simulasi]
    N --> O{Pembayaran sukses?}
    O -->|Tidak| P[Order Dibatalkan]
    P --> H
    O -->|Ya| Q[Update Order Paid dan Akun Terjual]
    Q --> R[Tampilkan Detail Akun Hasil Pembelian]
    R --> S[Simpan ke Riwayat Pesanan]
    S --> T{User Logout?}
    T -->|Tidak| H
    T -->|Ya| U([Selesai])
```

## Alur Admin

```mermaid
graph TD
    A([Login sebagai Admin]) --> B[Dashboard Admin]
    B --> C{Pilih Menu}
    C -->|Kelola Akun| D[Tambah, Ubah, atau Hapus Akun Game]
    C -->|Lihat Pesanan| E[Tampilkan Daftar Pesanan]
    D --> F[Validasi Data Input]
    F --> G{Data valid?}
    G -->|Tidak| H[Tampilkan Error]
    H --> D
    G -->|Ya| I[Simpan ke Database]
    E --> J[Filter Berdasarkan Status]
    I --> K{Logout?}
    J --> K
    K -->|Tidak| B
    K -->|Ya| L([Selesai])
```

## Penjelasan Alur

1. **Mulai** - Aplikasi dibuka oleh pembeli atau admin
2. **Login** - Pengguna memasukkan username dan password
3. **Validasi** - Jika gagal, tampilkan error; jika berhasil, sistem cek role
4. **Katalog** - Pembeli melihat daftar akun game, bisa dicari dan difilter berdasarkan game atau harga
5. **Cek Ketersediaan** - Sistem memastikan akun belum terjual sebelum order dibuat
6. **Order Pending** - Order dibuat dengan status `pending` saat pembeli klik Beli
7. **Pembayaran** - Pembayaran diproses secara simulasi; jika gagal, order dibatalkan
8. **Update Status** - Jika sukses, order menjadi `paid` dan akun menjadi `terjual`
9. **Tampil Kredensial** - Data login akun hanya ditampilkan setelah pembayaran berhasil
10. **Simpan Riwayat** - Transaksi tersimpan di database dan bisa dilihat di riwayat pesanan
11. **Alur Admin** - Admin mengelola akun game dan memantau pesanan
12. **Loop** - Proses berulang hingga pengguna logout

## Diagram Alur Data

```mermaid
sequenceDiagram
    participant U as User
    participant B as Browser
    participant F as Flask Backend
    participant D as Database
    participant P as Pembayaran Simulasi

    U->>B: Buka Katalog
    B->>F: GET /api/akun
    F->>D: Ambil akun berstatus tersedia
    D-->>F: Daftar akun
    F-->>B: JSON response
    B-->>U: Tampilkan katalog

    U->>B: Klik Beli
    B->>F: POST /api/order
    F->>D: Cek status akun dan simpan order pending
    F->>P: Proses pembayaran
    P-->>F: Status sukses
    F->>D: Update order paid dan akun terjual
    F-->>B: JSON detail akun
    B-->>U: Tampilkan kredensial akun
    Note over B,D: Kredensial hanya tampil setelah status paid
```
