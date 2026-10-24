# Dokumentasi REST API SisaBaik

## 1. Gambaran Umum

REST API SisaBaik dikembangkan menggunakan Node.js, Express, dan PostgreSQL untuk mengelola penawaran makanan berlebih dan pemesanan.

Alamat dasar: `http://localhost:3000`

Request yang mengirim data menggunakan body JSON. Endpoint yang memerlukan autentikasi menggunakan token Bearer. Identitas pengguna dan peran diambil dari sesi yang telah diverifikasi oleh server.

## 2. Daftar Endpoint

| Method | Endpoint                        | Fungsi                                     |
| ------ | ------------------------------- | ------------------------------------------ |
| GET    | `/api/health`                   | Memeriksa status aplikasi                  |
| POST   | `/api/auth/registrasi`          | Registrasi pembeli                         |
| POST   | `/api/auth/registrasi-penyedia` | Registrasi penyedia                        |
| POST   | `/api/auth/login`               | Login                                      |
| GET    | `/api/auth/saya`                | Mendapatkan profil akun yang login         |
| POST   | `/api/auth/logout`              | Logout                                     |
| GET    | `/api/penawaran`                | Katalog publik                             |
| GET    | `/api/penawaran/:id`            | Detail penawaran                           |
| GET    | `/api/penawaran/milik-saya`     | Daftar penawaran milik penyedia yang login |
| POST   | `/api/penawaran`                | Membuat penawaran                          |
| PUT    | `/api/penawaran/:id`            | Mengubah penawaran milik sendiri           |
| DELETE | `/api/penawaran/:id`            | Menghapus penawaran milik sendiri          |
| GET    | `/api/pesanan`                  | Daftar pesanan sesuai hak akses            |
| GET    | `/api/pesanan/:id`              | Detail pesanan sesuai hak akses            |
| POST   | `/api/pesanan`                  | Membuat pesanan                            |

Endpoint yang memerlukan login harus mengirim header:

`Authorization: Bearer <accessToken>`

## 3. Autentikasi

### 3.1 Registrasi Pembeli

**Request:** `POST /api/auth/registrasi`

Body JSON:

```json
{
  "namaLengkap": "Siti Baru",
  "email": "siti@example.com",
  "kataSandi": "GunakanKataSandiKuat15"
}
```

Field:

- `namaLengkap`: teks 2–100 karakter.
- `email`: alamat email valid, 3–254 karakter.
- `kataSandi`: kata sandi baru minimal 15 karakter dan maksimum 72 byte UTF-8.

Field tambahan yang tidak didukung akan ditolak.

### 3.2 Registrasi Penyedia

**Request:** `POST /api/auth/registrasi-penyedia`

Body JSON:

```json
{
  "namaLengkap": "Rina Baru",
  "email": "rina@example.com",
  "kataSandi": "GunakanKataSandiKuat15",
  "namaUsaha": "Rina Bakery",
  "alamat": "Alamat usaha lengkap",
  "nomorTelepon": "+628123456789"
}
```

Field profil usaha:

- `namaUsaha`: teks 2–120 karakter.
- `alamat`: teks 5–1.000 karakter.
- `nomorTelepon`: 8–15 digit, boleh menggunakan tanda `+` di awal. Spasi, tanda kurung, dan tanda hubung akan dibersihkan sebelum disimpan.

Pendaftaran penyedia tidak otomatis berarti penyedia sudah disetujui. Peninjauan penyedia dilakukan melalui mekanisme verifikasi yang disediakan proyek.

### 3.3 Login

**Request:** `POST /api/auth/login`

Body JSON:

```json
{
  "email": "siti@example.com",
  "kataSandi": "GunakanKataSandiKuat15"
}
```

Login yang berhasil mengembalikan access token bertipe `Bearer`, informasi pengguna, dan masa berlaku token sesuai konfigurasi aplikasi.

Login ditolak jika kredensial tidak valid atau akun tidak aktif. Pesan kesalahan login tidak membedakan apakah email atau kata sandi yang salah.

### 3.4 Profil Sesi

**Request:** `GET /api/auth/saya`

Memerlukan token Bearer yang masih berlaku. Endpoint mengembalikan informasi akun yang sedang login.

### 3.5 Logout

**Request:** `POST /api/auth/logout`

Memerlukan token Bearer. Logout mencabut keabsahan sesi melalui mekanisme versi token akun. Token lama yang sudah dicabut tidak dapat digunakan kembali. Sesi lain pada akun yang sama juga tidak lagi berlaku apabila versi token akun berubah.

## 4. Kebijakan Katalog Publik

### 4.1 Daftar Penawaran

**Request:** `GET /api/penawaran`

Katalog publik hanya menampilkan penawaran yang berstatus `aktif` dan memiliki stok tersedia.

Parameter pencarian dan pagination yang didukung:

| Parameter  | Ketentuan                                              |
| ---------- | ------------------------------------------------------ |
| `q`        | Kata pencarian, maksimal 100 karakter                  |
| `kategori` | Kategori, maksimal 40 karakter                         |
| `status`   | Untuk katalog publik, hanya `aktif` yang diperbolehkan |
| `tersedia` | Katalog publik hanya menampilkan penawaran tersedia    |
| `page`     | Bilangan positif, default `1`                          |
| `limit`    | Bilangan positif, default `10`, maksimal `50`          |

Contoh:

`GET /api/penawaran?q=roti&kategori=Roti&page=1&limit=10`

Permintaan katalog publik yang secara eksplisit meminta status selain `aktif` atau `tersedia=false` ditolak oleh validasi katalog.

### 4.2 Detail Penawaran

**Request:** `GET /api/penawaran/:id`

Menampilkan detail penawaran berdasarkan ID yang valid, mengikuti aturan pembacaan data yang diterapkan oleh aplikasi.

### 4.3 Daftar Penawaran Milik Penyedia

**Request:** `GET /api/penawaran/milik-saya`

Memerlukan akun penyedia yang telah login. Hasil dibatasi pada penawaran milik penyedia tersebut.

## 5. Pengelolaan Penawaran

### 5.1 Membuat Penawaran

**Request:** `POST /api/penawaran`

Memerlukan autentikasi dan peran penyedia.

Body JSON:

```json
{
  "nama": "Paket Roti Sore",
  "kategori": "Roti",
  "hargaNormal": 20000,
  "hargaPenawaran": 15000,
  "stok": 5,
  "satuan": "paket",
  "status": "aktif",
  "berakhirPada": "2027-12-31T23:59:59+07:00"
}
```

Field:

- `nama`: teks 3–120 karakter.
- `kategori`: teks 1–40 karakter.
- `hargaNormal`: bilangan bulat positif sesuai batas validasi aplikasi.
- `hargaPenawaran`: bilangan bulat, tidak boleh melebihi `hargaNormal`.
- `stok`: bilangan bulat nonnegatif.
- `satuan`: teks 1–20 karakter.
- `status`: `draft`, `aktif`, `habis`, atau `kedaluwarsa`, mengikuti aturan bisnis.
- `berakhirPada`: tanggal dan waktu ISO 8601 dengan zona waktu, harus berada di masa depan.

`penyediaId` tidak dikirim melalui body. Identitas penyedia diambil dari akun yang login.

### 5.2 Mengubah Penawaran

**Request:** `PUT /api/penawaran/:id`

Memerlukan akun penyedia yang login dan merupakan pemilik penawaran. Body mengikuti field dan aturan validasi pada bagian 5.1.

### 5.3 Menghapus Penawaran

**Request:** `DELETE /api/penawaran/:id`

Memerlukan akun penyedia yang login dan merupakan pemilik penawaran. Respons berhasil menggunakan HTTP 204 tanpa body.

## 6. Pemesanan

### 6.1 Daftar Pesanan

**Request:** `GET /api/pesanan`

Memerlukan autentikasi. Daftar pesanan dibatasi sesuai hak akses pengguna yang login dan menggunakan pagination.

Parameter:

- `page`: bilangan positif, default `1`.
- `limit`: bilangan positif, default `10`, maksimal `50`.

### 6.2 Detail Pesanan

**Request:** `GET /api/pesanan/:id`

Memerlukan autentikasi dan pemeriksaan hak akses terhadap pesanan. Pesanan milik pengguna lain tidak diberikan kepada peminta yang tidak berhak.

### 6.3 Membuat Pesanan

**Request:** `POST /api/pesanan`

Memerlukan akun pembeli yang login.

Body JSON:

```json
{
  "items": [
    {
      "penawaranId": "1",
      "kuantitas": 2
    }
  ]
}
```

Field:

- `items`: array berisi 1–10 item.
- `items[].penawaranId`: ID penawaran positif yang valid.
- `items[].kuantitas`: bilangan bulat 1–1.000.

Ketentuan:

- ID penawaran tidak boleh berulang dalam satu pesanan.
- Satu pesanan hanya boleh berisi penawaran dari satu penyedia.
- Penawaran harus memenuhi aturan status dan ketersediaan stok.
- `pembeliId` dan email pembeli diambil dari akun yang login, bukan dari body request.
- Proses pemesanan mengikuti transaksi database untuk menjaga konsistensi data.

## 7. Batas Laju Request

Login dan registrasi menggunakan pembatasan laju terpisah.

| Jenis request                   |                   Batas | Periode  | Kode error                |
| ------------------------------- | ----------------------: | -------- | ------------------------- |
| Login                           | 20 percobaan yang gagal | 15 menit | `AUTH_RATE_LIMIT`         |
| Registrasi pembeli dan penyedia |              50 request | 15 menit | `REGISTRATION_RATE_LIMIT` |

Respons ketika batas tercapai menggunakan HTTP 429 dan menyediakan header `Retry-After`. Login yang berhasil tidak dihitung oleh limiter login. Kedua endpoint registrasi berbagi limiter registrasi.

## 8. Validasi dan Penanganan Error

Format umum respons error:

```json
{
  "success": false,
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "Pesan kesalahan."
  }
}
```

Kode dan status error utama:

| Status HTTP | Kode                      | Keterangan                                                                  |
| ----------- | ------------------------- | --------------------------------------------------------------------------- |
| 400         | `VALIDATION_ERROR`        | Data request tidak valid                                                    |
| 400         | `INVALID_JSON`            | Format JSON tidak valid                                                     |
| 400         | `DATA_CONSTRAINT`         | Pelanggaran aturan database                                                 |
| 401         | `LOGIN_FAILED`            | Login gagal                                                                 |
| 401         | `SESSION_REVOKED`         | Sesi/token tidak lagi berlaku                                               |
| 401         | `UNAUTHORIZED`            | Autentikasi tidak tersedia atau tidak valid, jika digunakan oleh middleware |
| 403         | —                         | Peran akun tidak memiliki izin                                              |
| 404         | —                         | Data tidak ditemukan atau akses terhadap data tidak diberikan               |
| 409         | `REGISTRATION_CONFLICT`   | Pendaftaran tidak dapat diproses karena konflik data                        |
| 409         | `REFERENCE_CONFLICT`      | Konflik relasi data                                                         |
| 409         | `DATA_CONFLICT`           | Konflik data                                                                |
| 409         | `TRANSACTION_BUSY`        | Transaksi belum dapat diselesaikan                                          |
| 413         | `PAYLOAD_TOO_LARGE`       | Payload melebihi 20 KB                                                      |
| 429         | `AUTH_RATE_LIMIT`         | Batas percobaan login tercapai                                              |
| 429         | `REGISTRATION_RATE_LIMIT` | Batas registrasi tercapai                                                   |
| 500         | `INTERNAL_SERVER_ERROR`   | Kesalahan internal server                                                   |
| 503         | `DATABASE_UNAVAILABLE`    | Database belum tersedia                                                     |

Status dan kode error aktual bergantung pada middleware dan penanganan error yang digunakan oleh endpoint.

## 9. Keamanan

- Kata sandi tidak boleh dikirim kembali dalam respons API.
- JWT dan hash kata sandi tidak boleh dicatat dalam dokumentasi atau log yang dibagikan.
- Identitas pengguna, peran, dan kepemilikan ditentukan melalui sesi yang diverifikasi.
- Pengguna tidak boleh mengubah atau membaca data milik pengguna lain tanpa otorisasi.
- File environment lokal tidak boleh dimasukkan ke repository.

API ini dikembangkan untuk pembelajaran dan pengujian lokal. Sebelum penggunaan produksi, lakukan pengujian keamanan dan pemeriksaan konfigurasi secara menyeluruh.
