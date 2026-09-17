# 06 — Analisis Penjualan Global Superstore 📈

Sebagai seorang **analis bisnis**, proyek ini membedah data transaksi *retail* dari dataset Sample Superstore untuk mengidentifikasi pola penjualan, menghitung margin keuntungan, mendeteksi anomali/produk yang merugi, serta menganalisis tren performa bisnis dari waktu ke waktu.

---

## 🎯 Tujuan Pembelajaran
* Membuat **pivot table** untuk meringkas total penjualan (*sales*) dan keuntungan (*profit*).
* Menghitung **margin keuntungan** ($\text{Profit} / \text{Sales} \times 100\%$).
* Mengidentifikasi sub-kategori produk yang mengalami kerugian (*loss*).
* Melakukan *parsing* tanggal pesanan untuk menganalisis **tren bulanan**.
* Memvisualisasikan data profit per kategori, *heatmap* performa wilayah (*region*) vs kategori, dan tren waktu menggunakan `matplotlib` dan `seaborn`.

---

## 📊 Tentang Dataset
Dataset **Sample - Superstore** mencakup sekitar 10.000 data transaksi ritel (furnitur, perlengkapan kantor, dan teknologi) di Amerika Serikat. 
* **Sumber:** [Kaggle — vivek468/superstore-dataset-final](https://www.kaggle.com/datasets/vivek468/superstore-dataset-final)
* **Encoding:** `latin-1`

| Kolom Utama | Deskripsi |
| :--- | :--- |
| `Order Date` | Tanggal pesanan dilakukan |
| `Ship Date` | Tanggal pengiriman pesanan |
| `Segment` | Segmen pelanggan (*Consumer*, *Corporate*, *Home Office*) |
| `Region` | Wilayah pemasaran (*East*, *West*, *Central*, *South*) |
| `Category` | Kategori produk utama |
| `Sub-Category` | Sub-kategori produk |
| `Sales` | Nilai total penjualan |
| `Quantity` | Jumlah unit barang yang dibeli |
| `Discount` | Besar diskon yang diberikan |
| `Profit` | Keuntungan bersih (bisa bernilai negatif jika merugi) |

---

## 🛠️ Tech Stack & Libraries
* **Python** (Bahasa pemrograman utama)
* **Pandas** & **NumPy** (Manipulasi, pembersihan data, dan agregasi *pivot table*)
* **Matplotlib** & **Seaborn** (Visualisasi data dan grafik analitik)

---

## 🔍 Ringkasan Hasil Analisis

1. **Performa Berdasarkan Kategori:**
   * Kategori **Technology** dan **Office Supplies** menyumbang profit tertinggi dengan margin keuntungan yang sehat (di kisaran 17%).
   * Kategori **Furniture** mencatatkan volume penjualan yang tinggi, namun memiliki **profit margin yang sangat tipis** (~2.49%), mengindikasikan adanya inefisiensi biaya atau diskon yang terlalu besar.

2. **Pembersihan & Transformasi Data:**
   * Memastikan tidak ada *missing value* pada dataset utama.
   * Mengubah tipe data kolom tanggal (`Order Date`) menjadi `datetime` dan mengekstrak kolom `Periode` (bulanan) untuk kebutuhan *resampling* tren penjualan serta profit dari tahun 2014 hingga 2017.

---

## 🚀 Cara Menjalankan Notebook

Jika Anda ingin menjalankan ulang notebook ini di Google Colab:
1. Pastikan Anda memiliki akun Kaggle dan file API token (`kaggle.json`) yang siap diunggah.
2. Jalankan sel kode pada notebook untuk mengunduh dataset secara otomatis melalui Kaggle API:
   ```python
   !pip install -q kaggle
   !kaggle datasets download vivek468/superstore-dataset-final
   !unzip -q superstore-dataset-final.zip