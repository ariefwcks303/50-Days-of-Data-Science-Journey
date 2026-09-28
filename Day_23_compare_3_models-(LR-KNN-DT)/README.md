# 🤖 Day 23 — Perbandingan Model Regresi: Linear Regression vs KNN vs Decision Tree

Project ini membahas perbandingan tiga algoritma **regression** untuk memprediksi harga rumah menggunakan dataset **Boston Housing**.

Tiga model yang dibandingkan:

* **Linear Regression**
* **K-Nearest Neighbors (KNN) Regressor**
* **Decision Tree Regressor**

Ketiga model dilatih dan dievaluasi menggunakan **data testing yang sama** dengan metrik **R², MAE, dan RMSE**.

Fokus utama project adalah memahami bahwa **tidak ada satu algoritma yang selalu terbaik**. Pemilihan model perlu mempertimbangkan karakteristik data, asumsi model, preprocessing, kompleksitas, dan trade-off antara bias dan variance.

---

## 🎯 Project Objectives

Tujuan pembelajaran dalam project ini:

* Memahami perbedaan karakteristik beberapa algoritma regression.
* Melatih Linear Regression, KNN Regressor, dan Decision Tree Regressor.
* Membandingkan performa model menggunakan dataset dan test set yang sama.
* Memahami pentingnya **feature scaling untuk KNN**.
* Menerapkan `StandardScaler`.
* Memahami konsep **data leakage pada preprocessing**.
* Membandingkan model menggunakan:

  * R²
  * MAE
  * RMSE
* Memahami secara sederhana konsep **bias–variance**.
* Membandingkan hasil prediksi dengan nilai aktual.

---

# 📂 Dataset

Dataset yang digunakan adalah:

**Boston Housing Dataset**

📌 Source: [Kaggle — fedesoriano/the-boston-houseprice-data](https://www.kaggle.com/datasets/fedesoriano/the-boston-houseprice-data)

Dataset terdiri dari:

* **506 baris**
* **14 kolom**
* Seluruh fitur yang digunakan bersifat numerik.

### Important Features

| Feature   | Description                                      |
| --------- | ------------------------------------------------ |
| `RM`      | Rata-rata jumlah kamar per rumah                 |
| `LSTAT`   | Persentase penduduk berstatus ekonomi rendah     |
| `PTRATIO` | Rasio murid terhadap guru                        |
| `CRIM`    | Tingkat kriminalitas                             |
| `NOX`     | Konsentrasi polusi NOx                           |
| `MEDV`    | Harga median rumah dalam ribuan USD — **target** |

Fitur lainnya:

```text
ZN
INDUS
CHAS
AGE
DIS
RAD
TAX
B
```

Target yang diprediksi:

```text
MEDV
```

---

# 🛠️ Tech Stack

* **Python**
* **Pandas** — Data manipulation
* **NumPy** — Numerical computation
* **Matplotlib** — Data visualization
* **Seaborn** — Statistical visualization
* **Scikit-learn** — Machine learning & evaluation

### Models

```text
Linear Regression
K-Nearest Neighbors Regressor
Decision Tree Regressor
```

### Preprocessing

```text
StandardScaler
```

---

# 🔎 Machine Learning Workflow

```text
Boston Housing Dataset
          │
          ▼
    Data Exploration
          │
          ├── Dataset Information
          ├── Descriptive Statistics
          └── Missing Value Check
          │
          ▼
          EDA
          │
          ├── Correlation Heatmap
          ├── RM vs MEDV
          └── LSTAT vs MEDV
          │
          ▼
     Train/Test Split
          │
          ├── 80% Training
          └── 20% Testing
          │
          ▼
       Preprocessing
          │
          └── StandardScaler
                │
                ├── KNN → Scaled Data
                └── LR/DT → Original Scale
          │
          ▼
        Modeling
          │
          ├── Linear Regression
          ├── KNN
          └── Decision Tree
          │
          ▼
      Model Evaluation
          │
          ├── R²
          ├── MAE
          └── RMSE
          │
          ▼
     Model Comparison
          │
          ▼
   Prediction vs Actual
```

---

# 🔍 1. Exploratory Data Analysis

## Correlation Heatmap

Correlation heatmap digunakan untuk melihat hubungan antar fitur dan target `MEDV`.

### Insight

Dua hubungan yang paling terlihat:

* `RM` memiliki korelasi positif kuat dengan `MEDV`.
* `LSTAT` memiliki korelasi negatif kuat dengan `MEDV`.

---

## 🏠 RM vs MEDV

Scatter plot menunjukkan hubungan positif antara:

```text
RM → MEDV
```

Secara umum:

> semakin tinggi rata-rata jumlah kamar (`RM`), semakin tinggi pula harga median rumah (`MEDV`).

Pada nilai `RM` yang lebih tinggi, terdapat beberapa rumah dengan `MEDV` yang mendekati batas maksimum sekitar 50 pada dataset.

---

## 📉 LSTAT vs MEDV

Terdapat hubungan negatif yang kuat antara:

```text
LSTAT → MEDV
```

Secara umum:

> semakin tinggi persentase penduduk berstatus ekonomi rendah (`LSTAT`), semakin rendah harga median rumah (`MEDV`).

Ketika `LSTAT` berada di atas sekitar 20%, harga rumah dalam dataset lebih banyak terkonsentrasi pada nilai yang lebih rendah.

Visualisasi juga menunjukkan bahwa hubungan `LSTAT` dan `MEDV` tidak sepenuhnya linear, yang menjadi salah satu alasan menarik untuk membandingkan model linear dengan model non-linear.

---

# ✂️ 2. Train/Test Split

Dataset dibagi menggunakan rasio:

```text
80% → Training
20% → Testing
```

Dengan:

```python
train_test_split(
    X,
    y,
    test_size=0.2,
    random_state=42
)
```

Semua model kemudian dievaluasi menggunakan **test set yang sama** agar perbandingan performa lebih fair.

---

# ⚖️ 3. Feature Scaling

Standardisasi dilakukan menggunakan:

```python
StandardScaler()
```

Data training:

```python
X_train_scaled = scaler.fit_transform(X_train)
```

Data testing:

```python
X_test_scaled = scaler.transform(X_test)
```

### Mengapa berbeda?

KNN menggunakan **jarak antar titik** untuk menentukan tetangga terdekat.

Jika skala fitur berbeda jauh, fitur dengan nilai numerik besar dapat mendominasi perhitungan jarak.

Contohnya:

```text
TAX → ratusan
NOX → < 1
```

Karena itu, scaling sangat penting untuk KNN.

Sementara itu, Linear Regression dan Decision Tree dalam implementasi notebook ini tidak membutuhkan scaling dengan alasan yang sama.

---

# ⚠️ Anti Data Leakage

Notebook menerapkan pola preprocessing yang benar:

```text
Training Data
     ↓
fit_transform()
     ↓
Scaler

Testing Data
     ↓
transform()
     ↓
Scaler yang sama
```

Scaler **tidak di-fit menggunakan seluruh dataset sebelum train/test split**.

Hal ini penting karena jika informasi dari test set digunakan saat proses training preprocessing, evaluasi model dapat menjadi terlalu optimistis.

Prinsip ini berlaku pada berbagai preprocessing yang **belajar dari data**, seperti:

* Scaling
* Imputation
* Encoding tertentu
* Feature transformation

---

# 🤖 4. Model Training

Tiga algoritma digunakan dalam project.

---

## 4.1 Linear Regression

Linear Regression digunakan sebagai model yang relatif sederhana dan mudah diinterpretasikan.

Secara konseptual:

```text
Features
   ↓
Linear Relationship
   ↓
Predicted MEDV
```

Kelebihannya adalah sederhana dan cepat, tetapi model memiliki asumsi hubungan yang lebih linear.

---

## 4.2 K-Nearest Neighbors

KNN memprediksi nilai berdasarkan observasi yang memiliki jarak terdekat.

Karena menggunakan distance calculation, KNN menggunakan:

```text
Standardized Features
```

Dalam notebook digunakan:

```python
KNeighborsRegressor(n_neighbors=5)
```

KNN lebih fleksibel terhadap pola non-linear, tetapi performanya sensitif terhadap:

* Nilai `k`
* Scaling
* Distribusi data
* Outlier

---

## 4.3 Decision Tree

Decision Tree membagi data berdasarkan kondisi tertentu untuk menghasilkan prediksi.

Model yang digunakan:

```python
DecisionTreeRegressor(
    random_state=42,
    max_depth=5
)
```

Parameter `max_depth=5` digunakan untuk membatasi kedalaman tree.

Hal ini penting karena Decision Tree yang terlalu dalam dapat memiliki variance tinggi dan berpotensi **overfit**.

---

# 📏 5. Model Evaluation

Ketiga model dievaluasi menggunakan:

### R²

Mengukur seberapa besar variasi target yang dapat dijelaskan oleh model.

> Semakin besar → semakin baik.

### MAE

Mengukur rata-rata absolute error antara nilai aktual dan prediksi.

> Semakin kecil → semakin baik.

### RMSE

Mirip dengan MAE tetapi memberikan penalti lebih besar terhadap error yang besar.

> Semakin kecil → semakin baik.

---

# 📊 6. Model Comparison

Hasil evaluasi pada test set:

| Model             |        R² |       MAE |      RMSE |
| ----------------- | --------: | --------: | --------: |
| Linear Regression |     0.669 |     3.189 |     4.929 |
| KNN               |     0.719 |     2.592 |     4.539 |
| Decision Tree     | **0.883** | **2.308** | **2.925** |

### Interpretasi

**Decision Tree** memperoleh:

* R² tertinggi
* MAE terendah
* RMSE terendah

Dalam eksperimen notebook ini, Decision Tree memberikan performa terbaik pada test set dibandingkan dua model lainnya.

Model mampu menjelaskan sekitar:

> **88.3% variasi MEDV**

dengan error rata-rata absolut sekitar:

> **2.308**

---

# 📈 7. Understanding the Results

### Linear Regression

```text
R²   = 0.669
MAE  = 3.189
RMSE = 4.929
```

Linear Regression memiliki performa paling rendah dari tiga model dalam eksperimen ini.

Salah satu interpretasi dari notebook adalah bahwa hubungan antara fitur dan harga rumah tidak sepenuhnya linear.

---

### KNN

```text
R²   = 0.719
MAE  = 2.592
RMSE = 4.539
```

KNN memberikan hasil lebih baik daripada Linear Regression.

Namun, perbedaan antara MAE dan RMSE menunjukkan adanya beberapa error prediksi yang relatif besar.

---

### Decision Tree

```text
R²   = 0.883
MAE  = 2.308
RMSE = 2.925
```

Decision Tree memberikan hasil terbaik pada test set dalam eksperimen ini.

Kemampuannya menangkap hubungan **non-linear dan interaksi fitur** membuatnya lebih sesuai dengan pola yang ditemukan pada dataset.

---

# 🎯 8. Prediction vs Actual

Prediksi model terbaik dibandingkan dengan nilai aktual menggunakan scatter plot.

Garis diagonal:

```text
Prediction = Actual
```

digunakan sebagai referensi.

### Insight

Sebagian besar titik berada relatif dekat dengan garis diagonal, menunjukkan bahwa prediksi model cukup dekat dengan nilai aktual.

Penyimpangan yang lebih besar terlihat terutama pada rumah dengan harga tinggi.

Notebook juga mencatat bahwa `MEDV` memiliki **cap sekitar 50**, sehingga model menghadapi keterbatasan ketika memprediksi observasi pada bagian atas distribusi tersebut.

---

# 🧠 Model Trade-Off

Project ini menunjukkan bahwa pemilihan model bukan hanya tentang mencari algoritma yang paling kompleks.

### Linear Regression

**Kelebihan:**

* Sederhana
* Cepat
* Mudah diinterpretasikan

**Keterbatasan:**

* Kurang fleksibel untuk hubungan non-linear.

---

### KNN

**Kelebihan:**

* Dapat menangkap pola non-linear.
* Konsep model relatif sederhana.

**Keterbatasan:**

* Membutuhkan scaling.
* Sensitif terhadap nilai `k`.
* Sensitif terhadap distribusi data dan outlier.

---

### Decision Tree

**Kelebihan:**

* Dapat menangkap hubungan non-linear.
* Dapat menangkap interaksi antar fitur.
* Tidak membutuhkan feature scaling.

**Keterbatasan:**

* Dapat memiliki variance tinggi.
* Berpotensi overfit jika tree terlalu dalam.

---

# 💡 Key Insights

1. **Tidak ada satu model yang selalu terbaik.** Performa bergantung pada karakteristik dataset dan objective modeling.

2. **Feature scaling penting untuk KNN** karena algoritma ini menggunakan jarak.

3. **Preprocessing harus dilakukan tanpa data leakage**, dengan scaler di-fit hanya pada training data.

4. `RM` memiliki hubungan positif kuat dengan `MEDV`, sedangkan `LSTAT` memiliki hubungan negatif kuat.

5. Dalam eksperimen ini, **Decision Tree menghasilkan performa terbaik** pada test set.

6. Perbedaan performa menunjukkan bahwa dataset memiliki pola yang tidak sepenuhnya linear.

7. Model selection merupakan trade-off antara **simplicity, flexibility, interpretability, dan predictive performance**.

---

# ⚠️ Modeling Pitfalls

### 1. Scaling Tidak Selalu Dibutuhkan

Scaling bukan aturan universal untuk semua model.

Dalam notebook:

```text
KNN → membutuhkan scaling
Linear Regression → tidak wajib
Decision Tree → tidak membutuhkan scaling
```

Kebutuhan preprocessing bergantung pada algoritma.

### 2. Test Set Harus Konsisten

Ketiga model menggunakan test set yang sama agar hasil perbandingan dapat dilakukan secara fair.

### 3. Best Model ≠ Universal Best Model

Decision Tree menjadi model terbaik **dalam eksperimen dan split data ini**.

Hal tersebut tidak berarti Decision Tree selalu menjadi model terbaik untuk dataset lain.

### 4. Satu Train/Test Split Memiliki Keterbatasan

Evaluasi hanya menggunakan satu train/test split.

Untuk evaluasi yang lebih robust, tahap berikutnya dapat menggunakan **cross-validation**.

---

# 📚 Learning Outcomes

Melalui project ini, beberapa konsep machine learning yang dipraktikkan:

* Regression Model Comparison
* Linear Regression
* KNN Regression
* Decision Tree Regression
* Train/Test Split
* Feature Scaling
* StandardScaler
* Data Leakage Prevention
* R²
* MAE
* RMSE
* Prediction vs Actual
* Bias–Variance Awareness
* Model Trade-Off
* Non-Linear Relationship
* Model Selection

---

# 🚀 Next Step

Setelah memahami perbandingan beberapa model regression, workflow dapat dikembangkan menjadi:

```text
Model Comparison
       ↓
Cross Validation
       ↓
Hyperparameter Tuning
       ↓
Pipeline
       ↓
Model Selection
       ↓
Error Analysis
       ↓
Final Model
```

Tahap berikutnya penting untuk memastikan model yang dipilih tidak hanya bagus pada **satu train/test split**, tetapi juga memiliki performa yang lebih konsisten pada data yang berbeda.

---

## 📁 Project Structure

```text
Day_23_Comparation_Model/
│
├── Day_23__Comparation_Model(LinearRegression_KNN_DecissionTree).ipynb
└── README.md
```

---

## 👤 Author

**Arief Wicaksono**

Aspiring AI Engineer | Data Science Student

---

⭐ If you find this project useful, feel free to star the repository!
