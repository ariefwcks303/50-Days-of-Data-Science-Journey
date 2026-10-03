# 🚗 Day 25 — Decision Tree & Random Forest Regressor

Project ini merupakan implementasi **Regression Machine Learning** untuk memprediksi harga mobil bekas menggunakan **Decision Tree Regressor** dan **Random Forest Regressor**.

Project berfokus pada bagaimana data mentah yang masih memiliki format tidak konsisten dapat dibersihkan dan diubah menjadi fitur yang dapat digunakan untuk membangun model regresi.

Selain membandingkan dua model berbasis tree, project ini juga mengevaluasi **feature importance** untuk memahami fitur yang paling berkontribusi terhadap prediksi harga.

---

## 🎯 Project Objectives

Tujuan utama project ini adalah:

* Memahami karakteristik dataset harga mobil bekas.
* Melakukan data cleaning pada fitur yang memiliki format tidak konsisten.
* Melakukan feature engineering seperti `Age` dan `Turbo`.
* Menghapus fitur yang tidak relevan atau berpotensi menghasilkan terlalu banyak kategori.
* Melakukan One-Hot Encoding pada fitur kategorikal.
* Membandingkan **Decision Tree Regressor** dan **Random Forest Regressor**.
* Mengevaluasi performa model menggunakan **MAE, R², dan RMSE**.
* Menganalisis feature importance dari Random Forest.
* Menghubungkan hasil model dengan potensi penggunaan bisnis sebagai **price estimator**.

---

## 📂 Dataset

Dataset yang digunakan adalah:

**Car Price Prediction Challenge**

📌 Source: [Kaggle — Car Price Prediction Challenge](https://www.kaggle.com/datasets/deepcontractor/car-price-prediction-challenge)

Dataset awal terdiri dari:

* **19,237 data mobil**
* **18 kolom**

Target yang digunakan:

```text
Price
```

### Important Features

| Feature            | Description                |
| ------------------ | -------------------------- |
| `Price`            | Harga mobil dalam USD      |
| `Levy`             | Nilai levy/pajak kendaraan |
| `Manufacturer`     | Produsen mobil             |
| `Model`            | Model kendaraan            |
| `Prod. year`       | Tahun produksi             |
| `Category`         | Kategori kendaraan         |
| `Leather interior` | Status interior kulit      |
| `Fuel type`        | Jenis bahan bakar          |
| `Engine volume`    | Kapasitas mesin            |
| `Mileage`          | Jarak tempuh               |
| `Cylinders`        | Jumlah silinder            |
| `Gear box type`    | Jenis transmisi            |
| `Drive wheels`     | Jenis penggerak            |
| `Wheel`            | Posisi kemudi              |
| `Airbags`          | Jumlah airbags             |

---

## 🛠️ Tech Stack

Project ini menggunakan:

* **Python**
* **Pandas** — Data manipulation
* **NumPy** — Numerical computation
* **Matplotlib** — Visualization
* **Seaborn** — Data visualization
* **Scikit-learn** — Machine Learning

Model yang digunakan:

* `DecisionTreeRegressor`
* `RandomForestRegressor`

Evaluation metrics:

* MAE
* R²
* RMSE

---

# 🔎 Analysis Workflow

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
     └── Engine volume
     │
     ▼
Feature Engineering
     │
     ├── Age
     └── Turbo
     │
     ▼
Feature Selection
     │
     ▼
One-Hot Encoding
     │
     ▼
Train-Test Split
     │
     ├───────────────┐
     ▼               ▼
Decision Tree   Random Forest
     │               │
     └───────┬───────┘
             ▼
        Model Evaluation
             │
             ├── MAE
             ├── R²
             └── RMSE
             │
             ▼
      Feature Importance
             │
             ▼
       Business Insight
```

---

# 🧹 1. Data Cleaning

Dataset memiliki beberapa kolom yang belum siap digunakan langsung untuk modeling.

### `Levy`

Nilai `-` dikonversi menjadi `0`, kemudian kolom diubah menjadi numerik.

### `Mileage`

Nilai mileage memiliki satuan:

```text
186005 km
192000 km
200000 km
```

Satuan `km` dihapus sehingga nilai dapat digunakan sebagai numerik.

### `Engine volume`

Beberapa nilai memiliki informasi `Turbo`, misalnya:

```text
2.0 Turbo
```

Informasi tersebut dipisahkan menjadi fitur baru:

```text
Turbo
```

dengan nilai:

```text
1 → Turbo
0 → Non-Turbo
```

Kemudian `Engine volume` dikonversi menjadi nilai numerik.

---

# ⚙️ 2. Feature Engineering

### Age

Usia kendaraan dibuat berdasarkan tahun produksi:

```python
Age = 2020 - Prod. year
```

Feature `Age` digunakan karena usia kendaraan lebih mudah diinterpretasikan dalam konteks harga mobil bekas dibandingkan hanya menggunakan tahun produksi.

### Turbo

Informasi Turbo yang sebelumnya berada di dalam `Engine volume` dipisahkan menjadi fitur binary:

```text
Turbo = 1 → memiliki Turbo
Turbo = 0 → tidak memiliki Turbo
```

---

# 🗑️ 3. Feature Removal & Outlier Handling

Beberapa fitur tidak digunakan dalam modeling:

### `ID`

Dihapus karena hanya merupakan identifier.

### `Prod. year`

Dihapus setelah feature `Age` dibuat.

### `Model`

Dihapus karena memiliki jumlah kategori yang sangat banyak dan dapat menghasilkan jumlah kolom encoding yang besar.

### Price Outlier

Untuk mengurangi pengaruh harga ekstrem, harga dibatasi pada rentang persentil **1%–99%**.

Kemudian harga di bawah:

```text
100 USD
```

juga dikeluarkan.

Setelah proses tersebut, dataset yang digunakan berjumlah:

```text
18,694 rows
```

dengan rentang harga:

```text
100 – 84,675 USD
```

---

# 📊 4. Exploratory Data Analysis

## A. Distribusi Harga

Distribusi `Price` menunjukkan pola yang **right-skewed**.

Sebagian besar mobil berada pada rentang harga yang lebih rendah, sedangkan hanya sebagian kecil kendaraan memiliki harga yang sangat tinggi.

---

## B. Age vs Price

Analisis hubungan antara `Age` dan `Price` menunjukkan pola menurun:

> Semakin tua kendaraan, harga cenderung semakin rendah.

Namun hubungan tersebut tidak sepenuhnya linear. Penurunan harga dapat berbeda pada kendaraan dengan usia yang berbeda.

---

## C. Fuel Type vs Price

Rata-rata harga berdasarkan `Fuel type` menunjukkan adanya perbedaan harga antar jenis bahan bakar.

Hal ini memberikan indikasi bahwa `Fuel type` dapat memberikan informasi yang berguna bagi model prediksi harga.

---

# 🔢 5. Feature Encoding

Fitur kategorikal yang digunakan:

```python
fit_cat = [
    "Manufacturer",
    "Category",
    "Leather interior",
    "Fuel type",
    "Gear box type",
    "Drive wheels",
    "Wheel"
]
```

Fitur numerik:

```python
fit_num = [
    "Levy",
    "Engine volume",
    "Mileage",
    "Cylinders",
    "Airbags",
    "Turbo",
    "Age"
]
```

Categorical features kemudian diubah menggunakan **One-Hot Encoding**:

```python
pd.get_dummies(
    data[fit_cat + fit_num],
    columns=fit_cat,
    drop_first=True
)
```

Hasil preprocessing:

```text
X shape = (18,694, 92)
y shape = (18,694,)
```

---

# ✂️ 6. Train-Test Split

Dataset dibagi menjadi:

```text
80% Training
20% Testing
```

dengan:

```python
random_state = 42
```

Hasil pembagian:

```text
Training : 14,955 rows
Testing  : 3,739 rows
```

---

# 🌳 7. Modeling

Dua model digunakan untuk membandingkan performa regresi berbasis decision tree.

## Decision Tree Regressor

```python
DecisionTreeRegressor(
    random_state=42,
    max_depth=12
)
```

Decision Tree menggunakan satu pohon keputusan untuk melakukan prediksi.

Kelebihannya sederhana dan mampu menangkap hubungan non-linear, tetapi single tree lebih rentan terhadap **overfitting**.

---

## Random Forest Regressor

```python
RandomForestRegressor(
    random_state=42,
    n_estimators=100,
    max_depth=12
)
```

Random Forest merupakan ensemble dari banyak Decision Tree.

Prediksi akhir diperoleh dengan menggabungkan prediksi dari banyak tree sehingga model cenderung lebih stabil dibandingkan satu Decision Tree.

---

# 📈 8. Model Evaluation

Model dievaluasi menggunakan:

### MAE — Mean Absolute Error

Mengukur rata-rata selisih absolut antara harga aktual dan harga prediksi.

Semakin kecil:

> semakin baik.

### R² — Coefficient of Determination

Mengukur seberapa besar variasi target yang dapat dijelaskan oleh model.

Semakin mendekati `1`:

> semakin baik.

### RMSE — Root Mean Squared Error

Memberikan penalti lebih besar terhadap error yang besar.

Semakin kecil:

> semakin baik.

---

## 📊 Results

| Model         |       MAE |    R² |      RMSE |
| ------------- | --------: | ----: | --------: |
| Decision Tree | 5,019.579 | 0.673 | 8,513.445 |
| Random Forest | 4,460.824 | 0.767 | 7,187.121 |

### Interpretation

Random Forest menghasilkan:

```text
R²   = 0.767
MAE  = 4,460.824 USD
RMSE = 7,187.121 USD
```

Sedangkan Decision Tree:

```text
R²   = 0.673
MAE  = 5,019.579 USD
RMSE = 8,513.445 USD
```

Pada eksperimen ini, Random Forest mampu menghasilkan error yang lebih rendah dan R² yang lebih tinggi dibandingkan Decision Tree.

Perbedaan tersebut menunjukkan bahwa ensemble beberapa decision tree dapat menghasilkan prediksi yang lebih stabil pada dataset ini.

> Hasil ini berlaku pada konfigurasi, preprocessing, dan train-test split yang digunakan dalam eksperimen ini; belum berarti Random Forest selalu lebih baik untuk seluruh dataset atau kondisi produksi.

---

# 🔍 9. Feature Importance

Random Forest menyediakan:

```python
rf.feature_importances_
```

untuk melihat kontribusi relatif fitur terhadap proses prediksi.

Beberapa fitur yang terlihat dominan dalam analisis antara lain:

* `Age`
* `Mileage`
* `Engine volume`
* `Levy`

Secara intuitif, fitur tersebut memang berkaitan dengan karakteristik kendaraan dan nilai jual kembali.

### Important Note

Feature importance menunjukkan kontribusi relatif terhadap prediksi model.

Hal ini **tidak berarti bahwa fitur tersebut merupakan penyebab langsung harga kendaraan**.

---

# 🎯 10. Prediction vs Actual

Visualisasi prediction vs actual digunakan untuk melihat seberapa dekat hasil prediksi terhadap nilai sebenarnya.

Sebagian besar prediksi berada relatif dekat dengan garis ideal:

```text
Predicted Price = Actual Price
```

Namun penyimpangan lebih besar terlihat pada kendaraan dengan harga tinggi.

Hal ini dapat terjadi karena jumlah observasi pada rentang harga tinggi relatif lebih sedikit dibandingkan kendaraan pada rentang harga yang lebih umum.

---

# 💡 Key Insights

Beberapa insight utama dari project:

### 1. Data Cleaning Matters

Data mentah tidak selalu siap digunakan untuk machine learning.

Contohnya:

```text
Levy          → "-"
Mileage       → "186005 km"
Engine volume → "2.0 Turbo"
```

Format tersebut perlu diproses terlebih dahulu sebelum dapat digunakan oleh model.

### 2. Feature Engineering Adds Context

Feature:

```text
Age
Turbo
```

memberikan representasi yang lebih sesuai untuk model dibandingkan hanya menggunakan data mentah.

### 3. Ensemble Can Improve Stability

Random Forest menghasilkan performa yang lebih baik daripada single Decision Tree pada eksperimen ini.

### 4. Tree-Based Models Do Not Require Feature Scaling

Berbeda dengan model seperti KNN atau beberapa algoritma berbasis jarak, Decision Tree dan Random Forest tidak membutuhkan standardization atau normalization untuk fitur numerik.

### 5. Feature Importance Helps Interpretation

Feature importance dapat digunakan sebagai alat interpretasi awal untuk memahami fitur yang paling banyak digunakan model dalam proses prediksi.

---

# 💼 Business Insight

Model ini memiliki potensi sebagai **initial price estimator** untuk bisnis mobil bekas.

Contoh penggunaan:

```text
Data Mobil
    ↓
Model Prediksi
    ↓
Estimated Price
    ↓
Dealer / Seller Decision Support
```

Dealer dapat menggunakan estimasi harga sebagai salah satu input ketika melakukan:

* Evaluasi harga beli kendaraan
* Estimasi harga jual
* Benchmark harga
* Initial vehicle valuation

Namun model belum seharusnya digunakan sebagai satu-satunya dasar keputusan harga tanpa validasi lebih lanjut terhadap kondisi pasar dan faktor kendaraan yang belum tersedia dalam dataset.

---

# ⚠️ Limitations

Beberapa keterbatasan eksperimen:

* Evaluasi menggunakan satu train-test split.
* Belum menggunakan cross-validation.
* Belum dilakukan hyperparameter tuning secara sistematis.
* Feature importance bukan causal analysis.
* Kondisi kendaraan secara detail tidak tersedia dalam dataset.
* Harga pasar dapat berubah berdasarkan waktu dan lokasi.
* Random Forest yang digunakan masih merupakan baseline/model eksperimen, belum production-ready.

---

# 📚 Learning Outcomes

Melalui project ini, beberapa konsep yang dipraktikkan:

* Regression Problem
* Data Cleaning
* Data Type Conversion
* Feature Engineering
* Outlier Handling
* One-Hot Encoding
* Train-Test Split
* Decision Tree Regression
* Random Forest Regression
* Ensemble Learning
* MAE
* R²
* RMSE
* Feature Importance
* Prediction vs Actual Analysis
* Business Interpretation

---

## 🚀 Next Step

Tahap berikutnya dapat dikembangkan menuju:

```text
Baseline Model
      ↓
Cross-Validation
      ↓
Hyperparameter Tuning
      ↓
Model Comparison
      ↓
Feature Optimization
      ↓
Error Analysis
      ↓
Model Selection
      ↓
Deployment
```

Dengan demikian, eksperimen dapat berkembang dari sekadar membandingkan algoritma menjadi proses **model development yang lebih sistematis**.

---

## 📁 Project Structure

```text
Day_25_random_forest_regressor/
│
├── Day_25_random_forest_regressor.ipynb
└── README.md
```

---

## 👤 Author

**Arief Wicaksono**

Aspiring AI Engineer | Data Science Student

---

⭐ If you find this project useful, feel free to star the repository!
