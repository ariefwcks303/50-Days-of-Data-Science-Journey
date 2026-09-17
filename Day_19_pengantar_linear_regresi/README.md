
# 🍷 Day 19 — Pengantar Machine Learning & Linear Regression

Project ini merupakan langkah pertama masuk ke dunia **Machine Learning** dengan fokus pada **Supervised Learning** dan algoritma **Regresi Linear** menggunakan **Scikit-Learn**.

Analisis mencakup **eksplorasi data, pemilihan fitur (X) dan target (y), pemisahan data (train/test split), pelatihan model, serta evaluasi performa model** untuk memprediksi kualitas anggur merah berdasarkan karakteristik kimianya.

---

## 🎯 Project Objectives

Tujuan utama project ini adalah:

* Memahami konsep dasar **Supervised Learning**.
* Membedakan antara **Features (X)** dan **Target (y)**.
* Melakukan instalasi dan ekstraksi dataset langsung dari Kaggle.
* Memeriksa dan membersihkan data numerik.
* Menganalisis korelasi antar fitur kimia dengan kualitas wine.
* Membagi dataset menggunakan `train_test_split`.
* Melatih model **Regresi Linear** sederhana.
* Mengevaluasi model menggunakan metrik **MAE, RMSE, dan R²**.

---

## 📂 Dataset

Dataset yang digunakan adalah:

**Red Wine Quality**

📌 Source: [Kaggle — uciml/red-wine-quality-cortez-et-al-2009](https://www.kaggle.com/datasets/uciml/red-wine-quality-cortez-et-al-2009)

Dataset berisi sekitar:

* **1,599 sampel pengujian**
* **12 kolom**
* Setiap baris merepresentasikan karakteristik kimia dari satu sampel anggur merah.

### Important Features

| Feature | Description |
| --- | --- |
| `fixed acidity` | Tingkat keasaman tetap |
| `volatile acidity` | Keasaman volatil (kandungan cuka) |
| `citric acid` | Kandungan asam sitrat |
| `residual sugar` | Sisa gula setelah fermentasi |
| `chlorides` | Kandungan garam |
| `sulphates` | Kadar sulfat (aditif wine) |
| `alcohol` | Kadar alkohol (% volume) |
| `quality` | **Target**: Skor kualitas (0–10) |

Target prediksi:

```text
y = quality (Skor Numerik) -> Masalah Regresi

```

---

## 🛠️ Tech Stack

* **Python**
* **Pandas & NumPy** — Data manipulation
* **Matplotlib & Seaborn** — Data visualization
* **Scikit-Learn** — Machine Learning (Model & Evaluation)

---

## 🔎 Analysis Workflow

```text
Red Wine Dataset
       │
       ▼
 Data Exploration
       │
       ├── Check Missing Values
       ├── Data Distribution
       └── Target Analysis (Quality)
       │
       ▼
 Feature Selection (EDA)
       │
       ├── Correlation Barplot
       ├── Scatter Plot (Alcohol vs Quality)
       └── Heatmap Correlation
       │
       ▼
 Data Preprocessing
       │
       └── Train/Test Split (80:20)
       │
       ▼
 Model Training
       │
       └── Linear Regression
       │
       ▼
 Model Evaluation
       │
       ├── R-Squared (R²)
       ├── Mean Absolute Error (MAE)
       └── Root Mean Squared Error (RMSE)

```

---

# 🧹 1. Data Exploration & Cleaning

Sebelum melatih model, data dieksplorasi untuk memastikan tidak ada data yang anomali atau kosong.

### Import Dataset

Dataset diunduh langsung dari Kaggle API:

```bash
!pip install -q kaggle
!kaggle datasets download -d uciml/red-wine-quality-cortez-et-al-2009
!unzip -q /content/red-wine-quality-cortez-et-al-2009.zip

```

### Check Data Integrity

Mengecek nilai kosong (null) dan ringkasan statistik:

```python
df.info()
df.isnull().sum().sum()

```

**Insight:**

* Semua data sudah dalam bentuk **numerik** (float/int), sehingga tidak perlu *categorical encoding*.
* Terdapat **0 missing values**, data sangat bersih dan siap diproses.
* Target `quality` memiliki rentang nilai 3 hingga 8, mayoritas sampel berada di skor menengah (5 dan 6).

---

# 📊 2. Feature Correlation (EDA)

Untuk mencari tahu komposisi kimia apa yang paling mempengaruhi kualitas wine, dilakukan analisis korelasi.

### Korelasi Terhadap Target

Menghitung korelasi fitur kimia terhadap `quality`:

```python
korelasi = df.corr(numeric_only=True)['quality'].sort_values(ascending=False)

```

### Insight

* **Alcohol (+0.48)**: Memiliki korelasi **positif tertinggi**. Semakin tinggi kadar alkohol, skor wine cenderung semakin baik.
* **Sulphates (+0.25)**: Memiliki pengaruh positif kedua terbesar.
* **Volatile Acidity (-0.39)**: Memiliki korelasi **negatif terdalam**. Semakin tinggi keasaman volatil (terasa seperti cuka), kualitas wine semakin menurun tajam.

---

# 🤖 3. Supervised Learning: Train/Test Split

Dalam Supervised Learning, kita membagi data menjadi:

* **`X` (Features)** = Variabel input (karakteristik kimiawi).
* **`y` (Target)** = Variabel output yang ingin ditebak (`quality`).

```python
X = df.drop('quality', axis=1)
y = df['quality']

X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=42)

```

### Mengapa Train/Test Split Penting?

Pemisahan ini vital agar model **benar-benar belajar mengenali pola** pada Data Latih (*Train*), dan bisa diuji validitasnya pada Data Uji (*Test*) yang sama sekali belum pernah ia lihat, sehingga mencegah model sekadar menghafal (Overfitting).

---

# 📈 4. Linear Regression Modeling

Melatih model Regresi Linear menggunakan fitur-fitur yang ada untuk memprediksi `quality`.

```python
model = LinearRegression()
model.fit(X_train, y_train)

y_pred = model.predict(X_test)

```

Model secara matematis akan mencari garis tren terbaik (Linear) yang merepresentasikan hubungan logis antara kandungan kimia (`X`) dengan skor akhir wine (`y`).

---

# 📏 5. Model Evaluation

Untuk mengukur seberapa jauh tebakan (prediksi) model dari nilai aslinya, metrik evaluasi regresi dihitung.

```python
print("MAE:", mean_absolute_error(y_test, y_pred))
print("RMSE:", np.sqrt(mean_squared_error(y_test, y_pred)))
print("R2 Score:", r2_score(y_test, y_pred))

```

### Metrik Insight

* **MAE (Mean Absolute Error)**: Mengukur rata-rata selisih mutlak prediksi model.
* **RMSE (Root Mean Squared Error)**: Memberikan penalti/bobot error lebih besar pada tebakan yang meleset terlalu ekstrem.
* **R² (R-Squared)**: Menunjukkan persentase seberapa baik fitur (`X`) yang kita pilih mampu menjelaskan variabilitas dari target (`y`).

---

# 💡 Key Insights

### 1. Alcohol is the Key Driver

Kadar alkohol bertindak sebagai prediktor terbaik (berpengaruh positif) untuk kualitas anggur merah dalam dataset ini.

### 2. Hindari Volatile Acidity (Cuka)

Kandungan asam volatil yang terlampau tinggi akan merusak cita rasa wine dan menjatuhkan skor kualitasnya secara drastis.

### 3. Baseline Sempurna

Kondisi dataset yang 100% numerik dan terdistribusi cukup baik membuat Regresi Linear menjadi batu loncatan (baseline model) yang sangat efisien tanpa perlu proses *preprocessing* yang berat.

### 4. Regression vs Classification

Walau target `quality` tampak seperti kelas (3, 4, 5, dsb), pendekatannya lebih organik menggunakan regresi karena skor tersebut merupakan besaran angka berurutan yang saling hierarkis.

---

# 💼 Business Insight

Penerapan algoritma Regresi Linear ini dalam ekosistem industri Wine:

```text
Wine Production
      ↓
Chemical Analysis (Input X)
      ↓
Machine Learning Model
      ↓
Predict Quality Score (Output y)
      ↓
Quality Control & Pricing
      ├── Premium Label (High Quality Prediction)
      └── Formulation Adjustment (Low Quality Prediction)

```

Dengan model ini, para *winemaker* bisa **memprediksi kualitas** akhir sejak tahap awal uji lab, sehingga memungkinkan kalibrasi komposisi (misal: menekan *volatile acidity* atau *adjust* *sulphates*) demi mempertahankan standar tinggi produk sebelum dilempar ke pasar.

---

# ⚠️ Data & Analysis Pitfalls

### Imbalanced Extreme Scores

Jumlah sampel wine dengan skor sangat ekstrem (kualitas 3 atau 8) jumlahnya sedikit. Ketidakseimbangan ini bisa membuat model kesulitan mereplikasi/menebak nilai-nilai langka tersebut.

### Linear Limitations

Algoritma ini terikat pada asumsi hubungan linier. Jika ternyata relasi antara bahan kimia dan rasa wine memiliki batasan kompleks/polinomial (misal: terlalu banyak alkohol juga merusak kualitas), model dasar ini bisa underfitting.

---

# 📚 Learning Outcomes

Melalui project ini, beberapa konsep yang dipraktikkan:

* Pengantar Supervised Learning
* Konsep Fitur (X) & Target (y)
* Train-Test Split (Data Leakage Prevention)
* Eksplorasi Data Berbasis Korelasi (EDA)
* Pemanfaatan Library Scikit-Learn
* Penggunaan `LinearRegression()`
* Siklus Model (`.fit()` dan `.predict()`)
* Evaluasi Model Regresi (MAE, RMSE, R-Squared)
* Baseline Modeling Mindset

---

# 🚀 Next Step

Setelah fundamental ini dikuasai, alur selanjutnya:

```text
Simple Linear Regression
       ↓
Multiple Linear Regression & Feature Scaling
       ↓
Classification Problem 
(Logistic Regression)
       ↓
Tree-Based Models
(Decision Tree / Random Forest)

```

Jika akurasi regresi dasar dirasa masih kurang tajam, eksperimen bisa dilanjutkan dengan menggunakan *StandardScaler* atau bahkan merubah masalah ini menjadi **Klasifikasi Biner** (Kualitas Bagus vs Buruk).

---

## 📁 Project Structure

```text
Day_19_intro_machine_learning/
│
├── Day_19_pengantar_ML_linear_regression.ipynb
└── README.md

```

---

## 👤 Author

**Arief Wicaksono**

Aspiring AI Engineer | Data Science Student

```

```