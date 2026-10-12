# ARSITEKTUR SISTEM — Toko Akun Game Online

## 1. Diagram Arsitektur

```mermaid
graph TB
    subgraph PRESENTASI[Lapisan Presentasi]
        A[Browser - HTML + CSS + JavaScript]
        A1[Halaman Pembeli - Katalog, Keranjang, Riwayat]
        A2[Halaman Admin - Kelola Akun & Pesanan]
    end

    subgraph APLIKASI[Lapisan Aplikasi]
        B[Flask Backend - Python]
        B1[Autentikasi & Role]
        B2[Manajemen Katalog Akun]
        B3[Logika Order & Stok]
        B4[API /api/akun, /api/order]
    end

    subgraph DATA[Lapisan Data]
        C[(SQLite Database)]
    end

    subgraph EKSTERNAL[Layanan Eksternal]
        D[Simulasi Pembayaran - Dummy]
        E[Payment Gateway - Future]
        F[Notifikasi Email - Future]
    end

    A --> A1
    A --> A2
    A -->|HTTP Request| B
    B --> B1
    B --> B2
    B --> B3
    B --> B4
    B -->|Query| C
    B -->|Konfirmasi Bayar| D
    E -.->|Future| B
    B -.->|Future| F
```

## 2. Penjelasan Layer

| Layer | Komponen | Fungsi |
|-------|----------|--------|
| **Presentasi** | HTML, CSS, JavaScript | Menampilkan katalog akun, keranjang, dan dashboard admin |
| **Aplikasi** | Flask (Python) | Memproses request, autentikasi, logika order dan stok |
| **Data** | SQLite | Menyimpan data user, akun game, pesanan, dan transaksi |
| **Layanan Eksternal** | Simulasi Pembayaran / Payment Gateway | Memproses dan mengonfirmasi pembayaran |

## 3. Alur Komunikasi

### 3.1 Alur Pembelian Akun

```mermaid
sequenceDiagram
    participant U as Pembeli
    participant B as Browser
    participant F as Flask
    participant S as SQLite
    participant P as Pembayaran

    U->>B: Pilih akun & klik Beli
    B->>F: POST /api/order
    F->>S: Cek status akun (tersedia?)
    S-->>F: Status = tersedia
    F->>S: INSERT INTO orders (status: pending)
    F->>P: Minta pembayaran
    P-->>F: Pembayaran sukses
    F->>S: UPDATE orders (paid), UPDATE akun (terjual)
    F-->>B: JSON (detail akun/kredensial)
    B-->>U: Tampilkan detail akun yang dibeli
```

### 3.2 Alur Admin Menambah Akun

```mermaid
sequenceDiagram
    participant A as Admin
    participant B as Browser
    participant F as Flask
    participant S as SQLite

    A->>B: Login & isi form akun baru
    B->>F: POST /api/akun
    F->>F: Validasi role admin
    F->>S: INSERT INTO akun_game
    F-->>B: JSON (berhasil)
    B-->>A: Akun tampil di katalog
```

## 4. Rancangan Tabel Database

```mermaid
erDiagram
    USERS ||--o{ ORDERS : membuat
    AKUN_GAME ||--o| ORDERS : dibeli_pada
    GAME ||--o{ AKUN_GAME : memiliki

    USERS {
        int id PK
        string username
        string password_hash
        string role
    }
    GAME {
        int id PK
        string nama_game
    }
    AKUN_GAME {
        int id PK
        int game_id FK
        string judul
        string deskripsi
        int harga
        string status
        string kredensial_terenkripsi
    }
    ORDERS {
        int id PK
        int user_id FK
        int akun_id FK
        int total_harga
        string status
        datetime created_at
    }
```

## 5. Teknologi yang Digunakan

| Komponen | Teknologi | Alasan |
|----------|-----------|--------|
| Backend | Flask | Ringan, mudah dipelajari |
| Database | SQLite | Tidak perlu server, portable |
| Frontend | HTML + CSS + JavaScript | Sederhana, tanpa framework berat |
| Keamanan | Werkzeug (hash password) | Password tidak disimpan sebagai teks biasa |
| Pembayaran | Simulasi (dummy) | Bisa didemokan tanpa akun merchant |
| Version Control | Git + GitHub | Standar industri |
| Diagram | Mermaid | Render langsung di GitHub |

## 6. Kelebihan Arsitektur Ini

- **Modular** - Setiap layer terpisah, mudah dikembangkan
- **Tanpa Layanan Berbayar** - Bisa jalan dengan simulasi pembayaran
- **Future-proof** - Siap diintegrasikan dengan payment gateway (mis. Midtrans) dan email notifikasi
- **Aman** - Ada pemisahan role (pembeli/admin) dan password ter-hash
- **Ringan** - Tidak butuh server besar
