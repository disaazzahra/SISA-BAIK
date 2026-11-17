# Refleksi Proyek SisaBaik

## 1. Nilai Bisnis

SisaBaik menyediakan sarana digital bagi penyedia untuk menawarkan makanan berlebih yang masih layak dikonsumsi. Pembeli dapat melihat penawaran dan memesan makanan dengan harga lebih rendah dari harga normal.

Alur digital membantu penyedia mengelola penawaran, stok, pesanan, dan pencatatan pengambilan. Pembeli juga dapat melihat informasi penawaran serta memeriksa status pesanannya.

## 2. Bukti Pengambilan dan Pembayaran

Dalam pengujian cloud, pesanan berhasil melalui tahap konfirmasi penyedia dan siap diambil. Sistem menghasilkan kode pengambilan SBX-1.

Penyelesaian serah terima dan pencatatan pembayaran dilaporkan telah dilakukan melalui akun penyedia. Aplikasi mencatat status pengambilan dan pembayaran pada alur transaksi.

Pengujian ini membuktikan alur pencatatan di aplikasi. Pengujian belum mencakup verifikasi melalui payment gateway atau bukti pembayaran eksternal.

## 3. Cakupan Akun Pengujian

Pengujian cloud menggunakan satu akun penyedia demo dan satu akun pembeli demo.

Pengujian tersebut mencakup pendaftaran, verifikasi penyedia, pembuatan penawaran, pemesanan, konfirmasi, serta pengambilan. Hasilnya belum mewakili pengujian dengan banyak penyedia dan pembeli secara luas.

## 4. Penghematan Pembeli

Simulasi menggunakan dua paket makanan dengan rincian:

- Harga normal per paket: Rp20.000.
- Harga penawaran per paket: Rp7.000.
- Jumlah pesanan: 2 paket.
- Total harga normal: Rp40.000.
- Total pembayaran yang tercatat: Rp14.000.
- Selisih harga: Rp26.000.

Dengan demikian, simulasi menunjukkan penghematan pembeli sebesar Rp26.000 atau 65% dibandingkan nilai normal kedua paket.

Angka ini merupakan hasil simulasi berdasarkan harga penawaran, bukan pengukuran penghematan seluruh pengguna.

## 5. Keterbatasan Perhitungan Biaya Penjual

Aplikasi belum menghitung penghematan biaya penjual. Metrik penghematan biaya penjual masih bernilai null.

Aplikasi belum memperhitungkan biaya produksi, penyimpanan, tenaga kerja, distribusi, dan biaya operasional lain. Karena itu, nilai transaksi tidak dapat langsung dianggap sebagai keuntungan bersih penjual.

## 6. Simulasi Dampak

Pada simulasi cloud, dua paket makanan berhasil dipesan dari stok awal tiga paket. Setelah pemesanan, stok tersisa satu paket.

Pengujian ini menunjukkan bahwa aplikasi dapat mencatat penawaran, reservasi stok, dan penyelesaian pesanan. Namun, simulasi ini belum membuktikan dampak pengurangan limbah makanan dalam skala nyata karena berat makanan dan jumlah limbah yang berhasil dicegah tidak diukur.

## 7. Batas Layanan Gratis

Aplikasi menggunakan Render dan Neon pada layanan gratis untuk kebutuhan demonstrasi dan pengujian.

Layanan gratis memiliki batas kapasitas, sumber daya, dan ketersediaan sesuai ketentuan masing-masing penyedia. Oleh sebab itu, konfigurasi ini belum dapat dianggap sebagai jaminan kapasitas untuk penggunaan produksi dengan banyak pengguna.

## 8. Kendala dan Perbaikan

Salah satu kendala saat deployment adalah pemeriksaan TLS pada koneksi Neon. Pemeriksaan awal menggunakan pg_stat_ssl menghasilkan status yang tidak sesuai dengan koneksi TLS dari Node.js.

Pemeriksaan kemudian disesuaikan agar memeriksa status enkripsi dan validitas sertifikat pada socket koneksi Node.js. Setelah perubahan, inisialisasi database cloud dan verifikasi penyedia berhasil dijalankan.

## 9. Keterbatasan Pengujian

Pengujian otomatis deployment dan alur transaksi berhasil dilakukan. Pengujian cloud juga mencakup satu alur transaksi antara akun demo penyedia dan pembeli.

Pengujian manual F01-F20 dan S01-S11 belum dilakukan. Audit Firefox menemukan masalah kontras warna yang belum diperbaiki. Pengukuran performa sebelum perubahan belum tersedia, sehingga perbandingan sebelum dan sesudah belum dapat disimpulkan.

## 10. Pengembangan Selanjutnya

Pengembangan berikutnya perlu mencakup perbaikan kontras warna, pengujian manual yang belum dilakukan, audit aksesibilitas lanjutan, pengukuran performa sebelum dan sesudah perubahan, serta evaluasi kapasitas layanan apabila aplikasi digunakan pada skala lebih besar.
