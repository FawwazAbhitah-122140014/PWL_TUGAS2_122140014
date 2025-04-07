# Microservices Order Systeme

**Mata Kuliah:** Pemrograman Web Lanjut (IF4024)  
**Nama:** Fawwaz Abhitah Sugiarto  
**NIM:** 122140014

## Gambaran Umum

Sistem ini menerapkan pola Saga yang melibatkan tiga layanan utama: order service, payment service, dan shipping service. Ketiga layanan berjalan secara berurutan, dan jika terjadi kegagalan di salah satu layanan, sistem akan menjalankan proses kompensasi untuk mengembalikan kondisi ke semula.

### 1. Order Service (`localhost:8000`)
- Menerima permintaan pemesanan produk.
- Setelah pesanan dibuat, akan memicu proses pembayaran.
- Jika proses berakhir sukses, status pesanan menjadi `COMPLETED`.

### 2. Payment Service (`localhost:8001`)
- Mengelola transaksi pembayaran.
- Jika berhasil, diteruskan ke layanan pengiriman.
- Jika gagal, memicu pembatalan order (kompensasi).

### 3. Shipping Service (`localhost:8002`)
- Menangani pengiriman produk.
- Jika gagal, sistem akan membatalkan proses shipping, refund pembayaran, dan menghapus order.

### 4. Orchestrator Service (`localhost:8003`)
- Bertindak sebagai pengatur alur komunikasi antar service.
- Menangani proses normal dan proses kompensasi bila terjadi kegagalan.

## Alur Saga (Normal & Pengembalian)

**Alur Normal**
1. Client membuat pesanan → Order Service
2. Order dikonfirmasi → Payment Service memproses pembayaran
3. Pembayaran sukses → Shipping Service memproses pengiriman
4. Pengiriman selesai → Transaksi dianggap sukses

**Alur Pengembalian**
Jika terjadi error di proses Shipping:
1. Pengiriman dibatalkan
2. Pembayaran dikembalikan (refund)
3. Order dibatalkan

## Menjalankan Service

Buka terminal terpisah untuk masing-masing service, lalu jalankan dengan:

```bash
go run main.go
