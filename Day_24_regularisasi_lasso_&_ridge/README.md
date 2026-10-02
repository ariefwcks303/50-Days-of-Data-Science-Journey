# 📉 Day 24 — Regularisasi Regression: Ridge & Lasso

Project ini membahas penerapan **Regularization** pada model regresi menggunakan dataset **Boston Housing**.

Regularisasi menambahkan penalti terhadap nilai koefisien model untuk membantu mengendalikan kompleksitas model. Dua metode yang dipelajari adalah **Ridge Regression (L2)** dan **Lasso Regression (L1)**, kemudian dibandingkan dengan Linear Regression sebagai baseline.

Fokus utama project ini adalah memahami pengaruh regularisasi terhadap performa prediksi, stabilitas koefisien, serta kemampuan Lasso melakukan seleksi fitur otomatis.

---

## 🎯 Project Objectives

Tujuan pembelajaran dalam project ini:

* Memahami alasan penggunaan regularisasi dalam regression.
* Membandingkan Linear Regression, Ridge, dan Lasso.
* Memahami pentingnya standardisasi fitur sebelum regularisasi.
* Menerapkan `StandardScaler` tanpa data leakage.
* Mengevaluasi model menggunakan R², MAE, MSE, dan RMSE.
* Memahami pengaruh parameter `alpha` terhadap performa model.
* Membandingkan penalti L1 dan L2.
* Mengamati bagaimana Lasso dapat membuat koefisien fitur menjadi nol.
* Memahami trade-off antara performa prediksi dan kesederhanaan model.

---

## 📂 Dataset

Dataset yang digunakan adalah **Boston Housing Dataset**.

📌 Source: [Kaggle — The Boston House Price Data](https://www.kaggle.com/datasets/fedesoriano/the-boston-houseprice-data)

Dataset terdiri dari:

* **506 baris**
* **14 kolom**
* **13 fitur input**
* **1 target prediksi**

### Important Features

| Feature   | Description                                  |
| --------- | -------------------------------------------- |
| `RM`      | Rata-rata jumlah kamar per rumah             |
| `LSTAT`   | Persentase penduduk berstatus ekonomi rendah |
| `PTRATIO` | Rasio murid terhadap guru                    |
| `CRIM`    | Tingkat kriminalitas                         |
| `NOX`     | Konsentrasi polusi NOx                       |
| `TAX`     | Tarif pajak properti                         |
| `DIS`     | Jarak tertimbang ke pusat pekerjaan          |
| `MEDV`    | Harga median rumah dalam ribuan USD — target |

Fitur lainnya meliputi `ZN`, `INDUS`, `CHAS`, `AGE`, `RAD`, dan `B`.

Dataset tidak memiliki missing value berdasarkan pemeriksaan awal notebook.

---

## 🛠️ Tech Stack

* **Python** — Programming language
* **Pandas** — Data manipulation
* **NumPy** — Numerical computation
* **Matplotlib** — Data visualization
* **Seaborn** — Statistical visualization
* **Scikit-learn** — Regression modeling, preprocessing, dan evaluasi

### Models

* Linear Regression
* Ridge Regression — L2 Regularization
* Lasso Regression — L1 Regularization

### Preprocessing

* `train_test_split`
* `StandardScaler`

---

## 🔎 Analysis & Modeling Workflow

```text
Boston Housing Dataset
          │
          ▼
    Data Exploration
          │
          ├── Missing Values
          ├── Descriptive Statistics
          └── Correlation Analysis
          │
          ▼
   Train/Test Split (80/20)
          │
          ▼
     Standardization
          │
          ├── Fit Scaler on Training Data
          └── Transform Test Data
          │
          ▼
      Model Training
          │
          ├── Linear Regression
          ├── Ridge Regression
          └── Lasso Regression
          │
          ▼
      Model Evaluation
          │
          ├── R²
          ├── MAE
          ├── MSE
          └── RMSE
          │
          ▼
   Alpha Sensitivity Analysis
          │
          ▼
   Coefficient Comparison
          │
          ▼
    Feature Selection Insight
```

---

## 🔍 1. Exploratory Data Analysis

### Correlation Analysis

Correlation heatmap digunakan untuk mengidentifikasi hubungan antarfitur dan target `MEDV`.

Beberapa temuan dari notebook:

* `RM` memiliki korelasi positif dengan `MEDV`, sekitar **0.70**.
* `LSTAT` memiliki korelasi negatif dengan `MEDV`, sekitar **-0.74**.
* Beberapa fitur saling berkorelasi, seperti `RAD` dengan `TAX`, serta `NOX` dengan `INDUS`.

Korelasi antarfitur dapat membuat koefisien Linear Regression kurang stabil dalam kondisi multikolinearitas. Regularisasi menjadi salah satu pendekatan untuk mengendalikan besarnya koefisien.

### Target Distribution

Distribusi `MEDV` menunjukkan sebagian besar harga rumah berada di kisaran **17–25 ribu USD**.

Terdapat penumpukan observasi pada nilai 50, yang menunjukkan adanya batas atas (*capping*) pada target dataset. Hal ini perlu dipertimbangkan ketika menginterpretasikan hasil prediksi.

---

## ⚙️ 2. Train/Test Split & Standardization

Dataset dibagi menggunakan:

```python
train_test_split(
    X,
    y,
    test_size=0.2,
    random_state=42
)
```

Pembagian data:

* **80%** untuk training.
* **20%** untuk testing.

Kemudian fitur distandardisasi menggunakan `StandardScaler`.

```python
scaler = StandardScaler()

X_train_scaled = scaler.fit_transform(X_train)
X_test_scaled = scaler.transform(X_test)
```

### Mengapa Standardisasi Penting?

Fitur memiliki rentang nilai yang berbeda. Contohnya, `TAX` dapat bernilai ratusan, sedangkan `NOX` bernilai kurang dari satu.

Karena regularisasi memberikan penalti pada koefisien, perbedaan skala fitur dapat memengaruhi besarnya penalti jika fitur tidak distandardisasi.

Standardisasi membantu membuat penalti lebih sebanding antarfitur.

### Data Leakage Prevention

Scaler hanya di-*fit* pada training data. Test data hanya menggunakan `transform()` dari scaler yang sama.

```text
Training Data
      │
      ▼
fit_transform()
      │
      ▼
     Scaler
      │
      ▼
Test Data → transform()
```

Dengan demikian, statistik dari test set tidak digunakan untuk mempelajari parameter standardisasi.

---

## 🤖 3. Linear Regression vs Ridge vs Lasso

### Linear Regression

Linear Regression digunakan sebagai baseline tanpa penalti regularisasi.

Model mempelajari hubungan linear antara fitur input dan target harga rumah.

### Ridge Regression — L2

Ridge menambahkan penalti berdasarkan jumlah kuadrat koefisien.

Secara konseptual:

$$
\text{Loss}_{Ridge}
=
\text{MSE}+\alpha\sum_j \beta_j^2
$$

Ridge mendorong koefisien menjadi lebih kecil, tetapi umumnya tidak membuat koefisien tepat nol.

**Karakteristik utama:**

* Mengurangi besarnya koefisien.
* Dapat membantu mengendalikan koefisien ketika fitur berkorelasi.
* Mempertahankan seluruh fitur dalam model.

### Lasso Regression — L1

Lasso menambahkan penalti berdasarkan jumlah nilai absolut koefisien.

$$
\text{Loss}_{Lasso}
=
\text{MSE}+\alpha\sum_j|\beta_j|
$$

Berbeda dari Ridge, Lasso dapat membuat sebagian koefisien menjadi tepat nol.

**Karakteristik utama:**

* Mengurangi besarnya koefisien.
* Dapat melakukan seleksi fitur otomatis.
* Menghasilkan model yang lebih ringkas ketika beberapa koefisien menjadi nol.

Dalam kedua metode, `alpha` mengatur kekuatan penalti. Semakin besar `alpha`, semakin kuat koefisien ditekan.

---

## 📏 4. Model Evaluation

Ketiga model dievaluasi menggunakan test set yang sama.

| Model             |    R² |   MAE |    MSE |  RMSE |
| ----------------- | ----: | ----: | -----: | ----: |
| Linear Regression | 0.669 | 3.189 | 24.291 | 4.929 |
| Ridge (`alpha=1`) | 0.668 | 3.186 | 24.313 | 4.931 |
| Lasso (`alpha=1`) | 0.624 | 3.474 | 27.578 | 5.251 |

### Interpretasi

**Linear Regression**

Menghasilkan R² sebesar 0.669. Model menjelaskan sekitar 66.9% variasi target pada test set.

**Ridge Regression**

Menghasilkan R² sebesar 0.668, hampir sama dengan Linear Regression. Pada `alpha=1`, regularisasi Ridge belum memberikan peningkatan performa prediksi yang terlihat pada eksperimen ini.

**Lasso Regression**

Menghasilkan R² sebesar 0.624 dengan MAE dan RMSE yang lebih tinggi dibandingkan kedua model lainnya.

Pada `alpha=1`, penalti Lasso mengurangi performa prediksi relatif terhadap baseline Linear Regression dalam eksperimen ini.

> Hasil ini berlaku untuk konfigurasi dan pembagian data pada notebook, bukan kesimpulan bahwa Ridge atau Lasso selalu lebih buruk daripada Linear Regression.

---

## 🎚️ 5. Alpha Sensitivity Analysis

Notebook menguji beberapa nilai `alpha`:

```python
alphas = [
    0.001, 0.01, 0.1, 1,
    5, 10, 50, 100
]
```

Tujuannya adalah mengamati perubahan RMSE ketika kekuatan regularisasi meningkat.

### Hasil Ridge vs Lasso

| Alpha | Ridge RMSE | Lasso RMSE |
| ----: | ---------: | ---------: |
| 0.001 |      4.929 |      4.929 |
|  0.01 |      4.929 |      4.933 |
|   0.1 |      4.929 |      5.065 |
|     1 |      4.931 |      5.251 |
|     5 |      4.939 |      7.242 |
|    10 |      4.949 |      8.663 |
|    50 |      4.996 |      8.663 |
|   100 |      5.033 |      8.663 |

### Insight Ridge

RMSE Ridge relatif stabil, meningkat dari **4.929** pada `alpha=0.001` menjadi **5.033** pada `alpha=100`.

Pada rentang parameter yang diuji, peningkatan penalti tidak menyebabkan perubahan performa yang drastis.

### Insight Lasso

RMSE Lasso meningkat lebih tajam ketika `alpha` diperbesar:

* `alpha=0.001`: RMSE 4.929
* `alpha=1`: RMSE 5.251
* `alpha=5`: RMSE 7.242
* `alpha=10`: RMSE 8.663

Pada nilai `alpha` yang besar, penalti Lasso membuat model terlalu sederhana untuk mempertahankan performa prediksi pada data ini.

Fenomena ini berkaitan dengan **over-regularization** dan risiko *underfitting*.

### Kesimpulan Alpha

Dalam eksperimen ini, nilai `alpha` kecil menghasilkan error lebih rendah, terutama untuk Lasso.

Namun, pemilihan `alpha` yang optimal sebaiknya dilakukan melalui validation set atau cross-validation, bukan hanya berdasarkan satu test set.

---

## 🧩 6. Lasso Feature Selection

Salah satu eksperimen utama adalah membandingkan koefisien Ridge dan Lasso pada `alpha=1`.

Hasil notebook menunjukkan:

> **Lasso menghasilkan 7 koefisien nol dari total 13 fitur.**

Beberapa fitur yang koefisiennya menjadi nol:

* `DIS`
* `NOX`
* `TAX`
* `AGE`
* `INDUS`
* `ZN`
* `RAD`

Sementara beberapa fitur tetap memiliki koefisien non-zero, seperti:

| Feature   | Ridge Coefficient | Lasso Coefficient |
| --------- | ----------------: | ----------------: |
| `LSTAT`   |             -3.60 |             -3.38 |
| `RM`      |              3.15 |              3.08 |
| `PTRATIO` |             -2.03 |             -1.22 |
| `B`       |              1.13 |              0.45 |
| `CHAS`    |              0.72 |              0.04 |

Koefisien tersebut berasal dari fitur yang telah distandardisasi.

### Interpretasi

Lasso mempertahankan beberapa fitur dengan koefisien non-zero dan menghilangkan kontribusi linear fitur lainnya dengan membuat koefisien menjadi nol.

Dalam eksperimen ini, `RM`, `LSTAT`, dan `PTRATIO` tetap memiliki koefisien non-zero.

Ridge, sebaliknya, mengecilkan koefisien tanpa menghilangkan fitur melalui koefisien nol pada hasil tersebut.

> Koefisien nol pada Lasso menunjukkan hasil seleksi oleh model, bukan bukti bahwa suatu fitur tidak memiliki hubungan atau relevansi dalam semua konteks.

---

## 💡 Key Insights

### 1. Regularization Mengendalikan Koefisien

Ridge dan Lasso menambahkan penalti untuk membatasi besarnya koefisien model.

### 2. Ridge Lebih Stabil pada Rentang Alpha yang Diuji

RMSE Ridge hanya berubah sedikit ketika `alpha` meningkat dari 0.001 hingga 100.

### 3. Lasso Dapat Melakukan Feature Selection

Pada `alpha=1`, Lasso menghasilkan tujuh koefisien nol dari 13 fitur.

### 4. Alpha Memengaruhi Performa

Penalti yang terlalu kuat dapat mengurangi kemampuan model dalam memprediksi target. Pengaturan `alpha` menjadi bagian penting dari model selection.

### 5. Model Ringkas Tidak Selalu Memiliki Error Terendah

Lasso menghasilkan model dengan lebih banyak koefisien nol, tetapi performanya pada `alpha=1` lebih rendah daripada Linear Regression dan Ridge dalam eksperimen ini.

### 6. Standardisasi Penting

Standardisasi membantu memastikan penalti regularisasi diterapkan pada fitur dengan skala yang sebanding.

---

## 💼 Business & Engineering Insight

Regularisasi membantu menjawab dua kebutuhan yang berbeda:

```text
Predictive Performance
        │
        ├── Ridge
        │    └── Mengecilkan koefisien
        │
        └── Lasso
             └── Mengecilkan koefisien
                 + seleksi fitur
```

Dalam konteks bisnis:

* **Ridge** dapat dipertimbangkan ketika banyak fitur masih ingin dipertahankan, tetapi koefisien perlu dikendalikan.
* **Lasso** dapat dipertimbangkan ketika model yang lebih ringkas dan seleksi fitur menjadi prioritas.
* Performa prediksi dan kompleksitas model perlu dievaluasi bersama sebelum menentukan model yang akan digunakan.

Pemilihan metode tetap bergantung pada tujuan bisnis, kualitas data, dan hasil evaluasi yang robust.

---

## ⚠️ Modeling Pitfalls

### 1. Data Leakage

Scaler harus di-fit pada training data saja. Test data tidak boleh digunakan untuk mempelajari parameter preprocessing.

### 2. Alpha Terlalu Besar

Penalti yang terlalu kuat dapat mengecilkan terlalu banyak koefisien dan menyebabkan underfitting.

### 3. Feature Selection Bukan Bukti Kausalitas

Fitur yang dipertahankan Lasso tidak otomatis merupakan penyebab harga rumah. Koefisien menggambarkan hubungan dalam model.

### 4. Test Set untuk Model Selection

Notebook membandingkan beberapa nilai `alpha` menggunakan RMSE pada test set. Untuk evaluasi yang lebih kuat, gunakan validation set atau cross-validation untuk memilih `alpha`, kemudian gunakan test set untuk evaluasi final.

### 5. Batas Target Dataset

Target `MEDV` memiliki nilai yang mencapai batas atas 50. Kondisi ini dapat memengaruhi error dan interpretasi performa prediksi.

---

## 📚 Learning Outcomes

Melalui project ini, konsep yang dipraktikkan meliputi:

* Regression Regularization
* Ridge Regression
* Lasso Regression
* L1 vs L2 Penalty
* StandardScaler
* Train/Test Split
* Data Leakage Prevention
* R², MAE, MSE, dan RMSE
* Alpha Sensitivity Analysis
* Coefficient Shrinkage
* Automatic Feature Selection
* Multicollinearity Awareness
* Over-Regularization
* Underfitting
* Model Complexity vs Predictive Performance

---

## 🚀 Next Step

Project ini menjadi fondasi untuk memahami pemilihan model dan optimasi hyperparameter.

```text
Linear Regression
        ↓
Ridge & Lasso
        ↓
Alpha Selection
        ↓
Cross-Validation
        ↓
Pipeline
        ↓
Hyperparameter Tuning
        ↓
Final Model Evaluation
```

Tahap selanjutnya adalah mempelajari **cross-validation dan hyperparameter tuning** agar pemilihan model tidak bergantung pada satu pembagian data saja.

---

## 📁 Project Structure

```text
Day_24_regularisasi_lasso_&_ridge/
│
├── Day_24_regularisasi_lasso_&_ridge.ipynb
└── README.md
```

---

## 👤 Author

**Arief Wicaksono**

Aspiring AI Engineer | Data Science Student

---

⭐ If you find this project useful, feel free to star the repository!
