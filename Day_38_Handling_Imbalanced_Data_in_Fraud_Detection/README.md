# Day 38 --- Handling Imbalanced Data in Fraud Detection

## 📌 Project Overview

Day 38 membahas **imbalanced data** melalui kasus **deteksi transaksi fraud kartu kredit**.

Fokus utama notebook: 
- Memahami mengapa **accuracy dapat menyesatkan** pada imbalanced classification. 
- Membuat sample ringan dari dataset besar. 
- Membandingkan Logistic Regression **baseline** dengan`class_weight="balanced"`. 
- Mengevaluasi **precision, recall, F1-score, PR-AUC**, dan Precision-Recall Curve. 
- Memahami trade-off **precision vs recall** melalui threshold tuning. 
- Memahami konsep undersampling dan SMOTE.

## 📊 Dataset

Dataset yang digunakan adalah **Credit Card Fraud Detection** dari Kaggle.

**Source:** Kaggle --- `mlg-ulb/creditcardfraud`

Dataset asli: 
- **284,807 baris** 
- **31 kolom** - Fraud hanya sekitar **0.17%** 
- Tidak ada missing value.

  Kolom         Arti
  ------------- -----------------------------------
  `Time`        Detik sejak transaksi pertama
  `V1`--`V28`   Fitur hasil transformasi PCA
  `Amount`      Nominal transaksi
  `Class`       Target: `0 = Normal`, `1 = Fraud`

### Sample Dataset

Karena dataset penuh cukup besar, notebook mengambil: 
- **492 seluruh transaksi fraud** 
- **8,000 transaksi normal** secara random dengan `random_state=42`

Total sample: **8,492 transaksi** dengan proporsi fraud sekitar **5.79%**.

## 🔍 Exploratory Data Analysis

EDA mencakup: 
- Distribusi kelas Normal vs Fraud. 
- Perbandingan `Amount` antara transaksi normal dan fraud. 
- Visualisasi class imbalance. 
- Analisis bahwa sample masih tetap imbalanced meskipun proporsi fraud sudah lebih representatif daripada dataset asli.

## 🧹 Data Preparation

Target:

``` python
y = df_sample["Class"]
```

Feature:

``` python
X = df_sample.drop("Class", axis=1)
```

Dataset dibagi secara stratified: 
- Training: **6,793 rows** 
- Testing: **1,699 rows** 
- Fraud training: **5.80%**
- Fraud testing: **5.77%**

Standardisasi menggunakan `StandardScaler`:

``` python
scaler = StandardScaler()

X_train = scaler.fit_transform(X_train)
X_test = scaler.transform(X_test)
```

## 🤖 Modeling

### 1. Baseline Logistic Regression

``` python
LogisticRegression(
    max_iter=1000,
    random_state=42
)
```

### 2. Balanced Logistic Regression

``` python
LogisticRegression(
    max_iter=1000,
    random_state=42,
    class_weight="balanced"
)
```

`class_weight="balanced"` membuat model lebih memperhatikan kelas fraud
sebagai kelas minoritas.

## 📈 Model Comparison

  Model                            Accuracy   Precision Fraud   Recall Fraud   F1 Fraud
  ------------------------------ ---------- ----------------- -------------- ----------
  Baseline Logistic Regression       99.00%            96.55%         85.71%       0.91
  Balanced Logistic Regression       97.06%            68.46%     **90.82%**       0.78

### Insight

Baseline memiliki accuracy dan precision lebih tinggi, tetapi lebih
banyak fraud yang lolos.

Balanced Logistic Regression: 
- Accuracy turun menjadi **97.06%** 
- Precision Fraud turun menjadi **68.46%** 
- Recall Fraud naik dari **85.71% → 90.82%**

Untuk fraud detection, peningkatan recall penting karena **False
Negative berarti transaksi fraud lolos dari sistem**.

## 🧩 Confusion Matrix --- Balanced Model

  Actual / Predicted     Normal   Fraud
  -------------------- -------- -------
  Normal                   1560      41
  Fraud                       9      89

Interpretasi: 
- **1,560** normal → normal 
- **89** fraud → fraud 
- **41** normal → fraud (**False Positive**) 
- **9** fraud → normal (**False Negative**)

Model berhasil menangkap **89 dari 98 transaksi fraud** pada data test.

## 📐 Precision-Recall Curve & PR-AUC

Untuk imbalanced classification, notebook menggunakan Precision-Recall
Curve dan **PR-AUC**.

PR-AUC dihitung dengan:

``` python
average_precision_score(y_test, prob)
```

Hasil notebook:

> **PR-AUC = 0.94**

Nilai ini menunjukkan performa kuat pada deteksi kelas minoritas pada
sample yang digunakan.

## 🎚️ Threshold Tuning

Notebook membandingkan threshold `0.3`, `0.5`, dan `0.7`.

    Threshold   Precision      Recall   Fraud Detected
  ----------- ----------- ----------- ----------------
          0.3       49.7%   **94.9%**          93 / 98
          0.5       68.5%       90.8%          89 / 98
          0.7   **80.7%**       89.8%          88 / 98

### Interpretation

**Threshold 0.3** 
- Recall tertinggi: **94.9%** 
- Menangkap **93/98 fraud** 
- Precision turun menjadi **49.7%** 
- Lebih banyak false alarm

**Threshold 0.5** 
- Recall **90.8%** 
- Menangkap **89/98 fraud** 
- Precision **68.5%** 
- Menjadi titik kompromi antara keamanan dan false alarm

**Threshold 0.7** 
- Precision tertinggi: **80.7%** 
- Recall **89.8%** 
- Menangkap **88/98 fraud** 
- Lebih sedikit false alarm

Pemilihan threshold bergantung pada **risk tolerance** dan biaya False
Positive vs False Negative.

## 🔄 Resampling & SMOTE

Notebook membahas dua pendekatan:

### Undersampling

Mengurangi data dari kelas mayoritas. Sampling awal notebook menggunakan
seluruh fraud dan sekitar 8,000 transaksi normal.

### SMOTE

**SMOTE (Synthetic Minority Over-sampling Technique)** membuat contoh
sintetis untuk kelas minoritas berdasarkan nearest neighbors.

> ⚠️ Notebook **tidak mengimplementasikan SMOTE secara langsung**. SMOTE
> hanya dibahas sebagai pendekatan lanjutan; eksperimen utama
> menggunakan `class_weight="balanced"`.

Resampling harus dilakukan **hanya pada data training setelah train-test
split** agar tidak terjadi data leakage.

## 🧠 Key Learning

### 1. Jangan hanya melihat Accuracy

Accuracy baseline mencapai **99%**, tetapi Recall Fraud masih **85.7%**.

### 2. Class Weight meningkatkan Recall

`class_weight="balanced"` meningkatkan Recall Fraud menjadi **90.8%**,
dengan konsekuensi precision turun menjadi **68.5%**.

### 3. Threshold adalah Business Decision

-   `0.3` → fokus menangkap fraud sebanyak mungkin.
-   `0.5` → kompromi keamanan dan false alarm.
-   `0.7` → fokus mengurangi false alarm.

## 🚧 Limitations

-   Modeling menggunakan sample dari dataset asli.
-   Fitur `V1`--`V28` bersifat anonim karena transformasi PCA.
-   Model yang digunakan hanya Logistic Regression.
-   SMOTE belum diimplementasikan.
-   Belum dilakukan cross-validation.
-   Belum dilakukan hyperparameter tuning.
-   Belum dilakukan cost-sensitive evaluation secara eksplisit.
-   Belum ada deployment dan monitoring.

## 🚀 Next Steps

-   Implementasi **SMOTE** dengan `imbalanced-learn`.
-   Bandingkan Logistic Regression dengan Random Forest, Gradient
    Boosting, atau XGBoost.
-   Hyperparameter tuning.
-   Cross-validation.
-   Optimasi threshold berdasarkan business cost.
-   Analisis ROC-AUC dan PR-AUC lebih lanjut.
-   Membuat cost matrix untuk False Positive dan False Negative.
-   Model monitoring untuk perubahan pola fraud.

## 🛠️ Tech Stack

-   Python
-   Pandas
-   NumPy
-   Matplotlib
-   Seaborn
-   Scikit-learn
-   Logistic Regression
-   StandardScaler
-   Precision / Recall / F1-score
-   Precision-Recall Curve
-   PR-AUC
-   Google Colab / Jupyter Notebook

## 📌 Workflow

``` text
Credit Card Fraud Dataset
        ↓
Initial Exploration
        ↓
Check Missing Values
        ↓
Create Sample
(All Fraud + 8,000 Normal)
        ↓
Class Imbalance Analysis
        ↓
EDA & Visualization
        ↓
Feature / Target Separation
        ↓
Stratified Train-Test Split
        ↓
StandardScaler
        ↓
Baseline Logistic Regression
        ↓
Balanced Logistic Regression
        ↓
Classification Report
        ↓
Confusion Matrix
        ↓
Precision-Recall Curve
        ↓
PR-AUC
        ↓
Threshold Tuning
        ↓
SMOTE / Resampling Discussion
```

## 🎯 Conclusion

Day 38 menunjukkan bahwa **imbalanced classification membutuhkan metrik
dan strategi evaluasi yang tepat**.

Baseline Logistic Regression mendapatkan accuracy sekitar **99%**,
tetapi Recall Fraud hanya **85.7%**. Dengan `class_weight="balanced"`,
Recall Fraud meningkat menjadi **90.8%**, meskipun precision turun
menjadi **68.5%**.

Threshold tuning menunjukkan bahwa model dapat disesuaikan dengan
kebutuhan bisnis. Threshold `0.3` menangkap **93 dari 98 fraud**,
sedangkan threshold `0.7` memberikan precision lebih tinggi untuk
mengurangi false alarm.

> **Key Takeaway:** Pada fraud detection, jangan hanya mengejar
> accuracy. Fokus pada **Recall, Precision, PR-AUC, threshold, dan
> business cost**.

## 👨‍💻 Author

**Arief Wicaksono**\
Aspiring AI Engineer \| Data Science Student
