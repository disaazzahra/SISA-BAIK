# Pengukuran Performa SisaBaik

Tanggal: 17 November 2026
Aplikasi: https://sisa-baik.onrender.com
Browser: Mozilla Firefox
Alat: Network Monitor, Analyze Performance
Lingkungan: Deployment cloud Render

## 1. Hasil Empty Cache

| Jenis | Jumlah Request | Size | Transferred | Time |
|---|---:|---:|---:|---:|
| Fonts | 5 | 210.86 KB | 210.86 KB | 0 s |
| JavaScript | 3 | 62.17 KB | 368.30 KB | 0 s |
| CSS | 1 | 33.50 KB | 8.12 KB | 0 s |
| Images | 4 | 7.47 KB | 0.29 KB | 0 s |
| HTML | 1 | 0.88 KB | 1.56 KB | 0.08 s |
| XHR | 1 | 0.52 KB | 1.43 KB | 0.98 s |

Total requests: 15
Total size: 315.41 KB
Transferred size: 590.57 KB
Total time: 1.06 seconds
Non-blocking time: 1.06 seconds
Cached responses: 0

## 2. Hasil Primed Cache

| Jenis | Jumlah Request | Size | Transferred | Time |
|---|---:|---:|---:|---:|
| HTML | 1 | 258.54 KB | 258.83 KB | 0.68 s |
| Fonts | 1 | 91.39 KB | 0 KB | 0 s |
| Images | 8 | 13.09 KB | 0.08 KB | 0 s |
| XHR | 5 | 0 KB | 5.14 KB | 2.61 s |

Total requests: 15
Total size: 363.01 KB
Transferred size: 264.05 KB
Total time: 3.29 seconds
Non-blocking time: 3.29 seconds
Cached responses: 0

## 3. Analisis Hasil

Pengujian empty cache mencatat waktu total 1.06 detik, sedangkan primed cache mencatat 3.29 detik.

Permintaan XHR menjadi bagian dengan waktu terbesar pada kedua kondisi, yaitu 0.98 detik pada empty cache dan 2.61 detik pada primed cache.

Data transferred pada empty cache sebesar 590.57 KB, sedangkan pada primed cache sebesar 264.05 KB.

Ukuran HTML yang tercatat juga berbeda cukup besar antara kedua pengujian. Oleh karena itu, hasil ini belum dapat digunakan untuk menyimpulkan bahwa penggunaan cache selalu mempercepat pemuatan aplikasi.

## 4. Batas Pengukuran

Pengukuran ini dilakukan pada aplikasi cloud setelah deployment.

Data performa sebelum perubahan aplikasi tidak tersedia. Karena itu, perbandingan performa sebelum dan sesudah perubahan belum dapat disimpulkan.

Hasil ini merupakan pengukuran kondisi cloud yang tercatat pada saat pengujian, bukan pengujian beban atau stress test.
