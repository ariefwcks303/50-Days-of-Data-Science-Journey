# Day 32 — Deteksi Penyakit Jantung dengan Machine Learning

## 📌 Overview

Penyakit jantung merupakan salah satu masalah kesehatan yang membutuhkan deteksi dan penanganan secara tepat. Pada proyek ini, machine learning digunakan untuk mempelajari pola dari data pemeriksaan pasien dan memprediksi apakah pasien memiliki indikasi penyakit jantung berdasarkan fitur yang tersedia.

Eksperimen ini membandingkan dua algoritma klasifikasi, yaitu **Logistic Regression** dan **Random Forest Classifier**, dengan perhatian khusus pada precision, recall, F1-score, dan confusion matrix.

Fokus utama bukan hanya mendapatkan akurasi tinggi, tetapi juga memahami kemampuan model dalam mendeteksi pasien yang memiliki penyakit jantung.

## 🎯 Objectives

* Melakukan Exploratory Data Analysis (EDA) pada dataset kesehatan.
* Mengidentifikasi missing values, duplikasi, dan distribusi kelas target.
* Memahami hubungan antarfitur melalui visualisasi dan korelasi.
* Melakukan preprocessing dan standardisasi fitur.
* Membangun model Logistic Regression dan Random Forest Classifier.
* Mengevaluasi performa model menggunakan classification metrics.
* Menganalisis false positive, false negative, dan feature importance.

## 📂 Dataset

* **Dataset:** Heart Disease Dataset
* **Source:** [Kaggle — Heart Disease Dataset](https://www.kaggle.com/datasets/johnsmith88/heart-disease-dataset)
* **File:** `heart.csv`
* **Jumlah data awal:** 1.025 baris
* **Jumlah kolom:** 14
* **Target:** `target`

Label target:

* `0` — Tidak terindikasi penyakit jantung berdasarkan label dataset.
* `1` — Terindikasi penyakit jantung berdasarkan label dataset.

### Feature Description

Dataset memiliki 13 fitur prediktor:

| Feature    | Description                                    |
| ---------- | ---------------------------------------------- |
| `age`      | Usia pasien                                    |
| `sex`      | Jenis kelamin                                  |
| `cp`       | Tipe nyeri dada                                |
| `trestbps` | Tekanan darah saat istirahat                   |
| `chol`     | Kadar kolesterol                               |
| `fbs`      | Indikator gula darah puasa                     |
| `restecg`  | Hasil EKG saat istirahat                       |
| `thalach`  | Detak jantung maksimum                         |
| `exang`    | Angina akibat olahraga                         |
| `oldpeak`  | Depresi segmen ST                              |
| `slope`    | Kemiringan segmen ST                           |
| `ca`       | Jumlah pembuluh darah besar yang diamati       |
| `thal`     | Kategori hasil pemeriksaan terkait thalassemia |

Kolom `target` digunakan sebagai label yang akan diprediksi oleh model.

## 🛠️ Tech Stack

* Python
* Pandas dan NumPy
* Matplotlib dan Seaborn
* Scikit-learn
* Google Colab / Jupyter Notebook

## 🔍 Workflow

### 1. Exploratory Data Analysis

Tahapan eksplorasi meliputi:

* Memeriksa struktur dan tipe data menggunakan `df.info()`.
* Memeriksa missing values.
* Mengidentifikasi baris duplikat.
* Menganalisis statistik deskriptif menggunakan `df.describe()`.
* Mengamati distribusi kelas target.
* Membandingkan distribusi usia berdasarkan target.
* Menganalisis korelasi antarfitur menggunakan heatmap.

**Temuan awal:**

* Dataset memiliki 1.025 baris dan 14 kolom.
* Tidak ditemukan missing values.
* Terdapat 723 baris duplikat.
* Distribusi kelas setelah penghapusan duplikasi relatif seimbang.

### 2. Data Cleaning

Duplikasi dihapus menggunakan `drop_duplicates()`.

Hasil setelah cleaning:

| Metric                      | Value |
| --------------------------- | ----: |
| Baris awal                  | 1.025 |
| Baris duplikat yang dihapus |   723 |
| Baris akhir                 |   302 |
| Kolom                       |    14 |
| Missing values              |     0 |

Penghapusan duplikasi dilakukan sebelum pembagian data latih dan data uji.

### 3. Feature Analysis

Analisis dilakukan melalui distribusi target, histogram usia, dan heatmap korelasi.

Beberapa fitur yang menunjukkan hubungan dengan target dalam analisis korelasi adalah `cp`, `thalach`, `exang`, `oldpeak`, dan `ca`.

Korelasi menggambarkan hubungan statistik, bukan bukti bahwa suatu fitur menyebabkan penyakit jantung.

### 4. Data Preprocessing

Tahapan preprocessing:

* Memisahkan fitur prediktor (`X`) dan target (`y`).
* Membagi dataset menjadi training set dan test set dengan rasio 80:20.
* Menggunakan `random_state=42` untuk reproduktibilitas.
* Menggunakan `stratify=y` untuk mempertahankan proporsi kelas.
* Melakukan standardisasi menggunakan `StandardScaler` untuk Logistic Regression.

StandardScaler hanya di-*fit* pada data latih, kemudian diterapkan pada data uji menggunakan `transform()` untuk menghindari data leakage.

Hasil pembagian data:

| Data         | Jumlah baris |
| ------------ | -----------: |
| Training set |          241 |
| Test set     |           61 |

Random Forest dilatih menggunakan fitur tanpa standardisasi karena algoritma berbasis pohon keputusan tidak memerlukan scaling fitur seperti Logistic Regression.

### 5. Model Development

Dua model klasifikasi yang digunakan:

**Logistic Regression**

* `max_iter=1000`
* `random_state=42`
* Menggunakan fitur yang telah distandardisasi.

**Random Forest Classifier**

* `n_estimators=200`
* `random_state=42`
* Menggunakan fitur tanpa standardisasi.

Kedua model dilatih pada training set dan dievaluasi menggunakan test set yang sama.

## 📊 Model Evaluation

### Perbandingan Performa

| Metric                   | Logistic Regression | Random Forest |
| ------------------------ | ------------------: | ------------: |
| Accuracy                 |             **80%** |           75% |
| Precision — Sakit (1)    |            **0.80** |          0.76 |
| Recall — Sakit (1)       |            **0.85** |          0.79 |
| F1-score — Sakit (1)     |            **0.82** |          0.78 |
| Recall — Tidak sakit (0) |            **0.75** |          0.71 |

Berdasarkan hasil pengujian ini, Logistic Regression memiliki performa yang lebih baik dibandingkan Random Forest pada metrik yang dievaluasi.

### Confusion Matrix

| Actual / Predicted | LR: Prediksi 0 | LR: Prediksi 1 |
| ------------------ | -------------: | -------------: |
| Actual 0           |             21 |              7 |
| Actual 1           |              5 |             28 |

| Actual / Predicted | RF: Prediksi 0 | RF: Prediksi 1 |
| ------------------ | -------------: | -------------: |
| Actual 0           |             20 |              8 |
| Actual 1           |              7 |             26 |

Interpretasi:

* **True Positive (TP):** Pasien dengan label penyakit jantung yang berhasil diprediksi sebagai kelas 1.
* **True Negative (TN):** Pasien dengan label kelas 0 yang berhasil diprediksi sebagai kelas 0.
* **False Positive (FP):** Pasien dengan label kelas 0 yang keliru diprediksi sebagai kelas 1.
* **False Negative (FN):** Pasien dengan label penyakit jantung yang keliru diprediksi sebagai kelas 0.

Dalam konteks screening medis, false negative perlu mendapat perhatian karena pasien yang berisiko dapat terlewat oleh model.

Logistic Regression menghasilkan 5 false negative, sedangkan Random Forest menghasilkan 7 pada test set ini.

## 🌳 Feature Importance

Feature importance dari Random Forest menunjukkan fitur yang paling banyak digunakan model dalam membuat keputusan klasifikasi.

Tiga fitur dengan importance tertinggi pada eksperimen ini:

| Feature   | Feature Importance |
| --------- | -----------------: |
| `cp`      |               0.17 |
| `thalach` |               0.13 |
| `ca`      |               0.11 |

Fitur lain yang juga berkontribusi antara lain `thal`, `oldpeak`, dan `age`.

Feature importance menunjukkan kontribusi relatif fitur dalam model Random Forest. Nilai tersebut bukan ukuran kausalitas atau bukti bahwa fitur tertentu secara langsung menyebabkan penyakit jantung.

## 💡 Key Insights

1. **Akurasi bukan satu-satunya metrik evaluasi.** Pada masalah klasifikasi medis, recall untuk kelas positif penting untuk menilai kemampuan model dalam mendeteksi kasus yang berisiko.

2. **Model yang lebih kompleks tidak selalu lebih baik.** Random Forest memiliki kemampuan menangkap pola nonlinier, tetapi Logistic Regression memperoleh hasil yang lebih baik dalam eksperimen ini.

3. **Data quality memengaruhi evaluasi.** Dari 1.025 baris awal, hanya 302 baris yang tersisa setelah penghapusan duplikasi. Pola duplikasi perlu dipahami karena dapat memengaruhi representativitas dataset dan interpretasi hasil.

4. **Feature importance membantu interpretasi model.** Fitur `cp`, `thalach`, dan `ca` memiliki importance tertinggi pada Random Forest, tetapi temuan ini tidak boleh dianggap sebagai kesimpulan klinis.

5. **Pemilihan model harus mengikuti tujuan penggunaan.** Jika prioritasnya meminimalkan kasus penyakit yang terlewat, recall kelas positif perlu dipertimbangkan bersama precision, false negative, dan kebutuhan evaluasi klinis.

## ⚠️ Limitations

* Evaluasi hanya menggunakan satu pembagian training dan test set.
* Belum dilakukan cross-validation atau hyperparameter tuning.
* Dataset menjadi jauh lebih kecil setelah penghapusan duplikasi.
* Hasil evaluasi hanya merepresentasikan test set yang digunakan, bukan jaminan performa pada populasi lain.
* Feature importance tidak membuktikan hubungan sebab-akibat.
* Model belum divalidasi secara klinis dan tidak boleh digunakan sebagai pengganti diagnosis atau keputusan dokter.

## 🚀 Future Improvements

* Menyelidiki asal dan pola duplikasi dataset sebelum menentukan strategi penanganan.
* Menggunakan stratified cross-validation untuk mengevaluasi kestabilan model.
* Melakukan hyperparameter tuning.
* Mengevaluasi recall, specificity, precision-recall curve, dan ROC-AUC.
* Menentukan threshold prediksi berdasarkan konsekuensi false negative dan false positive.
* Menguji generalisasi pada data eksternal yang representatif sebelum mempertimbangkan penggunaan klinis.

## 📚 Learning Outcomes

Melalui proyek ini, saya mempraktikkan:

* Exploratory Data Analysis pada data medis.
* Data cleaning dan penanganan duplikasi.
* Train-test split dan stratifikasi kelas.
* Standardisasi fitur tanpa data leakage.
* Implementasi Logistic Regression dan Random Forest Classifier.
* Evaluasi klasifikasi menggunakan precision, recall, F1-score, dan confusion matrix.
* Interpretasi feature importance.
* Pemilihan model berdasarkan kebutuhan masalah, bukan akurasi semata.

## 📁 Project Structure

```text
Day_32_Deteksi_Penyakit_Jantung/
├── Day_32_Deteksi_Penyakit_Jantung.ipynb
└── README.md
```

## 📝 Conclusion

Pada eksperimen ini, Logistic Regression memperoleh akurasi 80% dan recall 85% untuk kelas penyakit jantung, mengungguli Random Forest yang memperoleh akurasi 75% dan recall 79%.

Hasil ini menunjukkan bahwa model yang lebih sederhana dapat memberikan performa lebih baik pada dataset tertentu. Namun, performa tersebut masih merupakan hasil eksperimen awal dan belum cukup untuk mendukung penggunaan klinis.

Proyek ini menekankan pentingnya data quality, pemilihan metrik evaluasi yang sesuai dengan risiko, interpretasi model, dan validasi sebelum sistem machine learning digunakan dalam lingkungan nyata.

---

## 👤 Author

**Arief Wicaksono**  

*Aspiring AI Engineer | Data Science Student*

---

⭐ If you find this project useful, feel free to star the repository!
