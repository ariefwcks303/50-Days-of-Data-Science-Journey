# Day 41 — Customer Segmentation using K-Means Clustering

## 📌 Overview

Pada Day 41, saya mulai masuk ke **Unsupervised Learning**, yaitu pendekatan Machine Learning yang tidak menggunakan label atau target sebagai jawaban benar.

Project ini menggunakan **K-Means Clustering** untuk melakukan **customer segmentation** berdasarkan karakteristik pendapatan dan perilaku belanja pelanggan mall.

Tujuan akhirnya bukan hanya membagi pelanggan ke dalam beberapa cluster, tetapi juga **memahami karakteristik setiap segmen** sehingga hasil clustering dapat digunakan sebagai dasar strategi marketing.

> **Focus:** Unsupervised Learning, K-Means Clustering, Feature Scaling, Customer Segmentation, Cluster Profiling, & Business Insights.

---

## 🎯 Learning Objectives

Pada project ini saya mempelajari:

- Memahami konsep **Unsupervised Learning**
- Memahami konsep **Clustering**
- Memahami cara kerja **K-Means**
- Memahami konsep **centroid**
- Memahami pentingnya **feature scaling** pada clustering berbasis jarak
- Menggunakan `StandardScaler`
- Menggunakan `KMeans` dari Scikit-Learn
- Membuat clustering dengan `k=5`
- Memvisualisasikan hasil clustering dan centroid
- Melakukan **cluster profiling**
- Mengubah hasil clustering menjadi **business insights**

---

## 📊 Dataset

Dataset yang digunakan adalah **Mall Customer Segmentation**.

**Source:** Kaggle — `vjchoudhary7/customer-segmentation-tutorial-in-python`

Dataset terdiri dari:

- **200 rows**
- **5 columns**
- Tidak terdapat missing value
- Tidak terdapat duplicate data

### Features

| Column | Description |
|---|---|
| `CustomerID` | ID pelanggan |
| `Gender` | Jenis kelamin pelanggan |
| `Age` | Usia pelanggan |
| `Annual Income (k$)` | Pendapatan tahunan dalam ribu USD |
| `Spending Score (1-100)` | Skor belanja pelanggan |

Dataset memiliki rentang:

- Age: **18–70 tahun**
- Annual Income: **15–137 k$**
- Spending Score: **1–99**

Dalam proses clustering, fokus utama diberikan pada:

- `Annual Income (k$)`
- `Spending Score (1-100)`

---

## 🔎 Exploratory Data Analysis

Beberapa eksplorasi dilakukan sebelum proses clustering:

- Melihat struktur dataset menggunakan `df.info()`
- Melihat statistik deskriptif menggunakan `df.describe()`
- Mengecek missing value
- Mengecek duplicate data
- Melihat distribusi `Annual Income`
- Melihat distribusi `Spending Score`
- Membandingkan distribusi spending score berdasarkan gender

Hasil pengecekan menunjukkan bahwa dataset memiliki **200 data, 5 kolom, 0 missing value, dan 0 duplicate data**. 

---

## ⚙️ Feature Selection & Scaling

K-Means menggunakan jarak Euclidean untuk menentukan kedekatan antar data.

Karena setiap feature dapat memiliki skala yang berbeda, dilakukan standardisasi menggunakan `StandardScaler`.

Feature yang digunakan:

```python
feature_x = df[
    ['Annual Income (k$)', 'Spending Score (1-100)']
]

scaler = StandardScaler()

feature_x_scaled = scaler.fit_transform(feature_x)
```

Hasil scaling menghasilkan matrix dengan bentuk:

```text
(200, 2)
```

Scaling penting agar salah satu feature tidak mendominasi perhitungan jarak hanya karena memiliki rentang nilai yang lebih besar.

---

## 🤖 K-Means Clustering

Model K-Means yang digunakan:

```python
k_mns = KMeans(
    n_clusters=5,
    n_init=10,
    random_state=42
)

df["Cluster"] = k_mns.fit_predict(feature_x_scaled)
```

Parameter:

| Parameter | Value |
|---|---:|
| `n_clusters` | 5 |
| `n_init` | 10 |
| `random_state` | 42 |

Secara sederhana, K-Means bekerja dengan:

1. Menentukan centroid awal
2. Menempatkan setiap customer ke centroid terdekat
3. Menghitung ulang posisi centroid berdasarkan anggota cluster
4. Mengulangi proses sampai posisi cluster stabil

`n_init=10` digunakan untuk menjalankan beberapa inisialisasi berbeda dan memilih hasil terbaik, sedangkan `random_state=42` digunakan agar hasil dapat direproduksi.

---

## 📦 Cluster Distribution

Hasil clustering menghasilkan 5 cluster:

| Cluster | Jumlah Customer |
|---|---:|
| Cluster 0 | 81 |
| Cluster 1 | 39 |
| Cluster 2 | 22 |
| Cluster 3 | 35 |
| Cluster 4 | 23 |

Cluster 0 merupakan kelompok terbesar, sedangkan Cluster 2 dan Cluster 4 merupakan kelompok yang relatif kecil. Secara keseluruhan, ukuran cluster tidak merata.

---

## 📈 Cluster Visualization & Centroid

Hasil clustering divisualisasikan menggunakan scatter plot:

- **X-axis:** Annual Income
- **Y-axis:** Spending Score
- **Color:** Cluster
- **Marker:** Centroid

Centroid dikembalikan ke skala asli menggunakan:

```python
centroids = scaler.inverse_transform(
    k_mns.cluster_centers_
)
```

Hal ini membuat posisi centroid lebih mudah diinterpretasikan dalam konteks pendapatan dan spending score asli.

---

## 👥 Customer Segment Profiling

Untuk memahami karakteristik setiap cluster, dilakukan aggregation berdasarkan cluster:

```python
profil = df.groupby("Cluster").agg(
    Jumlah=("CustomerID", "count"),
    Rata_Usia=("Age", "mean"),
    Rata_Income=("Annual Income (k$)", "mean"),
    Rata_Spending=("Spending Score (1-100)", "mean"),
).round(1)
```

Hasil profiling:

| Cluster | Jumlah | Avg Age | Avg Income (k$) | Avg Spending |
|---|---:|---:|---:|---:|
| 0 | 81 | ~42.7 | ~55.3 | ~49.5 |
| 1 | 39 | ~32.7 | ~86.5 | ~82.1 |
| 2 | 22 | ~45.3 | ~25.7 | ~79.4 |
| 3 | 35 | ~41.1 | ~88.2 | ~17.1 |
| 4 | 23 | ~45.2 | ~26.3 | ~20.9 |



---

## 🧩 Cluster Interpretation

### Cluster 0 — The Standard Majority

- 81 customers
- Usia sekitar 43 tahun
- Pendapatan menengah
- Spending score sedang

Merupakan kelompok pelanggan terbesar dengan karakteristik yang relatif standar.

**Business strategy:**
- Customer retention
- Loyalty program
- Promosi berkala
- Produk kebutuhan sehari-hari dengan value yang baik

---

### Cluster 1 — The VIP Big Spenders

- 39 customers
- Usia sekitar 33 tahun
- Income tinggi
- Spending score sangat tinggi

Kelompok ini menjadi salah satu target marketing utama karena memiliki kombinasi **high income + high spending**.

**Business strategy:**
- Premium service
- Exclusive offers
- Limited edition products
- Private event
- VIP experience

---

### Cluster 2 — The Impulsive Shoppers

- 22 customers
- Income rendah
- Spending score sangat tinggi

Cluster ini memiliki karakteristik unik karena pelanggan memiliki pendapatan relatif rendah tetapi memiliki kecenderungan belanja yang tinggi.

**Business strategy:**
- Flash sale
- Bundling
- Promotional campaigns
- Flexible payment options

---

### Cluster 3 — The Frugal High-Earners

- 35 customers
- Income sangat tinggi
- Spending score sangat rendah

Kelompok ini memiliki daya beli tinggi tetapi cenderung selektif atau hemat dalam berbelanja.

**Business strategy:**
- Value-for-money products
- High-quality products
- Cashback
- Penawaran yang menekankan kualitas dan manfaat produk

---

### Cluster 4 — The Low-Value Customers

- 23 customers
- Income rendah
- Spending score rendah

Kelompok ini memiliki daya beli dan aktivitas belanja yang relatif rendah.

**Business strategy:**
- Basic discount
- Clearance sale
- Low-cost promotional campaigns

Profil dan strategi setiap cluster tersebut mengikuti interpretasi yang terdapat di notebook.

---

## 💡 Key Insights

Beberapa insight utama dari project ini:

1. **K-Means berhasil membagi 200 customer menjadi 5 segment.**
2. `Annual Income` dan `Spending Score` memberikan pola yang cukup jelas untuk customer segmentation.
3. Cluster terbesar adalah **Cluster 0 dengan 81 customer**.
4. **Cluster 1** menjadi kelompok potensial karena memiliki income dan spending score yang sama-sama tinggi.
5. **Cluster 2** menunjukkan pelanggan dengan income rendah tetapi spending score tinggi.
6. **Cluster 3** menunjukkan pelanggan dengan income tinggi tetapi spending score rendah.
7. **Cluster 4** memiliki income dan spending score yang sama-sama rendah.
8. Hasil clustering dapat digunakan untuk membuat strategi marketing yang berbeda untuk setiap customer segment.

Notebook menyimpulkan bahwa model K-Means dengan `k=5` berhasil memetakan pelanggan menjadi lima profil perilaku belanja yang berbeda.

---

## 🎯 Business Application

Hasil customer segmentation dapat digunakan untuk melakukan **targeted marketing**.

Contohnya:

```text
Customer Data
     │
     ▼
Feature Selection
     │
     ▼
StandardScaler
     │
     ▼
K-Means Clustering
     │
     ▼
5 Customer Segments
     │
     ├── Standard Majority
     ├── VIP Big Spenders
     ├── Impulsive Shoppers
     ├── Frugal High-Earners
     └── Low-Value Customers
     │
     ▼
Targeted Marketing Strategy
```

Dengan pendekatan ini, satu strategi marketing tidak harus diterapkan kepada seluruh pelanggan. Setiap segmen dapat diberikan pendekatan yang berbeda berdasarkan karakteristiknya.

---

## 🔄 End-to-End Workflow

```text
Import Dataset
      ↓
Data Understanding
      ↓
Exploratory Data Analysis
      ↓
Check Missing Values & Duplicates
      ↓
Select Clustering Features
      ↓
StandardScaler
      ↓
K-Means Clustering
      ↓
Assign Cluster Labels
      ↓
Cluster Visualization
      ↓
Centroid Visualization
      ↓
Cluster Profiling
      ↓
Customer Segment Interpretation
      ↓
Business Insights
```

---

## 🛠️ Tech Stack

- Python
- Pandas
- NumPy
- Matplotlib
- Seaborn
- Scikit-Learn
  - `StandardScaler`
  - `KMeans`
- Google Colab
- Kaggle Dataset

---

## 📂 Project Structure

```text
Day_41_Unsupervised_Model_Segmentation_Customers_using_KMeans_Clustering/
│
├── Day_41_Unsupervised_Model_Segmentation_Customers_using_KMeans_Clustering.ipynb
└── README.md
```

---

## 📌 Conclusion

Day 41 menjadi pengenalan saya terhadap **Unsupervised Learning**, khususnya **K-Means Clustering**.

Pada project ini, saya belajar bahwa clustering tidak berhenti pada proses menjalankan algoritma. Setelah cluster terbentuk, langkah penting berikutnya adalah **profiling dan interpretation** untuk memahami karakteristik setiap kelompok.

Dengan menggunakan `Annual Income` dan `Spending Score`, K-Means berhasil membagi 200 pelanggan menjadi **5 customer segments** dengan karakteristik yang berbeda dan dapat digunakan sebagai dasar untuk menyusun strategi marketing yang lebih targeted.

---

## 👨‍💻 Author

**Arief Wicaksono**  
Aspiring AI Engineer | Data Science Student

---

⭐ *Learning Machine Learning one day at a time.*