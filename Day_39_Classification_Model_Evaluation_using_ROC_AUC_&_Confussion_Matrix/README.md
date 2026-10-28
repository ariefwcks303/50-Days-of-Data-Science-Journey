# Day 39 — Evaluasi Model Klasifikasi Menggunakan ROC-AUC dan Confusion Matrix

## 📌 Gambaran Proyek

Membangun model klasifikasi saja belum cukup. Kita juga perlu memahami **jenis kesalahan prediksi**, kemampuan model membedakan kelas, serta konsekuensi pemilihan *threshold*. Notebook Day 39 berfokus pada evaluasi model **Logistic Regression** untuk klasifikasi indikasi penyakit jantung menggunakan **Confusion Matrix, Classification Report, ROC Curve, ROC-AUC, dan Precision-Recall Curve**.

> **Catatan:** Proyek ini merupakan eksperimen pembelajaran, bukan alat diagnosis atau skrining klinis yang telah tervalidasi.

## 🎯 Tujuan Pembelajaran

- Mengeksplorasi kualitas dan distribusi data pasien.
- Menangani data duplikat dan melakukan pembagian data latih/uji secara terstratifikasi.
- Melatih Logistic Regression menggunakan fitur yang telah distandardisasi.
- Menginterpretasikan **TP, TN, FP, dan FN** dalam konteks klasifikasi penyakit jantung.
- Memahami **accuracy, precision, recall, F1-score**, dan **ROC-AUC**.
- Mengevaluasi *trade-off* precision–recall dan dampak perubahan *threshold*.

## 📂 Dataset

- **Nama:** Heart Disease Dataset
- **Sumber:** [Kaggle — Heart Disease Dataset](https://www.kaggle.com/datasets/johnsmith88/heart-disease-dataset)
- **Berkas:** `heart.csv`
- **Data awal:** 1.025 baris × 14 kolom
- **Fitur prediktor:** 13
- **Target:** `target` (`0` = tidak terindikasi penyakit jantung, `1` = terindikasi penyakit jantung, berdasarkan label dataset)

| Fitur | Keterangan |
|---|---|
| `age` | Usia pasien |
| `sex` | Jenis kelamin yang dikodekan secara numerik |
| `cp` | Kategori nyeri dada |
| `trestbps` | Tekanan darah saat istirahat |
| `chol` | Kadar kolesterol serum |
| `fbs` | Indikator gula darah puasa |
| `restecg` | Kategori hasil EKG saat istirahat |
| `thalach` | Detak jantung maksimum yang dicapai |
| `exang` | Indikator angina akibat olahraga |
| `oldpeak` | Depresi segmen ST akibat aktivitas |
| `slope` | Kemiringan segmen ST |
| `ca` | Jumlah pembuluh besar yang teramati |
| `thal` | Kategori hasil pemeriksaan terkait thalassemia |

## 🛠️ Teknologi

Python, Pandas, NumPy, Matplotlib, Seaborn, Scikit-learn, dan Jupyter Notebook/Google Colab.

## 🔍 Alur Eksperimen

### 1. Eksplorasi dan pembersihan data

Notebook memeriksa struktur dataset, duplikasi, missing value, serta distribusi kelas target.

| Kondisi | Data awal | Setelah menghapus duplikasi |
|---|---:|---:|
| Jumlah baris | 1.025 | 302 |
| Jumlah kolom | 14 | 14 |
| Duplikasi | 723 | 0 |
| Missing value | 0 | 0 |
| Target 0 | 499 | 138 |
| Target 1 | 526 | 164 |

Sebanyak **723 baris duplikat** dihapus sebelum pemodelan. Proporsi kelas setelah pembersihan adalah sekitar **45,7% kelas 0** dan **54,3% kelas 1**.

### 2. Exploratory Data Analysis (EDA)

Visualisasi yang digunakan:

- **Countplot target:** melihat distribusi kedua kelas.
- **Scatterplot usia vs `thalach`:** membandingkan detak jantung maksimum menurut usia dan kelas target.
- **Heatmap korelasi:** mengeksplorasi hubungan statistik antarf fitur.

Notebook menyoroti fitur seperti `cp` dan `thalach` dalam analisis hubungan dengan target. **Korelasi tidak membuktikan hubungan sebab-akibat.**

### 3. Preprocessing

- Memisahkan `X` (13 fitur) dan `y` (`target`).
- Membagi data dengan `train_test_split(test_size=0.2, random_state=42, stratify=y)`.
- Menggunakan `StandardScaler`, dengan `fit_transform()` hanya pada data latih dan `transform()` pada data uji.

| Subset | Jumlah baris |
|---|---:|
| Data latih | 241 |
| Data uji | 61 |

Standardisasi dilakukan setelah pembagian data untuk mencegah **data leakage** dari statistik data uji.

### 4. Pemodelan

Algoritma yang digunakan adalah **Logistic Regression** dengan `max_iter=1000` dan `random_state=42`.

Model menghasilkan:

- `predict()` untuk label kelas.
- `predict_proba()[:, 1]` untuk probabilitas kelas positif yang digunakan pada kurva ROC dan Precision-Recall.

## 📊 Hasil Evaluasi

### Ringkasan metrik

| Metrik | Nilai |
|---|---:|
| Accuracy | **80,3%** (49 dari 61 prediksi benar) |
| Precision — kelas 1 | **0,80** |
| Recall — kelas 1 | **0,85** |
| F1-score — kelas 1 | **0,82** |
| Precision — kelas 0 | 0,81 |
| Recall — kelas 0 | 0,75 |
| F1-score — kelas 0 | 0,78 |
| ROC-AUC | **0,87** |

### 1. Confusion Matrix

| Aktual / Prediksi | Prediksi 0 | Prediksi 1 |
|---|---:|---:|
| **Aktual 0** | **21 (TN)** | **7 (FP)** |
| **Aktual 1** | **5 (FN)** | **28 (TP)** |

**Interpretasi:**

- **True Negative (TN = 21):** pasien kelas 0 diprediksi sebagai kelas 0.
- **True Positive (TP = 28):** pasien kelas 1 diprediksi sebagai kelas 1.
- **False Positive (FP = 7):** pasien kelas 0 keliru diprediksi sebagai kelas 1.
- **False Negative (FN = 5):** pasien kelas 1 keliru diprediksi sebagai kelas 0.

Dalam konteks deteksi penyakit, **false negative** patut diperhatikan karena kasus positif bisa terlewat. Namun, konsekuensi FP dan FN perlu ditentukan melalui konteks klinis yang nyata, bukan hanya dari notebook.

### 2. Classification Report

| Kelas | Precision | Recall | F1-score | Support |
|---|---:|---:|---:|---:|
| 0 — Tidak terindikasi | 0,81 | 0,75 | 0,78 | 28 |
| 1 — Terindikasi | 0,80 | **0,85** | **0,82** | 33 |

**Recall kelas 1 sebesar 85%** berarti 28 dari 33 kasus positif pada data uji berhasil dikenali. **Precision kelas 1 sebesar 80%** berarti 28 dari 35 prediksi positif benar-benar merupakan kelas 1 menurut label dataset.

### 3. ROC Curve dan ROC-AUC

Kurva ROC membandingkan **True Positive Rate (TPR)** dan **False Positive Rate (FPR)** pada berbagai nilai *threshold*.

Notebook melaporkan **ROC-AUC = 0,87**, yang menunjukkan kemampuan diskriminasi model yang baik pada data uji. AUC tidak sama dengan accuracy, dan tidak langsung menunjukkan apakah probabilitas model sudah terkalibrasi.

### 4. Precision-Recall Curve

Kurva Precision-Recall memperlihatkan perubahan precision dan recall ketika *threshold* prediksi diubah.

**Catatan konsistensi notebook:** bagian interpretasi menyebut **Average Precision (AP) = 0,90**, sedangkan judul grafik tertulis **0,87**. Kode menghitung `average_precision_score(y_test, y_pred_proba)` dan menampilkannya pada legenda grafik. Angka AP sebaiknya diperiksa dari hasil perhitungan tersebut; nilai 0,87 pada judul grafik ditulis secara manual.

## 💡 Insight Utama

1. **Accuracy bukan gambaran lengkap.** Nilai 80,3% perlu dibaca bersama confusion matrix dan metrik per kelas.
2. **Recall penting ketika kasus positif tidak boleh mudah terlewat.** Model masih menghasilkan 5 false negative pada 33 kasus positif di data uji.
3. **ROC-AUC mengukur kemampuan pemeringkatan kelas.** Nilai 0,87 menunjukkan diskriminasi yang baik pada sampel pengujian ini.
4. **Threshold menentukan trade-off.** Menurunkan threshold dari nilai default 0,5 dapat meningkatkan recall, tetapi biasanya juga meningkatkan false positive; dampaknya perlu diuji, bukan diasumsikan.
5. **Kualitas data memengaruhi validitas evaluasi.** Penghapusan 723 duplikat mengurangi risiko pengulangan observasi, tetapi asal duplikasi perlu dipahami.

## ⚠️ Keterbatasan

- Hanya menggunakan satu pembagian data latih dan uji, tanpa cross-validation.
- Data uji relatif kecil (**61 observasi**), sehingga estimasi metrik dapat berubah pada sampel lain.
- Belum ada analisis ketidakpastian metrik, kalibrasi probabilitas, atau validasi eksternal.
- Belum ada eksperimen perubahan threshold beserta perbandingan FP/FN yang dihasilkan.
- Belum ada validasi klinis; hasil tidak dapat dijadikan dasar diagnosis pasien.
- Terdapat perbedaan angka AP pada narasi dan judul grafik Precision-Recall yang perlu diselaraskan.

## 🚀 Pengembangan Berikutnya

- Melakukan **Stratified K-Fold Cross-Validation**.
- Menguji beberapa *threshold* berdasarkan kebutuhan sensitivitas dan spesifisitas.
- Membandingkan ROC-AUC dengan Average Precision dan PR Curve secara konsisten.
- Memeriksa **probability calibration** dan menambahkan interval kepercayaan metrik.
- Menguji performa pada data eksternal yang representatif.
- Menggunakan `Pipeline` untuk menggabungkan preprocessing dan model secara lebih terstruktur.

## 📚 Hasil Pembelajaran

Notebook ini memperkuat pemahaman bahwa **evaluasi klasifikasi bukan sekadar menghitung accuracy**. Confusion matrix menunjukkan jenis kesalahan, classification report menjelaskan performa per kelas, sementara ROC-AUC dan Precision-Recall membantu mengevaluasi kemampuan model pada berbagai threshold.

## 📁 Struktur Proyek

```text
Day_39_Evaluation_Classification_Model_using_ROC_AUC_&_Confusion_Matrix/
├── Day_39_Evaluation_Classification_Model_using_ROC_AUC_&_Confusion_Matrix.ipynb
└── README.md
```

## 📝 Kesimpulan

Logistic Regression memperoleh **accuracy 80,3%**, **recall kelas positif 85%**, dan **ROC-AUC 0,87** pada dataset uji. Model mengenali 28 dari 33 kasus positif, tetapi masih melewatkan 5 kasus positif dan menghasilkan 7 false positive.

Hasil ini cocok sebagai **studi evaluasi model klasifikasi**, sekaligus menegaskan bahwa pemilihan metrik dan threshold harus mengikuti konsekuensi kesalahan prediksi. Untuk penggunaan nyata di bidang kesehatan, diperlukan evaluasi dan validasi yang jauh lebih ketat.
