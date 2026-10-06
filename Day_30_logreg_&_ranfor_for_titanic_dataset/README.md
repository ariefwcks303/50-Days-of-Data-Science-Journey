# 🚢 Day 30 — Mini Project Klasifikasi: Prediksi Korban Selamat Titanic

Project ini merupakan mini project **Machine Learning klasifikasi** yang menggunakan data penumpang kapal Titanic untuk memprediksi apakah seorang penumpang **selamat atau tidak**.

Fokus utama project adalah mengubah data penumpang menjadi dataset yang siap dimodelkan melalui proses **data cleaning, feature engineering, encoding, standardisasi, dan train-test split**, kemudian membandingkan performa **Logistic Regression** dan **Random Forest Classifier**.

---

## 🎯 Project Objectives

Tujuan pembelajaran project ini adalah:

- Melakukan eksplorasi dataset Titanic.
- Mengidentifikasi dan menangani missing values.
- Melakukan **feature engineering** dengan membuat fitur `FamilySize`.
- Membuat fitur `IsAlone` untuk mengidentifikasi penumpang yang bepergian sendirian.
- Melakukan encoding pada fitur kategorikal.
- Mengubah `Sex` menjadi fitur numerik 0/1.
- Melakukan One-Hot Encoding pada `Embarked`.
- Menghapus fitur yang dianggap tidak relevan untuk modeling.
- Melakukan **train-test split** dengan proporsi 80:20.
- Menggunakan `StandardScaler` untuk preprocessing Logistic Regression.
- Melatih **Logistic Regression** dan **Random Forest Classifier**.
- Membandingkan performa kedua model menggunakan accuracy.
- Mengevaluasi model menggunakan **Confusion Matrix** dan `classification_report`.
- Menganalisis feature importance dari Random Forest.
- Menarik insight dari faktor-faktor yang berhubungan dengan keselamatan penumpang.

---

## 📂 Dataset

Dataset yang digunakan adalah:

**Titanic Dataset**

📌 Source: [Kaggle — yasserh/titanic-dataset](https://www.kaggle.com/datasets/yasserh/titanic-dataset)

Dataset terdiri dari:

- **891 baris**
- **12 kolom**

Target:

```text
Survived
```

Dengan:

```text
0 = Meninggal
1 = Selamat
```

### Important Features

| Feature | Description |
|---|---|
| `Survived` | Target — 1 = selamat, 0 = meninggal |
| `Pclass` | Kelas tiket penumpang (1/2/3) |
| `Sex` | Jenis kelamin |
| `Age` | Usia penumpang |
| `SibSp` | Jumlah saudara/pasangan di kapal |
| `Parch` | Jumlah orang tua/anak di kapal |
| `Fare` | Tarif tiket |
| `Embarked` | Pelabuhan keberangkatan (C/Q/S) |

Kolom awal dataset juga mencakup:

- `PassengerId`
- `Name`
- `Ticket`
- `Cabin`

---

## 🛠️ Tech Stack

Project ini menggunakan:

- **Python**
- **Pandas** — Data manipulation
- **NumPy** — Numerical computation
- **Matplotlib** — Visualization
- **Seaborn** — Data visualization
- **Scikit-learn** — Preprocessing, modeling, dan evaluation

Komponen utama scikit-learn:

- `train_test_split`
- `StandardScaler`
- `LogisticRegression`
- `RandomForestClassifier`
- `confusion_matrix`
- `classification_report`
- `accuracy_score`
- `ConfusionMatrixDisplay`

---

# 🔎 End-to-End Workflow

```text
Raw Dataset
     │
     ▼
Data Understanding
     │
     ▼
Explorasi Awal Dataset
     │
     ├── Dataset Info
     ├── Statistik Deskriptif
     └── Missing Value Analysis
     │
     ▼
Data Cleaning
     │
     ├── Age → Median Imputation
     ├── Embarked → Mode Imputation
     └── Cabin → Drop
     │
     ▼
Feature Engineering
     │
     ├── FamilySize
     └── IsAlone
     │
     ▼
Feature Encoding
     │
     ├── Sex → 0/1
     └── Embarked → One-Hot Encoding
     │
     ▼
Feature Selection
     │
     └── Drop Name, Ticket, Cabin, PassengerId
     │
     ▼
Train-Test Split
     │
     ├── 80% Training
     └── 20% Testing
     │
     ├───────────────┐
     ▼               ▼
StandardScaler    Random Forest
     │               │
     ▼               ▼
Logistic           Prediction
Regression            │
     │                 │
     └────────┬────────┘
              ▼
       Model Evaluation
              │
       ├── Accuracy
       ├── Confusion Matrix
       └── Classification Report
              │
              ▼
       Feature Importance
              │
              ▼
       Key Insights
```

---

# 🔍 1. Data Understanding

Dataset memiliki **891 baris dan 12 kolom**.

Beberapa kolom memiliki missing value:

| Column | Missing |
|---|---:|
| `Age` | 177 |
| `Cabin` | 687 |
| `Embarked` | 2 |
| Kolom lainnya | 0 |

`Cabin` memiliki jumlah missing value yang sangat besar sehingga kolom tersebut dibuang pada tahap cleaning.

Distribusi target menunjukkan:

| Survived | Proportion |
|---|---:|
| `0` — Meninggal | **62%** |
| `1` — Selamat | **38%** |

Target memang tidak seimbang, tetapi pada notebook dikategorikan masih dalam kondisi yang wajar.

---

# 🧹 2. Data Cleaning

Beberapa langkah cleaning dilakukan sebelum modeling.

### `Age`

Missing value pada `Age` diisi menggunakan **median**:

```python
data['Age'] = data['Age'].fillna(data['Age'].median())
```

### `Embarked`

Missing value pada `Embarked` diisi menggunakan **modus**:

```python
data['Embarked'] = data['Embarked'].fillna(
    data['Embarked'].mode()[0]
)
```

### `Cabin`

Kolom `Cabin` dibuang karena memiliki **687 missing values dari 891 baris**.

---

# ⚙️ 3. Feature Engineering

Feature engineering digunakan untuk membuat representasi yang lebih bermakna dari informasi keluarga penumpang.

## `FamilySize`

Fitur baru dibuat dari:

```text
FamilySize = SibSp + Parch + 1
```

Angka `1` merepresentasikan penumpang itu sendiri.

```python
data['FamilySize'] = data['SibSp'] + data['Parch'] + 1
```

## `IsAlone`

Fitur binary dibuat berdasarkan `FamilySize`:

```python
data['IsAlone'] = (data['FamilySize'] == 1).astype(int)
```

Dengan:

```text
0 = Tidak sendirian
1 = Sendirian
```

`FamilySize` merangkum informasi `SibSp` dan `Parch` menjadi satu fitur yang lebih mudah digunakan untuk menangkap pola hubungan keluarga.

---

# 🔢 4. Feature Encoding

Karena model Machine Learning membutuhkan representasi numerik, fitur kategorikal diubah menjadi angka.

## `Sex`

Encoding:

```text
Female → 0
Male   → 1
```

Implementasi:

```python
data['Sex'] = data['Sex'].map({
    'female': 0,
    'male': 1
})
```

## `Embarked`

`Embarked` diubah menggunakan One-Hot Encoding:

```python
data = pd.get_dummies(
    data,
    columns=['Embarked'],
    prefix=['Emb']
)
```

Sehingga menghasilkan:

```text
Emb_C
Emb_Q
Emb_S
```

---

# 🗑️ 5. Feature Selection

Beberapa kolom dianggap tidak relevan untuk modeling dan dihapus:

```python
drp_clm = [
    'Name',
    'Ticket',
    'Cabin',
    'PassengerId'
]

data.drop(
    columns=drp_clm,
    inplace=True,
    errors='ignore'
)
```

Fitur yang digunakan setelah preprocessing meliputi:

```text
Pclass
Sex
Age
SibSp
Parch
Fare
FamilySize
IsAlone
Emb_C
Emb_Q
Emb_S
```

Target:

```text
Survived
```

---

# 📊 6. Exploratory Data Analysis

Notebook melakukan analisis visual untuk memahami hubungan beberapa fitur dengan tingkat keselamatan penumpang.

## A. Tingkat Keselamatan Berdasarkan Jenis Kelamin

Analisis menunjukkan adanya perbedaan tingkat keselamatan berdasarkan `Sex`.

Perempuan memiliki tingkat keselamatan yang jauh lebih tinggi dibandingkan laki-laki.

Hal ini menjadi salah satu pola paling kuat yang kemudian juga terlihat pada model Machine Learning.

---

## B. Pengaruh Kelas Penumpang

`Pclass` digunakan sebagai representasi kelas tiket.

Penumpang **Pclass 1** memiliki peluang selamat yang lebih tinggi dibandingkan penumpang **Pclass 3**.

Hal ini menunjukkan bahwa faktor sosial-ekonomi memiliki hubungan dengan peluang keselamatan pada dataset Titanic.

---

## C. Faktor Usia

`Age` juga menjadi salah satu fitur penting dalam prediksi.

Kelompok usia anak-anak tercatat memiliki rasio bertahan hidup yang lebih tinggi dibandingkan kelompok usia produktif dan lansia.

---

# ✂️ 7. Train-Test Split

Data dipisahkan menjadi fitur dan target:

```python
X = data.drop(columns=['Survived'])
y = data['Survived']
```

Kemudian dataset dibagi menggunakan:

```python
train_test_split(
    X,
    y,
    test_size=0.2,
    random_state=42,
    stratify=y
)
```

Pembagian:

```text
80% Training
20% Testing
```

Dengan:

```text
random_state = 42
```

dan menggunakan:

```text
stratify = y
```

agar distribusi target tetap terjaga pada data training dan testing.

---

# 📏 8. Standardization

Standardisasi digunakan untuk **Logistic Regression**.

`StandardScaler` di-fit hanya menggunakan data training, kemudian digunakan untuk mentransformasi data testing.

```text
Training Data
     │
     ▼
StandardScaler.fit()
     │
     ▼
Transform Training
     │
     └──────────────► Transform Testing
```

Hal ini dilakukan untuk mencegah **data leakage**, karena informasi dari data testing tidak boleh digunakan dalam proses fitting preprocessing.

Random Forest tidak membutuhkan scaling karena merupakan model berbasis tree.

---

# 🤖 9. Model Training

Dua algoritma klasifikasi digunakan dan dibandingkan:

### Logistic Regression

```python
lr = LogisticRegression()

lr.fit(
    X_train_scaled,
    y_train
)

y_pred_lr = lr.predict(X_test_scaled)
```

Logistic Regression merupakan model linear dan pada project ini digunakan bersama `StandardScaler`.

### Random Forest

```python
rf = RandomForestClassifier(
    random_state=42
)

rf.fit(
    X_train,
    y_train
)

y_pred_rf = rf.predict(X_test)
```

Random Forest merupakan ensemble model berbasis Decision Tree sehingga tidak membutuhkan standardisasi fitur.

---

# 📈 10. Model Evaluation & Final Model Selection

Kedua model dibandingkan menggunakan **Accuracy**.

## Test Set Results

| Model | Scaling | Accuracy | Role |
|---|---|---:|---|
| **Logistic Regression** | Yes | **80.4%** | **Final Model** |
| Random Forest | No | **79.9%** | Supplementary Analysis |

### Final Model: Logistic Regression

**Logistic Regression dipilih sebagai final model** karena memberikan accuracy tertinggi pada test set:

```text
80.4%
```

dibandingkan Random Forest:

```text
79.9%
```

Perbedaannya memang relatif kecil, tetapi berdasarkan hasil evaluasi pada notebook, Logistic Regression menjadi model utama yang digunakan untuk analisis performa dan classification report.

Random Forest tetap dipertahankan sebagai **supplementary analysis**, terutama untuk melihat feature importance.

> **Catatan:** feature importance Random Forest digunakan untuk analisis tambahan terhadap kontribusi relatif fitur. Nilai tersebut bukan coefficient atau interpretasi langsung dari Logistic Regression.

---

# 🎯 11. Confusion Matrix

Confusion Matrix digunakan untuk melihat distribusi prediksi benar dan salah pada masing-masing kelas.

Label yang digunakan:

```text
0 = Meninggal
1 = Selamat
```

Interpretasi:

- Diagonal kiri-atas dan kanan-bawah menunjukkan prediksi yang benar.
- Sel kanan-atas menunjukkan penumpang yang sebenarnya meninggal tetapi diprediksi selamat.
- Sel kiri-bawah menunjukkan penumpang yang sebenarnya selamat tetapi diprediksi meninggal.

Pada model Logistic Regression, kesalahan prediksi relatif terbagi pada kedua jenis kelas.

---

# 📋 12. Classification Report

Hasil `classification_report` untuk Logistic Regression:

| Class | Precision | Recall | F1-Score | Support |
|---|---:|---:|---:|---:|
| Meninggal (0) | 0.82 | 0.88 | 0.85 | 110 |
| Selamat (1) | 0.78 | 0.68 | 0.73 | 69 |
| **Accuracy** | | | **0.80** | **179** |
| Macro Avg | 0.80 | 0.78 | 0.79 | 179 |
| Weighted Avg | 0.80 | 0.80 | 0.80 | 179 |

### Interpretation

**Akurasi keseluruhan = 80%**

Model berhasil memprediksi status penumpang dengan benar pada sekitar 80% dari total **179 data testing**.

### Kelas Meninggal (0)

- Precision: **82%**
- Recall: **88%**
- F1-score: **85%**

Model memiliki kemampuan yang cukup baik dalam mengenali penumpang yang tidak selamat.

### Kelas Selamat (1)

- Precision: **78%**
- Recall: **68%**
- F1-score: **73%**

Recall yang lebih rendah menunjukkan masih terdapat sebagian penumpang yang sebenarnya selamat tetapi gagal dikenali oleh model.

---

# 🌲 13. Supplementary Analysis — Feature Importance Random Forest

Karena **Logistic Regression merupakan final model**, feature importance pada bagian ini tidak digunakan untuk menentukan model utama.

Random Forest digunakan sebagai **supplementary analysis** untuk melihat kontribusi relatif setiap fitur terhadap keputusan model.

### Tiga fitur paling dominan

| Feature | Importance |
|---|---:|
| `Fare` | **26.9%** |
| `Sex` | **25.1%** |
| `Age` | **24.5%** |

Ketiga fitur tersebut menyumbang lebih dari **76%** total feature importance model.

### Faktor pendukung

| Feature | Importance |
|---|---:|
| `Pclass` | **8.1%** |
| `FamilySize` | **4.7%** |
| `SibSp` | **2.7%** |
| `Parch` | **2.5%** |

### Faktor dengan pengaruh lebih kecil

Fitur:

```text
Emb_S
Emb_C
Emb_Q
IsAlone
```

berada di bawah 2% dan memiliki pengaruh relatif lebih kecil pada Random Forest.

---

# 💡 Key Insights

Insight feature importance pada bagian ini perlu dibaca sebagai **analisis tambahan dari Random Forest**, sedangkan performa final project tetap mengacu pada Logistic Regression.

### 1. Gender Merupakan Faktor Penting

`Sex` merupakan salah satu prediktor paling dominan.

Perempuan memiliki tingkat keselamatan yang jauh lebih tinggi dibandingkan laki-laki.

Hal ini konsisten dengan pola evakuasi yang dikenal sebagai **"Women and Children First"**.

---

### 2. Fare dan Pclass Berhubungan dengan Keselamatan

`Fare` merupakan fitur dengan feature importance tertinggi pada Random Forest:

```text
26.9%
```

`Pclass` juga memberikan kontribusi:

```text
8.1%
```

Penumpang pada **Pclass 1** memiliki peluang selamat lebih besar dibandingkan Pclass 3.

Hal ini menunjukkan adanya hubungan antara faktor sosial-ekonomi dan peluang keselamatan.

---

### 3. Age Merupakan Prediktor Penting

`Age` memiliki feature importance sebesar:

```text
24.5%
```

Kelompok usia anak-anak tercatat memiliki rasio bertahan hidup lebih tinggi dibandingkan kelompok usia produktif dan lansia.

---

### 4. Family Size Memberikan Informasi Tambahan

Feature engineering:

```text
FamilySize = SibSp + Parch + 1
```

memberikan representasi yang lebih sederhana mengenai ukuran keluarga penumpang.

Penumpang yang bepergian bersama keluarga dalam jumlah kecil hingga sedang cenderung memiliki peluang bertahan hidup lebih tinggi dibandingkan penumpang yang bepergian sendirian.

---

# 🔐 14. Data Leakage Prevention

Salah satu pelajaran penting dari project ini adalah urutan preprocessing.

Data dibagi terlebih dahulu:

```text
Train-Test Split
       ↓
StandardScaler
```

bukan:

```text
StandardScaler
       ↓
Train-Test Split
```

`StandardScaler` di-fit hanya pada training data.

Dengan pendekatan tersebut, data testing tidak ikut memberikan informasi kepada proses preprocessing sehingga evaluasi model menjadi lebih valid.

---

# 🧠 15. Final Modeling Summary

```text
Model Comparison
       │
       ├── Logistic Regression
       │       └── Accuracy: 80.4%
       │              ↓
       │        FINAL MODEL
       │
       └── Random Forest
               └── Accuracy: 79.9%
                      ↓
             Supplementary Analysis
                      ↓
             Feature Importance
```

Dengan demikian, struktur modeling pada project ini adalah:

- **Logistic Regression** → model final berdasarkan hasil evaluasi test set.
- **Random Forest** → model pembanding dan supplementary analysis.
- **Random Forest Feature Importance** → digunakan untuk memahami kontribusi relatif fitur, bukan sebagai interpretasi coefficient Logistic Regression.

---

# 🧠 16. Key Learning

Project ini menunjukkan beberapa konsep penting dalam Machine Learning classification:

- Dataset dunia nyata membutuhkan data cleaning.
- Missing value harus ditangani sebelum modeling.
- Feature engineering dapat membuat representasi data menjadi lebih bermakna.
- Fitur kategorikal harus diubah menjadi representasi numerik.
- Scaling penting untuk model tertentu seperti Logistic Regression.
- Random Forest tidak membutuhkan scaling.
- Train-test split harus dilakukan sebelum preprocessing yang dipelajari dari data.
- Confusion Matrix membantu memahami jenis kesalahan model.
- Accuracy saja belum cukup untuk memahami performa setiap kelas.
- Precision dan recall memberikan informasi yang lebih detail.
- Feature importance dapat digunakan untuk memahami kontribusi fitur terhadap model.

---

# ⚠️ Limitations

Project ini masih merupakan eksperimen pembelajaran dan belum merupakan model production-ready.

Beberapa keterbatasan:

- Hanya menggunakan dua algoritma klasifikasi.
- Belum dilakukan systematic hyperparameter tuning.
- Belum dilakukan cross-validation.
- Accuracy Logistic Regression dan Random Forest sangat berdekatan sehingga perbedaannya belum tentu signifikan.
- Feature importance Random Forest tidak menunjukkan hubungan sebab-akibat.
- Beberapa fitur asli seperti `Name`, `Ticket`, dan `Cabin` dibuang.
- Target memiliki distribusi 62% meninggal dan 38% selamat.
- Belum dilakukan deployment dan monitoring model.

---

# 🚀 Production Perspective

Workflow pada project ini sudah mencakup beberapa tahap dasar Machine Learning:

```text
Raw Data
   ↓
Data Understanding
   ↓
Data Cleaning
   ↓
Feature Engineering
   ↓
Encoding
   ↓
Train-Test Split
   ↓
Preprocessing
   ↓
Model Training
   ↓
Prediction
   ↓
Evaluation
   ↓
Feature Analysis
```

Apabila dikembangkan menjadi sistem production, tahap selanjutnya dapat mencakup:

```text
Data Validation
      ↓
Feature Pipeline
      ↓
Model Training
      ↓
Hyperparameter Tuning
      ↓
Cross-Validation
      ↓
Model Validation
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

- Classification Workflow
- Data Understanding
- Data Cleaning
- Missing Value Handling
- Feature Engineering
- `FamilySize`
- `IsAlone`
- Label / Binary Encoding
- One-Hot Encoding
- StandardScaler
- Train-Test Split
- Stratified Split
- Logistic Regression
- Random Forest Classification
- Accuracy
- Confusion Matrix
- Classification Report
- Precision
- Recall
- F1-Score
- Feature Importance
- Data Leakage Prevention
- Exploratory Data Analysis
- Model Comparison
- Error Analysis

---

# 📁 Project Structure

```text
Day_30_Prediksi_Korban_Selamat_Titanic/
│
├── Day_30_Prediksi_Korban_Selamat_Titanic.ipynb
├── Titanic-Dataset.csv
└── README.md
```

---

# 👤 Author

**Arief Wicaksono**

Aspiring AI Engineer | Data Science Student

---

⭐ If you find this project useful, feel free to star the repository!
