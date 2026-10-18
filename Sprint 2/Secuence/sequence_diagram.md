> Sprint 2 · [Context](01-context.md) | [Use Case](02-usecase.md) | **Sequence** | [Class](04-class.md) | [State](05-state.md) · [README](README.md)

# Sequence Diagram — GameVault

Sequence diagram menggambarkan **urutan pesan antar peserta** dalam satu skenario, dibaca dari atas ke bawah. Nomor pada tiap panah (autonumber) menunjukkan urutan kejadian. Panah penuh adalah permintaan, panah putus-putus adalah balasan, dan kotak `alt` adalah percabangan kondisi.

> Penjelasan tiap skenario ada di file terpisah: [03-sequence-penjelasan.md](03-sequence-penjelasan.md)

### 3.1 Skenario A: Pembelian dan Pembayaran oleh Pembeli
```mermaid
sequenceDiagram
    autonumber
    actor P as Pembeli
    participant UI as Antarmuka Web
    participant API as Backend
    participant DB as Database
    participant PG as Payment Gateway

    P->>UI: Buka keranjang, tekan Checkout
    UI->>API: POST /checkout (daftar akun)
    API->>DB: Cek status akun (tersedia?)
    DB-->>API: Status akun
    alt Semua akun tersedia
        API->>DB: Simpan Pesanan dan DetailPesanan
        API->>DB: Ubah status akun menjadi Dipesan
        API->>PG: Buat permintaan pembayaran
        PG-->>API: Instruksi pembayaran
        API-->>UI: Pesanan dibuat (Menunggu pembayaran)
        UI-->>P: Tampilkan instruksi dan total
        P->>UI: Upload bukti pembayaran
        UI->>API: POST /pembayaran (bukti)
        API->>DB: Simpan Pembayaran, status Menunggu verifikasi
        API-->>UI: Berhasil
        UI-->>P: Bukti terkirim, menunggu verifikasi
    else Ada akun tidak tersedia
        API-->>UI: Error: akun sudah terjual
        UI-->>P: Pesan error dan saran pilih akun lain
    end
```

### 3.2 Skenario B: Verifikasi dan Pengiriman Akun oleh Admin
```mermaid
sequenceDiagram
    autonumber
    actor A as Admin
    participant UI as Dashboard Admin
    participant API as Backend
    participant DB as Database
    participant EM as Layanan Email
    actor P as Pembeli

    A->>UI: Buka daftar pembayaran masuk
    UI->>API: GET /admin/pembayaran?status=menunggu
    API->>DB: Ambil data pembayaran
    DB-->>API: Daftar pembayaran
    API-->>UI: Daftar pembayaran
    A->>UI: Periksa bukti, tekan Verifikasi
    UI->>API: PATCH /admin/pembayaran/{id} (valid)
    API->>DB: Ubah status pembayaran dan pesanan
    API->>EM: Kirim notifikasi ke pembeli
    EM-->>P: Email: pembayaran terverifikasi
    A->>UI: Tekan Kirim data akun
    UI->>API: POST /admin/pesanan/{id}/kirim-akun
    API->>DB: Simpan data akun terkirim, status Selesai
    API->>DB: Ubah status akun menjadi Terjual
    API->>EM: Kirim data akun ke pembeli
    EM-->>P: Email: data akun
    API-->>UI: Berhasil
```

### 3.3 Skenario C: Registrasi Pembeli
```mermaid
sequenceDiagram
    autonumber
    actor P as Pembeli
    participant UI as Antarmuka Web
    participant API as Backend
    participant DB as Database
    participant EM as Layanan Email

    P->>UI: Isi form registrasi (nama, email, password)
    UI->>API: POST /register
    API->>DB: Cek apakah email sudah terdaftar
    DB-->>API: Hasil pengecekan
    alt Email belum terdaftar
        API->>DB: Simpan user baru (password di-hash)
        API->>EM: Kirim email selamat datang
        API-->>UI: Registrasi berhasil
        UI-->>P: Arahkan ke halaman login
    else Email sudah terdaftar
        API-->>UI: Error: email sudah dipakai
        UI-->>P: Tampilkan pesan dan saran login
    end
```

### 3.4 Skenario D: Login (Pembeli dan Admin)
```mermaid
sequenceDiagram
    autonumber
    actor U as Pembeli / Admin
    participant UI as Antarmuka Web
    participant API as Backend
    participant DB as Database

    U->>UI: Isi email dan password
    UI->>API: POST /login
    API->>DB: Ambil user berdasarkan email
    DB-->>API: Data user (hash password, role)
    alt Kredensial valid
        API-->>UI: Token sesi dan role
        alt Role = Admin
            UI-->>U: Buka dashboard admin
        else Role = Pembeli
            UI-->>U: Buka beranda katalog
        end
    else Kredensial salah
        API-->>UI: Error: email atau password salah
        UI-->>U: Tampilkan pesan error
    end
```

### 3.5 Skenario E: Beri Ulasan
```mermaid
sequenceDiagram
    autonumber
    actor P as Pembeli
    participant UI as Antarmuka Web
    participant API as Backend
    participant DB as Database

    P->>UI: Buka riwayat pesanan, pilih Beri ulasan
    UI->>API: POST /ulasan (akunId, rating, komentar)
    API->>DB: Cek pesanan milik pembeli untuk akun tersebut
    DB-->>API: Data pesanan dan status
    alt Pesanan berstatus Selesai dan belum diulas
        API->>DB: Simpan ulasan
        API->>DB: Perbarui rating rata-rata
        API-->>UI: Ulasan tersimpan
        UI-->>P: Ulasan tampil di detail akun
    else Pesanan belum selesai atau sudah diulas
        API-->>UI: Error: ulasan tidak diizinkan
        UI-->>P: Tampilkan alasan penolakan
    end
```
