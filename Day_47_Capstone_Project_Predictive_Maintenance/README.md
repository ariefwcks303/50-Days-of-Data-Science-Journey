# Day 47 — Capstone Project: Predictive Maintenance — Failure Prediction pada Mesin Manufaktur

## Overview

Pada Day 47, saya mengerjakan **Capstone Project** dengan menggabungkan tahapan pembelajaran Data Science ke dalam satu studi kasus industri, yaitu **Predictive Maintenance** (perawatan prediktif).

Proyek ini bertujuan memprediksi kemungkinan kegagalan mesin manufaktur berdasarkan data sensor operasional. Dengan pendekatan ini, potensi kerusakan dapat dikenali lebih awal sehingga teknisi dapat melakukan tindakan sebelum terjadi kerusakan yang menyebabkan *unplanned downtime*.

Workflow utama:

**Exploratory Data Analysis → Feature Engineering → Data Preprocessing → Model Training → Model Evaluation → Feature Importance → Business Insights**

## Project Objectives

- Memahami konteks bisnis Predictive Maintenance dan risiko *downtime* pada mesin manufaktur.
- Melakukan Exploratory Data Analysis (EDA) terhadap data sensor mesin.
- Mengidentifikasi pola perbedaan antara mesin normal dan mesin yang mengalami kegagalan.
- Membuat fitur baru berdasarkan karakteristik operasional mesin.
- Menangani *class imbalance* pada target kegagalan.
- Melatih dan membandingkan Random Forest dan Gradient Boosting.
- Mengevaluasi model menggunakan Precision, Recall, F1-score, ROC-AUC, Confusion Matrix, dan Classification Report.
- Mengidentifikasi fitur yang paling berpengaruh terhadap prediksi kegagalan.
- Menyusun insight dan rekomendasi pemeliharaan mesin berdasarkan hasil analisis.

## Dataset

Dataset yang digunakan adalah **Machine Predictive Maintenance Classification** dari Kaggle.

- **Source:** [Kaggle — Machine Predictive Maintenance Classification](https://www.kaggle.com/datasets/shivamb/machine-predictive-maintenance-classification)
- **Rows:** 10,000
- **Columns:** 10
- **Missing values:** 0
- **Duplicate rows:** 0
- **Target:** `Target` (`0` = Normal, `1` = Failure)

Dataset merupakan data sintetis yang dirancang menyerupai data sensor mesin manufaktur. Dataset berisi informasi suhu udara, suhu proses, kecepatan putar, torsi, keausan alat, tipe produk, dan status kegagalan mesin.

### Target Distribution

- `0` (Normal): 9,661 data (96.61%)
- `1` (Failure): 339 data (3.39%)

Distribusi target menunjukkan ketidakseimbangan kelas (*class imbalance*) yang cukup tinggi. Oleh karena itu, evaluasi model tidak cukup hanya menggunakan accuracy. Kemampuan model mendeteksi kelas kegagalan, terutama Recall, menjadi perhatian penting.

## 1. Exploratory Data Analysis

EDA dilakukan untuk memahami karakteristik sensor dan pola yang berkaitan dengan kegagalan mesin.

### Sensor Distribution

- `Air temperature` dan `Process temperature` memiliki distribusi yang relatif menyerupai kurva normal.
- Mayoritas kecepatan putar mesin berada pada kisaran 1,400–1,500 RPM, dengan beberapa nilai tinggi hingga mendekati 3,000 RPM.
- Distribusi `Torque` relatif simetris dengan pusat sekitar 40 Nm.
- `Tool wear` tersebar dari 0 hingga sekitar 200 menit, kemudian jumlah observasi menurun pada durasi yang lebih tinggi.

### Normal Machine vs Failure

Berdasarkan perbandingan box plot:

- Mesin gagal cenderung memiliki median suhu yang sedikit lebih tinggi dibandingkan mesin normal.
- Mesin gagal memiliki median kecepatan putar yang cenderung lebih rendah, tetapi terdapat sejumlah *outlier* pada kecepatan tinggi.
- Mesin gagal menunjukkan nilai `Torque` yang jauh lebih tinggi.
- Mesin gagal juga cenderung memiliki `Tool wear` yang lebih tinggi.

### Correlation Analysis

- `Air temperature` dan `Process temperature` memiliki korelasi positif yang kuat sebesar **0.88**.
- `Rotational speed` dan `Torque` memiliki korelasi negatif yang kuat sebesar **-0.88**.
- Korelasi linear fitur sensor terhadap `Target` relatif rendah.
- `Torque` memiliki korelasi terbesar terhadap target, sekitar **0.19**, diikuti `Tool wear` sekitar **0.11**.

### Failure Ratio by Product Type

Rasio kegagalan berdasarkan tipe produk:

| Product Type | Failure Ratio |
|---|---:|
| L (Low) | 3.92% |
| M (Medium) | 2.77% |
| H (High) | 2.09% |

Produk tipe **L (Low)** memiliki rasio kegagalan tertinggi, sedangkan tipe **H (High)** memiliki rasio kegagalan terendah. Hasil ini menunjukkan adanya kecenderungan tingkat kegagalan yang berbeda berdasarkan kategori produk.

## 2. Feature Engineering

Feature engineering dilakukan untuk membentuk informasi tambahan dari parameter operasional mesin.

Fitur baru yang dibuat:

1. **`temp_diff`**  
   Selisih suhu proses dengan suhu udara:

   ```python
   temp_diff = Process temperature - Air temperature
   ```

   Fitur ini digunakan untuk merepresentasikan perbedaan suhu antara proses mesin dan lingkungan sekitar.

2. **`power`**  
   Perkalian torsi dengan kecepatan putar:

   ```python
   power = Torque * Rotational speed
   ```

   Fitur ini membentuk indikator gabungan antara beban torsi dan kecepatan putar mesin.

3. **One-Hot Encoding**  
   Kolom `Type` diubah menjadi fitur numerik:
   - `Type_H`
   - `Type_L`
   - `Type_M`

Kolom yang tidak digunakan dalam modeling:

- `UDI` — hanya berfungsi sebagai identitas observasi.
- `Product ID` — kode identitas produk.
- `Failure Type` — informasi jenis kegagalan yang berpotensi menyebabkan *data leakage* karena merupakan konsekuensi kegagalan.

Setelah feature engineering dan encoding, data modeling memiliki **10 fitur prediktor** dan 1 kolom target.

> **Catatan teknis:** Kode notebook menghitung `power` sebagai `Torque × RPM`. Walaupun markdown notebook menjelaskan konsep daya mekanik menggunakan kecepatan sudut, implementasi kode belum mengonversi RPM menjadi radian per detik. Karena itu, fitur `power` pada eksperimen ini lebih tepat dipahami sebagai indikator gabungan torsi dan RPM, bukan nilai daya mekanik dalam Watt.

## 3. Data Preprocessing

Tahapan preprocessing:

- Memisahkan fitur (`X`) dan target (`y`).
- Membagi data menggunakan `train_test_split`.
- Menggunakan stratifikasi agar proporsi kelas normal dan failure tetap terjaga.
- Melakukan standardisasi fitur menggunakan `StandardScaler`.

Konfigurasi pembagian data:

| Parameter | Nilai |
|---|---:|
| `test_size` | 0.2 |
| `random_state` | 42 |
| `stratify` | `y` |

Hasil pembagian data:

| Data | Jumlah |
|---|---:|
| Training set | 8,000 |
| Testing set | 2,000 |

Komposisi kelas pada training set:

- Mesin normal: 7,729
- Mesin gagal: 271

Stratifikasi digunakan untuk menjaga proporsi target pada data training dan testing. `StandardScaler` di-*fit* menggunakan data training, kemudian diterapkan pada data training dan testing.

## 4. Model Training

Dua model klasifikasi berbasis ensemble dilatih dan dibandingkan:

1. **Random Forest Classifier**
   - `n_estimators=300`
   - `random_state=42`
   - `class_weight="balanced"`
   - `n_jobs=1`

   Parameter `class_weight="balanced"` digunakan untuk memberikan bobot lebih besar pada kelas minoritas, yaitu mesin gagal.

2. **Gradient Boosting Classifier**
   - `n_estimators=300`
   - `learning_rate=0.1`
   - `max_depth=3`
   - `random_state=42`

Kedua model dilatih menggunakan data training yang telah melalui proses scaling.

## 5. Model Comparison

Evaluasi model dilakukan menggunakan:

- Precision
- Recall
- F1-score
- ROC-AUC

Hasil evaluasi pada testing set:

| Model | Precision | Recall | F1-score | ROC-AUC |
|---|---:|---:|---:|---:|
| Random Forest | 0.94 | 0.68 | 0.79 | 0.97 |
| Gradient Boosting | **0.95** | **0.79** | **0.86** | **0.98** |

### Analysis

- **Random Forest** memperoleh Precision 0.94, Recall 0.68, F1-score 0.79, dan ROC-AUC 0.97.
- **Gradient Boosting** memperoleh hasil lebih tinggi pada seluruh metrik yang dibandingkan.
- Gradient Boosting memiliki Recall sebesar **0.79**, yang berarti mampu mendeteksi sekitar 79% mesin gagal pada data testing.
- F1-score Gradient Boosting sebesar **0.86**, lebih tinggi dibandingkan Random Forest sebesar 0.79.
- ROC-AUC Gradient Boosting mencapai **0.98**, sedangkan Random Forest mencapai 0.97.

Berdasarkan hasil evaluasi tersebut, **Gradient Boosting** menjadi model dengan performa terbaik pada eksperimen ini.

> **Catatan konsistensi notebook:** Kode memilih model menggunakan `result['F1'].idxmax()`. Dengan demikian, model final dipilih berdasarkan F1-score, bukan Recall secara langsung. Gradient Boosting tetap menjadi model terpilih dan juga memiliki Recall tertinggi pada hasil perbandingan.

## 6. Final Model Evaluation — Gradient Boosting

Confusion Matrix:

```text
[[1929    3]
 [  14   54]]
```

Dengan urutan label `Normal` dan `Gagal`:

- True Negative (TN): 1,929
- False Positive (FP): 3
- False Negative (FN): 14
- True Positive (TP): 54

### Classification Report

| Class | Precision | Recall | F1-score | Support |
|---|---:|---:|---:|---:|
| Normal | 0.99 | 1.00 | 1.00 | 1,932 |
| Gagal | 0.95 | 0.79 | 0.86 | 68 |
| **Accuracy** |  |  | **0.99** | **2,000** |

### Interpretation

- Model berhasil mendeteksi **54 dari 68** mesin yang benar-benar mengalami kegagalan.
- Sebanyak **14 mesin gagal** masih terlewat dan diprediksi sebagai mesin normal (*False Negative*).
- Hanya **3 mesin normal** yang salah diprediksi sebagai gagal (*False Positive*).
- Precision kelas gagal mencapai **0.95**, yang berarti sebagian besar prediksi kegagalan model sesuai dengan kondisi aktual.
- Recall kelas gagal mencapai **0.79**, sehingga model mendeteksi sekitar 79% kasus kegagalan pada data testing.

Model menunjukkan kemampuan mendeteksi kegagalan yang baik, dengan jumlah *False Positive* yang rendah. Namun, 14 kasus *False Negative* tetap perlu menjadi perhatian karena dapat menyebabkan kerusakan mesin tidak terdeteksi.

Notebook juga menampilkan Confusion Matrix dan ROC Curve untuk mengevaluasi model final.

## 7. ROC Curve

- Nilai ROC-AUC Gradient Boosting mencapai **0.98**.
- Nilai tersebut menunjukkan kemampuan diskriminasi yang sangat baik dalam membedakan mesin normal dan mesin gagal.
- Kurva ROC berada jauh di atas garis tebakan acak (AUC = 0.5).

Hasil ROC-AUC mendukung hasil evaluasi bahwa Gradient Boosting memiliki performa klasifikasi yang sangat baik pada testing set.

## 8. Feature Importance

Feature importance dihitung menggunakan atribut `feature_importances_` dari **Random Forest**.

Empat fitur dengan nilai importance tertinggi:

| Feature | Importance |
|---|---:|
| `Rotational speed [rpm]` | 0.209 |
| `Torque [Nm]` | 0.204 |
| `Tool wear [min]` | 0.203 |
| `power` | 0.197 |

Fitur lainnya:

| Feature | Importance |
|---|---:|
| `temp_diff` | 0.089 |
| `Air temperature [K]` | 0.048 |
| `Process temperature [K]` | 0.036 |

### Analysis

- **Faktor utama:** `Rotational speed`, `Torque`, `Tool wear`, dan `power` merupakan empat fitur dengan importance tertinggi.
- **Faktor pendukung:** `temp_diff`, `Air temperature`, dan `Process temperature` memiliki kontribusi yang lebih kecil dibandingkan fitur mekanik.
- **Fitur kategori:** `Type_H`, `Type_L`, dan `Type_M` memiliki nilai importance yang sangat kecil.

Hasil ini menunjukkan bahwa parameter mekanik dan keausan alat lebih banyak digunakan oleh Random Forest dalam menentukan prediksi kegagalan dibandingkan kategori tipe produk.

> **Catatan:** Feature importance di atas berasal dari Random Forest, bukan Gradient Boosting. Nilai tersebut menjelaskan kontribusi fitur pada model Random Forest yang dianalisis dan tidak boleh dianggap sebagai feature importance model final Gradient Boosting.

## 9. Key Business Insights

### Machine Failure Characteristics

- Kegagalan mesin merupakan kejadian yang relatif langka, yaitu hanya **3.39%** dari seluruh data.
- Mesin gagal cenderung memiliki nilai torsi dan keausan alat yang lebih tinggi.
- Produk tipe L memiliki rasio kegagalan tertinggi sebesar 3.92%.
- Parameter mekanik seperti kecepatan putar, torsi, dan keausan alat menjadi fitur paling penting pada hasil Random Forest.

### Important Predictive Features

- `Rotational speed [rpm]`
- `Torque [Nm]`
- `Tool wear [min]`
- `power`
- `temp_diff`

Kelima fitur tersebut dapat menjadi bahan pertimbangan dalam pemantauan kondisi operasional mesin.

## 10. Business Recommendations

Berdasarkan insight pada notebook, beberapa rekomendasi untuk operasional pabrik:

1. **Fokus pada Tiga Sensor Utama**

   Prioritaskan pemantauan `Rotational speed`, `Torque`, dan `Tool wear`, karena ketiganya memiliki nilai feature importance tertinggi. Sistem pemantauan dapat memberikan peringatan ketika parameter menunjukkan pola yang tidak normal.

2. **Manajemen Keausan Alat (*Tool Wear*)**

   Lakukan inspeksi dan penggantian alat secara terencana dengan mempertimbangkan akumulasi waktu keausan. Notebook menyoroti area sekitar 200 menit sebagai titik yang perlu diperhatikan dalam pemantauan.

3. **Pemantauan Indikator `power`**

   Integrasikan indikator gabungan torsi dan RPM ke dalam dashboard pemantauan agar perubahan beban mekanik mesin dapat diamati lebih awal.

4. **Evaluasi Prediksi Kegagalan**

   Gunakan hasil prediksi Gradient Boosting sebagai salah satu sinyal untuk menentukan prioritas inspeksi teknisi. Kasus *False Negative* tetap perlu dievaluasi agar potensi kerusakan yang terlewat dapat dikurangi.

## 11. Limitations & Further Improvements

Beberapa hal yang dapat dikembangkan pada eksperimen berikutnya:

- Melakukan threshold tuning untuk mencari keseimbangan Precision dan Recall sesuai biaya operasional.
- Menggunakan cross-validation dan hyperparameter tuning untuk mengevaluasi kestabilan performa model.
- Mengevaluasi model pada data mesin atau periode waktu lain untuk mengetahui kemampuan generalisasi.
- Mempertimbangkan metode penanganan *class imbalance* lainnya sebagai pembanding.
- Memastikan perhitungan fitur `power` menggunakan konversi RPM ke radian per detik apabila tujuan analisisnya adalah memperoleh daya mekanik dalam Watt.
- Menguji feature importance dengan metode lain, seperti permutation importance atau SHAP.
- Mengembangkan pipeline preprocessing dan modeling agar proses prediksi pada data baru lebih konsisten.

## 12. Workflow Summary

1. Import libraries and dataset.
2. Inspect dataset structure, data types, missing values, and duplicates.
3. Analyze target distribution and class imbalance.
4. Perform Exploratory Data Analysis on machine sensor features.
5. Compare sensor characteristics between normal and failure machines.
6. Analyze correlation and failure ratio by product type.
7. Create `temp_diff` and `power` features.
8. Drop identifier and leakage columns, then apply One-Hot Encoding.
9. Split data into training and testing sets using stratification.
10. Apply StandardScaler.
11. Train Random Forest and Gradient Boosting.
12. Compare Precision, Recall, F1-score, and ROC-AUC.
13. Select Gradient Boosting based on the highest F1-score.
14. Evaluate the final model using Confusion Matrix, Classification Report, and ROC Curve.
15. Analyze Random Forest feature importance.
16. Translate findings into business insights and maintenance recommendations.

## Tech Stack

- Python
- Pandas
- NumPy
- Matplotlib
- Seaborn
- Scikit-learn
  - `train_test_split`
  - `StandardScaler`
  - `RandomForestClassifier`
  - `GradientBoostingClassifier`
  - `precision_score`
  - `recall_score`
  - `f1_score`
  - `roc_auc_score`
  - `confusion_matrix`
  - `classification_report`
  - `RocCurveDisplay`
- Jupyter Notebook / Google Colab
- Kaggle Dataset

## Project Structure

```text
Day_47_Capstone_Project_Predictive_Maintenance/
│
├── Day_47_Capstone_Project_Predictive_Maintenance.ipynb
└── README.md
```

## Conclusion

Capstone Project Day 47 menggabungkan proses EDA, feature engineering, preprocessing, training, evaluasi model, hingga penerjemahan hasil analisis menjadi rekomendasi bisnis pada kasus Predictive Maintenance.

Dari dua model yang diuji, **Gradient Boosting** memperoleh performa terbaik dengan **Recall kelas gagal sebesar 0.79**, **Precision sebesar 0.95**, **F1-score sebesar 0.86**, dan **ROC-AUC sebesar 0.98**. Model berhasil mendeteksi 54 dari 68 mesin gagal pada data testing, dengan 14 kasus kegagalan yang masih terlewat.

Hasil feature importance Random Forest menunjukkan bahwa `Rotational speed`, `Torque`, `Tool wear`, dan `power` merupakan fitur yang paling berpengaruh dalam model tersebut.

Proyek ini menunjukkan bahwa predictive maintenance dapat membantu proses identifikasi risiko kegagalan mesin melalui data sensor. Namun, hasil prediksi tetap perlu divalidasi dengan data operasional nyata dan dikombinasikan dengan pemeriksaan teknisi sebelum diterapkan sebagai sistem pemeliharaan di lingkungan industri.

---

## 👨‍💻 Author

**Arief Wicaksono**  
Aspiring AI Engineer | Data Science Student

---

⭐ *Learning Machine Learning one day at a time.*
