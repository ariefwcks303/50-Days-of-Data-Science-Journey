# Day 37 — Classification using Support Vector Machine & Naive Bayes

## 📌 Project Overview

Pada Day 37, project berfokus pada pengenalan dan perbandingan dua algoritma klasifikasi klasik:

- **Support Vector Machine (SVM)**
- **Naive Bayes**

Keduanya digunakan untuk memprediksi apakah sebuah **red wine** tergolong **berkualitas baik atau tidak** berdasarkan karakteristik kimia hasil pengujian laboratorium.

Project ini juga membahas pentingnya **feature scaling untuk SVM**, penanganan **class imbalance**, serta evaluasi model menggunakan **accuracy, precision, recall, F1-score, dan confusion matrix**.

---

## 🎯 Learning Objectives

- Memahami konsep dasar **Support Vector Machine (SVM)**.
- Memahami konsep dasar **Naive Bayes**.
- Mengubah target numerik `quality` menjadi target biner `good_quality`.
- Memahami mengapa **scaling penting untuk SVM**, tetapi tidak digunakan untuk Gaussian Naive Bayes pada eksperimen ini.
- Melatih `SVC` dan `GaussianNB`.
- Membandingkan performa kedua model.
- Membaca **confusion matrix** dan **classification report**.
- Memahami dampak **imbalanced dataset** terhadap evaluasi klasifikasi.

---

## 📊 Dataset

Dataset yang digunakan adalah **Red Wine Quality**, yang berisi hasil pengujian kimia terhadap **1.599 sampel red wine** dari Portugal.

**Source:** Kaggle — Red Wine Quality Dataset  
https://www.kaggle.com/datasets/uciml/red-wine-quality-cortez-et-al-2009

### Dataset Information

- Rows: **1,599**
- Columns awal: **12**
- Missing values: **0**
- Target awal: `quality`
- Rentang `quality`: **3–8**

### Features

| Feature | Description |
|---|---|
| `fixed acidity` | Kandungan fixed acidity |
| `volatile acidity` | Kandungan volatile acidity |
| `citric acid` | Kandungan citric acid |
| `residual sugar` | Sisa gula |
| `chlorides` | Kandungan chloride |
| `free sulfur dioxide` | Free sulfur dioxide |
| `total sulfur dioxide` | Total sulfur dioxide |
| `density` | Kepadatan wine |
| `pH` | Tingkat keasaman |
| `sulphates` | Kandungan sulphates |
| `alcohol` | Kadar alkohol |
| `quality` | Skor kualitas wine |

---

## 🏷️ Target Engineering

Target asli `quality` berupa skor numerik.

Pada project ini dibuat target biner:

```python
df['good_quality'] = (df['quality'] >= 7).astype(int)
```

Sehingga:

- `0` → **Tidak Baik**
- `1` → **Baik**

### Target Distribution

| Class | Count | Proportion |
|---|---:|---:|
| Tidak Baik (`0`) | 1,382 | 86.4% |
| Baik (`1`) | 217 | 13.6% |

Dataset memiliki **class imbalance**, karena hanya sekitar **13.6%** sampel yang termasuk wine berkualitas baik.

Karena itu, evaluasi tidak hanya menggunakan accuracy, tetapi juga melihat precision, recall, F1-score, dan confusion matrix.

---

## 🔎 Exploratory Data Analysis

EDA dilakukan untuk memahami distribusi target dan hubungan fitur dengan kualitas wine.

### Target Distribution

Terlihat bahwa kelas `Tidak Baik` jauh lebih dominan dibandingkan kelas `Baik`.

### Alcohol vs Quality

Visualisasi boxplot digunakan untuk melihat distribusi kadar alkohol berdasarkan kelas kualitas.

Notebook mencatat bahwa **kadar alkohol cenderung lebih tinggi pada wine berkualitas baik**.

### Insight dari EDA

- Dataset memiliki class imbalance yang cukup kuat.
- Mayoritas wine memiliki quality score **5 dan 6**.
- Wine dengan quality score **7 ke atas** relatif sedikit.
- Alcohol menunjukkan hubungan positif dengan kualitas wine.
- Volatile acidity menunjukkan hubungan negatif dengan kualitas wine.

---

## 🧹 Data Preparation

Target `quality` dan target hasil engineering `good_quality` tidak digunakan sebagai feature.

```python
X = df.drop(['quality', 'good_quality'], axis=1)
y = df['good_quality']
```

### Train-Test Split

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

Hasil pembagian:

- Training data: **1,279 samples**
- Testing data: **320 samples**
- Split: **80:20**
- `random_state=42`
- Menggunakan `stratify=y`

Proporsi kelas `good_quality` tetap relatif sama:

- Train: **13.6%**
- Test: **13.4%**

---

## ⚖️ Feature Scaling

Untuk SVM digunakan `StandardScaler`:

```python
scaler = StandardScaler()

X_train_scaled = scaler.fit_transform(X_train)
X_test_scaled = scaler.transform(X_test)
```

Scaling digunakan karena SVM sensitif terhadap skala fitur.

Untuk **Gaussian Naive Bayes**, notebook menggunakan data original tanpa scaling karena model bekerja berdasarkan pendekatan probabilistik per fitur.

---

# 🤖 Modeling

## A. Support Vector Machine

Model yang digunakan:

```python
SVC(
    kernel='rbf',
    class_weight='balanced',
    random_state=42
)
```

### Configuration

- Algorithm: **Support Vector Machine**
- Kernel: `rbf`
- `class_weight='balanced'`
- `random_state=42`
- Training menggunakan data yang sudah di-scale

### Accuracy

**84.69%**

SVM menghasilkan accuracy:

```text
0.846875
```

---

## B. Gaussian Naive Bayes

Model yang digunakan:

```python
GaussianNB()
```

Model dilatih menggunakan data training tanpa scaling.

### Accuracy

**86.56%**

Gaussian Naive Bayes menghasilkan accuracy:

```text
0.865625
```

---

# 📈 Model Evaluation

Karena dataset tidak seimbang, accuracy saja tidak cukup untuk menggambarkan performa model.

Evaluasi dilakukan menggunakan:

- Accuracy
- Precision
- Recall
- F1-score
- Confusion Matrix
- Classification Report

---

## Confusion Matrix

Notebook menunjukkan:

### SVM

- True Negative: **238**
- False Positive: **39**
- False Negative: **10**
- True Positive: **33**

### Naive Bayes

- True Negative: **246**
- False Positive: **31**
- False Negative: **12**
- True Positive: **31**

Interpretasi utama:

- Kedua model cukup kuat dalam mengenali wine yang **Tidak Baik**.
- SVM memiliki recall kelas **Baik** lebih tinggi.
- Naive Bayes memiliki lebih sedikit false positive.
- Naive Bayes menghasilkan lebih banyak true negative.

---

## Classification Report — SVM

| Class | Precision | Recall | F1-Score | Support |
|---|---:|---:|---:|---:|
| Tidak Baik | 0.96 | 0.86 | 0.91 | 277 |
| Baik | 0.46 | 0.77 | 0.57 | 43 |
| **Accuracy** | | | **0.85** | **320** |

SVM memiliki recall kelas `Baik` sebesar **76.74%**, sehingga relatif lebih sensitif dalam menangkap wine berkualitas baik.

Namun precision kelas `Baik` hanya **45.83%**, sehingga false positive masih cukup tinggi.

---

## Classification Report — Naive Bayes

| Class | Precision | Recall | F1-Score | Support |
|---|---:|---:|---:|---:|
| Tidak Baik | 0.95 | 0.89 | 0.92 | 277 |
| Baik | 0.50 | 0.72 | 0.59 | 43 |
| **Accuracy** | | | **0.87** | **320** |

Naive Bayes memiliki:

- Accuracy: **86.56%**
- Precision kelas `Baik`: **50.00%**
- Recall kelas `Baik`: **72.09%**
- Precision kelas `Tidak Baik`: **95.35%**
- Recall kelas `Tidak Baik`: **88.81%**

---

# 🏆 Model Comparison

| Model | Accuracy | Precision Baik | Recall Baik | Precision Tidak Baik | Recall Tidak Baik |
|---|---:|---:|---:|---:|---:|
| SVM | 84.69% | 45.83% | **76.74%** | **95.97%** | 85.92% |
| **Naive Bayes** | **86.56%** | **50.00%** | 72.09% | 95.35% | **88.81%** |

### Best Model: Naive Bayes

Berdasarkan eksperimen pada notebook, **Naive Bayes dipilih sebagai model terbaik** karena:

1. Memiliki accuracy lebih tinggi:
   - Naive Bayes: **86.56%**
   - SVM: **84.69%**
2. Memiliki precision kelas `Baik` yang lebih tinggi:
   - Naive Bayes: **50.00%**
   - SVM: **45.83%**
3. Lebih baik dalam mendeteksi kelas `Tidak Baik` berdasarkan recall:
   - Naive Bayes: **88.81%**
   - SVM: **85.92%**

Namun, SVM memiliki **recall kelas Baik yang lebih tinggi**, yaitu **76.74%** dibandingkan Naive Bayes **72.09%**.

---

# 💡 Key Insights

### 1. Naive Bayes unggul secara keseluruhan

Naive Bayes memperoleh accuracy **86.56%**, lebih tinggi daripada SVM yang memperoleh **84.69%**.

### 2. SVM lebih sensitif terhadap wine berkualitas baik

Recall kelas `Baik`:

- SVM: **76.74%**
- Naive Bayes: **72.09%**

Artinya, SVM lebih banyak menangkap sampel wine yang benar-benar berkualitas baik.

### 3. Class imbalance menjadi tantangan utama

Hanya **13.6%** data yang termasuk kelas `Baik`.

Hal ini membuat accuracy dapat terlihat tinggi meskipun performa pada kelas minoritas masih perlu ditingkatkan.

### 4. Alkohol menjadi salah satu insight penting

Notebook menunjukkan bahwa wine berkualitas tinggi cenderung memiliki kadar alkohol lebih tinggi.

### 5. Volatile acidity berkorelasi negatif dengan kualitas

Wine dengan volatile acidity yang lebih tinggi cenderung memiliki kualitas yang lebih rendah berdasarkan analisis notebook.

---

# ⚠️ Limitations

Beberapa keterbatasan project:

- Dataset memiliki class imbalance yang cukup kuat.
- Precision kelas `Baik` masih relatif rendah pada kedua model.
- Belum dilakukan hyperparameter tuning yang lebih mendalam.
- Belum menggunakan cross-validation.
- Belum dilakukan threshold tuning.
- Belum dilakukan oversampling seperti SMOTE.
- Evaluasi masih berfokus pada satu train-test split.

---

# 🚀 Next Steps

Beberapa pengembangan yang dapat dilakukan:

1. **Handle Class Imbalance**
   - SMOTE
   - Oversampling
   - Undersampling
   - Threshold tuning

2. **Hyperparameter Tuning**
   - SVM `C`
   - `gamma`
   - kernel
   - parameter GaussianNB

3. **Cross-Validation**
   - Stratified K-Fold
   - Compare mean performance dan variance

4. **Feature Engineering**
   - Membuat fitur baru dari kombinasi karakteristik acidity.
   - Menganalisis rasio antar fitur kimia.

5. **Compare Additional Models**
   - Logistic Regression
   - Random Forest
   - Gradient Boosting
   - XGBoost

6. **Additional Metrics**
   - ROC-AUC
   - PR-AUC
   - Precision-Recall Curve

---

# 🛠️ Tech Stack

- Python
- Pandas
- NumPy
- Matplotlib
- Seaborn
- Scikit-learn
- Support Vector Machine (`SVC`)
- Gaussian Naive Bayes (`GaussianNB`)
- Jupyter Notebook / Google Colab

---

# 📚 Workflow

```text
Red Wine Quality Dataset
        ↓
Initial Data Exploration
        ↓
Data Quality Check
        ↓
Target Engineering
quality >= 7 → good_quality = 1
quality < 7  → good_quality = 0
        ↓
Exploratory Data Analysis
        ↓
Feature & Target Separation
        ↓
Train-Test Split (80:20, Stratified)
        ↓
StandardScaler → SVM
        ↓
SVM (RBF + Balanced Class Weight)
        ↓
Gaussian Naive Bayes → Original Features
        ↓
Confusion Matrix
        ↓
Classification Report
        ↓
Model Comparison
        ↓
Naive Bayes Selected as Best Model
```

---

# 📌 Conclusion

Day 37 menunjukkan perbandingan dua pendekatan klasifikasi yang berbeda untuk memprediksi kualitas red wine.

**SVM** memberikan recall yang lebih tinggi untuk wine berkualitas baik, sehingga lebih sensitif dalam menemukan kelas positif.

Sementara itu, **Gaussian Naive Bayes** memberikan accuracy yang lebih tinggi, precision kelas `Baik` yang lebih baik, serta recall kelas `Tidak Baik` yang lebih tinggi.

Berdasarkan hasil eksperimen pada notebook, **Naive Bayes menjadi model terbaik** dengan accuracy **86.56%**.

Namun, karena kelas `Baik` hanya sekitar **13.6%** dari dataset, model masih membutuhkan pengembangan lebih lanjut terutama pada penanganan class imbalance dan peningkatan precision untuk kelas positif.

---

## 👤 Author

**Arief Wicaksono**  
Aspiring AI Engineer | Data Science Student
