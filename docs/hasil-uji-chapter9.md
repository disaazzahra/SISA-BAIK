# Laporan Hasil Uji Chapter 9 — Komponen Vue SisaBaik

## 1. Informasi Pengujian

- **Penguji**: Developer SisaBaik
- **Versi Node.js / npm**: Node.js v20.x / npm v10.x
- **Commit**: `feat: build SisaBaik Vue components`

---

## 2. Hasil Pengujian Skenario (Langkah 12)

| Skenario                                                                        | Ekspektasi                                          | Hasil Aktual                                                    |
| :------------------------------------------------------------------------------ | :-------------------------------------------------- | :-------------------------------------------------------------- |
| Halaman baru, mode pembeli                                                      | 3 kartu; 15 unit; 2 penyedia                        | Sesuai (3 kartu, 15 unit, 2 penyedia)                           |
| Pencarian `LAPAU`                                                               | Dua kartu penawaran Lapau Uni Rina                  | Sesuai (Pencarian nama dan penyedia _case-insensitive_)         |
| Pencarian `tidak-ada-produk`                                                    | Daftar kosong dan pesan yang sesuai                 | Sesuai (Kondisi `v-if/v-else` mengikuti _computed_ katalog)     |
| Reset filter lalu pilih kategori roti                                           | Satu kartu Paket Roti Sore                          | Sesuai (Event anak memperbarui filter induk)                    |
| Pilih Paket Roti Sore                                                           | Ringkasan mendapat fokus; stok tetap 6              | Sesuai (Event pilih tidak membuat pesanan)                      |
| Beralih ke mode penyedia                                                        | 3 kartu Rina; 2 tersedia; 9 unit                    | Sesuai (_Draft_ tetap tampil; perhitungan hanya dari data Rina) |
| Submit form kosong                                                              | Kesalahan mendapat fokus; daftar tetap 3            | Sesuai (_Payload_ tidak di-_emit_ ketika _invalid_)             |
| Isi harga normal 20000 dan penawaran 30000                                      | Kesalahan harga, daftar tidak bertambah             | Sesuai (Validasi hubungan dua _field_ berjalan)                 |
| Isi nama Paket Gratis Latihan, harga 20000/0, stok 2, status aktif, waktu besok | Data Rina bertambah; isian kosong; angka 4 / 3 / 11 | Sesuai (Nol sah; form _emit_, induk menambah _state_)           |
| Kembali ke pembeli dan reset filter                                             | 4 kartu; 17 unit; 2 penyedia                        | Sesuai (_Computed_ memakai _array_ terbaru)                     |
| Beralih lagi ke penyedia                                                        | Form kosong, data baru masih ada                    | Sesuai (_State_ form lokal berbeda dari _state_ daftar induk)   |
| Refresh browser                                                                 | 3 kartu; 15 unit; 2 penyedia                        | Sesuai (Memori halaman bukan penyimpanan PostgreSQL)            |
| Batas waktu: fixture sementara                                                  | Setelah batas 15 detik: 2 kartu, 11 unit            | Sesuai (Timer berjalan setiap detik memperbarui tampilan)       |

---

## 3. Aliran Props dan Emits

- **Katalog / Daftar Penawaran (`Aplikasi.vue` -> `Katalog.vue` -> `KartuPenawaran.vue`)**:
  - `props`: `daftarPenawaran`, `filterKategori`, `kataKunci`.
  - `emits`: `pilih-penawaran`, `perbarui-filter`.
- **Form Penawaran (`FormPenawaran.vue` -> `Aplikasi.vue`)**:
  - `props`: `kategoriPilihan`.
  - `emits`: `tambah-penawaran`.

---

## 4. Perhitungan Ringkasan Pembeli dan Penyedia

- **Ringkasan Pembeli**:
  - _Total Penawaran Aktif_: Menghitung kartu dengan `stok > 0` dan `sisaWaktu > 0`.
  - _Total Unit_: Penjumlahan seluruh unit dari penawaran aktif (`reduce`).
  - _Penyedia Aktif_: Jumlah penyedia unik (`Set`) yang memiliki penawaran aktif.
- **Ringkasan Penyedia**:
  - _Total Penawaran_: Jumlah seluruh penawaran milik penyedia login.
  - _Tersedia_: Penawaran milik penyedia dengan status `aktif` dan `stok > 0`.
  - _Total Unit_: Jumlah stok unit milik penyedia login.

---

## 5. Bukti Tampilan dan Screenshot

1. **Katalog Pembeli**: Menampilkan 3 kartu awal, total unit 15, dan 2 penyedia.
2. **Empty State**: Tampilan pencarian `tidak-ada-produk` dengan pesan daftar kosong.
3. **Form Invalid**: Pesan error validasi saat submit form kosong atau harga penawaran > harga normal.
4. **Form Valid**: Form berhasil menambahkan "Paket Gratis Latihan" dengan harga penawaran 0.
5. **Tampilan Mobile**: Uji responsivitas pada lebar layar 375px tanpa _horizontal scroll_.
6. **Terminal Build**: Hasil eksekusi `npm run build` dan `npm run preview` berjalan sukses pada port 4173.
