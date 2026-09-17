# 🏠 Day 20 — Prediksi Harga Rumah Boston (Boston Housing Dataset)

Proyek ini berfokus pada penerapan **Regresi Linear Multivariat** untuk memprediksi nilai median harga rumah di Boston (`MEDV`) berdasarkan karakteristik lingkungan, tingkat kejahatan, serta kondisi fisik properti.

---

## 🎯 Project Objectives

Tujuan utama proyek ini adalah:

* Memahami dan mengimplementasikan algoritma **Regresi Linear Multivariat**.
* Melakukan **Feature Selection & Correlation Analysis** untuk mengidentifikasi prediktor harga rumah terbaik.
* Menafsirkan **koefisien regresi** dan interaksinya terhadap variabel target (`MEDV`).
* Mengevaluasi performa model menggunakan metrik regresi standar: **MAE**, **RMSE**, dan **R² Score**.

---

## 📂 Dataset

Dataset yang digunakan adalah:

**Boston House Prices**

📌 Source: [Kaggle — Boston House Prices](https://www.kaggle.com/datasets/fedesoriano/the-boston-houseprice-data)

Dataset terdiri dari:

* **506 baris**
* **14 kolom**

### Important Features

| Feature | Description |
| :--- | :--- |
| `CRIM` | Tingkat kriminalitas per kapita berdasarkan kota |
| `ZN` | Proporsi lahan pemukiman untuk kaveling > 25.000 sq.ft |
| `INDUS` | Proporsi luas bisnis non-retail per kota |
| `CHAS` | Variabel dummy Charles River (= 1 jika membatasi sungai; 0 jika tidak) |
| `NOX` | Konsentrasi nitrogen oksida (polusi udara) |
| `RM` | Rata-rata jumlah kamar per hunian |
| `AGE` | Proporsi unit yang dibangun sebelum tahun 1940 |
| `DIS` | Jarak tertimbang ke lima pusat lapangan kerja Boston |
| `RAD` | Indeks aksesibilitas ke jalan raya radial |
| `TAX` | Tarif pajak properti nilai penuh per $10.000 |
| `PTRATIO` | Rasio murid-guru berdasarkan kota |
| `B` | $1000(Bk - 0.63)^2$ di mana $Bk$ adalah proporsi penduduk kulit hitam |
| `LSTAT` | % status populasi tingkat bawah (ekonomi rendah) |
| **`MEDV`** | **Target**: Nilai median rumah yang dihuni pemilik (dalam ribuan USD) |

---

## 🛠️ Tech Stack

* **Python**
* **Pandas & NumPy** — Data manipulation & numerical analysis
* **Matplotlib & Seaborn** — Visualisasi data, korelasi heatmap, dan residual plot
* **Scikit-Learn** — Machine Learning (`train_test_split`, `LinearRegression`, & evaluation metrics)

---

## 🔎 Analysis Workflow

```text
Boston Housing Dataset
          │
          ▼
    Data Loading
          │
          ▼
Data Cleaning & Profiling
          │
          ├── Missing Values Check
          ├── Outlier Detection
          └── Summary Statistics
          │
          ▼
   Correlation Analysis
          │
          ├── Feature Selection
          └── Target Distribution (MEDV)
          │
          ▼
  Train / Test Split
          │
          ▼
    Model Training
          │
          ▼
 Performance Evaluation
          │
          ├── MAE, RMSE, R² Score
          └── Residual Analysis

```

---

## 🧹 1. Exploratory Data Analysis & Cleaning

Sebelum melatih model, data dieksplorasi untuk memastikan tidak ada anomali atau data yang hilang.

### Check Data Integrity

```python
df.info()
df.isnull().sum().sum()

```

### Insights

* Semua fitur berjenis numerik (`float64` dan `int64`), sehingga tidak memerlukan proses *categorical encoding*.
* Tidak ditemukan *missing values* (0 null values), sehingga dataset siap diproses.
* Terdeteksi adanya kondisi *data censoring/capping* pada variabel target (`MEDV`) di angka **50.0 (ribuan USD)**.

---

## 📊 2. Feature Correlation (EDA)

Analisis korelasi Pearson dilakukan untuk mengetahui fitur mana yang memiliki pengaruh paling signifikan terhadap harga rumah (`MEDV`).

```python
korelasi = df.corr(numeric_only=True)['MEDV'].sort_values(ascending=False)

```

### Insights

* **`RM` (+0.70):** Memiliki korelasi positif terkuat. Semakin banyak jumlah kamar pada rumah, semakin tinggi harga jualnya.
* **`LSTAT` (-0.74):** Memiliki korelasi negatif terkuat. Semakin tinggi persentase penduduk berpenghasilan rendah di wilayah tersebut, semakin rendah median harga rumahnya.
* **`PTRATIO` (-0.51) & `INDUS` (-0.48):** Rasio murid-guru yang tinggi serta tingginya proporsi kawasan industri berbanding terbalik dengan nilai properti.

---

## 🤖 3. Pemodelan Regresi Linear Multivariat

Membagi dataset menjadi **Data Latih (Train)** dan **Data Uji (Test)** dengan rasio 80:20 untuk menguji performa generalisasi model pada data baru.

```python
from sklearn.model_selection import train_test_split
from sklearn.linear_model import LinearRegression

X = df.drop('MEDV', axis=1)
y = df['MEDV']

X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=42)

model = LinearRegression()
model.fit(X_train, y_train)

y_pred = model.predict(X_test)

```

---

## 📏 4. Evaluasi Performa Model

Mengukur seberapa akurat prediksi model terhadap data uji asli menggunakan metrik evaluasi regresi.

```python
from sklearn.metrics import mean_absolute_error, mean_squared_error, r2_score
import numpy as np

print("MAE:", mean_absolute_error(y_test, y_pred))
print("RMSE:", np.sqrt(mean_squared_error(y_test, y_pred)))
print("R2 Score:", r2_score(y_test, y_pred))

```

### Metric Insights

* **MAE (Mean Absolute Error):** Mengukur rata-rata kesalahan mutlak prediksi harga dalam satuan ribuan USD.
* **RMSE (Root Mean Squared Error):** Memberikan penalti bobot lebih tinggi pada kesalahan prediksi yang berjarak jauh dari nilai riil.
* **R² Score:** Menunjukkan berapa persentase variansi harga rumah (`MEDV`) yang berhasil dijelaskan oleh kombinasi fitur $X$.

---

## 💡 Key Insights

### 1. Primary Price Drivers

Fitur `RM` (jumlah kamar) dan `LSTAT` (kondisi sosio-ekonomi penduduk) merupakan dua faktor paling dominan dalam menentukan estimasi nilai properti.

### 2. Multi-Factor Impact

Harga rumah tidak hanya dipengaruhi oleh kondisi fisik bangunan, tetapi juga oleh faktor lingkungan seperti tingkat kejahatan (`CRIM`) dan kualitas fasilitas pendidikan setempat (`PTRATIO`).

### 3. Baseline Model Capability

Regresi Linear Multivariat mampu menjadi *baseline model* yang kuat dan transparan, memberikan interpretasi koefisien yang jelas untuk setiap penambahan unit fitur.

---

## 💼 Business Application

Penerapan model ini dalam ekosistem industri Real Estat:

```text
Property Features (X) -> Machine Learning Model -> Predict Valuation (y) -> Pricing & Investment Strategy

```

* **Real Estate Valuation:** Membantu agen dan investor memperkirakan nilai pasar wajar (*fair market value*) suatu properti secara otomatis.
* **Urban Development:** Membantu perencana kota memahami bagaimana peningkatatan kualitas lingkungan/pendidikan berdampak langsung pada nilai ekonomi kawasan.

---

## ⚠️ Data & Analysis Pitfalls

* **Target Capping (Extreme Values):** Batas nilai maksimum $50.0k pada target `MEDV` dapat memicu bias prediksi pada properti kelas *luxury*.
* **Multicollinearity:** Beberapa fitur independen (seperti `RAD` dan `TAX`) memiliki interkorelasi yang tinggi, yang berpotensi memengaruhi kestabilan koefisien regresi.

---

## 📚 Learning Outcomes

Melalui proyek ini, beberapa konsep yang dipraktikkan:

* Pemahaman & Penerapan Regresi Linear Multivariat
* Feature Selection berbasis Korelasi
* Pencegahan Data Leakage via Train-Test Split
* Interpretasi Koefisien Model Regresi
* Evaluasi Performa Model (MAE, RMSE, R²)

---

## 🚀 Next Step

```text
Simple Linear Regression
        ↓
Multiple Linear Regression
        ↓
Feature Engineering & Scaling
        ↓
Regularized Regression (Ridge / Lasso)
        ↓
Tree-Based Models (Random Forest / XGBoost)

```

Eksperimen berikutnya dapat melibatkannya teknik **Feature Scaling** (StandardScaler) serta penggunaan **Ridge/Lasso Regression** untuk mengatasi multikolinearitas dan mengecek peningkatan nilai R² Score.

---

## 📁 Project Structure

```text
Day_20_boston_housing_prediction/
│
├── Day_20_boston_housing_prediction.ipynb
└── README.md

```

---

## 👤 Author

**Arief Wicaksono**

Aspiring AI Engineer | Data Science Student

---

⭐ If you find this project useful, feel free to star the repository!

```

```