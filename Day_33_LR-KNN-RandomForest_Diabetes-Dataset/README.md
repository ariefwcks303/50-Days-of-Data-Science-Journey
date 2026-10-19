# Day 33 — Diabetes Prediction: Classification (LR, KNN & RF)

## 📌 Project Overview

Proyek ini bertujuan memprediksi status diabetes pasien berdasarkan data pemeriksaan kesehatan menggunakan tiga algoritma machine learning: **Logistic Regression, K-Nearest Neighbors (KNN), dan Random Forest Classifier**.

Fokus eksperimen adalah memahami pengaruh feature scaling terhadap algoritma berbasis jarak, membandingkan performa klasifikasi, serta mengevaluasi trade-off antara accuracy dan recall dalam konteks risiko medis.

## 🎯 Learning Objectives

- Melakukan Exploratory Data Analysis (EDA) pada data kesehatan.
- Mengidentifikasi nilai nol yang tidak valid secara medis.
- Menangani missing values menggunakan median imputation.
- Memahami pengaruh StandardScaler terhadap KNN.
- Membandingkan Logistic Regression, KNN, dan Random Forest.
- Mengevaluasi accuracy, precision, recall, F1-score, dan confusion matrix.
- Menginterpretasikan feature importance dan risiko false negative.

## 📂 Dataset

**Dataset:** Pima Indians Diabetes Dataset

**Reference:** [Kaggle — Diabetes Dataset](https://www.kaggle.com/datasets/akshaydattatraykhare/diabetes-dataset)

**Data loading:** CSV diunduh dari repository Plotly melalui GitHub.

| Attribute | Value |
|---|---|
| Total observations | 768 |
| Total columns | 9 |
| Predictor features | 8 |
| Target | `Outcome` |
| Problem type | Binary Classification |

Target labels:

- `0`: Tidak diabetes berdasarkan label dataset.
- `1`: Diabetes berdasarkan label dataset.

### Feature Description

| Feature | Description |
|---|---|
| `Pregnancies` | Jumlah kehamilan |
| `Glucose` | Kadar glukosa plasma |
| `BloodPressure` | Tekanan darah diastolik |
| `SkinThickness` | Ketebalan lipatan kulit trisep |
| `Insulin` | Kadar insulin serum |
| `BMI` | Body Mass Index |
| `DiabetesPedigreeFunction` | Indikator riwayat diabetes keluarga |
| `Age` | Usia pasien |

## 🛠️ Tech Stack

- Python
- Pandas & NumPy
- Matplotlib & Seaborn
- Scikit-learn
- Jupyter Notebook

## 🔍 Project Workflow

### 1. Exploratory Data Analysis

Eksplorasi awal dilakukan menggunakan `info()`, `describe()`, dan pemeriksaan missing values.

Dataset tidak memiliki nilai `NaN` eksplisit. Namun, beberapa fitur medis memiliki nilai nol yang tidak realistis.

Visualisasi mencakup:

- Distribusi kelas target.
- Boxplot glukosa berdasarkan status diabetes.
- Heatmap korelasi antarfitur.

Distribusi target:

| Class | Proportion |
|---|---:|
| Non-diabetes (0) | 65% |
| Diabetes (1) | 35% |

Distribusi tersebut menunjukkan class imbalance, sehingga accuracy saja tidak cukup untuk menilai kemampuan deteksi diabetes.

### 2. Data Cleaning & Median Imputation

Nilai nol pada fitur tertentu diubah menjadi `NaN`, kemudian diimputasi menggunakan median.

| Feature | Invalid Zero Values |
|---|---:|
| `Glucose` | 5 |
| `BloodPressure` | 35 |
| `SkinThickness` | 227 |
| `Insulin` | 374 |
| `BMI` | 11 |

Setelah imputasi, tidak ada missing values yang tersisa.

**Engineering note:** Notebook melakukan imputasi sebelum train-test split. Untuk menghindari data leakage, pendekatan yang lebih tepat adalah mempelajari median hanya dari training set menggunakan `SimpleImputer` dan `Pipeline`.

### 3. Train-Test Split & Feature Scaling

Data dibagi menggunakan:

- Training set: 80%
- Test set: 20%
- `random_state=42`

Standardisasi menggunakan `StandardScaler`:

- `fit_transform()` pada training set.
- `transform()` pada test set.

Logistic Regression dan KNN menggunakan fitur yang telah distandardisasi, sedangkan Random Forest menggunakan fitur tanpa scaling.

### 4. Scaling Experiment — KNN

Eksperimen dilakukan menggunakan KNN dengan `n_neighbors=5`.

| Experiment | Accuracy |
|---|---:|
| KNN without scaling | 68% |
| KNN with scaling | 72% |

Standardisasi meningkatkan akurasi KNN sebesar **4 percentage points** pada eksperimen ini.

Hal tersebut menunjukkan bahwa perbedaan skala fitur dapat memengaruhi perhitungan jarak pada algoritma KNN.

Notebook juga membandingkan akurasi KNN untuk nilai `k` dari 1 hingga 20.

### 5. Model Development

Tiga algoritma yang dibandingkan:

**Logistic Regression**
- `max_iter=1000`
- `random_state=42`
- Menggunakan standardized features.

**K-Nearest Neighbors**
- `n_neighbors=11`
- Menggunakan standardized features.

**Random Forest Classifier**
- `n_estimators=200`
- `random_state=42`
- Menggunakan unscaled features.

## 📊 Model Evaluation

### Model Comparison

| Metric | Logistic Regression | KNN (k=11) | Random Forest |
|---|---:|---:|---:|
| Accuracy | 75% | **77%** | 75% |
| Precision (Diabetes) | 0.67 | **0.69** | 0.64 |
| Recall (Diabetes) | 0.62 | 0.64 | **0.71** |
| F1-score (Diabetes) | 0.64 | 0.66 | **0.67** |
| False Negative | 21 | 20 | **16** |

**Best accuracy:** KNN (k=11), dengan akurasi 77%.

**Best diabetes recall:** Random Forest, dengan recall 71%.

### Confusion Matrix

| Model | TN | FP | FN | TP |
|---|---:|---:|---:|---:|
| Logistic Regression | 82 | 17 | 21 | 34 |
| KNN (k=11) | 83 | 16 | 20 | 35 |
| Random Forest | 77 | 22 | 16 | 39 |

Dalam konteks screening diabetes, false negative perlu diperhatikan karena menunjukkan kasus diabetes yang diprediksi sebagai non-diabetes.

Random Forest menghasilkan false negative paling sedikit dalam pengujian ini, tetapi memiliki false positive lebih banyak.

Dengan demikian, pemilihan model bergantung pada prioritas evaluasi dan konsekuensi kesalahan prediksi.

## 🌳 Random Forest Feature Importance

| Feature | Importance |
|---|---:|
| `Glucose` | 0.257 |
| `BMI` | 0.166 |
| `Age` | 0.141 |
| `DiabetesPedigreeFunction` | 0.119 |
| `Insulin` | 0.090 |
| `BloodPressure` | 0.081 |
| `SkinThickness` | 0.073 |
| `Pregnancies` | 0.072 |

Tiga fitur dengan importance tertinggi adalah **Glucose, BMI, dan Age**.

Feature importance menunjukkan kontribusi relatif fitur terhadap prediksi Random Forest, bukan bukti hubungan sebab-akibat.

## 💡 Key Insights

1. **Feature scaling matters.** StandardScaler meningkatkan performa KNN karena algoritma ini bergantung pada jarak antarobservasi.

2. **Accuracy is not enough.** KNN memiliki akurasi tertinggi, tetapi Random Forest memiliki recall tertinggi untuk kelas diabetes.

3. **False negatives are important.** Dalam konteks screening, kasus diabetes yang terlewat dapat memiliki konsekuensi serius.

4. **Model selection requires trade-offs.** Pemilihan model perlu mempertimbangkan precision, recall, dan konsekuensi false positive maupun false negative.

5. **Glucose is the strongest feature in this experiment.** Berdasarkan feature importance Random Forest, Glucose memberikan kontribusi terbesar terhadap prediksi.

## ⚠️ Limitations

- Imputasi median dilakukan sebelum train-test split sehingga berpotensi menyebabkan data leakage.
- Train-test split tidak menggunakan stratifikasi kelas.
- Pemilihan nilai `k` dievaluasi menggunakan test set, sehingga berpotensi menimbulkan bias pemilihan hyperparameter.
- Evaluasi hanya menggunakan satu train-test split.
- Belum dilakukan cross-validation dan hyperparameter tuning yang terpisah dari test set.
- Belum dilakukan threshold optimization dan external validation.
- Model belum divalidasi untuk penggunaan klinis dan tidak dapat menggantikan diagnosis tenaga medis.

## 🚀 Future Improvements

- Menggunakan `Pipeline` dan `SimpleImputer` untuk mencegah data leakage.
- Menggunakan stratified train-test split.
- Melakukan pemilihan hyperparameter menggunakan cross-validation pada training set.
- Mengevaluasi ROC-AUC, PR-AUC, specificity, dan probability calibration.
- Mengeksplorasi threshold classification berdasarkan kebutuhan screening.
- Melakukan validasi menggunakan dataset eksternal.

## 📚 Learning Outcomes

Melalui notebook ini, saya mempraktikkan:

- Data cleaning pada dataset medis.
- Median imputation untuk invalid zero values.
- Exploratory Data Analysis dan correlation analysis.
- StandardScaler dan pengaruhnya terhadap KNN.
- Logistic Regression, KNN, dan Random Forest Classification.
- Confusion matrix dan classification report.
- Model comparison berdasarkan accuracy dan recall.
- Feature importance interpretation.
- Trade-off antara performa prediksi dan risiko kesalahan klasifikasi.

## 📁 Project Structure

```text
Day_33_Prediction_Diabetes_Classification/
├── Day_33_Prediction_Diabetes_Classification.ipynb
└── README.md
```

## 📝 Conclusion

Pada eksperimen Day 33, **KNN (k=11)** menghasilkan akurasi tertinggi sebesar **77%**, sedangkan **Random Forest** menghasilkan recall tertinggi untuk kelas diabetes sebesar **71%**.

Eksperimen juga menunjukkan peningkatan akurasi KNN dari 68% menjadi 72% setelah feature scaling pada `k=5`.

Hasil ini menegaskan bahwa pemilihan model tidak hanya bergantung pada accuracy, tetapi juga pada tujuan prediksi, karakteristik data, dan konsekuensi kesalahan klasifikasi.

Proyek ini merupakan eksperimen pembelajaran machine learning dan bukan sistem diagnosis medis.

---

## 👤 Author

**Arief Wicaksono**  

*Aspiring AI Engineer | Data Science Student*

---

⭐ If you find this project useful, feel free to star the repository!