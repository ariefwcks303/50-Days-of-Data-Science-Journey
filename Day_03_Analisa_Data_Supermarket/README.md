# Day 03: Analisis Penjualan Ritel Supermarket — Data Parsing & Eksplorasi Skala Besar

Selamat datang di hari ke-3 perjalanan **50-Days-of-Data-Science-Journey** saya! Setelah kemarin belajar membersihkan data kotor di dataset Titanic, hari ini saya naik tingkat ke skenario dunia nyata yang lebih menantang: **menangani dataset transaksi skala besar (878.000+ baris) menggunakan strategi *sampling* dan manipulasi data deret waktu (*time series*)**.

Dalam proyek ini, saya berperan sebagai analis data ritel untuk membedah catatan transaksi penjualan sayuran sebuah supermarket guna mengungkap tren harian, produk terlaris, serta pola pendapatan.

## 🎯 Mengapa Proyek Ini Penting?
Dalam operasional bisnis ritel modern, data transaksi masuk setiap detik dalam jumlah masif. Tantangan utamanya adalah:
* **Skalabilitas Data:** File mentah berukuran besar (>878 ribu baris) dapat membuat proses komputasi lokal melambat jika tidak ditangani dengan strategi yang tepat (seperti pengambilan sampel acak yang representatif)[cite: 1].
* **Transformasi Tipe Data:** Kolom tanggal dan waktu yang awalnya terbaca sebagai teks (*object*) harus dikonversi ke format `datetime` agar bisa dianalisis berdasarkan rentang waktu[cite: 1].
* **Feature Engineering Bisnis:** Menghitung metrik esensial seperti nilai total penjualan aktual (`Sales = Quantity × Unit Price`) dari data transaksi mentah[cite: 1].

## 🛠 Apa yang Saya Pelajari?
1. **Otomasi Unduh Dataset:** Mengintegrasikan API Kaggle langsung di lingkungan Google Colab menggunakan `kaggle.json`.
2. **Manajemen Memori & Sampling:** Mengambil sampel acak terkontrol dari ratusan ribu baris data agar proses eksplorasi dan visualisasi tetap efisien dan cepat[cite: 1].
3. **Data Parsing & Cleaning:** Mengonversi kolom teks tanggal menjadi tipe data `datetime` menggunakan `pd.to_datetime`[cite: 1].
4. **Agregasi Data Lanjutan:** Menggunakan fungsi `groupby` dan operasi matematis dasar untuk merangkum performa penjualan harian[cite: 1].
5. **Visualisasi Tren Bisnis:** Membangun *line chart* harian menggunakan Seaborn dan Matplotlib untuk memonitor fluktuasi pendapatan ritel[cite: 1].

## 📊 Insight & Temuan
* **Efisiensi Analisis:** Dengan mengambil sampel representatif sebanyak 50.000 transaksi dari total 878.000+ baris data, analisis tren dapat dilakukan secara responsif tanpa mengorbankan akurasi pola makro.
* **Fluktuasi Pendapatan Harian:** Visualisasi *line chart* memperlihatkan adanya naik-turun (volatilitas) omzet harian yang dapat menjadi acuan bagi manajemen stok barang di masa mendatang.
* **Penyaringan Data Bisnis:** Memisahkan data transaksi sukses (`sale`) dari status pengembalian barang (`return`) memastikan angka pendapatan bersih yang dihitung benar-benar akurat.

## 🚀 Cara Menjalankan
1. Pastikan Anda memiliki file kredensial Kaggle (`kaggle.json`).
2. Jalankan seluruh tahapan analisis di Google Colab melalui file: `Day_3_Analisis_Penjualan_Supermarket.ipynb`.

---
*Proyek ini adalah bagian dari rangkaian 50 hari perjalanan belajar Data Science saya. Mari terhubung di [LinkedIn Anda].*