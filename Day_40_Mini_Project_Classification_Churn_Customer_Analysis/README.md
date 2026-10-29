# Day 40 — End-to-End Mini Project Classification

## 📌 Overview

Day 40 merupakan **proyek penutup modul klasifikasi** yang merangkai beberapa konsep Machine Learning menjadi satu workflow end-to-end.

Studi kasus yang digunakan adalah **prediksi customer churn** pada perusahaan telekomunikasi.

Pada project ini, workflow dimulai dari data mentah, exploratory data analysis, data cleaning, preprocessing menggunakan `ColumnTransformer`, kemudian membangun `Pipeline` dengan Logistic Regression, melakukan hyperparameter tuning menggunakan `GridSearchCV`, dan akhirnya mengevaluasi model menggunakan metrik klasifikasi serta menerjemahkan hasilnya menjadi **insight bisnis**.

> 🎯 **Tujuan utama:** membangun workflow Machine Learning yang lebih rapi, terstruktur, dan mampu menghubungkan hasil model dengan kebutuhan bisnis.

---

## 🎯 Learning Objectives

- Membangun workflow Machine Learning menggunakan **Pipeline**.
- Menggunakan **ColumnTransformer** untuk preprocessing fitur numerik dan kategorikal.
- Melakukan **encoding** fitur kategorikal secara otomatis menggunakan `OneHotEncoder`.
- Melakukan **scaling** fitur numerik menggunakan `StandardScaler`.
- Menghindari data leakage dengan menempatkan preprocessing di dalam Pipeline.
- Melakukan hyperparameter tuning menggunakan **GridSearchCV**.
- Menggunakan **ROC-AUC** sebagai scoring untuk data yang tidak seimbang.
- Mengevaluasi model menggunakan Accuracy, Precision, Recall, F1-Score, dan ROC-AUC.
- Menginterpretasikan hasil model dari perspektif bisnis.
- Menghubungkan prediksi churn dengan strategi customer retention.

---

## 📊 Dataset

Dataset yang digunakan adalah **Telco Customer Churn**.

**Source:** Kaggle — `blastchar/telco-customer-churn`

Dataset berisi informasi pelanggan perusahaan telekomunikasi, seperti demografi, layanan yang digunakan, informasi kontrak, pembayaran, tagihan, dan status churn.

### Dataset Information

- **7.043 baris**
- **21 kolom**
- **0 duplicate**
- **0 missing value** berdasarkan pemeriksaan awal
- Proporsi target `Churn = Yes`: **26.54%**

Namun, notebook menemukan bahwa kolom `TotalCharges` bertipe `object` karena terdapat nilai kosong berbentuk string/spasi. Nilai tersebut kemudian dikonversi menjadi numerik dan nilai kosong diisi dengan `0`.

### Target

| Value | Meaning |
|---|---|
| `0` | Bertahan / Tidak Churn |
| `1` | Churn |

Target `Churn` dikonversi dari `Yes/No` menjadi `1/0`.

---

## 🗂️ Dataset Features

Beberapa kelompok fitur yang digunakan dalam dataset:

| Feature Group | Examples |
|---|---|
| Demografi | `gender`, `SeniorCitizen`, `Partner`, `Dependents` |
| Customer Tenure | `tenure` |
| Phone Service | `PhoneService`, `MultipleLines` |
| Internet Service | `InternetService`, `OnlineSecurity`, `OnlineBackup` |
| Device & Support | `DeviceProtection`, `TechSupport` |
| Streaming | `StreamingTV`, `StreamingMovies` |
| Contract | `Contract` |
| Billing | `PaperlessBilling`, `PaymentMethod` |
| Charges | `MonthlyCharges`, `TotalCharges` |
| Target | `Churn` |

Kolom `customerID` dihapus karena hanya berfungsi sebagai identifier unik dan tidak digunakan sebagai fitur prediktif.

---

## 🧹 Data Cleaning

Tahapan cleaning yang dilakukan:

### 1. Check Dataset

Dataset awal:

```text
7043 rows × 21 columns
```

Pemeriksaan awal menghasilkan:

```text
Duplicate       : 0
Missing Value   : 0
Churn           : 26.54%
```

Walaupun tidak terdeteksi missing value secara standar, `TotalCharges` masih bertipe `object`. Notebook mengidentifikasi adanya nilai kosong berupa string/spasi pada pelanggan tertentu.

### 2. Convert `TotalCharges`

`TotalCharges` dikonversi menjadi numerik:

```python
df["TotalCharges"] = pd.to_numeric(
    df["TotalCharges"],
    errors="coerce"
)

df["TotalCharges"] = df["TotalCharges"].fillna(0)
```

### 3. Remove `customerID`

```python
df.drop(
    columns=["customerID"],
    inplace=True
)
```

### 4. Encode Target

```python
df["Churn"] = (
    df["Churn"] == "Yes"
).astype(int)
```

Setelah preprocessing awal, dataset memiliki **20 kolom**, dengan fitur numerik dan kategorikal yang siap diproses menggunakan Pipeline.

---

## 📊 Exploratory Data Analysis

EDA digunakan untuk memahami pola customer churn sebelum membangun model.

### 1. Churn Berdasarkan Jenis Kontrak

Notebook membandingkan churn berdasarkan:

- `Month-to-month`
- `One year`
- `Two year`

Hasil visualisasi menunjukkan bahwa:

- **Month-to-month** memiliki tingkat churn paling tinggi.
- **One year** memiliki churn lebih rendah.
- **Two year** memiliki tingkat churn paling rendah.

Notebook menyimpulkan bahwa pelanggan dengan kontrak yang lebih panjang cenderung memiliki kemungkinan churn yang lebih rendah.

### 2. Tenure vs Churn

Notebook juga menganalisis distribusi:

- `tenure`
- `Churn`

Analisis ini digunakan untuk melihat hubungan antara lama pelanggan berlangganan dengan kemungkinan churn.

---

## ⚖️ Class Imbalance

Target churn memiliki proporsi:

```text
Churn = 26.54%
```

Artinya kelas churn merupakan **minority class**.

Karena itu, notebook menekankan bahwa evaluasi tidak sebaiknya hanya bergantung pada Accuracy. Metric seperti:

- Recall
- F1-Score
- ROC-AUC

lebih relevan untuk melihat kemampuan model dalam mendeteksi pelanggan yang berisiko churn.

---

## ⚙️ Preprocessing Pipeline

Salah satu fokus utama Day 40 adalah membangun preprocessing secara otomatis menggunakan `ColumnTransformer`.

### Numerical Features

Notebook menggunakan:

```text
SeniorCitizen
tenure
MonthlyCharges
TotalCharges
```

Jumlah:

> **4 numerical columns**

Numerical features diproses menggunakan:

```python
StandardScaler()
```

### Categorical Features

Terdapat:

> **15 categorical columns**

Categorical features diproses menggunakan:

```python
OneHotEncoder(
    handle_unknown="ignore"
)
```



---

## 🧱 ColumnTransformer

Preprocessing kemudian digabungkan dalam `ColumnTransformer`:

```python
preprocessor = ColumnTransformer([
    (
        "num",
        StandardScaler(),
        num_col
    ),
    (
        "cat",
        OneHotEncoder(
            handle_unknown="ignore"
        ),
        cat_col
    )
])
```

Keuntungan pendekatan ini adalah preprocessing numerik dan kategorikal dapat dikelola dalam satu objek dan kemudian dimasukkan langsung ke dalam Pipeline.

---

## ✂️ Train-Test Split

Dataset dibagi menggunakan:

```python
train_test_split(
    X,
    y,
    test_size=0.2,
    random_state=42,
    stratify=y
)
```

Konfigurasi:

- **80% training**
- **20% testing**
- `random_state = 42`
- `stratify = y`

Dengan stratification, distribusi target dipertahankan antara training dan testing.

---

## 🤖 Modeling

Model yang digunakan adalah:

> **Logistic Regression**

Model ditempatkan bersama preprocessing dalam satu `Pipeline`.

```python
pipe = Pipeline([
    (
        "prep",
        preprocessor
    ),
    (
        "algo",
        LogisticRegression(
            max_iter=1000,
            class_weight="balanced",
            solver="liblinear",
            random_state=42
        )
    )
])
```

Penggunaan:

```python
class_weight="balanced"
```

bertujuan membuat model lebih sensitif terhadap minority class, yaitu pelanggan yang churn.

---

## 🔍 Hyperparameter Tuning

Hyperparameter yang dituning adalah:

> `C`

Nilai yang diuji:

```python
[
    0.001,
    0.01,
    0.1,
    1,
    10,
    100
]
```

GridSearch menggunakan:

```python
GridSearchCV(
    pipe,
    param_grid=param_grid,
    scoring="roc_auc",
    cv=5,
    n_jobs=1
)
```

Scoring menggunakan **ROC-AUC** karena dataset memiliki class imbalance.

### Best Parameter

Hasil tuning:

```text
Best parameter: C = 100
Best CV ROC-AUC: 0.846
```

Dari seluruh nilai `C` yang diuji, `C = 100` menghasilkan performa cross-validation terbaik berdasarkan ROC-AUC.

---

# 📏 Model Evaluation

Model terbaik kemudian dievaluasi menggunakan test set.

### Test Results

| Metric | Score |
|---|---:|
| Accuracy | **73.81%** |
| ROC-AUC | **0.840** |



---

## 📋 Classification Report

| Class | Precision | Recall | F1-Score | Support |
|---|---:|---:|---:|---:|
| Bertahan | 0.90 | 0.72 | 0.80 | 1035 |
| Churn | 0.50 | 0.78 | 0.61 | 374 |
| **Accuracy** | | | **0.74** | **1409** |
| Macro Avg | 0.70 | 0.75 | 0.71 | 1409 |
| Weighted Avg | 0.80 | 0.74 | 0.75 | 1409 |



### Interpretation

#### Bertahan

- Precision = **90%**
- Recall = **72%**
- F1-Score = **80%**

Model cukup baik ketika memprediksi pelanggan yang akan bertahan.

#### Churn

- Precision = **50%**
- Recall = **78%**
- F1-Score = **61%**

Recall sebesar **78%** menjadi metrik penting karena model berhasil mendeteksi sebagian besar pelanggan yang benar-benar churn.

Namun, Precision hanya **50%**, sehingga sebagian pelanggan yang diprediksi churn ternyata masih bertahan.

---

## 📈 ROC-AUC

Model memperoleh:

> **ROC-AUC = 0.840**

ROC-AUC digunakan untuk mengukur kemampuan model membedakan pelanggan yang churn dan yang tidak churn berdasarkan probabilitas prediksi.

Notebook menunjukkan bahwa kurva ROC berada jauh di atas random baseline dengan AUC `0.500`.

---

## 🧩 Confusion Matrix

Notebook juga memvisualisasikan:

- Confusion Matrix
- ROC Curve

Confusion Matrix digunakan untuk memahami kesalahan prediksi antara:

```text
Bertahan
Churn
```

sedangkan ROC Curve digunakan untuk mengevaluasi kemampuan model dalam membedakan kedua kelas.

---

# 💼 Business Insight

Salah satu bagian penting dari Day 40 adalah menerjemahkan hasil model menjadi **insight bisnis**.

Model menggunakan:

```python
class_weight="balanced"
```

sehingga memiliki kecenderungan lebih sensitif terhadap pelanggan yang berpotensi churn.

### Benefit

Model berhasil mencapai:

> **Recall Churn = 78%**

Artinya model dapat menangkap sekitar 78% pelanggan yang sebenarnya churn.

Hal ini dapat membantu tim bisnis melakukan tindakan retention sebelum pelanggan benar-benar berhenti berlangganan.

### Trade-Off

Precision churn hanya:

> **50%**

Artinya, ketika model memprediksi pelanggan akan churn, sekitar setengah dari prediksi tersebut merupakan false alarm.

Karena itu, notebook menyarankan agar strategi retention atau promo yang diberikan kepada pelanggan berisiko sebaiknya menggunakan pendekatan yang **berbiaya rendah**, sehingga false alarm tidak menghasilkan pemborosan anggaran yang besar.

---

## 💡 Key Insights

1. **Month-to-month contract** merupakan kelompok dengan tingkat churn paling tinggi.
2. Pelanggan dengan kontrak **Two year** memiliki churn paling rendah.
3. Durasi kontrak yang lebih panjang berkaitan dengan kemungkinan churn yang lebih rendah berdasarkan EDA notebook.
4. Dataset memiliki class imbalance dengan churn sekitar **26.54%**.
5. Karena imbalance, ROC-AUC lebih informatif dibandingkan hanya menggunakan Accuracy.
6. `Pipeline` membantu menggabungkan preprocessing dan model dalam satu workflow.
7. `ColumnTransformer` memungkinkan numerical dan categorical features diproses secara berbeda.
8. `OneHotEncoder` digunakan untuk categorical features.
9. `StandardScaler` digunakan untuk numerical features.
10. `GridSearchCV` menemukan **C = 100** sebagai parameter terbaik.
11. Best cross-validation ROC-AUC mencapai **0.846**.
12. Test ROC-AUC mencapai **0.840**.
13. Test Accuracy mencapai **73.81%**.
14. Recall kelas churn mencapai **78%**.
15. Precision kelas churn sebesar **50%** menunjukkan adanya false alarm yang cukup besar.
16. Dalam konteks bisnis, model lebih diarahkan untuk **menangkap pelanggan yang berisiko churn** daripada sekadar memaksimalkan Accuracy.  

---

## 🔄 End-to-End Workflow

```text
Telco Customer Churn Dataset
            ↓
Initial Data Exploration
            ↓
Check Duplicate & Missing Value
            ↓
Clean TotalCharges
            ↓
Drop customerID
            ↓
Encode Target Churn
            ↓
Exploratory Data Analysis
            ↓
Analyze Class Imbalance
            ↓
Train-Test Split
            ↓
ColumnTransformer
     ↙              ↘
Numerical         Categorical
     ↓                 ↓
StandardScaler    OneHotEncoder
     ↘              ↙
          Pipeline
             ↓
     Logistic Regression
             ↓
        GridSearchCV
             ↓
       Best C = 100
             ↓
       Test Evaluation
             ↓
 Accuracy / Precision / Recall
             ↓
        ROC-AUC = 0.840
             ↓
      Business Interpretation
             ↓
      Customer Retention Insight
```

---

## 🛠️ Tech Stack

- Python
- Pandas
- NumPy
- Matplotlib
- Seaborn
- Scikit-learn
- Logistic Regression
- Pipeline
- ColumnTransformer
- OneHotEncoder
- StandardScaler
- GridSearchCV
- Confusion Matrix
- Classification Report
- ROC Curve
- ROC-AUC

---

## 📁 Project Structure

```text
Day_40_Mini_Project_Classification/
│
├── Day_40_Mini_Project_Classification.ipynb
└── README.md
```

---

## 🧠 Conclusion

Day 40 menjadi **end-to-end mini project** sekaligus penutup modul klasifikasi.

Berbeda dari beberapa project sebelumnya yang berfokus pada satu algoritma atau satu teknik evaluasi, project ini menggabungkan seluruh workflow mulai dari data exploration hingga business interpretation.

Model Logistic Regression dengan `class_weight="balanced"` dan hyperparameter tuning menggunakan `GridSearchCV` menghasilkan:

```text
Best C            : 100
Best CV ROC-AUC   : 0.846
Test Accuracy     : 73.81%
Test ROC-AUC      : 0.840
Churn Recall      : 78%
Churn Precision   : 50%
Churn F1-Score    : 61%
```

Hasil tersebut menunjukkan bahwa model cukup sensitif dalam mendeteksi pelanggan yang berpotensi churn, tetapi memiliki trade-off berupa false alarm yang cukup tinggi.

Dari sisi bisnis, model dapat digunakan sebagai alat bantu untuk **mengidentifikasi pelanggan yang berisiko churn** sehingga perusahaan dapat menjalankan strategi retention lebih awal.

Namun, hasil model tetap perlu dipertimbangkan bersama biaya strategi retention, karena tidak semua pelanggan yang diprediksi churn benar-benar akan berhenti berlangganan.

---

## 👨‍💻 Author

**Arief Wicaksono**  
Aspiring AI Engineer | Data Science Student

---

⭐ *Learning Machine Learning one day at a time.*