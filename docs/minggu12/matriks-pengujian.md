# Matriks Pengujian SisaBaik
## Dokumentasi Minggu 12

Tanggal dokumentasi: 17 November 2026
Branch: feature/deployment-sisabaik
Commit aplikasi saat pengujian awal: 0fa52fd. SHA deployment dokumentasi akhir dicatat setelah push terakhir.
URL publik: https://sisa-baik.onrender.com

## 1. Pengujian Deployment

Perintah:
npm run deploy:test -- https://sisa-baik.onrender.com

| No. | Pemeriksaan | Hasil |
|---|---|---|
| 1 | Endpoint /healthz | Lulus |
| 2 | Endpoint /api/health | Lulus |
| 3 | HTML Vue | Lulus |
| 4 | Aset JavaScript dan CSS | Lulus |
| 5 | Endpoint tidak dikenal mengembalikan 404 | Lulus |
| 6 | Endpoint autentikasi tanpa login mengembalikan 401 | Lulus |
| 7 | Dashboard tanpa login mengembalikan 401 | Lulus |
| 8 | Pesanan tanpa login mengembalikan 401 | Lulus |
| 9 | File .env tidak dapat diakses | Lulus |
| 10 | File server.js tidak dapat diakses | Lulus |

Hasil aktual: 10 dari 10 pemeriksaan lulus.
Tidak ada data yang ditulis selama pengujian.

Bukti:
bukti/pengujian-deployment.txt

## 2. Pengujian Alur Transaksi Otomatis

Perintah:
npm run workflow:test

| No. | Skenario | Hasil |
|---|---|---|
| 1 | Penawaran tanpa foto menggunakan fallback dan versi awal | Lulus |
| 2 | Reservasi mengurangi stok sekali dan memakai harga server | Lulus |
| 3 | Pembeli tidak dapat menerima pesanan sendiri | Lulus |
| 4 | Penyedia dan pembeli lain tidak dapat mengakses pesanan | Lulus |
| 5 | Pembatalan pembeli sebelum konfirmasi mengembalikan stok | Lulus |
| 6 | Pengulangan pembatalan tidak mengembalikan stok dua kali | Lulus |
| 7 | Penolakan penyedia tercatat dan memulihkan reservasi | Lulus |
| 8 | Konfirmasi membatasi pembatalan pembeli tanpa mengurangi stok lagi | Lulus |
| 9 | Pesanan siap belum menjadi pendapatan atau pengambilan | Lulus |
| 10 | Serah terima berbayar mencatat pembayaran dan pengambilan secara atomik | Lulus |
| 11 | Pesanan selesai tidak dapat diselesaikan atau dibatalkan ulang | Lulus |
| 12 | Makanan gratis selesai tanpa menambah pendapatan | Lulus |
| 13 | Dashboard menghitung belanja, diskon, unit, nilai normal, dan pembatalan dengan tepat | Lulus |
| 14 | Dashboard usaha lain tetap nol | Lulus |
| 15 | Foto PNG diproses sebagai WebP dan dilayani dari database | Lulus |
| 16 | SVG dan berkas palsu ditolak tanpa penawaran parsial | Lulus |
| 17 | Edit tanpa foto baru mempertahankan foto yang sudah ada | Lulus |
| 18 | Edit hanya untuk pemilik dan foto dapat dikembalikan ke default | Lulus |
| 19 | Edit usang ditolak dan riwayat transaksi tidak mengikuti edit baru | Lulus |
| 20 | Penyedia boleh membatalkan setelah konfirmasi dan memulihkan stok | Lulus |
| 21 | Penyedia membatalkan pesanan siap dan menutup pengambilan | Lulus |
| 22 | Konfirmasi dan pembatalan bersamaan menghasilkan satu keputusan yang sah | Lulus |
| 23 | Pemulihan stok kedaluwarsa tidak menampilkan makanan kembali ke katalog | Lulus |
| 24 | Dua permintaan untuk stok terakhir menghasilkan satu reservasi | Lulus |
| 25 | Dua pembatalan memulihkan stok sekali dan mengaktifkan stok habis | Lulus |
| 26 | Dashboard mencakup semua halaman tanpa menghitung reservasi sebagai pendapatan | Lulus |

Hasil aktual: 26 dari 26 skenario lulus.
Fixture pengujian sudah dihapus.

Bukti:
bukti/pengujian-alur-transaksi.txt

## 3. Pengujian Alur Cloud

| No. | Pengujian | Hasil |
|---|---|---|
| 1 | Pendaftaran akun penyedia Eldante | Berhasil |
| 2 | Verifikasi akun penyedia melalui script cloud | Berhasil |
| 3 | Penyimpanan penawaran pertama | Berhasil |
| 4 | Pendaftaran akun pembeli Caco | Berhasil |
| 5 | Pembeli membuat pesanan 2 paket | Berhasil |
| 6 | Total pesanan Rp14.000 | Berhasil |
| 7 | Pesanan masuk ke akun penyedia | Berhasil |
| 8 | Penyedia mengonfirmasi pesanan | Berhasil |
| 9 | Kode pengambilan SBX-1 muncul | Berhasil |
| 10 | Pemeriksaan status dari akun pembeli | Dilakukan |
| 11 | Penyelesaian serah terima dan pembayaran | Dilaporkan selesai, bukti akhir belum dilampirkan |
| 12 | Pemeriksaan dashboard penyedia | Dilakukan, angka metrik belum dicatat |

Catatan:
Hasil pengujian cloud berdasarkan proses yang dilakukan dan dikonfirmasi pengguna. Screenshot pendukung dikumpulkan secara terpisah.

## 4. Pengujian yang Belum Dilakukan

| No. | Pengujian | Status |
|---|---|---|
| 1 | Pengujian manual F01-F20 | Belum diuji |
| 2 | Pengujian keamanan manual S01-S11 | Belum diuji |
| 3 | Navigasi keyboard dasar | Lulus pada skenario yang diuji |
| 4 | Validasi formulir pendaftaran dan penawaran | Lulus untuk skenario yang diuji |
| 5 | Responsivitas mobile, tablet, dan desktop | Lulus pada ukuran yang diuji |
| 6 | Audit awal Firefox Accessibility Inspector | Dilakukan, ditemukan masalah kontras warna |
| 7 | Pengukuran performa sebelum dan sesudah | Belum diukur |
| 8 | Pengukuran performa cloud menggunakan Firefox Network Monitor | Diukur, hasil tercatat di performa.md |

## 5. Catatan Hasil Pengujian

Pengujian otomatis menunjukkan bahwa pemeriksaan deployment dan skenario alur transaksi berhasil dijalankan.

Pengujian cloud menunjukkan bahwa akun penyedia dan pembeli dapat digunakan untuk menjalankan alur penawaran dan pemesanan.

Pemeriksaan dashboard telah dilakukan. Namun, angka metrik akhir belum didokumentasikan secara terpisah.

Pengujian manual, aksesibilitas, dan performa belum dilakukan. Oleh karena itu, hasilnya tidak dinyatakan lulus.



## 6. Pemeriksaan Formulir

| No. | Skenario | Hasil |
|---|---|---|
| 1 | Pendaftaran tanpa mengisi data | Validasi muncul |
| 2 | Email dengan format tidak valid | Ditolak |
| 3 | Kata sandi kurang dari 15 karakter | Validasi muncul |
| 4 | Konfirmasi kata sandi berbeda | Validasi muncul |
| 5 | Simpan penawaran dengan formulir kosong | Penyimpanan ditolak dan validasi muncul |

Hasil aktual:
Seluruh skenario validasi formulir yang diuji berjalan sesuai harapan.

Cakupan:
Hasil ini hanya berlaku untuk skenario di atas, bukan seluruh kemungkinan input formulir.
