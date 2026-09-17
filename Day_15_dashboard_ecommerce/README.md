# 📈 Day 15 — Dashboard E-Commerce (KPI)

Project ini merupakan implementasi pengolahan data E-Commerce nyata untuk menghitung KPI penjualan dan menyusun visualisasinya dalam satu canvas menyerupai Dashboard. 
Analisis mencakup pembersihan data, pembuatan metrik baru, penghitungan metrik bisnis utama, serta pembuatan komponen visualisasi seperti tren bulanan dan distribusi nilai pesanan.

## 🎯 Project Objectives
Tujuan utama project ini adalah:
- Menghitung KPI Penjualan: Total Revenue, Jumlah Order Unik, AOV (Average Order Value).
- Menemukan Top Negara dan Top Product dari data transaksi.
- Menyajikan tabel ringkasan KPI eksekutif secara terstruktur.
- Menyusun beberapa grafik dalam 1 figur (subplot) sebagai Dashboard pelaporan.

## 📂 Dataset
Dataset yang digunakan adalah:
**Online Retail / E-commerce Data**
- 📌 Source: Kaggle — carrie1/ecommerce-data
- Berisi transaksi peritel Online Inggris periode 2010–2011.
- Memiliki 541,909 baris data awal dan 8 kolom.
- Tiap baris mewakili 1 item dalam sebuah faktur.

### Important Features
| Kolom | Arti |
|-------|------|
| `InvoiceNo` | Nomor faktur (diawali 'C' = pembatalan) |
| `StockCode` | Kode produk |
| `Description` | Nama produk |
| `Quantity` | Jumlah unit (bisa negatif = retur) |
| `InvoiceDate` | Tanggal & waktu transaksi |
| `UnitPrice` | Harga satuan (GBP) |
| `CustomerID` | ID pelanggan |
| `Country` | Negara pelanggan |

Metrik baru yang dibuat:
- **Revenue** = Quantity × UnitPrice

## 🛠️ Tech Stack
- **Python**
- **Pandas** — Data manipulation
- **NumPy** — Numerical computation
- **Matplotlib** — Data visualization & dashboard layout
- **Seaborn** — Statistical visualization

## 🔎 Analysis Workflow
```text
Online Retail Dataset
        │
        ▼
   Data Exploration
        │
        ▼
    Data Cleaning
        │
        ├── Convert InvoiceDate → datetime
        ├── Remove Quantity ≤ 0
        └── Remove UnitPrice ≤ 0
        │
        ▼
 Create Revenue Metric
        │
        ▼
   Calculate Main KPI
        │
        ├── Total Revenue & AOV
        ├── Unique Orders & Customers
        └── Top Countries
        │
        ▼
   Dashboard Construction
        (Matplotlib Subplots)
```

## 🧹 1. Data Cleaning
Sebelum melakukan penghitungan agregasi, data yang tidak valid dibersihkan karena analisis KPI membutuhkan data transaksi yang akurat.

**Convert Date**
Kolom `InvoiceDate` yang awalnya berupa teks dikonversi menjadi tipe datetime.

**Remove Invalid Transactions**
Transaksi dengan `Quantity` ≤ 0 dan `UnitPrice` ≤ 0 dibuang. Sebanyak 11,805 baris dihapus, menyisakan 530,104 baris transaksi yang valid.

**Create Revenue**
Metrik `revenue` dibuat dari perkalian `Quantity` dan `UnitPrice`.

## 📊 2. Menghitung KPI Utama
Tabel ringkasan eksekutif dibuat untuk menyajikan hasil KPI:
- **Total Revenue**: ~£ 10,666,685
- **Jumlah Order**: 19,960
- **AOV (Average Order Value)**: £ 534.40 (Metrik penting untuk *upselling*)
- **Jumlah Pelanggan**: 4,338 dari 38 negara berbeda.

## 📈 3. Komponen Dashboard & Visualisasi
Empat panel visualisasi digabungkan menggunakan fungsi `subplot` berukuran 2x2:
1. **Tren Revenue Bulanan**: Menggambarkan pergerakan agregasi revenue setiap bulan.
2. **Top Negara (Non-UK)**: Menampilkan penyumbang revenue terbesar selain UK (karena UK sangat mendominasi dataset). Belanda (Netherlands) dan EIRE memimpin pasar Non-UK.
3. **Top Produk / Top Country (Overall)**: Melihat perbandingan pangsa pasar tertinggi.
4. **Distribusi Nilai Order (<P95)**: Analisis histogram untuk persebaran nilai per faktur dan membandingkannya dengan garis batas rata-rata pesanan (AOV).

## 💡 Key Insights & Business Recommendations
1. **Dominasi Pasar**: Pasar E-commerce ini sangat didominasi oleh *United Kingdom* dengan margin yang jauh melebihi negara lainnya, sehingga analisis tambahan untuk melihat performa negara di luar UK (*Top Non-UK*) sangat diperlukan.
2. **Pentingnya AOV**: Dengan mengetahui *Average Order Value* berada di angka £534.40, tim pemasaran dapat membuat strategi *bundling* atau *upselling* dengan ambang batas diskon di atas £550 untuk mendorong nilai keranjang belanja.
3. **Visualisasi Terpadu**: Menggabungkan metrik seperti tren bulanan, negara dengan performa terbaik, dan persebaran pesanan dalam satu layout memberikan konteks gambaran besar yang sangat krusial bagi pemangku kepentingan (*stakeholders*).

## 📚 Learning Outcomes
Melalui project ini, konsep-konsep berikut telah dipraktikkan:
- Menghitung **Business Metrics (KPI)** dari data agregasi E-Commerce.
- Memformat dan menyajikan tabel data pelaporan yang ringkas dengan `df.style`.
- Menggunakan `plt.subplots` untuk menggabungkan beberapa grafik berbeda dalam 1 figur / *canvas*.
- Identifikasi dan penanganan data anomali / retur (kuantitas negatif).

## 📁 Project Structure
```text
Day_15_dashboard_ecommerce_kpi/
│
├── Day_15_Create_Dashboard_E_commerce.ipynb
└── README.md
```

👤 **Author**

Arief Wicaskono

Aspiring AI Engineer | Data Science Student

⭐ *If you find this project useful, feel free to star the repository!*