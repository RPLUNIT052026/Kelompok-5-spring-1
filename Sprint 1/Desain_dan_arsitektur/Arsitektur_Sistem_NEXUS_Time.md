# ARSITEKTUR SISTEM — NEXUS Time (Toko Jam Tangan Online dengan Digital Twin)

## 1. Diagram Arsitektur

```mermaid
graph TB
    subgraph PRESENTASI[Lapisan Presentasi]
        A[Browser - HTML + CSS + JavaScript Responsif]
        A1[Halaman Pelanggan - Katalog, Profil Twin, Keranjang, Status Pesanan]
        A2[Halaman Admin - Dashboard Twin, Kelola Jam & Unit, Kelola Pesanan]
    end

    subgraph APLIKASI[Lapisan Aplikasi]
        B[Flask Backend - Python]
        B1[Autentikasi & Role]
        B2[Manajemen Katalog Jam & Unit]
        B3[Logika Order & Penguncian Unit Twin]
        B4[Mesin Status & Peringatan Twin]
        B5[API /api/jam, /api/order, /api/admin/twin]
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
    A -->|HTTP Request + Polling Status| B
    B --> B1
    B --> B2
    B --> B3
    B --> B4
    B --> B5
    B -->|Query| C
    B -->|Konfirmasi Bayar| D
    E -.->|Future| B
    B -.->|Future| F
```

## 2. Penjelasan Layer

| Layer | Komponen | Fungsi |
|-------|----------|--------|
| **Presentasi** | HTML, CSS, JavaScript | Menampilkan katalog jam, profil digital twin, keranjang, dan dashboard admin. Tampilan responsif untuk desktop dan HP |
| **Aplikasi** | Flask (Python) | Memproses request, autentikasi, logika order, penguncian unit, serta perhitungan status dan peringatan twin |
| **Data** | SQLite | Menyimpan data user, jam, unit twin, pesanan, pembayaran, dan log servis |
| **Layanan Eksternal** | Simulasi Pembayaran / Payment Gateway | Memproses dan mengonfirmasi pembayaran |

## 3. Alur Komunikasi

### 3.1 Alur Pembelian Jam (Status Twin Berubah Otomatis)

```mermaid
sequenceDiagram
    participant U as Pelanggan
    participant B as Browser
    participant F as Flask
    participant S as SQLite
    participant P as Pembayaran

    U->>B: Pilih jam dan klik Checkout
    B->>F: POST /api/order
    F->>S: Cek status unit_jam - tersedia?
    S-->>F: Status = tersedia
    F->>S: INSERT pesanan - Menunggu Pembayaran
    F->>S: UPDATE unit_jam menjadi di-booking
    F-->>B: JSON - pesanan dibuat
    U->>B: Pilih metode dan klik Bayar
    B->>F: POST /api/pembayaran
    F->>P: Minta pembayaran
    alt Pembayaran sukses
        P-->>F: Sukses
        F->>S: UPDATE pesanan - Diproses, INSERT pembayaran berhasil
        F->>S: UPDATE unit_jam menjadi terjual
        F-->>B: JSON - konfirmasi pesanan
        B-->>U: Tampilkan nomor pesanan dan bukti bayar
    else Pembayaran gagal atau dibatalkan
        P-->>F: Gagal
        F->>S: UPDATE pesanan - Dibatalkan
        F->>S: UPDATE unit_jam kembali tersedia
        F-->>B: JSON - gagal
        B-->>U: Tampilkan pesan gagal
    end
```

### 3.2 Alur Admin Menambah Jam dan Unit Twin

```mermaid
sequenceDiagram
    participant A as Admin
    participant B as Browser
    participant F as Flask
    participant S as SQLite

    A->>B: Login dan isi form jam dan unit baru
    B->>F: POST /api/admin/unit
    F->>F: Validasi role admin
    F->>S: INSERT INTO jam
    F->>S: INSERT INTO unit_jam - nomor seri, kondisi, garansi, status tersedia
    F-->>B: JSON - berhasil
    B-->>A: Unit tampil di dashboard twin dan katalog
```

### 3.3 Alur Pemantauan dan Peringatan Twin

```mermaid
sequenceDiagram
    participant A as Admin
    participant B as Browser
    participant F as Flask
    participant S as SQLite

    A->>B: Buka dashboard twin
    loop Polling setiap 5 detik
        B->>F: GET /api/admin/twin
        F->>S: SELECT semua unit_jam
        S-->>F: Data unit
        F->>F: Hitung peringatan - garansi kurang dari 30 hari, jadwal servis, skor kondisi rendah
        F-->>B: JSON - unit dan peringatan
        B-->>A: Grid unit berwarna dan daftar peringatan
    end
    A->>B: Klik tombol simulasi - turunkan skor kondisi
    B->>F: POST /api/admin/twin/simulasi
    F->>S: UPDATE skor_kondisi
    F->>S: UPDATE status menjadi perlu_servis jika di bawah ambang
    F-->>B: JSON - berhasil
    B-->>A: Unit berubah merah pada polling berikutnya
```

## 4. Siklus Status Digital Twin

```mermaid
stateDiagram-v2
    [*] --> Tersedia: Barang masuk
    Tersedia --> DiBooking: Pelanggan checkout
    DiBooking --> Terjual: Pembayaran berhasil
    DiBooking --> Tersedia: Pembayaran gagal atau dibatalkan
    Tersedia --> PerluServis: Jadwal servis tiba atau skor kondisi rendah
    PerluServis --> Tersedia: Servis selesai
    Terjual --> [*]
```

## 5. Rancangan Tabel Database

```mermaid
erDiagram
    USERS ||--o{ KERANJANG : memiliki
    USERS ||--o{ PESANAN : membuat
    JAM ||--o{ UNIT_JAM : memiliki
    UNIT_JAM ||--o{ KERANJANG : dimasukkan_ke
    UNIT_JAM ||--o{ DETAIL_PESANAN : dibeli_pada
    UNIT_JAM ||--o{ LOG_SERVIS : memiliki
    PESANAN ||--|{ DETAIL_PESANAN : berisi
    PESANAN ||--o| PEMBAYARAN : dibayar_dengan

    USERS {
        int id PK
        string nama
        string email
        string password_hash
        string no_hp
        string alamat
        string role
    }
    JAM {
        int id PK
        string nama
        string merek
        string kategori
        string tipe
        int harga
        string deskripsi
        string gambar
    }
    UNIT_JAM {
        int id PK
        int jam_id FK
        string nomor_seri
        string kondisi
        int skor_kondisi
        date tanggal_beli
        date garansi_sampai
        date jadwal_servis
        string status
    }
    KERANJANG {
        int id PK
        int user_id FK
        int unit_id FK
        int jumlah
    }
    PESANAN {
        int id PK
        int user_id FK
        datetime tanggal
        int total_harga
        string status
    }
    DETAIL_PESANAN {
        int id PK
        int pesanan_id FK
        int unit_id FK
        int harga
        int subtotal
    }
    PEMBAYARAN {
        int id PK
        int pesanan_id FK
        string metode
        int jumlah_bayar
        string status_pembayaran
        datetime waktu_bayar
    }
    LOG_SERVIS {
        int id PK
        int unit_id FK
        date tanggal
        string catatan
        int biaya
    }
```

## 6. Daftar Endpoint API

| Method | Endpoint | Akses | Fungsi |
|--------|----------|-------|--------|
| POST | `/api/auth/register` | Publik | Registrasi pelanggan |
| POST | `/api/auth/login` | Publik | Login dan pengarahan sesuai role |
| GET | `/api/jam` | Publik | Katalog jam dengan filter kategori, tipe, harga |
| GET | `/api/jam/<id>` | Publik | Detail jam, daftar unit, dan profil twin |
| GET/POST/DELETE | `/api/keranjang` | Pelanggan | Lihat, tambah, dan hapus item keranjang |
| POST | `/api/order` | Pelanggan | Checkout dan kunci unit twin menjadi di-booking |
| POST | `/api/pembayaran` | Pelanggan | Bayar (simulasi) dan ubah status twin |
| GET | `/api/order/saya` | Pelanggan | Status dan riwayat pesanan |
| POST/PUT/DELETE | `/api/admin/unit` | Admin | Kelola jam dan unit twin |
| GET | `/api/admin/twin` | Admin | Dashboard twin dan peringatan |
| POST | `/api/admin/twin/simulasi` | Admin | Simulasi perubahan kondisi unit |
| GET/PUT | `/api/admin/pesanan` | Admin | Daftar pesanan dan ubah status |

## 7. Teknologi yang Digunakan

| Komponen | Teknologi | Alasan |
|----------|-----------|--------|
| Backend | Flask | Ringan, mudah dipelajari |
| Database | SQLite | Tidak perlu server, portable |
| Frontend | HTML + CSS + JavaScript | Sederhana, responsif untuk HP dan desktop |
| Sinkronisasi Twin | Polling `fetch` setiap 5 detik | Sederhana, status unit tetap terasa real-time |
| Keamanan | Werkzeug (hash password) + session | Password tidak disimpan sebagai teks biasa |
| Pembayaran | Simulasi (dummy) | Bisa didemokan tanpa akun merchant |
| Version Control | Git + GitHub | Standar industri |
| Diagram | Mermaid | Render langsung di GitHub |

## 8. Kelebihan Arsitektur Ini

- **Digital Twin Jelas** - Satu jam fisik sama dengan satu baris `unit_jam`, sehingga identitas dan kondisi tiap unit terpantau
- **Modular** - Setiap layer terpisah, mudah dikembangkan
- **Tanpa Layanan Berbayar** - Bisa jalan dengan simulasi pembayaran dan SQLite
- **Aman** - Ada pemisahan role (pelanggan/admin) dan password ter-hash
- **Mencegah Pemesanan Ganda** - Unit dikunci "di-booking" lewat satu query atomik (`UPDATE ... WHERE status='tersedia'`)
- **Future-proof** - Siap diintegrasikan dengan payment gateway (mis. Midtrans) dan email notifikasi
