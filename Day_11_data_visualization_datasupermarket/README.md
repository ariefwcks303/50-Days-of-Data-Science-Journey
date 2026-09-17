# 📊 Day 11 — Dasar Visualisasi dengan Matplotlib

Notebook ini membahas dasar-dasar **data visualization menggunakan Matplotlib** dengan menggunakan dataset **Global Superstore**.

Fokus utama project adalah memahami bagaimana mengubah data mentah menjadi visualisasi yang dapat membantu menemukan **tren, distribusi, perbandingan, dan pola bisnis**.

Visualisasi yang dibuat meliputi **line chart, bar chart, histogram, pie chart, dan subplot**, sekaligus mempraktikkan penggunaan judul, label, legenda, warna, dan anotasi.

---

## 🎯 Project Objectives

Tujuan pembelajaran dalam project ini adalah:

* Memahami dasar visualisasi menggunakan Matplotlib.
* Membuat **line chart** untuk melihat tren penjualan.
* Membuat **bar chart** untuk membandingkan penjualan antar region.
* Membuat **histogram** untuk melihat distribusi profit.
* Membuat **pie chart** untuk melihat komposisi penjualan berdasarkan segment.
* Menggabungkan beberapa visualisasi menggunakan **subplot**.
* Menggunakan judul, label sumbu, legenda, dan anotasi.
* Menghasilkan insight bisnis dari visualisasi data.

---

## 📂 Dataset

Dataset yang digunakan adalah:

**Global Superstore**

📌 Source: [Kaggle — Superstore Dataset](https://www.kaggle.com/datasets/vivek468/superstore-dataset-final)

Dataset berisi transaksi retail yang mencakup informasi mengenai penjualan, profit, pelanggan, produk, dan wilayah.

### Dataset Overview

* **9.994 baris**
* **21 kolom**
* Tidak terdapat missing value pada dataset.

### Important Features

| Feature      | Description         |
| ------------ | ------------------- |
| `Order Date` | Tanggal pesanan     |
| `Segment`    | Segmen pelanggan    |
| `Region`     | Wilayah penjualan   |
| `Category`   | Kategori produk     |
| `Sales`      | Nilai penjualan     |
| `Profit`     | Keuntungan          |
| `Quantity`   | Jumlah unit terjual |

---

## 🛠️ Tech Stack

Project ini menggunakan:

* **Python**
* **Pandas** — Data manipulation & aggregation
* **NumPy** — Numerical computation
* **Matplotlib** — Data visualization
* **Seaborn** — Plot styling

Fokus utama visualisasi pada notebook ini adalah **Matplotlib**.

---

# 🔎 Analysis Workflow

```text
Dataset
   │
   ▼
Data Loading
   │
   ▼
Data Understanding
   │
   ▼
Data Preparation
   │
   ├── Convert Order Date
   └── Create Monthly Period
   │
   ▼
Data Aggregation
   │
   ▼
Data Visualization
   │
   ├── Line Chart
   ├── Bar Chart
   ├── Histogram
   ├── Pie Chart
   └── Subplot
   │
   ▼
Business Insights
```

---

# 🧹 1. Data Preparation

Kolom `Order Date` pada awalnya masih bertipe object/string sehingga perlu dikonversi menjadi datetime.

```python
df["Order Date"] = pd.to_datetime(
    df["Order Date"],
    format="%m/%d/%Y",
    errors="coerce"
)
```

Kemudian dibuat kolom baru:

```text
Month
```

Kolom tersebut digunakan untuk melakukan agregasi data secara bulanan.

```python
df["Month"] = (
    df["Order Date"]
    .dt.to_period("M")
    .dt.to_timestamp()
)
```

Tahap ini penting agar data berbasis waktu dapat dianalisis dengan benar.

---

# 📈 2. Line Chart — Tren Penjualan Bulanan

Line chart digunakan untuk melihat perubahan **total penjualan dari bulan ke bulan**.

Data diagregasikan menggunakan:

```python
monthly_sales = df.groupby("Month")["Sales"].sum()
```

### Insight

* Penjualan menunjukkan pola peningkatan pada periode akhir tahun.
* Terdapat pola **seasonality** dengan lonjakan penjualan menjelang akhir tahun.
* Anotasi digunakan untuk menandai bulan dengan penjualan tertinggi.

### Business Insight

Pola musiman dapat menjadi pertimbangan dalam:

* Perencanaan inventory.
* Persiapan stok menjelang peak season.
* Perencanaan campaign marketing.
* Forecasting kebutuhan produk.

---

# 📊 3. Bar Chart — Total Penjualan per Region

Bar chart digunakan untuk membandingkan total penjualan antar wilayah.

Region yang dianalisis:

* West
* East
* Central
* South

### Insight

Region dengan penjualan tertinggi adalah:

> **West**

Kemudian diikuti oleh:

> **East → Central → South**

Region South memiliki kontribusi penjualan paling rendah dibandingkan region lainnya.

### Business Insight

Performa region dapat digunakan sebagai dasar untuk melakukan analisis lebih lanjut terhadap:

* Customer behavior.
* Demographic characteristics.
* Purchasing power.
* Product preference.
* Marketing effectiveness.

Strategi marketing sebaiknya tidak hanya berfokus pada pemberian diskon, tetapi terlebih dahulu memahami penyebab rendahnya performa suatu region.

---

# 📉 4. Histogram — Distribusi Profit

Histogram digunakan untuk melihat distribusi nilai `Profit`.

Grafik menggunakan:

* 50 bins.
* Range profit -600 hingga 600.
* Garis vertikal pada `Profit = 0` sebagai **breakeven point**.

### Insight

* Mayoritas transaksi memiliki profit yang relatif kecil dan berada di sekitar titik 0.
* Terdapat transaksi dengan profit negatif.
* Profit negatif menunjukkan adanya transaksi yang menghasilkan kerugian.

Kerugian tersebut dapat menjadi indikasi yang perlu ditelusuri lebih lanjut, misalnya berdasarkan:

* Discount.
* Promotion.
* Product.
* Region.
* Customer segment.

---

# 🥧 5. Pie Chart — Komposisi Penjualan per Segment

Pie chart digunakan untuk melihat kontribusi penjualan berdasarkan customer segment:

* Consumer
* Corporate
* Home Office

### Distribution

| Segment     | Sales Contribution |
| ----------- | -----------------: |
| Consumer    |              50.6% |
| Corporate   |              30.7% |
| Home Office |              18.7% |

### Insight

**Consumer** merupakan segment dengan kontribusi penjualan terbesar, yaitu sekitar **50.6%** dari total penjualan.

Kemudian:

* Corporate → **30.7%**
* Home Office → **18.7%**

### Business Insight

Karena Consumer merupakan contributor terbesar terhadap sales, segment ini menjadi salah satu segment penting yang perlu dipertahankan melalui strategi customer retention dan engagement yang sesuai.

---

# 📊 6. Subplot — Multiple Visualization

Subplot digunakan untuk menampilkan beberapa visualisasi dalam satu canvas.

Notebook menggabungkan:

### Left Plot

**Total Profit per Category**

Digunakan untuk melihat kontribusi profit dari setiap kategori produk.

### Right Plot

**Total Quantity per Month**

Digunakan untuk melihat perubahan jumlah unit yang terjual setiap bulan.

### Insight

Dengan menggunakan subplot, beberapa perspektif data dapat ditampilkan secara bersamaan.

Pendekatan ini berguna sebagai dasar untuk membuat **simple analytical dashboard**.

---

# 🎨 Visualization Principles

Notebook juga mempraktikkan beberapa elemen penting dalam membuat visualisasi:

### Title

Memberikan konteks mengenai informasi yang ditampilkan.

### Axis Labels

Menjelaskan variabel yang terdapat pada sumbu X dan Y.

### Legend

Membantu mengidentifikasi elemen visual tertentu.

### Annotation

Digunakan untuk menyoroti informasi penting, seperti:

> Peak Sales

### Color

Digunakan sebagai elemen visual untuk membantu membedakan kategori atau memberikan emphasis.

---

# ⚠️ Data Visualization Pitfalls

Beberapa hal penting yang perlu diperhatikan:

### 1. Date harus memiliki tipe yang tepat

`Order Date` awalnya terbaca sebagai teks.

Jika tidak dikonversi menggunakan `pd.to_datetime()`, pengurutan dan analisis berbasis waktu dapat menghasilkan urutan yang salah.

---

### 2. Jangan menggunakan Pie Chart untuk terlalu banyak kategori

Pie chart lebih mudah dibaca ketika jumlah kategori sedikit.

Untuk kategori yang banyak, **bar chart** biasanya lebih efektif karena perbedaan antar kategori lebih mudah dibandingkan.

---

### 3. Visualisasi bukan hanya tentang estetika

Grafik seharusnya membantu menjawab pertanyaan bisnis.

```text
Data
 ↓
Aggregation
 ↓
Visualization
 ↓
Pattern
 ↓
Insight
 ↓
Decision
```

Visualisasi yang baik membantu stakeholder memahami pola data tanpa harus membaca seluruh angka mentah.

---

# 💡 Key Insights

Beberapa insight utama dari analisis:

1. 📈 Penjualan menunjukkan **pola musiman**, dengan peningkatan pada periode akhir tahun.

2. 🌎 **West** merupakan region dengan total penjualan tertinggi, sedangkan **South** berada pada posisi terendah.

3. 💰 Sebagian transaksi memiliki **profit negatif**, sehingga perlu dilakukan analisis lebih lanjut terhadap faktor yang menyebabkan kerugian.

4. 👥 **Consumer** merupakan segment dengan kontribusi penjualan terbesar, yaitu sekitar **50.6%**.

5. 📊 **Subplot** memungkinkan beberapa perspektif analisis ditampilkan dalam satu canvas dan dapat menjadi dasar untuk membangun dashboard sederhana.

---

# 📚 Learning Outcomes

Melalui project ini, konsep yang dipraktikkan meliputi:

* Data loading
* Data inspection
* Datetime conversion
* Data aggregation
* Time-series aggregation
* Line chart
* Bar chart
* Histogram
* Pie chart
* Subplot
* Annotation
* Visualization customization
* Business insight generation
* Data visualization best practices

---

# 🚀 Next Step

Visualisasi merupakan salah satu tahap penting dalam **Data Science workflow**.

```text
Data
 ↓
Data Understanding
 ↓
Data Preparation
 ↓
Exploratory Data Analysis
 ↓
Visualization
 ↓
Insight
 ↓
Decision
```

Pemahaman visualisasi ini menjadi fondasi sebelum masuk ke analisis yang lebih kompleks seperti **statistical analysis, machine learning, dan dashboarding**.

---

## 📁 Project Structure

```text
Day_11_Datavisualize_using_matplotlib/
│
├── Day_11_Datavisualize_using_matplotlib.ipynb
└── README.md
```

---

## 👤 Author

**Arief Wicaksono**

Aspiring AI Engineer | Data Science Student

---

⭐ If you find this project useful, feel free to star the repository!
