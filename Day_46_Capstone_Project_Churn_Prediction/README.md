# Day 46 — Capstone Project: Customer Churn Prediction in Telecommunications

## Overview

Pada Day 46, saya mengerjakan **Capstone Project pertama** dengan menggabungkan tahapan pembelajaran Data Science ke dalam satu studi kasus bisnis end-to-end.

Proyek ini bertujuan memprediksi pelanggan perusahaan telekomunikasi yang berpotensi berhenti berlangganan (*customer churn*), memahami karakteristik pelanggan yang rentan churn, serta menyusun rekomendasi retensi yang dapat ditindaklanjuti.

Workflow utama:

**Exploratory Data Analysis → Data Cleaning → Feature Engineering & Preprocessing → Model Training → Model Evaluation → Business Insights**

## Project Objectives

- Menjalankan workflow Data Science secara end-to-end.
- Melakukan Exploratory Data Analysis (EDA) untuk memahami pola churn.
- Membersihkan data dan menyiapkan fitur untuk pemodelan.
- Melakukan One-Hot Encoding pada fitur kategorikal.
- Melatih dan membandingkan Logistic Regression, Random Forest, dan Gradient Boosting.
- Mengevaluasi model menggunakan Accuracy, Recall Churn, ROC-AUC, Confusion Matrix, dan Classification Report.
- Memilih model berdasarkan kebutuhan bisnis, terutama kemampuan mendeteksi pelanggan yang berpotensi churn.
- Menyusun insight dan rekomendasi strategi retensi pelanggan.

## Dataset

Dataset yang digunakan adalah **Telco Customer Churn** dari Kaggle.

- **Source:** [Kaggle — Telco Customer Churn](https://www.kaggle.com/datasets/blastchar/telco-customer-churn)
- **Rows:** 7,043
- **Columns:** 21
- **Missing values awal:** 0
- **Duplicate rows:** 0
- **Target:** `Churn` (`Yes` / `No`)

Dataset berisi informasi pelanggan, layanan telekomunikasi yang digunakan, lama berlangganan, tagihan, jenis kontrak, metode pembayaran, dan status churn.

### Target Distribution

- `No` (bertahan): sekitar 73%
- `Yes` (churn): sekitar 27%

Distribusi target menunjukkan adanya ketidakseimbangan kelas (*class imbalance*), sehingga evaluasi tidak hanya berfokus pada accuracy.

## 1. Exploratory Data Analysis

EDA dilakukan untuk memahami pola churn berdasarkan karakteristik pelanggan.

### Contract Type vs Churn

- Pelanggan dengan kontrak **Month-to-month** memiliki jumlah churn paling besar.
- Pelanggan dengan kontrak **One year** dan **Two year** lebih banyak bertahan.
- Kontrak bulanan terlihat lebih rentan terhadap churn dibandingkan kontrak jangka panjang.

### Tenure Distribution

- Churn banyak terjadi pada bulan-bulan awal masa berlangganan.
- Pelanggan yang telah bertahan dalam waktu lama cenderung lebih loyal.
- Masa awal berlangganan menjadi periode penting untuk program onboarding dan retensi.

### Monthly Charges vs Churn

- Pelanggan yang churn cenderung memiliki tagihan bulanan lebih tinggi.
- Median `MonthlyCharges` pelanggan churn berada di sekitar 80, sedangkan pelanggan yang bertahan sekitar 65.

### Additional Findings

Notebook juga mencatat kecenderungan churn yang lebih tinggi pada pelanggan pengguna layanan **Fiber Optic** dan metode pembayaran **Electronic Check**. Pelanggan yang menggunakan **Tech Support** dan **Online Security**, serta pelanggan dengan tanggungan (*Dependents*), cenderung lebih loyal.

## 2. Data Cleaning

Tahapan pembersihan data:

1. Membuat salinan dataset ke `df_clean`.
2. Mengubah kolom `TotalCharges` menjadi tipe numerik menggunakan `pd.to_numeric()` dengan `errors="coerce"`.
3. Mengisi nilai hasil konversi yang menjadi `NaN` pada `TotalCharges` dengan `0`. Notebook mengaitkan nilai tersebut dengan pelanggan yang memiliki `tenure` 0.
4. Menghapus kolom `customerID` karena hanya berfungsi sebagai identitas pelanggan.
5. Mengubah target `Churn` menjadi nilai biner:
   - `No` → `0`
   - `Yes` → `1`

Setelah cleaning, dataset berisi **7,043 baris × 20 kolom** dan tidak memiliki missing value.

## 3. Feature Engineering & Preprocessing

- Memisahkan fitur (`X`) dan target (`y`).
- Menggunakan `pd.get_dummies(..., drop_first=True)` untuk melakukan One-Hot Encoding pada fitur kategorikal.
- Setelah encoding, terdapat **30 fitur**.
- Membagi data menggunakan `train_test_split` dengan:
  - `test_size=0.2`
  - `random_state=42`
  - `stratify=y`

Hasil pembagian data:

| Data | Jumlah |
|---|---:|
| Training set | 5,634 |
| Testing set | 1,409 |

Pembagian stratified digunakan untuk menjaga proporsi kelas target pada data training dan testing.

## 4. Model Training

Tiga model klasifikasi dilatih dan dibandingkan:

1. **Logistic Regression** — menggunakan `class_weight="balanced"`.
2. **Random Forest Classifier** — menggunakan `class_weight="balanced"`.
3. **Gradient Boosting Classifier**.

Evaluasi model dilakukan menggunakan:

- Accuracy
- Recall untuk kelas churn
- ROC-AUC

Recall churn menjadi metrik penting karena perusahaan perlu mengidentifikasi sebanyak mungkin pelanggan yang berpotensi berhenti berlangganan.

## 5. Model Comparison

Hasil evaluasi pada testing set:

| Model | Accuracy | Recall Churn | ROC-AUC |
|---|---:|---:|---:|
| Logistic Regression | 0.736 | **0.759** | 0.838 |
| Random Forest | 0.789 | 0.495 | 0.827 |
| Gradient Boosting | **0.798** | 0.508 | **0.842** |

### Analysis

- **Logistic Regression** memiliki accuracy paling rendah, tetapi recall churn tertinggi, yaitu **75.9%**.
- **Random Forest** memiliki accuracy 78.9%, tetapi recall churn hanya 49.5%.
- **Gradient Boosting** memperoleh accuracy tertinggi (79.8%) dan ROC-AUC tertinggi (0.842), tetapi recall churn sebesar 50.8%.
- Logistic Regression dipilih sebagai model final karena lebih banyak menangkap pelanggan yang benar-benar churn.

> **Catatan konsistensi notebook:** Salah satu markdown menyebut pemilihan model berdasarkan ROC-AUC, tetapi kode pemilihan `best_model` menggunakan `max(..., key=lambda k: result[k]["recall_churn"])`. Dengan demikian, model final yang dipilih oleh kode adalah Logistic Regression berdasarkan recall churn, bukan ROC-AUC tertinggi.

## 6. Final Model Evaluation — Logistic Regression

Confusion Matrix:

```text
[[753 282]
 [ 90 284]]
```

Dengan urutan label `Bertahan` dan `Churn`:

- True Negative (TN): 753
- False Positive (FP): 282
- False Negative (FN): 90
- True Positive (TP): 284

Classification Report:

| Class | Precision | Recall | F1-score | Support |
|---|---:|---:|---:|---:|
| Bertahan | 0.89 | 0.73 | 0.80 | 1,035 |
| Churn | 0.50 | 0.76 | 0.60 | 374 |
| **Accuracy** |  |  | **0.74** | **1,409** |

### Interpretation

- Model berhasil mendeteksi **284 dari 374** pelanggan yang benar-benar churn.
- Sebanyak **90** pelanggan churn tidak terdeteksi (*false negative*).
- Model memprediksi 282 pelanggan setia sebagai churn (*false positive*).
- Recall churn sekitar **76%**, sedangkan precision churn sekitar **50%**.
- Artinya, model cukup agresif dalam menandai pelanggan yang berisiko churn, tetapi masih menghasilkan alarm palsu yang cukup banyak.

Notebook juga memvisualisasikan Confusion Matrix dan ROC Curve untuk model final.

## 7. Key Business Insights

### Customers at Higher Risk of Churn

- Pelanggan dengan `tenure` rendah, terutama pada bulan-bulan awal.
- Pelanggan dengan kontrak `Month-to-month`.
- Pelanggan dengan `MonthlyCharges` relatif tinggi.
- Pelanggan pengguna layanan Fiber Optic.
- Pelanggan yang menggunakan metode pembayaran Electronic Check.

### Retention Drivers

- Penggunaan layanan tambahan seperti `TechSupport` dan `OnlineSecurity`.
- Memiliki tanggungan (*Dependents*).
- Masa berlangganan yang lebih panjang.

## 8. Business Recommendations

Berdasarkan insight pada notebook, beberapa strategi retensi yang disarankan:

1. **Program onboarding pelanggan baru**  
   Berikan pendampingan dan penawaran loyalitas pada 1–3 bulan pertama untuk membantu pelanggan melewati periode awal yang rentan churn.

2. **Bundling layanan proteksi**  
   Pertimbangkan bundling `Tech Support` dan `Online Security`, terutama pada paket Fiber Optic, untuk meningkatkan nilai layanan dan keterikatan pelanggan.

3. **Insentif kontrak jangka panjang dan pembayaran otomatis**  
   Dorong pelanggan kontrak bulanan dan pengguna Electronic Check untuk beralih ke kontrak satu atau dua tahun serta metode pembayaran otomatis melalui penawaran yang sesuai.

## 9. Limitations & Further Improvements

Beberapa hal yang dapat dikembangkan pada eksperimen berikutnya:

- Logistic Regression menghasilkan `ConvergenceWarning` karena batas iterasi default tercapai. Eksperimen lanjutan dapat mengatur `max_iter` dan mengevaluasi preprocessing/scaling yang sesuai.
- Precision churn sebesar 50% menunjukkan masih banyak prediksi churn yang merupakan false positive.
- Threshold tuning dapat dieksplorasi untuk mencari trade-off yang sesuai antara recall dan precision berdasarkan biaya bisnis.
- Evaluasi lanjutan dapat menambahkan cross-validation dan tuning hyperparameter agar perbandingan model lebih menyeluruh.
- Perlu dilakukan validasi lebih lanjut sebelum model digunakan dalam sistem retensi pelanggan nyata.

## 10. Workflow Summary

1. Import libraries and dataset.
2. Inspect dataset structure and target distribution.
3. Perform Exploratory Data Analysis.
4. Clean `TotalCharges`, drop `customerID`, and encode target.
5. Apply One-Hot Encoding.
6. Split data into training and testing sets with stratification.
7. Train Logistic Regression, Random Forest, and Gradient Boosting.
8. Compare Accuracy, Recall Churn, and ROC-AUC.
9. Select Logistic Regression based on Recall Churn.
10. Evaluate the final model using Confusion Matrix, Classification Report, and ROC Curve.
11. Translate model and EDA findings into business recommendations.

## Tech Stack

- Python
- Pandas
- NumPy
- Matplotlib
- Seaborn
- Scikit-learn
  - `train_test_split`
  - `LogisticRegression`
  - `RandomForestClassifier`
  - `GradientBoostingClassifier`
  - Classification metrics
- Jupyter Notebook / Google Colab
- Kaggle Dataset

## Project Structure

```text
Day_46_Capstone_Project_Churn_Prediction_Customer_Telecomunication/
│
├── Day_46_Capstone_Project_Churn_Prediction_Customer_Telecomunication.ipynb
└── README.md
```

## Conclusion

Capstone Project Day 46 menggabungkan proses EDA, data cleaning, preprocessing, training, evaluasi model, hingga penerjemahan hasil analisis menjadi rekomendasi bisnis.

Dari tiga model yang diuji, **Logistic Regression** dipilih karena memiliki Recall Churn tertinggi sebesar **75.9%** dan ROC-AUC sebesar **0.838**. Walaupun accuracy-nya lebih rendah dibandingkan Random Forest dan Gradient Boosting, kemampuan menangkap pelanggan churn menjadi pertimbangan utama dalam konteks strategi retensi.

Proyek ini menunjukkan bahwa pemilihan model tidak cukup hanya berdasarkan accuracy tertinggi. Metrik evaluasi perlu disesuaikan dengan tujuan dan konsekuensi bisnis.

---

## 👨‍💻 Author

**Arief Wicaksono**  
Aspiring AI Engineer | Data Science Student

---

⭐ *Learning Machine Learning one day at a time.*
