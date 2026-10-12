# SisaBaik API

SisaBaik adalah proyek pengembangan web pada mata kuliah Pengembangan Web. Pada Chapter 7, proyek ini dikembangkan menggunakan Node.js, Express, dan PostgreSQL untuk mengelola data penawaran makanan berlebih dan pemesanan melalui REST API. Pengembangan ini melanjutkan proyek sebelumnya dengan menghubungkan aplikasi ke basis data dan menambahkan validasi serta pengelolaan transaksi pemesanan.

## Capaian Chapter 7

- Menghubungkan aplikasi dengan PostgreSQL dari Chapter 6.
- Mengembangkan REST API untuk mengelola penawaran dan pemesanan.
- Menerapkan validasi data pada request.
- Menggunakan `emailPembeli` pada request pemesanan sebagai pengganti `namaPemesan`.
- Menggunakan transaksi database untuk menjaga konsistensi data dan stok.
- Menangani respons dan kesalahan HTTP.
- Menguji fungsi API dan pemesanan saat stok diakses secara bersamaan.

## Bagian Proyek

- `server.js`: menjalankan server aplikasi.
- `src/app.js`: mengatur aplikasi Express.
- `src/routes/`: mengatur rute API.
- `src/controllers/`: menangani request dan response.
- `src/services/`: menangani proses bisnis aplikasi.
- `src/repositories/`: mengelola akses data.
- `src/db/`: mengatur koneksi dan transaksi database.
- `src/lib/`: berisi fungsi pendukung aplikasi.
- `public/`: berisi halaman pengujian API.
- `scripts/`: berisi script pengujian API dan konkurensi.
- `docs/`: berisi dokumentasi API dan hasil pengujian.

## Menjalankan Proyek

Pastikan Node.js, npm, dan PostgreSQL sudah tersedia. Database `sisabaik_dev` dari Chapter 6 harus sudah disiapkan dan konfigurasi koneksinya disesuaikan dengan pengaturan lokal.

Buka terminal pada folder proyek, lalu jalankan pemeriksaan koneksi database:

```bash
npm run db:check -- 0
```

Untuk menjalankan server dan script pengujian, gunakan perintah yang tersedia pada `package.json`.

## Dokumentasi dan Pengujian

Dokumentasi endpoint, format request, parameter query, contoh response, dan status error dicatat dalam `docs/API.md`. Hasil pengujian API dan konkurensi dicatat dalam `docs/hasil-uji-chapter7.md`.

ID yang diterima melalui request API diperlakukan sebagai string dan divalidasi sebelum diproses. Satu pesanan hanya dapat berisi penawaran dari satu penyedia.

## Batasan Aplikasi

Aplikasi masih digunakan untuk pengembangan dan pengujian secara lokal. Autentikasi pengguna belum diterapkan sehingga aplikasi belum ditujukan untuk penggunaan produksi.
