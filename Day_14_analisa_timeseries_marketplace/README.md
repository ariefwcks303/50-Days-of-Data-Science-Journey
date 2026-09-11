# 📈 Analisis Tren Waktu (Time Series Penjualan E-commerce)

Analisis mendalam mengenai tren penjualan harian dan bulanan menggunakan data transaksi e-commerce nyata (*Online Retail*). Proyek ini berfokus pada teknik pengolahan data berbasis waktu (*time series*), pembersihan data anomali, perhitungan *moving average* untuk memuluskan tren, serta identifikasi pola waktu tersibuk dalam aktivitas transaksi bisnis.

---

## 🎯 Tujuan Pembelajaran
* Mengubah kolom tanggal teks mentah menjadi format `datetime` standar menggunakan `pd.to_datetime`.
* Membersihkan data anomali dari transaksi tidak valid (seperti `Quantity` $\le 0$ atau `UnitPrice` $\le 0$).
* Melakukan **resampling** data transaksi ke tingkat harian (`Daily`) dan bulanan (`Monthly`).
* Menghitung dan memvisualisasikan **Moving Average** untuk menganalisis tren jangka panjang secara lebih mulus.
* Mengidentifikasi **hari dan bulan tersibuk** berdasarkan volume penjualan (*Revenue*).

---

## 📊 Tentang Dataset
Dataset yang digunakan adalah **E-Commerce Data ("Online Retail")** yang memuat sekitar 541 ribu catatan transaksi dari sebuah peritel online di Inggris terhitung sejak Desember 2010 hingga Desember 2011.

* **Sumber Dataset:** [Kaggle — carrie1/ecommerce-data](https://www.kaggle.com/datasets/carrie1/ecommerce-data)
* **Ukuran Data Awal:** $\pm 541.909$ baris $	imes$ 8 kolom (dimuat dengan `encoding="latin-1"`).

| Kolom | Tipe Data | Keterangan / Arti |
| :--- | :--- | :--- |
| `InvoiceNo` | Object | Nomor faktur unik (diawali huruf 'C' menandakan pembatalan/retur) |
| `StockCode` | Object | Kode unik produk |
| `Description`| Object | Nama/deskripsi produk |
| `Quantity` | Integer | Jumlah unit barang per transaksi (bisa bernilai negatif menandakan retur) |
| `InvoiceDate`| Datetime | Tanggal dan waktu faktur/transaksi dibuat |
| `UnitPrice` | Float | Harga satuan barang dalam mata uang GBP |
| `CustomerID`| Float | Nomor ID unik pelanggan |
| `Country` | Object | Nama negara tempat pelanggan berasal |

> **Catatan Analisis:** Kolom metrik penunjang baru ditambahkan yaitu **`Revenue` = Quantity $	imes$ UnitPrice** untuk mengukur total pendapatan kotor.

---

## 🛠️ Tech Stack & Libraries
Proyek ini dikembangkan menggunakan bahasa pemrograman Python dengan pustaka analisis data standar industri:
* **Pandas & NumPy:** Manipulasi struktur data tabular, agregasi, dan penanganan deret waktu (*time series*).
* **Matplotlib & Seaborn:** Visualisasi grafik tren penjualan secara visual.

---

## 🔍 Alur Pengerjaan (Workflow)

### 1. Eksplorasi & Pemahaman Data Awal
* Pengecekan struktur DataFrame, tipe data, serta nilai yang hilang (*missing values*) pada kolom `CustomerID` dan `Description`.
* Ringkasan statistik deskriptif menunjukkan adanya nilai minimum negatif pada `Quantity` dan `UnitPrice` yang perlu ditangani.

### 2. Pembersihan Data (*Data Cleaning*)
* **Konversi Tanggal:** Mengubah kolom `InvoiceDate` dari tipe teks (*object*) menjadi format `datetime64[ns]`.
* **Filter Transaksi Valid:** Membuang baris data anomali di mana `Quantity` $\le 0$ atau `UnitPrice` $\le 0$ (berhasil memfilter sebanyak **11.805 baris** data tidak valid).
* **Feature Engineering:** Membuat kolom baru `Revenue` dari perkalian `Quantity` dan `UnitPrice` untuk memudahkan analisis agregasi pendapatan.

### 3. Analisis Tren Penjualan Harian & Bulanan
* Mengatur kolom `InvoiceDate` sebagai indeks waktu (*time index*) agar proses pengurutan dan *resample* data berjalan optimal.
* Melakukan agregasi dengan fungsi `.resample("D").sum()` untuk melihat fluktuasi harian (*Daily Sales*).
* Memvisualisasikan pergerakan tren menggunakan grafik garis (*line plot*) untuk mengidentifikasi lonjakan transaksi dan tren musiman bisnis.

---

## 🚀 Cara Menjalankan Notebook
1. Pastikan pustaka yang dibutuhkan sudah terinstal:
   ```bash
   pip install pandas numpy matplotlib seaborn kaggle
   ```
2. Unduh dataset melalui Kaggle API (pastikan file `kaggle.json` sudah terkonfigurasi di lingkungan Anda):
   ```bash
   kaggle datasets download -d carrie1/ecommerce-data
   unzip ecommerce-data.zip
   ```
3. Jalankan sel-sel kode secara berurutan di dalam Jupyter Notebook atau Google Colab.