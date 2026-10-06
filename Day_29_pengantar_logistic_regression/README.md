# Day 29 — Pengantar Klasifikasi: Logistic Regression

## 📌 Overview

Day 29 menjadi awal modul **Klasifikasi**, setelah sebelumnya membahas regresi.

Jika regresi digunakan untuk memprediksi **nilai numerik**, maka klasifikasi digunakan untuk memprediksi **kategori atau label**.

Pada notebook ini digunakan **Logistic Regression** untuk memprediksi apakah seorang pasien terindikasi diabetes atau tidak berdasarkan beberapa fitur medis.

### 🎯 Tujuan Pembelajaran

- Memahami perbedaan **regresi vs klasifikasi**.
- Memahami konsep dasar **Logistic Regression**.
- Menangani nilai `0` yang tidak masuk akal sebagai **missing value**.
- Melakukan imputasi missing value menggunakan **median**.
- Menerapkan **Feature Scaling** sebelum training Logistic Regression.
- Mengevaluasi model menggunakan:
  - Accuracy
  - Confusion Matrix
  - Classification Report
- Menginterpretasikan koefisien Logistic Regression sebagai indikasi arah dan kekuatan pengaruh fitur.

---

## 📊 Dataset

Notebook menggunakan **Pima Indians Diabetes Dataset**.

**Sumber:** [Kaggle — Diabetes Dataset](https://www.kaggle.com/datasets/akshaydattatraykhare/diabetes-dataset)

Dataset terdiri dari:

- **768 baris**
- **9 kolom**

### Features

| Kolom | Deskripsi |
|---|---|
| `Pregnancies` | Jumlah kehamilan |
| `Glucose` | Kadar glukosa darah |
| `BloodPressure` | Tekanan darah |
| `SkinThickness` | Ketebalan lipatan kulit |
| `Insulin` | Kadar insulin |
| `BMI` | Indeks massa tubuh |
| `DiabetesPedigreeFunction` | Skor riwayat keluarga |
| `Age` | Usia |
| `Outcome` | Target: `1` = diabetes, `0` = tidak diabetes |

### Target

```text
Outcome
├── 0 → Tidak Diabetes
└── 1 → Diabetes
```

---

## 🔍 Data Cleaning

Beberapa fitur medis memiliki nilai `0` yang dianggap tidak masuk akal secara klinis dan diperlakukan sebagai missing value.

Kolom yang diproses:

```python
[
    'Glucose',
    'BloodPressure',
    'SkinThickness',
    'Insulin',
    'BMI'
]
```

Nilai `0` diubah menjadi `NaN`.

### Imputasi

Missing value kemudian diisi menggunakan:

```python
SimpleImputer(strategy='median')
```

Median dipilih karena lebih robust terhadap outlier dibandingkan mean.

---

## 📈 Exploratory Data Analysis

Notebook melakukan beberapa eksplorasi:

### 1. Distribusi Target

Melihat distribusi kelas:

- `0` → Tidak Diabetes
- `1` → Diabetes

Hal ini membantu memahami proporsi masing-masing kelas sebelum melakukan modeling.

### 2. Distribusi Glucose

Distribusi `Glucose` dibandingkan berdasarkan kelas `Outcome` untuk melihat perbedaan karakteristik antara pasien diabetes dan non-diabetes.

### 3. Correlation Heatmap

Heatmap digunakan untuk melihat hubungan/korelasi antar fitur numerik dan target.

---

## ⚙️ Preprocessing

### 1. Memisahkan Feature dan Target

```python
X = df.drop(columns=['Outcome'])
y = df['Outcome']
```

### 2. Train-Test Split

Data dibagi dengan rasio:

```text
80% → Training
20% → Testing
```

Menggunakan:

```python
train_test_split(
    X,
    y,
    test_size=0.2,
    random_state=42,
    stratify=y
)
```

`stratify=y` digunakan agar proporsi kelas pada data training dan testing tetap terjaga.

### 3. Feature Scaling

Digunakan:

```python
StandardScaler()
```

Scaling diperlukan karena fitur memiliki skala yang berbeda. Contohnya, `Insulin` dapat memiliki nilai hingga ratusan, sedangkan `DiabetesPedigreeFunction` berada pada rentang yang jauh lebih kecil.

Proses scaling:

```text
Training Data
     │
     ▼
fit_transform()
     │
     ▼
X_train_scaled

Testing Data
     │
     ▼
transform()
     │
     ▼
X_test_scaled
```

Scaler hanya di-`fit` pada training data untuk menghindari data leakage dari test set.

---

## 🤖 Modeling — Logistic Regression

Model yang digunakan:

```python
LogisticRegression(random_state=42)
```

Model dilatih menggunakan data yang sudah melalui StandardScaler:

```python
model.fit(X_train_scaled, y_train)
```

Kemudian menghasilkan:

```python
y_pred = model.predict(X_test_scaled)
```

dan probabilitas kelas positif:

```python
y_pred_proba = model.predict_proba(X_test_scaled)[:, 1]
```

---

## 📊 Model Evaluation

### Accuracy

Hasil notebook:

| Metric | Result |
|---|---:|
| Logistic Regression Accuracy | **70.78%** |
| Baseline Accuracy | **64.94%** |

Model mengungguli baseline sederhana yang selalu memprediksi kelas mayoritas (`0`).

Namun, accuracy saja belum cukup untuk menilai model klasifikasi, terutama ketika kesalahan antar kelas memiliki konsekuensi berbeda.

---

## 🧩 Confusion Matrix & Classification Report

Notebook juga menggunakan:

- `confusion_matrix`
- `classification_report`

Hasil utama yang ditekankan notebook:

### Kelas Tidak Diabetes (`0`)

- Precision: **75%**
- Recall: **82%**

Model relatif lebih baik dalam mengenali pasien yang tidak diabetes.

### Kelas Diabetes (`1`)

- Precision: **60%**
- Recall: **50%**

Recall sebesar **50%** berarti model hanya berhasil mendeteksi sekitar setengah dari pasien diabetes yang sebenarnya terdapat pada data pengujian.

Dalam konteks medis, False Negative menjadi perhatian penting karena pasien yang sebenarnya diabetes dapat diprediksi sebagai tidak diabetes.

---

## 🧠 Feature Importance melalui Koefisien

Logistic Regression tidak menggunakan feature importance seperti tree-based model. Pada notebook ini, interpretasi dilakukan melalui **koefisien model**.

Koefisien kemudian diurutkan berdasarkan nilai absolutnya.

### Insight

Fitur dengan pengaruh positif paling kuat:

- `Glucose`
- `BMI`

Fitur lain yang juga memiliki koefisien positif:

- `Pregnancies`
- `DiabetesPedigreeFunction`
- `Age`

Sementara:

- `Insulin`
- `BloodPressure`
- `SkinThickness`

memiliki pengaruh yang relatif lebih kecil atau negatif dalam model yang dilatih.

> Interpretasi koefisien menunjukkan hubungan fitur terhadap prediksi model, bukan bukti bahwa fitur tersebut secara kausal menyebabkan diabetes.

---

## 🔄 End-to-End Workflow

Workflow pada Day 29:

```text
Raw Dataset
     │
     ▼
Exploratory Data Analysis
     │
     ▼
Handle Invalid Zero Values
     │
     ▼
Median Imputation
     │
     ▼
Feature / Target Split
     │
     ▼
Train-Test Split (80:20)
     │
     ▼
StandardScaler
     │
     ▼
Logistic Regression
     │
     ▼
Prediction
     │
     ▼
Evaluation
 ┌───┼───────────────┐
 ▼   ▼               ▼
Accuracy   Confusion Matrix   Classification Report
     │
     ▼
Coefficient Analysis
```

---

## 💡 Key Takeaways

### 1. Classification ≠ Regression

Regression memprediksi nilai kontinu, sedangkan classification memprediksi kelas atau label.

### 2. Preprocessing memengaruhi kualitas model

Nilai `0` yang tidak masuk akal diperlakukan sebagai missing value dan kemudian diimputasi menggunakan median.

### 3. Scaling penting untuk Logistic Regression

StandardScaler membantu menempatkan fitur dengan skala berbeda ke skala yang lebih seragam sebelum training.

### 4. Accuracy bukan satu-satunya metrik

Model mencapai accuracy **70.78%**, tetapi recall untuk kelas diabetes hanya **50%**.

Ini menunjukkan bahwa model yang memiliki accuracy cukup baik belum tentu memiliki performa yang baik pada kelas yang paling penting.

### 5. Error memiliki konteks bisnis/domain

Pada kasus medis, **False Negative** dapat menjadi perhatian serius. Karena itu, pemilihan threshold dan metrik evaluasi perlu mempertimbangkan tujuan penggunaan model.

---

## ⚠️ Limitations & Next Steps

Notebook ini merupakan pengantar klasifikasi dan belum ditujukan sebagai sistem diagnosis medis.

Beberapa pengembangan berikutnya dapat dilakukan:

- Menangani **class imbalance** dengan pendekatan yang sesuai.
- Mengeksplorasi threshold probabilitas selain default `0.5`.
- Membandingkan Logistic Regression dengan algoritma klasifikasi lainnya.
- Mengevaluasi metrik tambahan seperti Precision, Recall, F1-Score, ROC-AUC, dan PR-AUC.
- Melakukan cross-validation.
- Menggunakan pipeline preprocessing + model untuk workflow yang lebih robust.
- Melakukan hyperparameter tuning.
- Mengevaluasi model dengan pendekatan yang lebih sesuai untuk risiko False Negative.

---

## 🛠️ Tools & Libraries

- Python
- Pandas
- NumPy
- Matplotlib
- Seaborn
- Scikit-learn

Model dan tools utama:

```python
LogisticRegression
StandardScaler
SimpleImputer
train_test_split
accuracy_score
confusion_matrix
classification_report
```

---

## 📁 Notebook

Notebook utama:

```text
Day_29_Pengantar_Klasifikasi_Logistic_Regression.ipynb
```

---

## 🚀 Learning Progress

Day 29 menandai perpindahan dari **Regression** ke **Classification** dalam perjalanan 50 Days of Data Science.

Fokus utama:

```text
Regression
   │
   ▼
Classification
   │
   ▼
Logistic Regression
   │
   ▼
Classification Evaluation
   │
   ▼
Next: Model Classification lainnya
```

---

**Arief Wicaksono**

Aspiring AI Engineer | Data Science Student

A hands-on learning journey covering Data Analysis, Machine Learning, Model Evaluation, and applied Data Science.

---

⭐ If you find this project useful, feel free to star the repository!



