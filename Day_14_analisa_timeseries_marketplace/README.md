# 📈 Day 14 — Analisis Time Series Penjualan Marketplace

Project ini merupakan implementasi **Time Series Analysis** menggunakan **Pandas, Matplotlib, dan Seaborn** untuk menganalisis tren penjualan berdasarkan dimensi waktu.

Analisis mencakup **data cleaning, resampling harian & bulanan, moving average, serta analisis pola mingguan** untuk menemukan tren dan insight dari data transaksi e-commerce.

---

## 🎯 Project Objectives

Tujuan utama project ini adalah:

* Mengubah data tanggal menjadi format `datetime`.
* Membersihkan transaksi dengan `Quantity` dan `UnitPrice` tidak valid.
* Membuat metrik `Revenue`.
* Melakukan **resampling** data secara harian dan bulanan.
* Menggunakan **Moving Average** untuk melihat tren penjualan.
* Menganalisis pola penjualan berdasarkan hari dalam seminggu.
* Mengidentifikasi pola musiman dan periode data yang tidak lengkap.
* Menghasilkan insight bisnis dari pola time series.

---

## 📂 Dataset

Dataset yang digunakan adalah:

**Online Retail / E-commerce Data**

📌 Source: [Kaggle — E-commerce Data](https://www.kaggle.com/datasets/carrie1/ecommerce-data)

Dataset berisi sekitar:

* **541,909 transaksi**
* **8 kolom**
* Periode: **Desember 2010 – Desember 2011**
* Setiap baris merepresentasikan item dalam sebuah invoice.

### Important Features

| Feature       | Description               |
| ------------- | ------------------------- |
| `InvoiceNo`   | Nomor faktur              |
| `StockCode`   | Kode produk               |
| `Description` | Nama produk               |
| `Quantity`    | Jumlah unit               |
| `InvoiceDate` | Tanggal & waktu transaksi |
| `UnitPrice`   | Harga satuan dalam GBP    |
| `CustomerID`  | ID pelanggan              |
| `Country`     | Negara pelanggan          |

Metrik baru yang dibuat:

```text
Revenue = Quantity × UnitPrice
```

---

## 🛠️ Tech Stack

* **Python**
* **Pandas** — Data manipulation & time series analysis
* **NumPy** — Numerical computation
* **Matplotlib** — Data visualization
* **Seaborn** — Statistical visualization

---

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
   Time Series Analysis
        │
        ├── Daily Revenue
        ├── Moving Average
        ├── Monthly Revenue
        └── Weekly Pattern
        │
        ▼
   Insights & Business
     Recommendations
```

---

# 🧹 1. Data Cleaning

Beberapa tahap preprocessing dilakukan sebelum analisis time series.

### Convert Date

Kolom `InvoiceDate` yang awalnya berupa teks dikonversi menjadi:

```python
df["InvoiceDate"] = pd.to_datetime(df["InvoiceDate"])
```

### Remove Invalid Transactions

Transaksi dengan:

```text
Quantity ≤ 0
UnitPrice ≤ 0
```

dihapus karena tidak sesuai untuk perhitungan revenue penjualan.

Sebanyak **11,805 baris** dihapus.

### Create Revenue

Kemudian dibuat metrik:

```python
df["Revenue"] = df["Quantity"] * df["UnitPrice"]
```

Revenue ini menjadi metrik utama dalam analisis time series.

---

# 📊 2. Daily Sales Trend

Data transaksi diubah menjadi time series dengan `InvoiceDate` sebagai index.

Kemudian revenue diagregasikan secara harian menggunakan:

```python
daily_sales = time_series["Revenue"].resample("D").sum()
```

### Insight

* Revenue harian memiliki **fluktuasi yang cukup tinggi**.
* Terdapat peningkatan aktivitas penjualan menjelang akhir tahun 2011.
* Beberapa periode menunjukkan revenue sangat rendah atau tidak ada transaksi.

Hal ini menunjukkan bahwa analisis pada level harian memiliki **noise yang cukup tinggi**, sehingga diperlukan smoothing untuk melihat tren yang lebih jelas.

---

# 📈 3. Moving Average

Untuk mengurangi fluktuasi harian, digunakan **Moving Average** dengan window:

* **7 hari** → tren jangka pendek
* **30 hari** → tren jangka lebih panjang

```python
MA_7 = daily_sales.rolling(window=7).mean()
MA_30 = daily_sales.rolling(window=30).mean()
```

### Insight

**7 Days Moving Average**

Lebih responsif terhadap perubahan jangka pendek dan pola mingguan.

**30 Days Moving Average**

Lebih smooth dan membantu melihat arah tren bisnis secara keseluruhan.

Dari visualisasi terlihat bahwa revenue relatif stabil pada awal tahun, kemudian mengalami **pertumbuhan signifikan mulai sekitar September hingga November 2011**.

---

# 📅 4. Monthly Sales Trend

Revenue juga diagregasikan pada level bulanan:

```python
monthly_sales = time_series["Revenue"].resample("M").sum()
```

### Insight

* Penjualan relatif stabil pada paruh pertama periode pengamatan.
* Terjadi peningkatan signifikan mulai **September 2011**.
* Revenue mencapai peak sekitar **November 2011**.
* Terjadi penurunan tajam pada Desember.

### ⚠️ Important Data Insight

Penurunan Desember perlu diinterpretasikan dengan hati-hati karena **data berhenti di pertengahan Desember 2011**.

Artinya, penurunan tersebut belum tentu menunjukkan penurunan performa bisnis.

```text
Observed Drop
     │
     ├── Business decline?
     │
     └── Incomplete data period?
                 ↓
          Check date range
```

Ini merupakan contoh penting bahwa **data completeness harus diperiksa sebelum menarik business conclusion**.

---

# 📆 5. Weekly Sales Pattern

Revenue dikelompokkan berdasarkan nama hari:

```python
df["day"] = df["InvoiceDate"].dt.day_name()
```

Kemudian revenue dibandingkan untuk setiap hari dalam seminggu.

### Insight

Terlihat bahwa aktivitas transaksi lebih banyak terjadi pada **hari kerja**, sedangkan **Sabtu tidak menunjukkan aktivitas transaksi** dalam dataset.

Hal ini dapat menjadi sinyal adanya pola operasional tertentu pada bisnis.

> ⚠️ Interpretasi mengenai alasan tidak adanya transaksi pada Sabtu tidak dapat dipastikan hanya dari dataset. Diperlukan informasi bisnis tambahan untuk mengetahui apakah penyebabnya adalah kebijakan operasional, hari libur, atau faktor lainnya.

---

# 💡 Key Insights

### 1. Daily Revenue is Highly Volatile

Revenue harian memiliki fluktuasi tinggi sehingga diperlukan smoothing untuk melihat tren utama.

### 2. Moving Average Reveals the Trend

Moving Average 7 dan 30 hari membantu memisahkan **noise harian** dari **underlying trend**.

### 3. Strong Q4 Growth

Terjadi peningkatan revenue yang signifikan pada sekitar **September–November 2011**, dengan November sebagai periode peak.

### 4. Weekly Pattern

Aktivitas transaksi lebih tinggi pada hari kerja dan tidak terdapat transaksi pada Sabtu dalam dataset.

### 5. Data Completeness Matters

Penurunan revenue pada Desember tidak boleh langsung dianggap sebagai business decline karena periode data tidak lengkap.

---

# 💼 Business Insight

Pola time series dapat digunakan untuk mendukung keputusan operasional seperti:

```text
Historical Sales
      ↓
Identify Pattern
      ↓
Peak Period Detection
      ↓
Planning
      ├── Inventory
      ├── Staffing
      └── Marketing Campaign
```

Dengan mengetahui periode ramai, bisnis dapat menyesuaikan **stok, tenaga kerja, dan campaign** berdasarkan demand aktual daripada hanya menggunakan rata-rata keseluruhan.

---

# ⚠️ Data & Analysis Pitfalls

### Negative Quantity

`Quantity` dapat bernilai negatif karena adanya **retur atau pembatalan**.

Jika tidak ditangani, nilai revenue dapat menjadi tidak sesuai untuk analisis penjualan.

### Incomplete Period

Desember 2011 merupakan periode yang tidak lengkap sehingga perbandingan dengan bulan penuh lainnya dapat menyesatkan.

### Correlation vs Explanation

Pola yang terlihat pada time series menunjukkan **apa yang terjadi**, tetapi tidak selalu menjelaskan **mengapa hal tersebut terjadi**.

Business context tetap diperlukan untuk mencari penyebab di balik pola tersebut.

---

# 📚 Learning Outcomes

Melalui project ini, beberapa konsep yang dipraktikkan:

* Time Series Analysis
* `pd.to_datetime()`
* Time Series Index
* Data Cleaning
* Revenue Calculation
* Resampling
* Daily Aggregation
* Monthly Aggregation
* Moving Average
* Rolling Window
* Trend Analysis
* Seasonality Awareness
* Weekly Pattern Analysis
* Business Insight Generation
* Data Completeness Awareness

---

# 🚀 Next Step

Time Series Analysis menjadi fondasi sebelum masuk ke tahap **forecasting**.

```text
Time Series Data
       ↓
Trend Analysis
       ↓
Seasonality
       ↓
Moving Average
       ↓
Forecasting
       ↓
Demand Prediction
       ↓
Business Decision
```

Pemahaman terhadap pola historis menjadi dasar untuk membangun sistem **forecasting dan predictive analytics** pada tahap selanjutnya.

---

## 📁 Project Structure

```text
Day_14_analisa_timeseries_marketplace/
│
├── Day_14_analisa_timeseries_marketplace.ipynb
└── README.md
```

---

## 👤 Author

**Arief Wicaksono**

Aspiring AI Engineer | Data Science Student

---

⭐ If you find this project useful, feel free to star the repository!
