# 🚗 Day 28 — Mini Project Regresi End-to-End: Prediksi Harga Mobil Bekas

Project ini merupakan **penutup modul regresi** yang menggabungkan beberapa konsep yang telah dipelajari sebelumnya menjadi satu workflow Machine Learning end-to-end.

Fokus utama project adalah mengubah **data mobil bekas dunia nyata yang masih kotor** menjadi model regresi yang dapat digunakan untuk memprediksi harga, dengan seluruh proses preprocessing dan modeling dibungkus menggunakan **`Pipeline`** dan **`ColumnTransformer`** dari scikit-learn.

---

## 🎯 Project Objectives

Tujuan pembelajaran project ini adalah:

- Membersihkan data dunia nyata yang memiliki format tidak konsisten.
- Melakukan konversi data numerik yang masih tersimpan sebagai teks.
- Melakukan **feature engineering** dengan membuat fitur `Age`.
- Melakukan exploratory data analysis untuk memahami pola harga.
- Memisahkan fitur numerik dan kategorikal.
- Menggunakan **`ColumnTransformer`** untuk preprocessing berbeda pada setiap tipe fitur.
- Menggunakan **`Pipeline`** untuk menggabungkan preprocessing dan model.
- Mencegah data leakage selama preprocessing dan cross-validation.
- Melatih **Random Forest Regressor** untuk prediksi harga mobil.
- Mengevaluasi model menggunakan MAE, RMSE, dan R².
- Menggunakan **5-Fold Cross-Validation** untuk melihat kestabilan performa.
- Membandingkan harga aktual dan harga prediksi.
- Menyimpan hasil prediksi beserta selisih antara harga aktual dan prediksi.

---

## 📂 Dataset

Dataset yang digunakan adalah:

**Car Price Prediction Challenge**

📌 Source: [Kaggle — Car Price Prediction Challenge](https://www.kaggle.com/datasets/deepcontractor/car-price-prediction-challenge)

Dataset awal terdiri dari:

- **19,237 baris**
- **18 kolom**

Target:

```text
Price
```

### Important Features

| Feature | Description |
|---|---|
| `Price` | Harga mobil — target |
| `Levy` | Nilai levy/pajak kendaraan |
| `Manufacturer` | Produsen kendaraan |
| `Model` | Model kendaraan |
| `Prod. year` | Tahun produksi |
| `Category` | Kategori kendaraan |
| `Leather interior` | Status interior kulit |
| `Fuel type` | Jenis bahan bakar |
| `Engine volume` | Kapasitas mesin |
| `Mileage` | Jarak tempuh |
| `Cylinders` | Jumlah silinder |
| `Gear box type` | Jenis transmisi |
| `Drive wheels` | Jenis penggerak |
| `Airbags` | Jumlah airbags |

---

## 🛠️ Tech Stack

Project ini menggunakan:

- **Python**
- **Pandas** — Data manipulation
- **NumPy** — Numerical computation
- **Matplotlib** — Visualization
- **Seaborn** — Data visualization
- **Scikit-learn** — Preprocessing, pipeline, modeling, dan evaluation

Komponen utama scikit-learn:

- `Pipeline`
- `ColumnTransformer`
- `SimpleImputer`
- `StandardScaler`
- `OneHotEncoder`
- `RandomForestRegressor`
- `train_test_split`
- `KFold`
- `cross_val_score`

---

# 🔎 End-to-End Workflow

```text
Raw Dataset
     │
     ▼
Data Understanding
     │
     ▼
Data Cleaning
     │
     ├── Levy
     ├── Mileage
     └── Engine Volume
     │
     ▼
Feature Engineering
     │
     └── Age
     │
     ▼
Outlier Handling
     │
     ▼
Exploratory Data Analysis
     │
     ├── Price Distribution
     ├── Age vs Price
     └── Fuel Type vs Price
     │
     ▼
Feature Selection
     │
     ├── Numerical Features
     └── Categorical Features
     │
     ▼
Train-Test Split
     │
     ▼
Pipeline + ColumnTransformer
     │
     ├── Numerical Preprocessing
     │     ├── Median Imputation
     │     └── StandardScaler
     │
     └── Categorical Preprocessing
           ├── Most Frequent Imputation
           └── One-Hot Encoding
     │
     ▼
Random Forest Regressor
     │
     ▼
Model Evaluation
     │
     ├── MAE
     ├── RMSE
     └── R²
     │
     ▼
5-Fold Cross-Validation
     │
     ▼
Prediction vs Actual
     │
     ▼
Prediction Result
```

---

# 🧹 1. Data Cleaning

Dataset memiliki beberapa kolom yang secara konsep merupakan numerik tetapi tersimpan sebagai teks karena mengandung karakter tambahan.

### `Levy`

Kolom `Levy` memiliki nilai:

```text
-
```

Nilai tersebut diganti menjadi `0`, kemudian dikonversi menjadi numerik.

```python
data['Levy'] = pd.to_numeric(
    data['Levy'].replace('-', 0),
    errors='coerce'
).fillna(0)
```

### `Mileage`

Nilai mileage memiliki satuan:

```text
186005 km
192000 km
200000 km
```

Satuan `km` dihapus dan nilai kemudian dikonversi menjadi numerik.

### `Engine volume`

Beberapa nilai memiliki informasi:

```text
2.0 Turbo
```

Informasi `Turbo` dipisahkan menjadi fitur baru:

```text
Turbo
```

Kemudian teks `Turbo` dihapus dari `Engine volume` sehingga kapasitas mesin dapat digunakan sebagai fitur numerik.

---

# ⚙️ 2. Feature Engineering

Feature engineering dilakukan dengan membuat:

```text
Age = 2020 - Prod. year
```

`Age` digunakan sebagai representasi usia kendaraan.

Secara intuitif, usia kendaraan relevan terhadap harga karena mobil yang lebih tua cenderung mengalami depresiasi.

Selain itu, informasi `Turbo` yang sebelumnya berada di dalam `Engine volume` dipisahkan menjadi fitur binary.

---

# 🗑️ 3. Outlier Handling

Harga mobil memiliki beberapa nilai ekstrem.

Contohnya, pada statistik awal ditemukan nilai harga yang sangat rendah hingga sangat tinggi.

Untuk mengurangi pengaruh outlier ekstrem, digunakan batas:

```text
1st percentile
        ↓
Price
        ↓
99th percentile
```

Proses tersebut mengurangi dataset dari:

```text
19,237 rows
```

menjadi:

```text
18,864 rows
```

Kemudian harga di bawah:

```text
100 USD
```

dikeluarkan.

Rentang harga setelah filtering:

```text
100 – 84,675 USD
```

Tujuannya adalah membuat target lebih representatif terhadap harga kendaraan yang digunakan untuk modeling.

---

# 📊 4. Exploratory Data Analysis

## A. Distribusi Harga

Distribusi `Price` menunjukkan pola:

> **Right-skewed**

Artinya sebagian besar mobil berada pada rentang harga yang lebih rendah, sementara sebagian kecil mobil memiliki harga jauh lebih tinggi.

---

## B. Age vs Price

Scatter plot antara `Age` dan `Price` menunjukkan tren menurun.

Secara umum:

> Semakin tua mobil, harga cenderung semakin rendah.

Namun hubungan tersebut tidak sepenuhnya linear.

Depresiasi juga terlihat dapat melambat pada kendaraan yang sangat tua atau memiliki karakteristik tertentu.

Hal ini menjadi salah satu alasan penggunaan model berbasis tree yang mampu menangkap hubungan non-linear.

---

## C. Fuel Type vs Price

Rata-rata harga dianalisis berdasarkan `Fuel type`.

Pada dataset ini:

- **Diesel** dan **Plug-in Hybrid** memiliki rata-rata harga relatif tinggi.
- **CNG** memiliki rata-rata harga paling rendah, sekitar **8.5 ribu USD**.

Perbedaan tersebut menunjukkan bahwa fitur kategorikal seperti `Fuel type` berpotensi memberikan informasi penting bagi model.

---

# 🧩 5. Feature Selection

Fitur numerik yang digunakan:

```python
fit_num = [
    "Levy",
    "Engine volume",
    "Mileage",
    "Cylinders",
    "Airbags",
    "Age"
]
```

Fitur kategorikal:

```python
fit_cat = [
    "Manufacturer",
    "Category",
    "Leather interior",
    "Fuel type",
    "Gear box type",
    "Drive wheels"
]
```

Fitur berikut tidak digunakan:

- `ID` — identifier.
- `Price` — target.
- `Prod. year` — sudah direpresentasikan melalui `Age`.
- `Model` — memiliki terlalu banyak kategori sehingga berpotensi menghasilkan jumlah fitur encoding yang sangat besar.

### Manufacturer Grouping

Kategori `Manufacturer` dibatasi pada **10 manufacturer dengan jumlah data terbanyak**.

Manufacturer lainnya dikelompokkan menjadi:

```text
Lain
```

Tujuannya adalah mengontrol jumlah kategori sebelum One-Hot Encoding.

---

# 🔧 6. Pipeline & ColumnTransformer

Bagian utama project ini adalah penggunaan:

```text
Pipeline
+
ColumnTransformer
```

Pipeline menyatukan seluruh proses preprocessing dan modeling menjadi satu objek.

### Numerical Pipeline

Fitur numerik diproses dengan:

```text
SimpleImputer(strategy="median")
        ↓
StandardScaler
```

### Categorical Pipeline

Fitur kategorikal diproses dengan:

```text
SimpleImputer(strategy="most_frequent")
        ↓
OneHotEncoder(handle_unknown="ignore")
```

Kemudian kedua preprocessing tersebut digabungkan menggunakan:

```python
ColumnTransformer
```

dan diteruskan ke:

```python
RandomForestRegressor(
    n_estimators=120,
    random_state=42,
    n_jobs=1
)
```

Struktur akhirnya:

```text
Input Data
    │
    ▼
ColumnTransformer
    │
    ├── Numerical Pipeline
    │       ├── Imputer
    │       └── StandardScaler
    │
    └── Categorical Pipeline
            ├── Imputer
            └── OneHotEncoder
    │
    ▼
RandomForestRegressor
    │
    ▼
Prediction
```

### Why Pipeline?

Penggunaan Pipeline membuat preprocessing dan model:

- Konsisten.
- Reusable.
- Lebih mudah dipelihara.
- Dapat digunakan kembali saat inference.
- Lebih aman terhadap **data leakage**, karena preprocessing dipelajari dari data training dalam proses fitting.

---

# ✂️ 7. Train-Test Split

Dataset dibagi menjadi:

```text
80% Training
20% Testing
```

dengan:

```python
random_state = 42
```

Data training digunakan untuk melatih pipeline, sedangkan data testing digunakan untuk mengevaluasi kemampuan generalisasi model terhadap data yang belum dilihat.

---

# 🌲 8. Random Forest Regression

Model utama yang digunakan adalah:

```python
RandomForestRegressor(
    n_estimators=120,
    random_state=42,
    n_jobs=1
)
```

Random Forest merupakan ensemble model yang menggabungkan banyak Decision Tree.

Dengan menggunakan banyak tree, model dapat menangkap pola non-linear dan mengurangi ketergantungan terhadap satu pohon keputusan.

---

# 📈 9. Model Evaluation

Model dievaluasi menggunakan tiga metrik:

### MAE — Mean Absolute Error

Mengukur rata-rata selisih absolut antara harga aktual dan prediksi.

Semakin kecil nilai MAE:

> semakin kecil rata-rata error prediksi.

### RMSE — Root Mean Squared Error

Memberikan penalti lebih besar terhadap error yang besar.

Semakin kecil:

> semakin baik.

### R² — Coefficient of Determination

Mengukur proporsi variasi target yang dapat dijelaskan oleh model.

Semakin mendekati `1`:

> semakin baik.

---

## 📊 Test Set Results

Hasil evaluasi pada data testing:

| Metric | Result |
|---|---:|
| MAE | **3,746.91 USD** |
| RMSE | **6,899.54 USD** |
| R² | **0.79** |

### Interpretation

**R² = 0.79**

Model mampu menjelaskan sekitar **79% variasi harga** pada data testing.

**MAE = 3,746.91 USD**

Secara rata-rata, prediksi model berbeda sekitar **3,746.91 USD** dari harga aktual.

**RMSE = 6,899.54 USD**

Nilai RMSE yang lebih tinggi dibandingkan MAE menunjukkan adanya beberapa prediksi dengan error yang relatif besar.

Error besar tersebut terlihat terutama pada kendaraan dengan harga tinggi.

---

# 🔄 10. Cross-Validation

Untuk melihat apakah performa model stabil pada subset data yang berbeda, digunakan:

```python
KFold(
    n_splits=5,
    shuffle=True,
    random_state=42
)
```

Metric yang digunakan:

```text
RMSE
```

### RMSE per Fold

| Fold | RMSE |
|---|---:|
| 1 | 6,880.44 |
| 2 | 6,990.88 |
| 3 | 7,104.73 |
| 4 | 6,620.24 |
| 5 | 7,182.09 |

### Mean RMSE

```text
6,955.68 USD
```

Rentang RMSE antar-fold relatif berdekatan, yaitu sekitar:

```text
6,620 – 7,182 USD
```

Hal ini menunjukkan bahwa performa model relatif konsisten ketika dievaluasi pada beberapa pembagian data.

> Cross-validation memberikan gambaran kestabilan performa yang lebih baik daripada hanya mengandalkan satu train-test split.

---

# 🎯 11. Prediction vs Actual

Visualisasi prediction vs actual digunakan untuk melihat seberapa dekat hasil prediksi dengan harga sebenarnya.

Garis diagonal:

```text
Predicted Price = Actual Price
```

merepresentasikan prediksi ideal.

Sebagian besar titik berada cukup dekat dengan garis tersebut.

Namun, pada kendaraan dengan harga tinggi, penyebaran error menjadi lebih besar dan model cenderung melakukan **underestimation**.

Terutama pada harga:

```text
> 40,000 USD
```

prediksi terlihat lebih sering berada di bawah harga aktual.

---

# 📋 12. Prediction Result

Hasil prediksi disimpan dalam DataFrame yang berisi:

```text
Manufacturer
Age
Mileage
Harga Asli
Harga Prediksi
Selisih
```

Contoh:

| Manufacturer | Age | Mileage | Harga Asli | Harga Prediksi | Selisih |
|---|---:|---:|---:|---:|---:|
| HYUNDAI | 4 | 60,604 | 45,210 | 44,297 | -913 |
| TOYOTA | 6 | 166,560 | 39,201 | 35,929 | -3,272 |
| LEXUS | 5 | 143,619 | 13,956 | 13,730 | -226 |
| HYUNDAI | 5 | 110,000 | 48,403 | 43,896 | -4,507 |
| HYUNDAI | 4 | 331,445 | 11,290 | 11,784 | +494 |

Kolom:

```text
Selisih = Harga Prediksi - Harga Asli
```

digunakan untuk melihat arah dan besar kesalahan prediksi pada masing-masing kendaraan.

---

# 💡 Key Insights

### 1. Real-World Data Is Often Messy

Data dunia nyata tidak selalu langsung siap untuk machine learning.

Contohnya:

```text
Levy          → "-"
Mileage       → "186005 km"
Engine volume → "2.0 Turbo"
```

Data tersebut harus dibersihkan sebelum digunakan oleh model.

### 2. Feature Engineering Matters

Fitur sederhana seperti:

```text
Age
```

dapat memberikan representasi yang lebih relevan terhadap harga kendaraan dibandingkan hanya menggunakan `Prod. year`.

### 3. Pipeline Reduces Data Leakage Risk

Dengan membungkus preprocessing dan model dalam satu Pipeline, proses:

```text
Imputation
+
Scaling
+
Encoding
+
Model
```

menjadi satu workflow yang konsisten.

Saat cross-validation dilakukan, preprocessing juga berada di dalam pipeline sehingga parameter preprocessing dapat dipelajari pada data training masing-masing fold.

### 4. Random Forest Handles Non-Linear Relationships

Hubungan antara usia kendaraan dan harga tidak sepenuhnya linear.

Random Forest dapat menangkap pola non-linear tersebut tanpa harus menentukan bentuk persamaan secara manual.

### 5. Model Performance Is Relatively Stable

Hasil 5-Fold Cross-Validation menghasilkan RMSE rata-rata:

```text
6,955.68 USD
```

dengan rentang:

```text
6,620.24 – 7,182.09 USD
```

yang menunjukkan performa relatif stabil antar-fold.

---

# ⚠️ Limitations

Project ini masih merupakan eksperimen pembelajaran dan belum merupakan model production-ready.

Beberapa keterbatasan:

- Hanya menggunakan satu konfigurasi Random Forest.
- Belum dilakukan systematic hyperparameter tuning.
- Belum dilakukan comparison dengan model regresi lain pada workflow end-to-end ini.
- Feature `Model` tidak digunakan karena cardinality tinggi.
- Harga kendaraan dapat dipengaruhi faktor lain yang tidak tersedia pada dataset.
- Performa pada kendaraan dengan harga tinggi masih memiliki error yang lebih besar.
- Belum dilakukan deployment dan monitoring model.

---

# 🚀 Production Perspective

Workflow pada project ini sudah mulai mendekati pola Machine Learning pipeline yang reusable:

```text
Raw Data
   ↓
Data Validation
   ↓
Preprocessing
   ↓
Feature Transformation
   ↓
Model
   ↓
Prediction
   ↓
Evaluation
```

Penggunaan:

```text
Pipeline
+
ColumnTransformer
```

juga membuat preprocessing dan model dapat dipaketkan sebagai satu objek yang lebih mudah digunakan kembali pada inference.

Tahap selanjutnya apabila dikembangkan menjadi sistem production dapat mencakup:

```text
Model Validation
      ↓
Experiment Tracking
      ↓
Model Versioning
      ↓
Model Serialization
      ↓
API / Model Serving
      ↓
Monitoring
      ↓
Retraining
```

---

# 📚 Learning Outcomes

Melalui project ini, beberapa konsep yang dipraktikkan:

- End-to-End Regression Workflow
- Data Cleaning
- Data Type Conversion
- Feature Engineering
- Outlier Handling
- Exploratory Data Analysis
- Feature Selection
- Numerical Preprocessing
- Categorical Preprocessing
- One-Hot Encoding
- StandardScaler
- SimpleImputer
- ColumnTransformer
- Pipeline
- Random Forest Regression
- Train-Test Split
- MAE
- RMSE
- R²
- K-Fold Cross-Validation
- Prediction vs Actual Analysis
- Error Analysis
- Data Leakage Prevention
- Reusable Machine Learning Workflow

---

# 📁 Project Structure

```text
Day_28_mini_project_regression_end_to_end_prediction_used_car_prices/
│
├── Day_28_mini_project_regression_end_to_end_prediction_used_car_prices.ipynb
└── README.md
```

---

## 👤 Author

**Arief Wicaksono**

Aspiring AI Engineer | Data Science Student

---

⭐ If you find this project useful, feel free to star the repository!
