> Sprint 2 · [Context](01-context.md) | [Use Case](02-usecase.md) | [Sequence](03-sequence.md) | **Class** | [State](05-state.md) · [README](README.md)

# Class Diagram — GameVault

### 4.1 Diagram
```mermaid
classDiagram
    class User {
        <<abstract>>
        -int id
        -String nama
        -String email
        -String password
        +login() boolean
        +logout() void
    }
    class Pembeli {
        +register() boolean
        +cariAkun(keyword) List
        +checkout() Pesanan
        +uploadBukti(pesananId, file) Pembayaran
        +beriUlasan(akunId, rating, komentar) Ulasan
    }
    class Admin {
        +kelolaAkun() void
        +verifikasiPembayaran(id, valid) void
        +kirimDataAkun(pesananId) void
        +lihatLaporan(periode) Laporan
    }
    class KategoriGame {
        -int id
        -String namaGame
    }
    class AkunGame {
        -int id
        -String judul
        -String deskripsi
        -double harga
        -String status
        +ubahStatus(status) void
        +tampilDetail() String
    }
    class Keranjang {
        -int id
        +tambahAkun(akun) void
        +hapusAkun(akun) void
        +hitungTotal() double
    }
    class Pesanan {
        -int id
        -Date tanggal
        -double total
        -String status
        +buat() void
        +ubahStatus(status) void
        +batalkan() void
    }
    class DetailPesanan {
        -int id
        -double harga
    }
    class Pembayaran {
        -int id
        -String bukti
        -String status
        -Date tglVerifikasi
        +verifikasi() void
        +tolak() void
    }
    class Ulasan {
        -int id
        -int rating
        -String komentar
    }

    User <|-- Pembeli
    User <|-- Admin
    KategoriGame "1" --> "0..*" AkunGame : mengelompokkan
    Pembeli "1" --> "1" Keranjang : memiliki
    Keranjang "0..*" o-- "0..*" AkunGame : berisi
    Pembeli "1" --> "0..*" Pesanan : membuat
    Pesanan "1" *-- "1..*" DetailPesanan : terdiri dari
    DetailPesanan "0..*" --> "1" AkunGame : merujuk
    Pesanan "1" --> "0..1" Pembayaran : dibayar dengan
    Pembeli "1" --> "0..*" Ulasan : menulis
    Ulasan "0..*" --> "1" AkunGame : tentang
    Admin ..> Pembayaran : memverifikasi
    Admin ..> AkunGame : mengelola
```

### 4.2 Penjelasan Relationship
| Relasi | Jenis | Keterangan |
|---|---|---|
| User → Pembeli, Admin | Inheritance | Pembeli dan Admin mewarisi atribut login dari User |
| Pesanan ◆— DetailPesanan | Composition | DetailPesanan tidak bisa ada tanpa Pesanan |
| Keranjang ◇— AkunGame | Aggregation | Akun tetap ada di katalog meski keranjang dikosongkan |
| Pesanan → Pembayaran | Association (1 ke 0..1) | Pesanan baru punya pembayaran setelah bukti diunggah |
| Admin ⇢ Pembayaran | Dependency | Admin memakai Pembayaran untuk verifikasi |

Class di atas selaras dengan tabel pada ERD Sprint 1 (Users, Kategori_Game, Akun_Game, Keranjang, Pesanan, Detail_Pesanan, Pembayaran, Ulasan).
