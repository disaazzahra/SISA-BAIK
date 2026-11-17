# Log Pengujian Cloud SisaBaik

Tanggal: 17 November 2026
Aplikasi: https://sisa-baik.onrender.com
Hosting: Render
Database: Neon PostgreSQL

## 1. Persiapan Database

- Database Neon berhasil diinisialisasi.
- Enam tabel aplikasi berhasil dibuat.
- Seluruh tabel memiliki jumlah data awal nol.
- Koneksi database menggunakan TLS.

## 2. Pengujian Akun

| Pengujian | Hasil |
|---|---|
| Pendaftaran akun penyedia | Berhasil |
| Verifikasi akun penyedia | Berhasil |
| Login akun penyedia | Berhasil |
| Pendaftaran akun pembeli | Berhasil |
| Login akun pembeli | Berhasil |

Identitas akun demo tidak dicantumkan dalam log yang dibagikan.

## 3. Pengujian Penawaran dan Pesanan

| Rincian | Hasil |
|---|---|
| Penyedia demo | Eldante |
| Pembeli demo | Caco |
| Penawaran | Paket Roti Isi Cokelat |
| Harga normal per paket | Rp20.000 |
| Harga penawaran per paket | Rp7.000 |
| Stok awal | 3 paket |
| Jumlah dipesan | 2 paket |
| Total pesanan | Rp14.000 |
| Nomor pesanan | 1 |
| Stok setelah pemesanan | 1 paket |

## 4. Konfirmasi dan Pengambilan

- Pesanan berhasil masuk ke akun penyedia.
- Penyedia mengonfirmasi pesanan.
- Pesanan ditandai siap diambil.
- Sistem menghasilkan kode pengambilan SBX-1.
- Status pesanan diperiksa melalui akun pembeli.
- Penyelesaian serah terima dan pembayaran dilaporkan telah dilakukan.

Catatan:
Log ini disusun dari hasil pengujian yang dilaporkan selama proses demonstrasi. Bukti transaksi eksternal atau kuitansi pembayaran tidak tersedia dalam dokumentasi ini.

## 5. Dashboard

Dashboard penyedia telah diperiksa setelah pengujian transaksi. Nilai metrik akhir belum dicatat secara terpisah sehingga tidak dicantumkan sebagai angka hasil.

## 6. Keamanan Dokumentasi

Log ini tidak mencantumkan email lengkap, kata sandi, JWT secret, password database, token akses, atau connection string.
