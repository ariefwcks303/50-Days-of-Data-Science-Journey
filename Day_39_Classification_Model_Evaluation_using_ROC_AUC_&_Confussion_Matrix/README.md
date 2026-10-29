# Day 39 — Evaluation Classification Model Using ROC, AUC & Confusion Matrix

## 📌 Overview

Pada Day 39, fokus pembelajaran adalah **mengevaluasi model klasifikasi secara menyeluruh**, bukan hanya melihat accuracy.

Studi kasus yang digunakan adalah **prediksi penyakit jantung** menggunakan Heart Disease Dataset. Model yang digunakan adalah **Logistic Regression**, kemudian dievaluasi menggunakan Confusion Matrix, Classification Report, ROC Curve, ROC-AUC, dan Precision-Recall Curve.

Tujuan utama notebook ini adalah memahami bagaimana membaca performa model dari berbagai sudut pandang, terutama pada kasus prediksi medis yang memiliki konsekuensi berbeda antara False Positive dan False Negative.

---

## 🎯 Learning Objectives

- Memahami cara membaca **Confusion Matrix**.
- Memahami **Precision, Recall, dan F1-Score**.
- Membuat dan menginterpretasikan **ROC Curve**.
- Memahami **ROC-AUC** sebagai ukuran kemampuan model membedakan dua kelas.
- Membuat dan membaca **Precision-Recall Curve**.
- Memahami trade-off antara Precision dan Recall.
- Memahami pentingnya **Recall** pada kasus prediksi penyakit.

---

## 📊 Dataset

Dataset yang digunakan adalah **Heart Disease Dataset** dari Kaggle.

**Source:** `johnsmith88/heart-disease-dataset`

Dataset awal memiliki:

- **1.025 baris**
- **14 kolom**
- Tidak terdapat missing value.
- Target:
  - `0` = tidak ada penyakit jantung
  - `1` = ada penyakit jantung

### Features

| Feature | Description |
|---|---|
| `age` | Usia pasien |
| `sex` | Jenis kelamin |
| `cp` | Tipe nyeri dada |
| `trestbps` | Tekanan darah istirahat |
| `chol` | Kolesterol |
| `fbs` | Gula darah puasa |
| `restecg` | Hasil resting electrocardiographic |
| `thalach` | Detak jantung maksimum |
| `exang` | Exercise-induced angina |
| `oldpeak` | Depresi ST |
| `slope` | Slope segmen ST |
| `ca` | Jumlah pembuluh utama |
| `thal` | Status thalassemia |
| `target` | Target penyakit jantung |

---

## 🧹 Data Cleaning

Pada tahap awal ditemukan:

- Dataset awal: **1.025 rows × 14 columns**
- Missing value: **0**
- Duplicate rows: **723**

Duplicate kemudian dihapus untuk menghindari data yang sama masuk ke proses training dan testing.

Setelah `drop_duplicates()`:

- Dataset menjadi **302 rows × 14 columns**
- Duplicate: **0**
- Missing value: **0**
- Target `1`: **164 data (54.3%)**
- Target `0`: **138 data (45.7%)**

Distribusi target relatif seimbang.

---

## 🔎 Exploratory Data Analysis

EDA dilakukan untuk memahami karakteristik dataset dan hubungan antara fitur dengan target.

Beberapa analisis yang dilakukan:

### Target Distribution

Distribusi target setelah penghapusan duplicate:

- 54.3% kelas `1`
- 45.7% kelas `0`

### Age vs Maximum Heart Rate

Notebook menggunakan scatter plot antara:

- `age`
- `thalach`
- `target`

Visualisasi ini digunakan untuk melihat pola hubungan usia dan detak jantung maksimum terhadap target penyakit jantung.

---

## ✂️ Train-Test Split

Dataset dibagi menjadi data training dan testing.

| Dataset | Shape |
|---|---:|
| X_train | `(241, 13)` |
| y_train | `(241,)` |
| X_test | `(61, 13)` |
| y_test | `(61,)` |

---

## ⚙️ Feature Scaling

Fitur distandardisasi menggunakan `StandardScaler` sebelum digunakan oleh Logistic Regression.

```python
scaler = StandardScaler()

X_train_scale = scaler.fit_transform(X_train)
X_test_scale = scaler.transform(X_test)
```

Scaler di-fit menggunakan data training kemudian digunakan untuk mentransformasi data testing.

---

## 🤖 Modeling

Model yang digunakan:

```python
LogisticRegression(
    max_iter=1000,
    random_state=42
)
```

### Test Accuracy

Model menghasilkan:

> **Accuracy = 80.3%**

Model berhasil memprediksi **49 dari 61** data testing dengan benar.

---

## 📏 Model Evaluation

### 1. Confusion Matrix

Confusion Matrix:

| Actual \\ Predicted | 0 | 1 |
|---|---:|---:|
| **0** | 21 | 7 |
| **1** | 5 | 28 |

Interpretasi:

- **TN = 21** → pasien sehat diprediksi sehat.
- **FP = 7** → pasien sehat diprediksi sakit.
- **FN = 5** → pasien sakit diprediksi sehat.
- **TP = 28** → pasien sakit diprediksi sakit.

Dalam konteks medis, **False Negative menjadi perhatian penting**, karena pasien yang sebenarnya sakit tetapi diprediksi sehat dapat terlewat dari penanganan.

---

### 2. Classification Report

| Class | Precision | Recall | F1-Score | Support |
|---|---:|---:|---:|---:|
| Sehat | 0.81 | 0.75 | 0.78 | 28 |
| Sakit | 0.80 | 0.85 | 0.82 | 33 |
| **Accuracy** | | | **0.80** | **61** |
| Macro Avg | 0.80 | 0.80 | 0.80 | 61 |
| Weighted Avg | 0.80 | 0.80 | 0.80 | 61 |

### Interpretation

Untuk kelas **Sakit (1)**:

- Precision = **80%**
- Recall = **85%**
- F1-score = **82%**

Recall menjadi metrik penting karena menunjukkan kemampuan model dalam menangkap pasien yang benar-benar memiliki penyakit jantung.

---

## 📈 ROC Curve & AUC

ROC Curve digunakan untuk melihat hubungan antara:

- **True Positive Rate (TPR / Recall)**
- **False Positive Rate (FPR)**

pada berbagai threshold.

Hasil notebook:

> **ROC-AUC = 0.87**

Nilai ini menunjukkan kemampuan diskriminasi model yang baik dalam membedakan pasien sehat dan pasien dengan penyakit jantung.

---

## 🎯 Precision-Recall Curve

Notebook juga menggunakan Precision-Recall Curve untuk melihat trade-off antara Precision dan Recall pada berbagai threshold.

Hasil evaluasi:

> **Average Precision (AP) = 0.90**

Metric ini memberikan informasi tambahan mengenai performa model pada kelas positif.

---

## ⚖️ Precision vs Recall Trade-Off

Untuk kasus prediksi penyakit jantung, pemilihan threshold merupakan hal penting.

- **Recall tinggi** → lebih banyak pasien sakit berhasil terdeteksi.
- **False Negative rendah** → lebih sedikit pasien sakit yang terlewat.
- **False Positive meningkat** → lebih banyak pasien sehat yang mungkin perlu pemeriksaan tambahan.

Notebook merekomendasikan eksperimen threshold, misalnya menurunkan threshold default `0.5` menjadi `0.4` atau `0.3`, apabila tujuan utama adalah menekan False Negative.

---

## 💡 Key Insights

1. **Accuracy saja tidak cukup** untuk mengevaluasi model klasifikasi.
2. Confusion Matrix membantu memahami jenis kesalahan model.
3. Logistic Regression menghasilkan **accuracy 80.3%**.
4. Model memperoleh **ROC-AUC 0.87**.
5. Recall kelas `Sakit` mencapai **85%**.
6. Precision kelas `Sakit` mencapai **80%**.
7. Precision-Recall Curve menghasilkan **Average Precision 0.90**.
8. Dalam konteks medis, False Negative perlu mendapat perhatian khusus.
9. Threshold dapat disesuaikan sesuai kebutuhan dan risiko aplikasi.
10. Fitur seperti `cp` dan `thalach` disebut dalam notebook memiliki hubungan kuat dengan target positif berdasarkan visualisasi EDA.

---

## ⚠️ Limitations

Beberapa hal yang masih dapat dikembangkan:

- Dataset menjadi jauh lebih kecil setelah penghapusan duplicate.
- Model yang digunakan hanya Logistic Regression.
- Belum dilakukan cross-validation.
- Belum dilakukan hyperparameter tuning.
- Belum dibandingkan dengan model lain seperti Random Forest, SVM, atau Gradient Boosting.
- Belum dilakukan threshold optimization secara sistematis.
- Konteks medis membutuhkan validasi dan evaluasi yang lebih ketat sebelum digunakan pada sistem nyata.

---

## 🚀 Next Steps

Pengembangan berikutnya dapat mencakup:

- **Cross-Validation**
- Hyperparameter tuning Logistic Regression
- Perbandingan dengan Random Forest
- Perbandingan dengan Support Vector Machine
- Perbandingan dengan Gradient Boosting
- Threshold tuning berdasarkan target recall
- Optimasi False Negative
- Perbandingan ROC-AUC dan PR-AUC antar model
- Evaluasi lebih lanjut sebelum penggunaan pada kasus nyata

---

## 🔄 Workflow

```text
Heart Disease Dataset
        ↓
Initial Exploration
        ↓
Check Missing Values & Duplicates
        ↓
Drop Duplicates
        ↓
Exploratory Data Analysis
        ↓
Train-Test Split
        ↓
StandardScaler
        ↓
Logistic Regression
        ↓
Prediction & Probability
        ↓
Confusion Matrix
        ↓
Classification Report
        ↓
ROC Curve & ROC-AUC
        ↓
Precision-Recall Curve & AP
        ↓
Threshold & Medical Trade-Off Analysis
        ↓
Conclusion & Next Steps
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
- StandardScaler
- Confusion Matrix
- Classification Report
- ROC Curve
- ROC-AUC
- Precision-Recall Curve
- Average Precision
- Google Colab / Jupyter Notebook

---

## 📁 Project Structure

```text
Day_39_Evaluation_Classification_Model_using_ROC_AUC_&_Confusion_Matrix/
│
├── Day_39_Evaluation_Classification_Model_using_ROC_AUC_&_Confusion_Matrix.ipynb
└── README.md
```

---

## 🧠 Conclusion

Day 39 berfokus pada pentingnya melakukan evaluasi model klasifikasi menggunakan lebih dari satu metrik.

Pada studi kasus prediksi penyakit jantung, Logistic Regression menghasilkan accuracy **80.3%**, ROC-AUC **0.87**, recall kelas `Sakit` **85%**, dan Average Precision **0.90**.

Hasil tersebut menunjukkan performa baseline yang cukup baik, tetapi evaluasi tidak berhenti pada accuracy. Confusion Matrix, Precision, Recall, F1, ROC-AUC, dan Precision-Recall Curve memberikan perspektif yang lebih lengkap mengenai kemampuan serta kesalahan model.

Terutama pada kasus medis, **Recall dan False Negative perlu diperhatikan secara serius**, karena kesalahan dalam melewatkan pasien yang sebenarnya sakit dapat memiliki konsekuensi lebih besar dibandingkan False Positive.

---

## 👨‍💻 Author

**Arief Wicaksono**  
Aspiring AI Engineer | Data Science Student

---

⭐ *Learning Machine Learning one day at a time.*