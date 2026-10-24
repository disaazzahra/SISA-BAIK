# Hasil Pengujian Chapter 8 — Autentikasi dan Hak Akses

## 1. Tujuan Pengujian

Pengujian ini dilakukan untuk memastikan autentikasi pengguna, pembatasan akses berdasarkan peran, pemeriksaan kepemilikan data, pencabutan sesi, dan pembatasan percobaan login pada REST API SisaBaik berjalan sesuai rancangan.

Pengujian dilakukan melalui request HTTP, script pengujian, dan pemeriksaan data pada PostgreSQL.

## 2. Hasil Pengujian Otomatis

Pengujian otomatis dijalankan menggunakan perintah `npm run auth:test`.

Hasil pengujian menunjukkan **44 pemeriksaan HTTP dan assertion kepemilikan/snapshot berhasil dilewati**. Script juga memeriksa akses terhadap penawaran dan pesanan. Selama pengujian, empat akun uji dibuat menggunakan kata sandi acak yang tidak dicetak pada output.

| No. | Aspek yang diuji                                       | Hasil |
| --- | ------------------------------------------------------ | ----- |
| 1   | Endpoint autentikasi dan respons HTTP                  | PASS  |
| 2   | Pemeriksaan kepemilikan penawaran                      | PASS  |
| 3   | Pemeriksaan kepemilikan pesanan                        | PASS  |
| 4   | Pemeriksaan kondisi data sebelum dan sesudah pengujian | PASS  |

## 3. Pengujian Hak Akses Berdasarkan Peran

### 3.1 Pembeli mencoba membuat penawaran

Akun pembeli mencoba mengirim request `POST /api/penawaran` dengan body kosong.

Hasil: HTTP 403.

Kesimpulan: request ditolak karena pembeli tidak memiliki izin untuk membuat penawaran. Pengujian ini menunjukkan bahwa endpoint penawaran membatasi akses berdasarkan peran akun.

### 3.2 Penyedia mencoba membuat pesanan

Akun penyedia mencoba mengirim request `POST /api/pesanan` dengan body kosong.

Hasil: HTTP 403.

Kesimpulan: request ditolak karena penyedia tidak memiliki izin untuk membuat pesanan sebagai pembeli.

## 4. Pengujian Kepemilikan Data

### 4.1 Penyedia mencoba mengubah penawaran milik penyedia lain

Akun Budi mencoba mengubah penawaran ID 16 yang dimiliki oleh Rina Bakery. Request menggunakan body yang valid agar pengujian berfokus pada pemeriksaan kepemilikan, bukan kesalahan validasi input.

Hasil: HTTP 404.

Pemeriksaan database menunjukkan bahwa penawaran ID 16 tetap memiliki data berikut:

| Field               | Nilai                    |
| ------------------- | ------------------------ |
| ID penawaran        | 16                       |
| Nama                | Roti Cokelat Rina Bakery |
| ID penyedia         | 3                        |
| ID pengguna pemilik | 7                        |
| Stok                | 9                        |

Kesimpulan: perubahan terhadap penawaran milik penyedia lain ditolak. Data penawaran tetap tidak berubah setelah request yang tidak berhak dilakukan.

### 4.2 Pembeli mencoba melihat pesanan milik pembeli lain

Akun Andi mencoba mengakses `GET /api/pesanan/8`, sedangkan pesanan ID 8 tercatat milik `siti.baru@example.test`.

Hasil: HTTP 404.

Kesimpulan: sistem tidak memberikan akses kepada pembeli untuk melihat pesanan milik pengguna lain.

## 5. Pengujian Sesi dan Logout

### 5.1 Akun dinonaktifkan saat masih memiliki token

Akun Andi dinonaktifkan sementara pada database. Ketika token lama digunakan untuk mengakses `GET /api/auth/saya`, server memberikan respons HTTP 401 dengan kode `SESSION_REVOKED`.

Pesan respons: `Sesi tidak berlaku. Silakan login lagi.`

Setelah pengujian, akun diaktifkan kembali.

Kesimpulan: token tidak dapat digunakan untuk mengakses endpoint terlindungi ketika akun sudah tidak aktif.

### 5.2 Pengujian logout

Request `POST /api/auth/logout` berhasil dengan respons HTTP 204. Ketika token lama digunakan kembali setelah logout, server memberikan respons HTTP 401.

Kesimpulan: logout membuat token lama tidak dapat digunakan kembali.

## 6. Pengujian Pembatasan Percobaan Login

Pengujian dilakukan dengan mengirim percobaan login menggunakan email yang tidak terdaftar dan kata sandi yang salah.

Hasil pengamatan:

| Percobaan                                | Hasil                                        |
| ---------------------------------------- | -------------------------------------------- |
| Percobaan gagal sebelum batas tercapai   | HTTP 401, `LOGIN_FAILED`                     |
| Percobaan ke-21 pada rangkaian pengujian | HTTP 429, `AUTH_RATE_LIMIT`                  |
| Header pembatasan                        | `Retry-After: 462` detik pada saat pengujian |

Angka `Retry-After` merupakan hasil pada waktu pengujian dan dapat berbeda pada percobaan berikutnya karena bergantung pada sisa waktu jendela pembatasan.

Kesimpulan: mekanisme pembatasan percobaan login berjalan dan menolak request setelah batas yang ditentukan tercapai.

## 7. Ringkasan Hasil

Berdasarkan pengujian yang telah dilakukan, autentikasi, pembatasan hak akses berdasarkan peran, pemeriksaan kepemilikan data, pencabutan token, dan pembatasan percobaan login menunjukkan hasil sesuai dengan skenario pengujian.

Beberapa pengujian tambahan belum dilakukan, yaitu pengujian kedaluwarsa token setelah melewati masa berlaku serta pengujian khusus batas laju registrasi. Kedua hal tersebut dicatat sebagai pengujian lanjutan.

## 8. Bukti Pengujian

Bukti pengujian yang digunakan meliputi:

- Output script `npm run auth:test`.
- Respons HTTP untuk percobaan akses yang tidak diizinkan.
- Respons HTTP setelah logout dan penggunaan kembali token lama.
- Respons HTTP saat batas percobaan login tercapai.
- Hasil query PostgreSQL untuk memastikan data penawaran dan pesanan tetap sesuai.

Tidak ada kata sandi, hash kata sandi, atau nilai token yang dicantumkan dalam dokumen ini.
