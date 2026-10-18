> Sprint 2 · **Context** | [Use Case](02-usecase.md) | [Sequence](03-sequence.md) | [Class](04-class.md) | [State](05-state.md) · [README](README.md)

# Context Diagram — GameVault

### 1.1 Tujuan
Menunjukkan GameVault sebagai satu sistem utuh beserta entitas eksternal yang berinteraksi dengannya dan aliran data di antaranya.

### 1.2 Diagram
```mermaid
%%{init: {"theme":"base","themeVariables":{"fontFamily":"Helvetica, Arial, sans-serif","fontSize":"15px","lineColor":"#2b2b2b","edgeLabelBackground":"#ffffff"},"flowchart":{"curve":"linear","nodeSpacing":70,"rankSpacing":150,"padding":18}}}%%
flowchart LR
    PB["<b>Pembeli</b><br/><i>aktor</i>"]
    PG["<b>Payment Gateway</b><br/><i>sistem eksternal</i>"]
    GV(("<b>GameVault</b><br/>Sistem Toko<br/>Akun Game"))
    AD["<b>Admin</b><br/><i>aktor</i>"]
    EM["<b>Layanan Email</b><br/><i>sistem eksternal</i>"]

    PB -- "Registrasi, login, pencarian,<br/>pesanan, bukti bayar, ulasan" --> GV
    GV -- "Katalog akun, invoice,<br/>status pesanan, data akun" --> PB

    PG -- "Status pembayaran" --> GV
    GV -- "Permintaan pembayaran" --> PG

    GV -- "Daftar pesanan,<br/>laporan penjualan dan stok" --> AD
    AD -- "Data akun game, verifikasi<br/>pembayaran, pengiriman akun" --> GV

    GV -- "Permintaan kirim notifikasi" --> EM
    EM -- "Status pengiriman" --> GV

    classDef aktor fill:#D3E3F3,stroke:#2b2b2b,stroke-width:1.5px,color:#1b1b1b
    classDef eksternal fill:#F6E6B4,stroke:#2b2b2b,stroke-width:1.5px,color:#1b1b1b
    classDef sistem fill:#D8EBD0,stroke:#2b2b2b,stroke-width:2.5px,color:#1b1b1b
    class PB,AD aktor
    class PG,EM eksternal
    class GV sistem
    linkStyle default stroke:#2b2b2b,stroke-width:1.5px
```

### 1.3 Penjelasan Entitas Eksternal
| Entitas | Peran | Input ke sistem | Output dari sistem |
|---|---|---|---|
| Pembeli | Pengguna yang membeli akun | Registrasi, login, pencarian, checkout, bukti bayar, ulasan | Katalog, invoice, status pesanan, data akun |
| Admin | Pengelola toko | Data akun game, verifikasi pembayaran, pengiriman akun | Daftar pesanan, laporan penjualan dan stok |
| Payment Gateway | Layanan pembayaran pihak ketiga | Status pembayaran | Permintaan pembayaran |
| Layanan Email | Pengirim notifikasi | Status pengiriman | Permintaan kirim notifikasi |

### 1.4 Batas Sistem
- **Di dalam sistem:** katalog, keranjang, checkout, verifikasi, pengiriman akun, ulasan, laporan.
- **Di luar sistem:** server game resmi, proses di bank/e-wallet, penyedia email.
