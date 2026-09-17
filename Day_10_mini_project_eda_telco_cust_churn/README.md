# 📊 Day 10 — Mini Project: EDA Telco Customer Churn

Mini project ini merupakan implementasi **Exploratory Data Analysis (EDA)** pada dataset **Telco Customer Churn**.

Berbeda dari analisis sebelumnya, pada project ini proses EDA dikembangkan menjadi sebuah fungsi Python yang **reusable**, sehingga informasi dasar dari dataset dapat dirangkum secara otomatis hanya dengan satu pemanggilan fungsi.

Project ini juga menggabungkan **data cleaning, statistical analysis, visualization, dan business insight** untuk memahami faktor-faktor yang berkaitan dengan customer churn.

---

## 🎯 Project Objectives

Tujuan utama project ini adalah:

* Memahami struktur dan karakteristik dataset Telco Customer Churn.
* Melakukan data cleaning dan memastikan tipe data sudah sesuai.
* Mengidentifikasi missing values dan data yang tidak valid.
* Membangun fungsi EDA otomatis yang reusable.
* Menganalisis distribusi customer churn.
* Menganalisis hubungan antara jenis kontrak dan churn.
* Menganalisis distribusi tenure berdasarkan status churn.
* Melakukan **Chi-Square Test** untuk menguji hubungan antara `Contract` dan `Churn`.
* Menghasilkan insight bisnis yang dapat digunakan sebagai dasar strategi customer retention.

---

## 📂 Dataset

Dataset yang digunakan adalah:

**Telco Customer Churn**

📌 Source: [Kaggle — Telco Customer Churn](https://www.kaggle.com/datasets/blastchar/telco-customer-churn)

Dataset terdiri dari:

* **7.043 pelanggan**
* **21 kolom**

Dataset berisi informasi mengenai profil pelanggan, layanan yang digunakan, jenis kontrak, biaya berlangganan, dan status churn.

### Important Features

| Feature          | Description                       |
| ---------------- | --------------------------------- |
| `gender`         | Jenis kelamin pelanggan           |
| `SeniorCitizen`  | Status senior citizen             |
| `Partner`        | Status memiliki pasangan          |
| `Dependents`     | Status memiliki tanggungan        |
| `tenure`         | Lama berlangganan dalam bulan     |
| `Contract`       | Jenis kontrak pelanggan           |
| `MonthlyCharges` | Biaya berlangganan bulanan        |
| `TotalCharges`   | Total biaya yang telah dibayarkan |
| `Churn`          | Status pelanggan: Yes / No        |

`Churn` digunakan sebagai indikator utama untuk mengetahui apakah pelanggan berhenti menggunakan layanan.

---

## 🛠️ Tech Stack

Project ini menggunakan:

* **Python**
* **Pandas** — Data manipulation & analysis
* **NumPy** — Numerical computation
* **Matplotlib** — Visualization
* **Seaborn** — Statistical visualization
* **SciPy** — Statistical hypothesis testing

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
Data Cleaning
   │
   ▼
Reusable EDA Function
   │
   ▼
Exploratory Data Analysis
   │
   ├── Churn Distribution
   ├── Contract vs Churn
   └── Tenure vs Churn
   │
   ▼
Statistical Testing
   │
   └── Chi-Square Test
   │
   ▼
Business Insights
   │
   ▼
Customer Retention Recommendation
```

---

# 🧹 1. Data Cleaning

Salah satu permasalahan penting ditemukan pada kolom:

```text
TotalCharges
```

Meskipun secara konsep merupakan data numerik, kolom tersebut tersimpan sebagai **string** dan terdapat beberapa nilai kosong/spasi.

Konversi dilakukan menggunakan:

```python
df["TotalCharges"] = pd.to_numeric(
    df["TotalCharges"],
    errors="coerce"
)
```

Nilai yang tidak dapat dikonversi menjadi `NaN`.

Ditemukan **11 missing values** pada `TotalCharges`.

Nilai tersebut kemudian ditangani sebagai `0`, karena pelanggan terkait memiliki:

```text
tenure = 0
```

yang mengindikasikan pelanggan baru.

Kolom `customerID` kemudian dihapus karena merupakan identifier dan tidak memberikan informasi analitis yang diperlukan dalam EDA.

---

# ⚙️ 2. Reusable EDA Function

Salah satu tujuan utama mini project ini adalah membangun fungsi:

```python
ringkas_eda(data)
```

Fungsi ini dirancang agar dapat digunakan pada berbagai dataset DataFrame.

Informasi yang dihasilkan meliputi:

* Jumlah baris
* Jumlah kolom
* Jumlah duplikat
* Data type setiap kolom
* Missing values
* Missing percentage
* Jumlah unique values
* Statistik deskriptif numerik
* Top categories untuk fitur kategorikal

Dengan demikian, proses eksplorasi awal dataset dapat dilakukan secara konsisten hanya melalui satu fungsi.

Contoh penggunaan:

```python
info = ringkas_eda(df)
```

---

# 📊 3. Exploratory Data Analysis

## A. Churn Distribution

Analisis distribusi `Churn` menunjukkan:

| Status    | Customers | Proportion |
| --------- | --------: | ---------: |
| No Churn  |     5,174 |       ~73% |
| Churn     |     1,869 |       ~27% |
| **Total** | **7,043** |   **100%** |

### Insight

Sekitar **27% pelanggan mengalami churn**, sedangkan sekitar **73% pelanggan masih bertahan**.

Hal ini menunjukkan bahwa dataset memiliki **class imbalance**.

Kondisi ini perlu diperhatikan apabila dataset nantinya digunakan untuk membangun model machine learning.

---

# 📑 4. Churn berdasarkan Contract

Analisis dilakukan untuk melihat distribusi churn berdasarkan jenis kontrak:

* Month-to-month
* One year
* Two year

Hasil eksplorasi menunjukkan bahwa pelanggan dengan kontrak:

> **Month-to-month**

memiliki tingkat churn yang jauh lebih tinggi dibandingkan pelanggan dengan kontrak jangka panjang.

Tingkat churn pada pelanggan month-to-month berada di sekitar **44%**.

### Business Insight

Pelanggan dengan kontrak jangka pendek merupakan kelompok yang lebih rentan untuk meninggalkan layanan.

Strategi retention dapat difokuskan pada:

* Pelanggan month-to-month
* Program upgrade kontrak
* Insentif untuk kontrak tahunan
* Penawaran paket yang lebih sesuai dengan kebutuhan pelanggan

---

# 📈 5. Tenure vs Churn

Analisis histogram digunakan untuk melihat distribusi `tenure` berdasarkan status churn.

`tenure` menunjukkan lama pelanggan menggunakan layanan dalam satuan bulan.

Rentang tenure pada dataset adalah:

```text
0 – 72 bulan
```

Visualisasi menggunakan **24 bins**, sehingga setiap bin merepresentasikan sekitar **3 bulan**.

### Insight

Churn terlihat lebih tinggi pada pelanggan dengan tenure yang masih relatif pendek.

Hal ini menunjukkan bahwa periode awal hubungan pelanggan dengan provider merupakan fase yang penting untuk customer retention.

Setelah pelanggan memiliki tenure yang lebih panjang, jumlah churn cenderung menurun.

### Business Implication

Provider dapat memberikan perhatian lebih besar kepada pelanggan baru melalui:

* Onboarding yang lebih baik
* Customer support
* Monitoring kepuasan pelanggan
* Promotional offers
* Personalized retention campaigns

---

# 🧪 6. Statistical Testing — Chi-Square Test

Visualisasi menunjukkan adanya perbedaan churn berdasarkan jenis kontrak.

Namun, perbedaan visual belum cukup untuk menyimpulkan bahwa kedua variabel benar-benar memiliki hubungan secara statistik.

Karena:

```text
Contract → Categorical
Churn    → Categorical
```

digunakan **Chi-Square Test of Independence**.

### Hypothesis

**H₀ (Null Hypothesis)**

Tidak terdapat hubungan antara `Contract` dan `Churn`.

**H₁ (Alternative Hypothesis)**

Terdapat hubungan antara `Contract` dan `Churn`.

### Decision Rule

```text
p-value < 0.05
→ Reject H₀
→ Terdapat hubungan yang signifikan
```

Hasil pengujian menghasilkan **p-value yang jauh lebih kecil dari 0.05**.

Dengan demikian:

> **Terdapat hubungan yang signifikan secara statistik antara jenis kontrak dan customer churn.**

Artinya, pola churn berdasarkan jenis kontrak yang terlihat pada visualisasi bukan sekadar variasi acak dalam dataset.

---

# 💡 Key Insights

Beberapa insight utama yang diperoleh:

### 1. Customer Churn

Sekitar **27% pelanggan mengalami churn**, sehingga terdapat kelompok pelanggan yang cukup signifikan yang perlu menjadi perhatian perusahaan.

### 2. Contract Type

Pelanggan **month-to-month** memiliki tingkat churn yang lebih tinggi dibandingkan pelanggan dengan kontrak satu atau dua tahun.

### 3. Customer Tenure

Pelanggan dengan tenure pendek merupakan kelompok yang lebih rentan terhadap churn.

### 4. Statistical Evidence

Chi-Square Test menunjukkan bahwa terdapat **hubungan signifikan antara Contract dan Churn**.

### 5. Data Quality Matters

`TotalCharges` secara konseptual merupakan data numerik, tetapi tersimpan sebagai string. Hal ini menunjukkan pentingnya melakukan validasi tipe data sebelum melakukan analisis maupun modeling.

---

# 💼 Business Recommendations

Berdasarkan hasil EDA, strategi retention dapat difokuskan pada:

### 🎯 1. Fokus pada Pelanggan Baru

Pelanggan dengan tenure pendek dapat diberikan perhatian lebih melalui program onboarding dan customer support.

### 🔄 2. Contract Upgrade

Dorong pelanggan month-to-month untuk berpindah ke kontrak jangka panjang melalui:

* Discount
* Promotional packages
* Loyalty benefits
* Contract upgrade incentives

### 📢 3. Early Churn Detection

Gunakan karakteristik pelanggan untuk mengembangkan sistem prediksi churn sehingga pelanggan berisiko tinggi dapat diidentifikasi lebih awal.

### 📊 4. Gunakan Metric yang Tepat

Karena churn hanya sekitar 27%, accuracy saja tidak cukup apabila dataset nantinya digunakan untuk machine learning.

Model churn sebaiknya dievaluasi menggunakan metrik seperti:

* Precision
* Recall
* F1-Score
* ROC-AUC

---

# ⚠️ Data & Modeling Pitfalls

Beberapa hal penting yang ditemukan dalam project:

### `TotalCharges` bukan langsung numerik

```text
TotalCharges → string
```

Perlu dilakukan conversion sebelum analisis numerik.

### Class Imbalance

Distribusi:

```text
No Churn ≈ 73%
Churn    ≈ 27%
```

Model yang selalu memprediksi **No Churn** dapat memperoleh accuracy sekitar 73% tanpa benar-benar mampu mendeteksi pelanggan yang churn.

Karena itu, evaluasi model harus mempertimbangkan metrik selain accuracy.

---

# 📁 Project Structure

```text
Day_10_mini_project_eda_telco_customer_churn/
│
├── Day_10_mini_project__eda_telco_customer_churn.ipynb
└── README.md
```

---

# 📚 Learning Outcomes

Melalui mini project ini, beberapa konsep yang dipraktikkan meliputi:

* Data loading
* Data inspection
* Data type validation
* Data cleaning
* Missing value handling
* Reusable Python function
* Automated EDA
* Descriptive statistics
* Categorical analysis
* Data visualization
* Crosstab analysis
* Customer churn analysis
* Chi-Square Test
* Hypothesis testing
* Business insight generation
* Class imbalance awareness

---

## 🚀 Next Step

Project ini menjadi fondasi untuk tahap berikutnya dalam **Data Science / Machine Learning workflow**:

```text
EDA
 ↓
Feature Engineering
 ↓
Machine Learning
 ↓
Churn Prediction
 ↓
Model Evaluation
 ↓
Deployment
 ↓
Monitoring
```

Dengan memahami pola churn melalui EDA terlebih dahulu, proses machine learning selanjutnya dapat dibangun berdasarkan pemahaman terhadap karakteristik data, bukan sekadar langsung melakukan training model.

---

## 👤 Author

**Arief Wicaksono**

Aspiring AI Engineer | Data Science Student

---

⭐ If you find this project useful, feel free to star the repository!
