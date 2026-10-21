# Day 36 — Prediction Customer Churn Using Random Forest & Feature Importance

## 📌 Project Overview

Bagi perusahaan, mempertahankan pelanggan lama jauh lebih murah daripada mencari pelanggan baru. Karena itu, memprediksi pelanggan yang berpotensi **berhenti berlangganan (churn)** dapat membantu perusahaan mengambil tindakan lebih awal.

Pada project ini saya membangun model **Random Forest Classifier** untuk memprediksi customer churn sekaligus menganalisis faktor-faktor yang paling berpengaruh menggunakan **Feature Importance**.

### 🎯 Tujuan Pembelajaran

- Membersihkan data dan menangani kolom bermasalah seperti `TotalCharges`.
- Melakukan encoding pada banyak fitur kategorikal.
- Melakukan Exploratory Data Analysis terhadap pola churn.
- Membagi dataset menggunakan train-test split dengan stratification.
- Membangun model **Random Forest Classifier**.
- Mengevaluasi model menggunakan accuracy, confusion matrix, dan classification report.
- Menganalisis fitur yang paling berpengaruh menggunakan `feature_importances_`.

---

## 📊 Dataset

Dataset yang digunakan adalah **Telco Customer Churn** dari Kaggle.

**Sumber:** Kaggle — Telco Customer Churn  
**Target:** `Churn`

| Value | Meaning |
|---|---|
| `No` | Customer tetap berlangganan |
| `Yes` | Customer melakukan churn |

Dataset memiliki **7.043 baris dan 21 kolom**.

Beberapa fitur yang digunakan:

| Feature | Description |
|---|---|
| `gender` | Gender customer |
| `SeniorCitizen` | Status senior citizen |
| `Partner` | Status memiliki pasangan |
| `Dependents` | Status memiliki tanggungan |
| `tenure` | Lama berlangganan dalam bulan |
| `InternetService` | Jenis layanan internet |
| `OnlineSecurity` | Status layanan online security |
| `TechSupport` | Status layanan technical support |
| `Contract` | Jenis kontrak |
| `PaymentMethod` | Metode pembayaran |
| `MonthlyCharges` | Tagihan bulanan |
| `TotalCharges` | Total tagihan |
| `Churn` | Target klasifikasi |

---

## 🔍 Exploratory Data Analysis

### 1. Churn Distribution

Distribusi target menunjukkan:

- **73%** customer tidak churn.
- **27%** customer melakukan churn.
- Sekitar **5.174 customer** tidak churn.
- Sekitar **1.869 customer** churn.

Hal ini menunjukkan bahwa dataset memiliki **class imbalance**, sehingga evaluasi tidak cukup hanya menggunakan accuracy.

### 2. Churn Berdasarkan Contract

EDA menunjukkan bahwa customer dengan kontrak **Month-to-month** memiliki risiko churn yang lebih tinggi dibandingkan customer dengan kontrak satu tahun atau dua tahun.

Hal ini memberikan insight bisnis bahwa perusahaan dapat mempertimbangkan strategi seperti:

- memberikan insentif kepada customer kontrak bulanan;
- menawarkan promosi untuk berpindah ke kontrak jangka panjang;
- meningkatkan retention pada customer dengan kontrak bulanan.

### 3. Tenure vs Churn

Customer yang tidak churn memiliki tenure yang lebih panjang.

Berdasarkan analisis notebook:

- Median tenure customer **No Churn** sekitar **38 bulan**.
- Median tenure customer **Churn** sekitar **10 bulan**.

Artinya, customer pada masa awal berlangganan merupakan kelompok yang perlu mendapatkan perhatian lebih besar untuk mengurangi kemungkinan churn.

---

## 🧹 Data Cleaning

Beberapa masalah ditemukan pada dataset.

### `TotalCharges`

Kolom `TotalCharges` awalnya bertipe `object`, walaupun secara konsep merupakan nilai numerik.

Ditemukan **11 baris** yang memiliki nilai berupa spasi/kosong.

Langkah preprocessing:

1. Mengubah spasi menjadi `NaN`.
2. Mengubah `TotalCharges` menjadi tipe numerik.
3. Mengisi nilai yang hilang dengan `0`.
4. Menghapus `customerID` karena hanya berfungsi sebagai identifier.

Setelah cleaning:

- Dataset tetap memiliki **7.043 baris**.
- Jumlah kolom menjadi **20**.
- `TotalCharges` sudah bertipe numerik.
- Tidak terdapat missing value.

---

## 🔄 Preprocessing

Karena dataset memiliki banyak fitur kategorikal, dilakukan encoding menggunakan **LabelEncoder**.

Target:

```text
No  → 0
Yes → 1
```

Kemudian seluruh kolom bertipe `object` juga dilakukan encoding menggunakan `LabelEncoder`.

Setelah itu dataset dipisahkan menjadi:

```text
X = Features
y = Target (Churn)
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

| Dataset | Jumlah |
|---|---:|
| Training | 5.634 |
| Testing | 1.409 |

Proporsi target tetap konsisten:

```text
No Churn  → 73%
Churn     → 27%
```

---

## 🌲 Modeling — Random Forest

Model yang digunakan adalah **Random Forest Classifier**.

Random Forest merupakan ensemble dari banyak Decision Tree yang menggunakan subset data dan fitur secara acak, kemudian menggabungkan hasil prediksi dari tree-tree tersebut.

Konfigurasi model:

```python
RandomForestClassifier(
    n_estimators=300,
    n_jobs=1,
    random_state=42
)
```

Model kemudian dilatih menggunakan data training dan digunakan untuk memprediksi data testing.

---

## 📈 Model Evaluation

### Accuracy

Model menghasilkan:

```text
Accuracy: 0.79
```

atau sekitar **79% accuracy** pada data testing.

### Confusion Matrix

```text
[[929 106]
 [185 189]]
```

Interpretasi:

| | Predicted 0 | Predicted 1 |
|---|---:|---:|
| Actual 0 | 929 | 106 |
| Actual 1 | 185 | 189 |

### Classification Report

| Class | Precision | Recall | F1-Score | Support |
|---|---:|---:|---:|---:|
| 0 — No Churn | 0.83 | 0.90 | 0.86 | 1035 |
| 1 — Churn | 0.64 | 0.51 | 0.57 | 374 |
| **Accuracy** | | | **0.79** | **1409** |
| Macro Avg | 0.74 | 0.70 | 0.71 | 1409 |
| Weighted Avg | 0.78 | 0.79 | 0.79 | 1409 |

### Interpretasi

Model memiliki performa yang cukup baik dalam mengenali customer yang **tidak churn**:

- Recall class 0 = **0.90**
- F1-score class 0 = **0.86**

Namun performanya masih kurang optimal untuk mendeteksi customer yang benar-benar **churn**:

- Precision = **0.64**
- Recall = **0.51**
- F1-score = **0.57**

Artinya, model masih cukup sering gagal mendeteksi customer yang sebenarnya akan melakukan churn.

---

## ⭐ Feature Importance

Untuk mengetahui faktor yang paling berpengaruh terhadap prediksi model, digunakan:

```python
rf.feature_importances_
```

Kemudian nilai importance diurutkan dan divisualisasikan menggunakan bar plot.

Feature Importance digunakan untuk membantu memahami fitur mana yang paling banyak berkontribusi terhadap keputusan model Random Forest.

> **Catatan:** Feature importance menunjukkan kontribusi fitur terhadap model, bukan berarti fitur tersebut secara langsung menyebabkan customer melakukan churn.

---

## 🔄 Machine Learning Workflow

```text
Telco Customer Churn Dataset
            ↓
      Initial Inspection
            ↓
     Data Cleaning
            ↓
  Handle TotalCharges
            ↓
    Drop customerID
            ↓
 Exploratory Data Analysis
            ↓
     Encode Features
            ↓
   Train-Test Split
       (80/20)
            ↓
     Random Forest
       Classifier
            ↓
       Prediction
            ↓
   Model Evaluation
            ↓
 Confusion Matrix &
 Classification Report
            ↓
   Feature Importance
```

---

## 💡 Key Findings

Beberapa insight utama dari project:

1. Dataset memiliki class imbalance dengan sekitar **73% customer tidak churn dan 27% customer churn**.
2. Customer dengan kontrak **Month-to-month** lebih rentan melakukan churn.
3. Customer dengan **tenure lebih pendek** memiliki kecenderungan churn yang lebih tinggi.
4. Random Forest memperoleh accuracy sekitar **79%**.
5. Model sangat baik dalam mengenali customer yang tidak churn, tetapi recall untuk class churn masih relatif rendah (**0.51**).
6. Feature Importance dapat digunakan untuk memahami fitur yang paling berkontribusi terhadap prediksi model.

---

## ⚠️ Limitations

Beberapa hal masih dapat dikembangkan dari project ini:

- Melakukan **hyperparameter tuning** pada Random Forest.
- Menggunakan **Cross-Validation**.
- Membandingkan dengan model lain seperti Logistic Regression, XGBoost, atau Gradient Boosting.
- Menangani class imbalance dengan pendekatan seperti `class_weight` atau teknik resampling.
- Mengoptimalkan **recall class churn**, karena mendeteksi customer yang berpotensi churn merupakan tujuan penting dalam kasus ini.
- Mengevaluasi model menggunakan metrik tambahan seperti ROC-AUC dan PR-AUC.
- Menggunakan preprocessing yang lebih robust untuk fitur kategorikal.

---

## 🛠️ Tech Stack

- Python
- Pandas
- NumPy
- Matplotlib
- Seaborn
- Scikit-learn
- Random Forest
- Jupyter Notebook / Google Colab


## 📌 Conclusion

Project ini menunjukkan bagaimana **Random Forest** dapat digunakan untuk melakukan prediksi customer churn pada dataset Telco Customer Churn.

Model berhasil memperoleh **79% accuracy**, tetapi hasil evaluasi menunjukkan bahwa tantangan utama berada pada pendeteksian customer yang benar-benar churn dengan recall sebesar **0.51**.

Karena itu, untuk kasus churn prediction, peningkatan **recall pada class churn** menjadi salah satu area penting yang dapat dikembangkan pada tahap berikutnya.

---

## 👨‍💻 Author

**Arief Wicaksono**  
Aspiring AI Engineer | Data Science Student

---
