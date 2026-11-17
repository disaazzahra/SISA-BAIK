# SisaBaik

SisaBaik adalah aplikasi web untuk mengelola penawaran makanan berlebih dan pemesanan makanan. Aplikasi ini menghubungkan penyedia makanan dengan pembeli melalui katalog digital. Penyedia dapat menawarkan makanan yang masih layak dikonsumsi dengan harga penawaran, sedangkan pembeli dapat melihat dan memesan makanan yang tersedia.

Aplikasi menyediakan pengelolaan penawaran, pemesanan, stok, konfirmasi pesanan, pengambilan makanan, pencatatan pembayaran, dan dashboard untuk pembeli maupun penyedia.

## Informasi Proyek

- Versi backend: 1.6.0
- Versi frontend: 1.1.0
- Repository: SISA-BAIK
- Branch deployment: `feature/deployment-sisabaik`
- URL publik: https://sisa-baik.onrender.com
- Database cloud: PostgreSQL melalui Neon
- Hosting aplikasi: Render

## Teknologi

### Backend
- Node.js 24
- Express 5
- PostgreSQL
- `pg` untuk koneksi database
- `bcryptjs` untuk hash kata sandi
- `jsonwebtoken` untuk autentikasi token
- `express-rate-limit` untuk pembatasan request
- Helmet untuk pengamanan HTTP
- Sharp untuk pemrosesan gambar

### Frontend
- Vue 3
- Vue Router
- Vite

### Infrastruktur
- Render untuk hosting aplikasi
- Neon untuk database PostgreSQL
- Git dan GitHub untuk pengelolaan kode sumber

## Struktur Proyek

- `server.js`: menjalankan server Express.
- `src/app.js`: konfigurasi aplikasi backend.
- `src/config/`: konfigurasi autentikasi.
- `src/routes/`: pengaturan endpoint API.
- `src/controllers/`: menangani request dan response.
- `src/services/`: proses bisnis aplikasi.
- `src/repositories/`: akses data pengguna.
- `src/middleware/`: autentikasi, pembatasan request, dan penanganan error.
- `src/db/`: koneksi database dan transaksi.
- `src/lib/`: fungsi pendukung, validasi, token, dan pemrosesan foto.
- `frontend/`: aplikasi frontend Vue.
- `database/`: schema dan migrasi database.
- `scripts/`: inisialisasi, migrasi, verifikasi, dan pengujian.
- `docs/`: dokumentasi proyek dan hasil pengujian.

## Persiapan Pengembangan Lokal

Pastikan Node.js dan PostgreSQL sudah terpasang.

Gunakan file `.env.example` sebagai template konfigurasi lokal. Salin menjadi `.env`, lalu isi sesuai database PostgreSQL lokal.

Untuk pengembangan lokal, gunakan database `sisabaik_dev` dan `PGSSL=disable`.

Jangan memasukkan password, JWT secret, atau kredensial database ke repository.

### Instalasi Dependensi

Jalankan dari direktori utama proyek:

`npm ci`

Instal dependensi frontend:

`npm --prefix frontend ci --include=dev`

### Pemeriksaan Database

Pastikan konfigurasi `.env` sudah sesuai, kemudian jalankan:

`npm run db:check -- 0`

### Menjalankan Mode Pengembangan

Jalankan backend:

`npm run dev`

Untuk menjalankan frontend dengan Vite, gunakan terminal terpisah:

`npm --prefix frontend run dev`

### Build Frontend

Untuk membuat hasil build frontend:

`npm run build`

Hasil build disimpan pada direktori `frontend/dist`.

### Menjalankan Aplikasi Lokal

Setelah build frontend berhasil, jalankan:

`npm run start:local`

### Menjalankan Server

Untuk menjalankan server:

`npm start`

## Deployment

Aplikasi publik tersedia di:

https://sisa-baik.onrender.com

Database cloud menggunakan PostgreSQL pada Neon. Koneksi database cloud menggunakan SSL dengan `PGSSL=require`.

Pengaturan deployment Render:

- Branch: `feature/deployment-sisabaik`
- Build command:

`npm ci && npm --prefix frontend ci --include=dev && npm run build`

- Start command:

`npm start`

- Health check: `/healthz`
- Node.js: versi 24

Render menyediakan variabel `PORT` secara otomatis. Jangan memasukkan file `.env` atau `.env.cloud` ke repository.

## Pendaftaran dan Pengelolaan Akun

SisaBaik menyediakan dua peran pengguna, yaitu pembeli dan penyedia.

### Pembeli

Pembeli dapat melakukan pendaftaran melalui halaman Daftar. Setelah pendaftaran berhasil, pembeli dapat login, melihat katalog, membuat pesanan, memeriksa status pesanan, dan melihat ringkasan aktivitas pembeli.

### Penyedia

Penyedia dapat mendaftarkan akun melalui pilihan peran Penyedia. Setelah pendaftaran berhasil, akun penyedia perlu melalui proses peninjauan.

Status verifikasi penyedia dikelola oleh operator melalui script:

`npm run penyedia:verifikasi -- email@domain.com`

Untuk database cloud:

`npm run penyedia:verifikasi:cloud -- email@domain.com`

Gunakan alamat email akun penyedia yang benar. Perintah cloud harus dijalankan dengan konfigurasi `.env.cloud` yang sesuai.

Setelah terverifikasi, penyedia dapat mengelola penawaran dan menangani pesanan.

### Autentikasi dan Keamanan Akun

- Pendaftaran tersedia untuk pembeli dan penyedia.
- Login menggunakan email dan kata sandi.
- Kata sandi disimpan dalam bentuk hash.
- Akses pengguna menggunakan token Bearer.
- Logout menggunakan mekanisme pencabutan versi token.
- Endpoint tertentu dibatasi berdasarkan peran dan kepemilikan data.
- Login dan registrasi menggunakan pembatasan laju request.

## Alur Penawaran dan Transaksi

1. Penyedia mendaftarkan akun.
2. Akun penyedia melalui proses verifikasi.
3. Penyedia membuat penawaran makanan dengan informasi harga, stok, satuan, dan waktu berakhir.
4. Pembeli melihat penawaran yang tersedia melalui katalog.
5. Pembeli membuat pesanan.
6. Sistem menghitung harga berdasarkan data penawaran di server dan melakukan reservasi stok.
7. Penyedia mengonfirmasi atau menolak pesanan.
8. Penyedia menandai pesanan siap diambil.
9. Sistem menyediakan kode pengambilan.
10. Setelah makanan diserahterimakan, penyedia mencatat pengambilan dan konfirmasi pembayaran.
11. Sistem memperbarui status pesanan dan ringkasan dashboard.

Satu pesanan hanya dapat berisi penawaran dari satu penyedia.

Pembatalan sebelum pengambilan mengikuti aturan status pesanan. Sistem memulihkan stok sesuai kondisi pembatalan agar stok tidak dikembalikan lebih dari satu kali.

## Definisi Metrik Dashboard

Dashboard menampilkan ringkasan berdasarkan akun yang sedang login. Pembeli melihat data pesanan miliknya, sedangkan penyedia melihat data pesanan dan penawaran usahanya.

### Metrik Pesanan

| Metrik | Definisi |
|---|---|
| Jumlah Pesanan Total | Seluruh pesanan, termasuk pesanan yang dibatalkan. |
| Jumlah Pesanan | Pesanan selain yang dibatalkan. |
| Menunggu Konfirmasi | Pesanan dengan status menunggu konfirmasi penyedia. |
| Dikonfirmasi | Pesanan yang telah diterima penyedia. |
| Siap Diambil | Pesanan yang telah ditandai siap diambil. |
| Selesai | Pesanan berstatus selesai dengan pengambilan yang tercatat. |
| Dibatalkan | Pesanan yang dibatalkan. |
| Pesanan Berjalan | Pesanan yang menunggu konfirmasi, telah dikonfirmasi, atau siap diambil. |

### Metrik Nilai dan Jumlah Makanan

| Metrik | Definisi |
|---|---|
| Nilai Pemesanan | Total nilai transaksi dari pesanan yang tidak dibatalkan. |
| Unit Dipesan | Jumlah unit makanan dari pesanan yang tidak dibatalkan. |
| Nilai Transaksi Selesai | Nilai transaksi dari pesanan yang sudah diserahterimakan. |
| Unit Tersalurkan | Jumlah unit makanan yang sudah dicatat diambil. |
| Nilai Normal Tersalurkan | Nilai normal makanan yang sudah diserahterimakan. |
| Total Belanja | Nilai transaksi pembeli yang sudah selesai dan berstatus dibayar atau gratis. |
| Pendapatan Diterima | Nilai transaksi penyedia yang sudah selesai dan tercatat dibayar atau gratis. |
| Penghematan Pembeli | Selisih nilai normal makanan dan nilai transaksi yang sudah diselesaikan serta tercatat dibayar atau gratis. |
| Diskon Diberikan | Nilai selisih harga normal dan harga penawaran pada transaksi yang memenuhi kriteria penyelesaian dan pembayaran. |
| Pembayaran Belum Tercatat | Jumlah pesanan yang sudah tersalur tetapi status pembayarannya belum tercatat. |

### Metrik Penawaran Penyedia

| Metrik | Definisi |
|---|---|
| Jumlah Penawaran | Jumlah seluruh penawaran milik penyedia. |
| Penawaran Aktif | Penawaran berstatus aktif, memiliki stok lebih dari nol, dan belum melewati waktu berakhir. |
| Stok Tersedia | Jumlah stok pada penawaran aktif yang belum melewati waktu berakhir. |

Penghematan pembeli dihitung dari selisih nilai stok normal yang terselamatkan dengan nilai transaksi. Perhitungan ini hanya memasukkan pesanan yang sudah diserahterimakan dan tercatat dibayar atau gratis.

Penghematan biaya penjual belum dihitung oleh aplikasi. Nilainya belum tersedia pada dashboard.

## Batas Fitur dan Penggunaan

- Satu pesanan hanya dapat berisi penawaran dari satu penyedia.
- Akun penyedia harus melalui verifikasi sebelum dapat menjalankan fungsi penyedia.
- Pembayaran dicatat ketika serah terima makanan. Aplikasi tidak menggunakan integrasi payment gateway dalam alur transaksi yang diterapkan.
- Penghematan biaya penjual belum dihitung.
- Penghematan pembeli menggunakan selisih nilai normal makanan dan nilai transaksi, bukan perhitungan biaya operasional.
- Stok dan status pesanan mengikuti transaksi yang tercatat di database.
- Deployment menggunakan layanan gratis untuk kebutuhan pengembangan dan demonstrasi. Kapasitas serta ketersediaan layanan mengikuti batas penyedia hosting.
- Pengujian manual dan audit aksesibilitas belum mencakup seluruh kemungkinan penggunaan.

## Pengujian

### Build

Jalankan:

`npm run build`

### Pengujian Alur Transaksi Otomatis

Jalankan:

`npm run workflow:test`

Pengujian mencakup pemesanan, reservasi stok, pembatalan, konfirmasi, penolakan, pengambilan, pembayaran, hak akses, dashboard, pemrosesan foto, dan kondisi transaksi bersamaan.

### Pengujian Deployment

Jalankan:

`npm run deploy:test -- https://sisa-baik.onrender.com`

Pemeriksaan mencakup health check, frontend, aset, autentikasi, respons endpoint, dan perlindungan file sensitif.

### Hasil Pengujian Minggu 12

- Pengujian deployment: 10 dari 10 pemeriksaan lulus.
- Pengujian alur transaksi otomatis: 26 skenario lulus.
- Pengujian cloud: pendaftaran penyedia dan pembeli, verifikasi penyedia, penyimpanan penawaran, pemesanan, dan alur konfirmasi telah dilakukan.
- Pemeriksaan keyboard dasar: berhasil pada skenario yang diuji.
- Pemeriksaan validasi formulir: skenario pendaftaran dan penawaran yang diuji berjalan sesuai harapan.
- Pemeriksaan responsivitas: mobile, tablet, dan desktop telah diperiksa.
- Audit Firefox menemukan satu masalah rasio kontras warna yang perlu diperbaiki.
- Pengujian manual F01-F20 dan S01-S11 belum dilakukan.
- Pengukuran performa cloud telah dicatat menggunakan Mozilla Firefox Network Monitor. Pengukuran sebelum perubahan belum tersedia sehingga perbandingan sebelum dan sesudah belum dapat disimpulkan.

Dokumentasi dan hasil pengujian tersedia pada:

- `docs/minggu12/matriks-pengujian.md`
- `docs/minggu12/audit-aksesibilitas.md`
- `docs/minggu12/performa.md`
- `docs/minggu12/log-pengujian-cloud.md`
- `docs/minggu12/refleksi.md`
- `docs/minggu12/bukti/`

## Keamanan dan Pengelolaan Data

- File `.env` dan `.env.cloud` tidak boleh di-commit.
- Jangan mengunggah password, JWT secret, token, atau kredensial database.
- Jangan mengunggah dump database ke repository publik.
- Gunakan `.env.example` dan `.env.cloud.example` sebagai template tanpa kredensial rahasia.
- Jangan membagikan akun pengujian melalui repository publik.
- Data dan akses pengguna dibatasi berdasarkan autentikasi, peran, serta kepemilikan.

## Pengembangan Selanjutnya

Pengembangan berikutnya dapat difokuskan pada perbaikan temuan kontras warna, pengujian manual yang belum dilakukan, audit aksesibilitas lebih menyeluruh, pengukuran performa sebelum dan sesudah perubahan, serta penyempurnaan dokumentasi dan evaluasi operasional aplikasi.
