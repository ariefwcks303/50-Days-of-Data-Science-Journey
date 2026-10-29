# Day 42 — Determining the Optimal Number of Clusters with K-Means

## 📌 Overview

Pada Day 42, saya melanjutkan pembelajaran **Unsupervised Learning** menggunakan **K-Means Clustering**.

Pada project sebelumnya, jumlah cluster `k=5` digunakan secara langsung. Pada project ini, saya mencoba menjawab pertanyaan:

> **"Kenapa harus menggunakan 5 cluster?"**

Untuk menentukan jumlah cluster yang lebih terukur, digunakan dua pendekatan:

- **Elbow Method menggunakan Inertia**
- **Silhouette Score**

Kedua metode tersebut digunakan untuk membandingkan beberapa nilai `k` dan menentukan jumlah cluster yang paling masuk akal sebelum membangun model K-Means final.

> **Focus:** Unsupervised Learning, K-Means Clustering, Elbow Method, Inertia, Silhouette Score, Optimal Cluster Selection, Customer Segmentation, & Business Insights.

---

## 🎯 Learning Objectives

Pada project ini saya mempelajari:

- Memahami konsep **Unsupervised Learning**
- Memahami kembali konsep **K-Means Clustering**
- Memahami **Inertia**
- Menggunakan **Elbow Method** untuk menentukan jumlah cluster
- Memahami **Silhouette Score**
- Menghitung Silhouette Score untuk beberapa nilai `k`
- Membandingkan hasil Elbow Method dan Silhouette Score
- Menentukan `k` optimal berdasarkan hasil evaluasi
- Melatih final K-Means menggunakan `k` yang telah dipilih
- Menginterpretasikan centroid setiap cluster
- Membuat customer segmentation
- Mengubah hasil clustering menjadi actionable business insights

---

## 📊 Dataset

Dataset yang digunakan adalah **Mall Customer Segmentation**.

**Source:** Kaggle — `vjchoudhary7/customer-segmentation-tutorial-in-python`

Dataset terdiri dari:

- **200 rows**
- **5 columns**
- **0 missing values**
- **0 duplicate data**

### Dataset Features

| Column | Description |
|---|---|
| `CustomerID` | ID unik pelanggan |
| `Gender` | Jenis kelamin pelanggan |
| `Age` | Usia pelanggan |
| `Annual Income (k$)` | Pendapatan tahunan dalam ribu USD |
| `Spending Score (1-100)` | Skor belanja pelanggan |

Dalam project ini, fokus clustering menggunakan dua feature:

- `Annual Income (k$)`
- `Spending Score (1-100)`

Dataset memiliki rentang:

- Annual Income: **15–137 k$**
- Spending Score: **1–99**

 

---

## 🔎 Exploratory Data Analysis

Sebelum melakukan clustering, dilakukan pemeriksaan terhadap struktur dan kualitas data.

Tahapan yang dilakukan:

- `df.head()`
- `df.info()`
- `df.describe()`
- Mengecek jumlah baris dan kolom
- Mengecek missing value
- Mengecek duplicate data
- Visualisasi hubungan Annual Income dan Spending Score

Hasil pengecekan:

```text
Rows             : 200
Columns          : 5
Missing Values   : 0
Duplicate Data   : 0
```

Dataset memiliki variasi yang cukup baik pada `Annual Income` dan `Spending Score`, sehingga kedua feature tersebut digunakan sebagai dasar customer segmentation. 

---

## ⚙️ Feature Selection & Standardization

Feature yang digunakan:

```python
X = df[
    ['Annual Income (k$)', 'Spending Score (1-100)']
].copy()
```

Karena K-Means menggunakan perhitungan jarak, feature perlu distandardisasi agar feature dengan rentang lebih besar tidak mendominasi proses clustering.

Digunakan `StandardScaler`:

```python
scaler = StandardScaler()

X_scaled = scaler.fit_transform(X)
```

Hasil:

```text
Shape              : (200, 2)
Mean               : [0, 0]
Standard Deviation : [1, 1]
```

Dengan demikian kedua feature berada pada skala yang sebanding sebelum digunakan oleh K-Means.

---

# 📐 Determining the Optimal Number of Clusters

Salah satu masalah penting pada K-Means adalah menentukan nilai `k`.

Pada project sebelumnya, `k=5` digunakan secara langsung. Pada project ini, dilakukan evaluasi untuk mencari alasan yang lebih kuat dalam menentukan jumlah cluster.

Dua metode digunakan:

1. **Elbow Method**
2. **Silhouette Score**

---

## 1. 📉 Elbow Method — Inertia

**Inertia** merupakan total squared distance antara setiap data point dengan centroid cluster-nya.

Secara umum:

- Semakin besar jumlah cluster, inertia akan semakin kecil.
- Tetapi kita tidak ingin menambah cluster tanpa batas.
- Kita mencari titik ketika penurunan inertia mulai melandai.
- Titik tersebut disebut sebagai **elbow**.

Pengujian dilakukan untuk:

```python
K_range = range(1, 11)

inertia_list = []

for k in K_range:
    kmeans = KMeans(
        n_clusters=k,
        random_state=42
    )

    kmeans.fit(X_scaled)
    inertia_list.append(kmeans.inertia_)
```

Kemudian hasilnya divisualisasikan menggunakan Elbow Curve.

### Result

Dari grafik, titik elbow berada pada:

> **k = 5**

Setelah `k=5`, penurunan inertia mulai terlihat lebih landai dan penambahan cluster tidak memberikan perubahan yang signifikan.

Dengan demikian, Elbow Method merekomendasikan:

```text
Optimal k = 5
```

 

---

## 2. 📊 Silhouette Score

Elbow Method kemudian dilengkapi dengan **Silhouette Score**.

Silhouette Score mengukur seberapa dekat sebuah data point dengan cluster-nya sendiri dibandingkan dengan cluster lain.

Nilai Silhouette berada pada rentang:

```text
-1 sampai 1
```

Secara umum:

- Mendekati **1** → cluster semakin compact dan terpisah dengan baik
- Mendekati **0** → terdapat overlap antar cluster
- Nilai negatif → terdapat kemungkinan data lebih dekat dengan cluster lain

Notebook menghitung Silhouette Score untuk:

```text
k = 2 sampai 10
```

### Results

| k | Silhouette Score |
|---:|---:|
| 2 | 0.3973 |
| 3 | 0.4666 |
| 4 | 0.4943 |
| **5** | **0.5547** |
| 6 | 0.5138 |
| 7 | 0.5020 |
| 8 | 0.4550 |
| 9 | 0.4567 |
| 10 | 0.4448 |

Hasil tertinggi diperoleh pada:

> **k = 5 → Silhouette Score = 0.5547**

Setelah `k=5`, nilai Silhouette Score kembali menurun.

---

## 🏆 Optimal Number of Clusters

Hasil dari kedua metode menunjukkan kesimpulan yang sama:

| Method | Recommended k |
|---|---:|
| Elbow Method | **5** |
| Silhouette Score | **5** |

Dengan demikian, `k=5` tidak lagi dipilih secara arbitrary seperti pada project sebelumnya.

Kedua metode memberikan validasi bahwa:

> **5 clusters merupakan pilihan yang paling masuk akal untuk dataset ini.**



---

# 🤖 Final K-Means Model

Setelah `k=5` ditentukan, model K-Means final dibuat:

```python
k_final = 5

kmeans_final = KMeans(
    n_clusters=k_final,
    random_state=42,
    n_init=10
)

df["Cluster"] = kmeans_final.fit_predict(X_scaled)
```

Model kemudian digunakan untuk memberikan label cluster kepada setiap pelanggan.



---

## 📍 Cluster Centroids

Centroid dikembalikan ke skala asli menggunakan:

```python
centroids = scaler.inverse_transform(
    kmeans_final.cluster_centers_
)
```

Hasil centroid:

| Cluster | Annual Income (k$) | Spending Score |
|---:|---:|---:|
| 0 | 55.30 | 49.52 |
| 1 | 86.54 | 82.13 |
| 2 | 25.73 | 79.36 |
| 3 | 88.20 | 17.11 |
| 4 | 26.30 | 20.91 |



---

# 👥 Customer Segmentation

Berdasarkan centroid tersebut, kelima cluster dapat diinterpretasikan menjadi:

### Cluster 0 — Menengah-Menengah

- Income: **55.30k**
- Spending Score: **49.52**
- Karakteristik: pelanggan dengan pendapatan dan pengeluaran pada tingkat menengah.

**Marketing Strategy:**

- Promo musiman rutin
- Standard membership / loyalty program
- Rekomendasi produk populer

---

### Cluster 1 — Prioritas / Sultan

- Income: **86.54k**
- Spending Score: **82.13**
- Karakteristik: pelanggan dengan income tinggi dan spending tinggi.

Cluster ini merupakan **target utama** karena memiliki potensi nilai pelanggan yang tinggi.

**Marketing Strategy:**

- VIP loyalty program
- Early access produk baru
- Undangan event khusus
- Personal shopper

---

### Cluster 2 — Impulsif / Pemburu Diskon

- Income: **25.73k**
- Spending Score: **79.36**
- Karakteristik: income rendah tetapi memiliki spending score tinggi.

**Marketing Strategy:**

- Flash sale
- Bundling produk
- Cicilan / paylater
- Promo melalui media sosial

---

### Cluster 3 — Potensial Hemat

- Income: **88.20k**
- Spending Score: **17.11**
- Karakteristik: income tinggi tetapi spending rendah.

Kelompok ini memiliki kemampuan finansial tinggi tetapi cenderung selektif dalam berbelanja.

**Marketing Strategy:**

- Produk premium berkualitas
- Value-for-money
- Produk dengan utility tinggi
- Marketing yang menekankan investasi jangka panjang

---

### Cluster 4 — Hemat / Minimalis

- Income: **26.30k**
- Spending Score: **20.91**
- Karakteristik: income rendah dan spending rendah.

**Marketing Strategy:**

- Produk kebutuhan pokok
- Direct discount
- Clearance sale
- Produk dengan harga ekonomis

Interpretasi centroid dan strategi segmentasi tersebut berasal dari hasil final K-Means pada notebook. 

---

## 💡 Key Insights

Beberapa insight utama dari project:

1. **K-Means membutuhkan pemilihan `k` yang tepat**, bukan sekadar memilih jumlah cluster secara arbitrary.
2. **Elbow Method** menunjukkan titik elbow pada `k=5`.
3. **Silhouette Score** mencapai nilai tertinggi pada `k=5` dengan score sekitar **0.55**.
4. Kedua metode memberikan rekomendasi yang konsisten yaitu **5 clusters**.
5. Cluster dengan income dan spending tinggi menjadi segmen pelanggan prioritas.
6. Pelanggan dengan income rendah tetapi spending tinggi menunjukkan pola perilaku yang berbeda dan membutuhkan strategi marketing tersendiri.
7. Pelanggan dengan income tinggi tetapi spending rendah merupakan kelompok potensial yang dapat ditargetkan menggunakan pendekatan berbasis value dan kualitas.
8. Hasil clustering dapat digunakan untuk menghindari pendekatan marketing massal dan membuat strategi yang lebih targeted. 

---

## 🎯 Business Application

Hasil project dapat diterapkan dalam proses customer segmentation:

```text
Customer Data
      ↓
Feature Selection
      ↓
StandardScaler
      ↓
Evaluate k = 1..10
      ↓
Elbow Method
      +
Silhouette Score
      ↓
Optimal k = 5
      ↓
Final K-Means
      ↓
5 Customer Segments
      ↓
Cluster Profiling
      ↓
Targeted Marketing Strategy
```

Dengan pendekatan ini, perusahaan dapat menyesuaikan strategi marketing berdasarkan karakteristik masing-masing kelompok pelanggan daripada memberikan promosi yang sama kepada seluruh customer.

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
Feature Selection
      ↓
StandardScaler
      ↓
Elbow Method
      ↓
Calculate Inertia
      ↓
Silhouette Score
      ↓
Compare k = 2..10
      ↓
Select Optimal k = 5
      ↓
Train Final K-Means
      ↓
Calculate Centroids
      ↓
Customer Segmentation
      ↓
Cluster Interpretation
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
  - `KMeans`
  - `StandardScaler`
  - `silhouette_score`
- Google Colab
- Kaggle Dataset

---

## 📂 Project Structure

```text
Day_42_Unsupervised_Model_Decided_Optimal_Cluster/
│
├── Day_42_Unsupervised_Model_Decided_Optimal_Cluster.ipynb
└── README.md
```

---

## 📌 Conclusion

Day 42 memperbaiki pendekatan pada project clustering sebelumnya.

Jika pada project sebelumnya `k=5` digunakan secara langsung, pada project ini jumlah cluster ditentukan melalui pendekatan yang lebih terukur menggunakan:

- **Elbow Method**
- **Silhouette Score**

Hasil analisis menunjukkan bahwa kedua metode sama-sama merekomendasikan:

```text
Optimal Number of Clusters = 5
Silhouette Score ≈ 0.55
```

Setelah `k=5` dipilih, final K-Means berhasil membagi customer menjadi lima segmen berdasarkan kombinasi **Annual Income** dan **Spending Score**.

Dengan demikian, proses clustering tidak hanya menghasilkan kelompok pelanggan, tetapi juga memberikan dasar yang lebih kuat untuk melakukan **customer segmentation** dan menyusun **targeted marketing strategy**.

---

## 👨‍💻 Author

**Arief Wicaksono**  
Aspiring AI Engineer | Data Science Student

---

⭐ *Learning Machine Learning one day at a time.*