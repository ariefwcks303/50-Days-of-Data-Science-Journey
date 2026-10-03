# 📈 Day 27 — Evaluasi Model Regresi (RMSE, R², dan Cross-Validation)

Project ini merupakan kelanjutan dari rangkaian pembelajaran **Regression Machine Learning** yang berfokus pada cara **mengevaluasi performa model regresi** secara benar dan adil.

Membuat model regresi mungkin terlihat sederhana, namun mengukur seberapa baik model tersebut bekerja di dunia nyata adalah keterampilan utama yang membedakan praktisi pemula dengan profesional. Project ini mendalami pemahaman metrik evaluasi standar serta teknik validasi yang lebih kuat seperti *Cross-Validation*.

---

## 🎯 Project Objectives

Tujuan utama project ini adalah:

* Memahami arti dan perbedaan metrik evaluasi regresi: **MAE**, **MSE**, **RMSE**, dan **R² Score**.
* Menghitung serta menafsirkan metrik evaluasi tersebut pada data uji (*test set*).
* Mengimplementasikan teknik **`cross_val_score`** (dengan $k$-fold CV) untuk mendapatkan estimasi performa model yang jauh lebih stabil dan objektif.
* Memvisualisasikan distribusi skor lintas *fold* serta memahami konsep dasar **overfitting** dalam pemodelan.

---

## 📂 Dataset

Dataset yang digunakan adalah:

**Boston Housing Dataset**

📌 Source: [Kaggle — fedesoriano/the-boston-houseprice-data](https://www.kaggle.com/datasets/fedesoriano/the-boston-houseprice-data)

Dataset ini terdiri dari:

* **506 baris data perumahan**
* **14 kolom / fitur numerik**

Target yang diprediksi:

```text
MEDV (Median value of owner-occupied homes in $1,000s)
```

### Important Features

| Feature | Description |
| :--- | :--- |
| `CRIM` | Tingkat kriminalitas per kapita di kawasan |
| `RM` | Rata-rata jumlah kamar per rumah |
| `LSTAT` | Persentase penduduk berstatus ekonomi rendah |
| `PTRATIO` | Rasio murid-guru berdasarkan kawasan |
| `NOX` | Konsentrasi oksida nitrat (tingkat polusi) |
| `MEDV` | Harga median rumah dalam ribuan USD (**Target**) |

---

## 🛠️ Tech Stack

Project ini menggunakan:

* **Python**
* **Pandas** — Manipulasi dan analisis data
* **NumPy** — Komputasi numerik
* **Matplotlib** — Visualisasi data
* **Seaborn** — Visualisasi data statistik
* **Scikit-learn** — Library Machine Learning

Model dan utilitas yang digunakan:

* `LinearRegression` (sebagai baseline model)
* `train_test_split`, `cross_val_score`, `KFold`
* `mean_absolute_error` (MAE)
* `mean_squared_error` (MSE / RMSE)
* `r2_score` (R² Coefficient of Determination)

---

# 🔎 Analysis Workflow

```text
Raw Dataset (Boston Housing)
     │
     ▼
Data Exploration & Information Check (No Missing Values)
     │
     ▼
Correlation Analysis (Heatmap)
     │
     ▼
Model Training (Linear Regression)
     │
     ├──────────────────────────┐
     ▼                          ▼
Hold-Out Evaluation       Cross-Validation (k=5)
     │                          │
     ├───────────────┐          │
     ▼               ▼          ▼
   MAE, MSE/RMSE    R² Score   Stabilitas Lintas Fold
     │                          │
     └───────────┬──────────────┘
                 ▼
     Analysis & Insights (Overfitting & Metric Choice)
```

---

# 🧹 1. Data Cleaning & Preparation

Berdasarkan pemeriksaan struktur data:
* Dataset berukuran 506 baris dan 14 kolom.
* Seluruh kolom bertipe numerik dan **tidak ditemukan adanya *missing value***, menjadikannya kondisi yang ideal untuk fokus mendalami teknik evaluasi.

---

# 📊 2. Exploratory Data Analysis

## A. Distribusi Target (`MEDV`)
Harga median rumah (`MEDV`) memiliki rentang yang cukup lebar dari sekitar 5 hingga 50 (dalam ribuan USD) dengan rata-rata di angka 22. Rentang nilai ini sangat penting untuk diingat saat menafsirkan besaran nilai *error* (seperti RMSE) agar interpretasinya relevan dengan skala data.

## B. Analisis Korelasi Fitur
Melalui visualisasi *heatmap* korelasi, terlihat bagaimana hubungan linier antara berbagai fitur lingkungan terhadap harga rumah, yang mendasari penggunaan model regresi linier sebagai model dasar (*baseline*).

---

# ✂️ 3. Train-Test Split & Modeling

Dataset dibagi ke dalam porsi latih dan uji untuk menguji kemampuan generalisasi model pada data yang belum pernah dilihat sebelumnya menggunakan algoritma `LinearRegression`.

---

# 📈 4. Model Evaluation Metrics

Evaluasi regresi dilakukan menggunakan berbagai metrik komprehensif:
* **MAE (Mean Absolute Error)**: Rata-rata selisih mutlak antara nilai prediksi dan aktual. Mudah diinterpretasikan karena berada dalam satuan yang sama dengan target.
* **MSE / RMSE (Root Mean Squared Error)**: Akar dari rata-rata kuadrat kesalahan. Memberikan penalti lebih besar pada kesalahan besar, sangat sensitif terhadap *outlier*.
* **R² Score (Coefficient of Determination)**: Mengukur seberapa baik variasi dari fitur independen mampu menjelaskan variasi dari variabel target.

---

# 🔄 5. Cross-Validation

Mengandalkan evaluasi dari satu kali *train-test split* saja terkadang bisa menyesatkan akibat pembagian data yang kurang representatif. Oleh karena itu, diterapkan teknik **`cross_val_score`** (dengan $k=5$):
* Membagi data menjadi 5 bagian (*folds*).
* Melatih dan menguji model secara bergantian sebanyak 5 kali.
* Menghasilkan distribusi skor yang lebih stabil, adil, dan meminimalkan bias dari pemilihan sampel data tertentu.

---

# 💡 Key Insights

1. **Pemilihan Metrik yang Tepat:** Tidak ada satu metrik yang sempurna; MAE bagus untuk memahami error rata-rata secara intuitif, sementara RMSE membantu mendeteksi adanya error besar yang fatal.
2. **Pentingnya Cross-Validation:** Evaluasi menggunakan *Cross-Validation* memberikan gambaran performa model yang jauh lebih reliabel dibanding sekadar mengandalkan satu kali pengujian pada *test set*.
3. **Keterbatasan Skala Target:** Besaran nilai RMSE harus selalu disesuaikan dengan skala dan sebaran data target (`MEDV`) agar bisa disimpulkan apakah model sudah cukup baik atau belum.

---

## 📁 Project Structure

```text
Day_27_regression_evaluation/
│
├── Day_27_regression_evaluation.ipynb
└── README.md
```

---

## 👤 Author

**Arief Wicaksono**

Aspiring AI Engineer | Data Science Student

---

⭐ If you find this project useful, feel free to star the repository!