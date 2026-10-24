# SisaBaik API

SisaBaik adalah aplikasi web untuk mengelola penawaran makanan berlebih dan pemesanan. Proyek ini menggunakan Node.js, Express, dan PostgreSQL. Pengembangan mencakup REST API, validasi data, transaksi pemesanan, autentikasi, pendaftaran mandiri, serta pemeriksaan peran dan kepemilikan data.

## Teknologi

- Node.js versi 22 atau lebih baru
- Express
- PostgreSQL
- `pg` untuk akses database
- `bcryptjs` untuk hash kata sandi
- `jsonwebtoken` untuk token akses
- `express-rate-limit` untuk pembatasan request
- Helmet untuk pengamanan header HTTP

## Struktur Proyek

- `server.js`: menjalankan server.
- `src/app.js`: konfigurasi aplikasi Express.
- `src/config/`: konfigurasi aplikasi dan autentikasi.
- `src/routes/`: endpoint API.
- `src/controllers/`: menangani request dan response.
- `src/services/`: menjalankan proses bisnis.
- `src/repositories/`: mengelola akses database.
- `src/middleware/`: autentikasi dan pembatasan request.
- `src/lib/`: fungsi validasi dan pendukung.
- `database/07_auth.sql`: migrasi database untuk autentikasi.
- `public/auth.html`, `public/auth.css`, `public/auth.js`: halaman autentikasi.
- `scripts/uji-auth.js`: pengujian autentikasi dan kepemilikan.
- `scripts/verifikasi-penyedia.js`: alat verifikasi penyedia.
- `docs/API.md`: dokumentasi endpoint API.
- `docs/hasil-uji-chapter8.md`: hasil pengujian Chapter 8.

## Konfigurasi Environment

Buat file `.env` berdasarkan `.env.example`, lalu sesuaikan nilainya dengan konfigurasi PostgreSQL lokal.

Nama variabel environment yang digunakan:

| Variabel     | Keterangan                           |
| ------------ | ------------------------------------ |
| `PORT`       | Port server, contoh `3000`           |
| `PGHOST`     | Host PostgreSQL                      |
| `PGPORT`     | Port PostgreSQL                      |
| `PGDATABASE` | Nama database, contoh `sisabaik_dev` |
| `PGUSER`     | Pengguna PostgreSQL                  |
| `PGPASSWORD` | Kata sandi PostgreSQL lokal          |

Jangan menyimpan kata sandi database, token, atau nilai rahasia di README maupun repository. File `.env` tidak boleh ikut di-commit. Gunakan `.env.example` sebagai template tanpa kredensial asli.

## Persiapan Database

Pastikan database PostgreSQL sudah disiapkan. Jalankan migrasi `database/07_auth.sql` pada database pengembangan untuk menyiapkan kebutuhan autentikasi, setelah memastikan skema awal tersedia dan migrasi sesuai dengan kondisi database.

Periksa koneksi database dengan perintah:

`npm run db:check -- 0`

## Menjalankan Aplikasi

Instal dependensi jika diperlukan:

`npm install`

Jalankan server dalam mode pengembangan:

`npm run dev`

Atau jalankan server biasa:

`npm start`

Server menggunakan port sesuai konfigurasi `PORT`, dengan nilai contoh `3000`. Halaman autentikasi dapat diakses melalui `http://localhost:3000/auth.html` saat server berjalan.

## Autentikasi dan Peran

SisaBaik menyediakan pendaftaran mandiri untuk pembeli dan penyedia, login, pemeriksaan sesi, serta logout.

- Pembeli dapat membuat pesanan dan mengakses pesanan sesuai hak aksesnya.
- Penyedia dapat mengelola penawaran miliknya sendiri.
- Pendaftaran penyedia tidak otomatis berarti penyedia telah lolos peninjauan.
- Identitas dan peran pengguna ditentukan oleh sistem, bukan dari nilai yang dikirim melalui body request.
- Kata sandi disimpan dalam bentuk hash, bukan teks asli.
- Endpoint yang memerlukan autentikasi menggunakan token Bearer.
- Logout membatalkan sesi/token sesuai mekanisme versi token akun sehingga token lama yang telah dicabut tidak dapat digunakan kembali.

## Pengujian

Jalankan pengujian autentikasi dan kepemilikan:

`npm run auth:test`

Jalankan pengujian API Chapter 7:

`npm run api:test:chapter7`

Jalankan alat verifikasi penyedia:

`npm run penyedia:verifikasi`

Pengujian harus dijalankan terhadap database pengembangan yang sesuai. Catat hasil aktual dan jangan memasukkan password, hash password, atau JWT ke dokumentasi.

Hasil pengujian autentikasi dan kontrol akses dicatat dalam `docs/hasil-uji-chapter8.md`. Dokumentasi endpoint, format request, serta status error tersedia di `docs/API.md`.

## Kebijakan Akses dan Keamanan

- Katalog publik hanya menampilkan penawaran aktif yang tersedia.
- Operasi penawaran dibatasi berdasarkan autentikasi, peran, dan kepemilikan.
- Pembeli tidak dapat membuat penawaran sebagai penyedia.
- Penyedia tidak dapat membuat pesanan sebagai pembeli.
- Pengguna tidak boleh membaca atau mengubah data milik pengguna lain tanpa otorisasi.
- Login dan registrasi memiliki pembatasan laju request yang terpisah.
- File `.env`, `node_modules`, password, hash password, dan token akses tidak boleh masuk ke commit.

Aplikasi ini merupakan proyek pengembangan dan pengujian. Penggunaan produksi memerlukan konfigurasi, pengujian keamanan, dan pengelolaan operasional lebih lanjut.
