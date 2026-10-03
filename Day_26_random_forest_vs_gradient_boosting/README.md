# 📈 Day 26 — Gradient Boosting Regressor

Project ini merupakan implementasi **Regression Machine Learning** untuk memprediksi harga rumah menggunakan **Gradient Boosting Regressor**.

Project ini berfokus pada bagaimana algoritma *boosting* bekerja secara berurutan dengan cara memperbaiki kesalahan (*residuals*) dari pohon sebelumnya, serta membandingkan performanya dengan **Random Forest Regressor**.

Selain membandingkan metrik evaluasi model, project ini juga mengevaluasi *feature importance* untuk memahami fitur mana yang paling dominan dalam memengaruhi estimasi harga rumah.

---

## 🎯 Project Objectives

Tujuan utama project ini adalah:

* Memahami konsep dasar algoritma *boosting* (pembelajaran bertahap dari kesalahan model sebelumnya).
* Melatih model `GradientBoostingRegressor` pada dataset tabular.
* Menyetel hyperparameter penting seperti `n_estimators` dan `learning_rate` serta mengamati dampaknya terhadap performa model.
* Membandingkan performa **Gradient Boosting** dengan **Random Forest**.
* Mengevaluasi model menggunakan metrik regresi standar.
* Menganalisis *feature importance* untuk mendapatkan wawasan mendalam terkait faktor penentu harga rumah.

---

## 📂 Dataset

Dataset yang digunakan adalah:

**Boston Housing Dataset**

📌 Source: [Kaggle — fedesoriano/the-boston-houseprice-data](https://www.kaggle.com/datasets/fedesoriano/the-boston-houseprice-data)

Dataset awal terdiri dari:

* **506 baris data perumahan**
* **14 kolom / fitur**

Target yang digunakan:

```text
MEDV (Median value of owner-occupied homes in $1,000s)
```

### Important Features

| Feature | Description |
| :--- | :--- |
| `CRIM` | Tingkat kriminalitas per kapita di kota |
| `ZN` | Proporsi lahan perumahan yang zonanya > 25.000 kaki² |
| `INDUS` | Proporsi lahan bisnis non-eceran |
| `CHAS` | Variabel dummy Sungai Charles (1 jika berbatasan) |
| `NOX` | Konsentrasi oksida nitrat (bagian per 10 juta) |
| `RM` | Rata-rata jumlah kamar per hunian |
| `AGE` | Proporsi unit hunian yang dibangun sebelum 1940 |
| `DIS` | Jarak tertimbang ke pusat kerja di Boston |
| `RAD` | Indeks aksesibilitas ke jalan raya radial |
| `TAX` | Nilai pajak properti penuh per $10.000 |
| `PTRATIO` | Rasio murid-guru berdasarkan kota |
| `B` | Proporsi penduduk kulit hitam per kota |
| `LSTAT` | Persentase status populasi sosial rendah |
| `MEDV` | Harga median rumah (dalam ribuan USD) |

---

## 🛠️ Tech Stack

Project ini menggunakan:

* **Python**
* **Pandas** — Manipulasi dan analisis data
* **NumPy** — Komputasi numerik
* **Matplotlib** — Visualisasi data
* **Seaborn** — Visualisasi data statistik
* **Scikit-learn** — Library Machine Learning

Model yang digunakan:

* `GradientBoostingRegressor`
* `RandomForestRegressor`

Evaluation metrics:

* MAE (Mean Absolute Error)
* MSE / RMSE (Root Mean Squared Error)
* R² Score (Coefficient of Determination)

---

# 🔎 Analysis Workflow

```text
Raw Dataset (Boston Housing)
     │
     ▼
Data Understanding & EDA
     │
     ▼
Checking Missing Values (None)
     │
     ▼
Correlation Analysis (Heatmap)
     │
     ▼
Train-Test Split
     │
     ├───────────────┐
     ▼               ▼
 Gradient     Random Forest
 Boosting       (Baseline)
     │               │
     └───────┬───────┘
             ▼
Hyperparameter Tuning (n_estimators, learning_rate)
             │
             ▼
     Model Evaluation
             │
             ├── MSE / RMSE
             ├── MAE
             └── R² Score
             │
             ▼
    Feature Importance
```

---

# 🧹 1. Data Cleaning & Preparation

Berdasarkan pemeriksaan struktur data:
* Dataset berukuran 506 baris dan 14 kolom.
* Tidak ditemukan adanya *missing value* pada dataset ini.
* Karena model berbasis pohon (*tree-based*) seperti Gradient Boosting dan Random Forest tidak sensitif terhadap skala fitur, proses *feature scaling* (seperti Standardized/Normalization) tidak wajib dilakukan.

---

# 📊 2. Exploratory Data Analysis

## A. Distribusi Target (`MEDV`)
Nilai median harga rumah (`MEDV`) berkisar dari sekitar 5 hingga 50 (dalam ribuan USD) dengan rata-rata di angka 22. Nilai maksimal 50 mengindikasikan adanya batasan (*capped*) pada pencatatan harga tertinggi di data asli.

## B. Analisis Korelasi Fitur
Melalui visualisasi *heatmap* korelasi, terlihat bahwa:
* Fitur **`RM`** memiliki korelasi positif yang kuat terhadap harga (`MEDV`).
* Fitur **`LSTAT`** memiliki korelasi negatif yang cukup tinggi terhadap harga rumah.

---

# ✂️ 3. Train-Test Split

Dataset dibagi menjadi data latih dan data uji dengan proporsi standar:

```text
80% Training
20% Testing
```

dengan `random_state = 42` untuk memastikan hasil yang konsisten dan dapat direplikasi.

---

# 🌲 4. Modeling

Dua model ensemble dibandingkan untuk melihat performa prediksinya pada data regresi harga rumah.

## Gradient Boosting Regressor
Gradient Boosting bekerja dengan melatih pohon secara iteratif, di mana setiap pohon baru difokuskan untuk memprediksi sisa kesalahan (*residual*) dari pohon sebelumnya.

Parameter utama yang dieksplorasi:
* `n_estimators`: Jumlah pohon (tahapan boosting).
* `learning_rate`: Ukuran langkah untuk mengecilkan kontribusi setiap pohon.

## Random Forest Regressor
Digunakan sebagai pembanding *ensemble* berbasis *bagging* (parallel) untuk melihat keunggulan pendekatan sekuensial dari Gradient Boosting.

---

# 📈 5. Model Evaluation & Comparison

Model dievaluasi menggunakan metrik regresi standar:
* **MAE (Mean Absolute Error)**: Rata-rata kesalahan mutlak antara nilai aktual dan prediksi.
* **RMSE (Root Mean Squared Error)**: Memberikan penalti lebih besar terhadap error yang tinggi.
* **R² Score**: Seberapa besar variasi target yang dapat dijelaskan oleh model.

### Hasil Eksperimen Hyperparameter (`n_estimators` & `learning_rate`)
* Penambahan nilai `n_estimators` yang terlalu tinggi tanpa kontrol dapat meningkatkan risiko *overfitting* pada Gradient Boosting.
* Penyesuaian `learning_rate` (misalnya dari 0.1 ke nilai yang lebih kecil dengan memperbanyak `n_estimators`) seringkali menaikkan stabilitas dan akurasi model.

---

# 🔍 6. Feature Importance

Analisis `feature_importances_` dari model Gradient Boosting menunjukkan fitur mana yang paling diandalkan oleh model dalam memprediksi harga rumah:
* **`LSTAT`** dan **`RM`** menduduki peringkat teratas sebagai kontributor paling signifikan terhadap variasi harga rumah.

> **Catatan:** Feature importance menunjukkan kontribusi relatif terhadap prediksi model, bukan sebagai analisis sebab-akibat (*causal analysis*).

---

# 💡 Key Insights

1. **Boosting vs Bagging:** Gradient Boosting seringkali mampu memberikan akurasi yang sangat tinggi pada data tabular karena koreksi kesalahan beruntun, meskipun membutuhkan waktu latih yang lebih sensitif terhadap pengaturan *learning rate*.
2. **Skala Data:** Model berbasis pohon tidak memerlukan normalisasi data, sehingga tahapan *preprocessing* lebih ringkas.
3. **Pengaruh Hyperparameter:** Kombinasi optimal antara `n_estimators` dan `learning_rate` adalah kunci utama untuk mendapatkan performa terbaik pada Gradient Boosting.

---

## 📁 Project Structure

```text
Day_26_gradient_boosting/
│
├── Day_26_gradient_boosting.ipynb
└── README.md
```

---

## 👤 Author

**Arief Wicaksono**

Aspiring AI Engineer | Data Science Student

---

⭐ If you find this project useful, feel free to star the repository!