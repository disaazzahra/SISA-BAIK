# Dokumentasi REST API SisaBaik

## 1. Gambaran Umum

REST API SisaBaik digunakan untuk mengelola data penawaran makanan berlebih dan pemesanan. API dikembangkan menggunakan Node.js, Express, dan PostgreSQL.

**Alamat dasar:** `http://localhost:3000`

Request yang mengirimkan data menggunakan body JSON. ID yang dikirim melalui request divalidasi dan dinormalisasi menjadi string oleh fungsi validasi.

## 2. Daftar Endpoint

| Method | Endpoint             | Fungsi                       |
| ------ | -------------------- | ---------------------------- |
| GET    | `/api/health`        | Memeriksa status aplikasi    |
| GET    | `/api/penawaran`     | Menampilkan daftar penawaran |
| GET    | `/api/penawaran/:id` | Menampilkan detail penawaran |
| POST   | `/api/penawaran`     | Menambahkan penawaran        |
| PUT    | `/api/penawaran/:id` | Mengubah penawaran           |
| DELETE | `/api/penawaran/:id` | Menghapus penawaran          |
| GET    | `/api/pesanan`       | Menampilkan daftar pesanan   |
| POST   | `/api/pesanan`       | Membuat pesanan              |

## 3. API Penawaran

### 3.1 Menampilkan Daftar Penawaran

**Request**

`GET /api/penawaran`

Endpoint ini digunakan untuk mengambil daftar penawaran. Parameter query yang didukung oleh fungsi validasi adalah sebagai berikut.

| Parameter  | Tipe            | Ketentuan                                     |
| ---------- | --------------- | --------------------------------------------- |
| `q`        | String          | Maksimal 100 karakter                         |
| `kategori` | String          | Maksimal 40 karakter                          |
| `status`   | String          | `draft`, `aktif`, `habis`, atau `kedaluwarsa` |
| `tersedia` | String          | `true` atau `false`; nilai default `false`    |
| `page`     | Integer positif | Default `1`, maksimal `1000000`               |
| `limit`    | Integer positif | Default `10`, maksimal `50`                   |

Contoh request:

`GET /api/penawaran?q=roti&kategori=Roti&status=aktif&tersedia=true&page=1&limit=10`

Parameter pencarian dan pagination digunakan untuk menyaring serta membatasi data yang ditampilkan. Hasil aktual bergantung pada data yang tersimpan dan implementasi query di aplikasi.

### 3.2 Menampilkan Detail Penawaran

**Request**

`GET /api/penawaran/:id`

Parameter `id` merupakan ID penawaran yang ingin ditampilkan. ID harus berupa string digit positif atau angka yang memenuhi aturan validasi.

Contoh:

`GET /api/penawaran/1`

### 3.3 Menambahkan Penawaran

**Request**

`POST /api/penawaran`

Endpoint ini digunakan untuk menambahkan penawaran baru. Data dikirim menggunakan body JSON.

| Field            | Tipe data         | Ketentuan                                                                      |
| ---------------- | ----------------- | ------------------------------------------------------------------------------ |
| `penyediaId`     | String atau angka | ID positif yang valid                                                          |
| `nama`           | String            | 3–120 karakter                                                                 |
| `kategori`       | String            | 1–40 karakter                                                                  |
| `hargaNormal`    | Integer           | 1–999.999.999.999                                                              |
| `hargaPenawaran` | Integer           | 0–999.999.999.999 dan tidak boleh melebihi `hargaNormal`                       |
| `stok`           | Integer           | 0–2.147.483.647                                                                |
| `satuan`         | String            | 1–20 karakter                                                                  |
| `status`         | String            | `draft`, `aktif`, `habis`, atau `kedaluwarsa`                                  |
| `berakhirPada`   | String            | Tanggal dan waktu ISO 8601 dengan zona waktu, serta harus berada di masa depan |

Contoh request:

```json
{
  "penyediaId": "1",
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

Semua field tersebut wajib dikirim. Body tidak boleh berisi field tambahan yang tidak didukung.

### 3.4 Mengubah Penawaran

**Request**

`PUT /api/penawaran/:id`

Endpoint ini digunakan untuk mengubah data penawaran berdasarkan ID. Data dikirim menggunakan body JSON dengan field dan aturan validasi yang sama seperti pada penambahan penawaran.

Contoh:

`PUT /api/penawaran/1`

Body request mengikuti format pada bagian 3.3.

### 3.5 Menghapus Penawaran

**Request**

`DELETE /api/penawaran/:id`

Endpoint ini digunakan untuk menghapus penawaran berdasarkan ID.

Contoh:

`DELETE /api/penawaran/1`

Keberhasilan penghapusan mengikuti aturan bisnis dan penanganan data yang diterapkan oleh aplikasi.

## 4. API Pemesanan

### 4.1 Menampilkan Daftar Pesanan

**Request**

`GET /api/pesanan`

Endpoint ini digunakan untuk mengambil daftar pesanan. Parameter pagination yang didukung oleh fungsi validasi adalah:

| Parameter | Tipe            | Ketentuan                       |
| --------- | --------------- | ------------------------------- |
| `page`    | Integer positif | Default `1`, maksimal `1000000` |
| `limit`   | Integer positif | Default `10`, maksimal `50`     |

Contoh request:

`GET /api/pesanan?page=1&limit=10`

Respons pagination dari fungsi metadata memiliki struktur berikut:

```json
{
  "jumlah": 25,
  "page": 1,
  "limit": 10,
  "totalPages": 3
}
```

Angka pada contoh tersebut hanya ilustrasi format metadata, bukan hasil pengujian aktual.

### 4.2 Membuat Pesanan

**Request**

`POST /api/pesanan`

Endpoint ini digunakan untuk membuat pesanan menggunakan body JSON.

Contoh request:

```json
{
  "emailPembeli": "siti@example.com",
  "items": [
    {
      "penawaranId": "1",
      "kuantitas": 2
    }
  ]
}
```

Ketentuan field:

| Field                 | Tipe data         | Ketentuan                                               |
| --------------------- | ----------------- | ------------------------------------------------------- |
| `emailPembeli`        | String            | Wajib, 3–254 karakter, dan harus mengikuti format email |
| `items`               | Array             | Wajib berisi 1–10 item                                  |
| `items[].penawaranId` | String atau angka | ID penawaran positif yang valid                         |
| `items[].kuantitas`   | Integer           | Wajib bernilai 1–1.000                                  |

Ketentuan tambahan:

- ID penawaran tidak boleh berulang dalam satu pesanan.
- Body tidak boleh memuat field tambahan yang tidak didukung.
- Satu pesanan hanya boleh berisi penawaran dari satu penyedia.
- Pemesanan harus memenuhi aturan status penawaran dan ketersediaan stok.
- Proses pemesanan menggunakan pengelolaan transaksi database sesuai implementasi aplikasi.

## 5. Validasi dan Penanganan Error

API menggunakan validasi request dan middleware untuk menangani kesalahan. Respons error menggunakan format JSON berikut:

```json
{
  "success": false,
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "Pesan kesalahan."
  }
}
```

Format di atas merupakan contoh struktur respons. Nilai `code` dan `message` menyesuaikan jenis kesalahan yang terjadi.

### 5.1 Daftar Status dan Kode Error

| Status HTTP | Kode Error              | Keterangan                                                        |
| ----------- | ----------------------- | ----------------------------------------------------------------- |
| `400`       | `VALIDATION_ERROR`      | Data request tidak memenuhi validasi                              |
| `400`       | `INVALID_JSON`          | Format JSON yang dikirim tidak valid                              |
| `400`       | `DATA_CONSTRAINT`       | Data melanggar aturan database                                    |
| `409`       | `REFERENCE_CONFLICT`    | Relasi tidak valid atau data masih digunakan oleh riwayat pesanan |
| `409`       | `DATA_CONFLICT`         | Data yang harus unik sudah digunakan                              |
| `409`       | `TRANSACTION_BUSY`      | Transaksi belum dapat diselesaikan                                |
| `413`       | `PAYLOAD_TOO_LARGE`     | Ukuran payload melebihi 20 KB                                     |
| `500`       | `INTERNAL_SERVER_ERROR` | Terjadi kesalahan internal pada server                            |
| `503`       | `DATABASE_UNAVAILABLE`  | Database belum tersedia                                           |

### 5.2 Ketentuan Validasi

Beberapa kondisi yang dapat menghasilkan error antara lain:

- Field wajib tidak tersedia atau memiliki tipe data yang tidak sesuai.
- Body request berisi field yang tidak didukung.
- ID tidak memenuhi format yang ditentukan.
- Nilai angka berada di luar rentang yang diperbolehkan.
- Status penawaran tidak dikenal.
- Format tanggal atau waktu tidak valid.
- Format email pembeli tidak valid.
- Daftar item pesanan kosong, terlalu banyak, atau memiliki ID penawaran yang berulang.
- Data melanggar batasan atau relasi pada database.

Klien sebaiknya memeriksa status HTTP dan kode error sebelum menentukan tindakan selanjutnya. Khusus error transaksi, aplikasi menyarankan agar kondisi data diperiksa sebelum request diulang.

## 6. Batasan Penggunaan

API ini digunakan untuk pengembangan dan pengujian lokal. Autentikasi pengguna belum diterapkan sehingga API belum ditujukan untuk penggunaan produksi.
