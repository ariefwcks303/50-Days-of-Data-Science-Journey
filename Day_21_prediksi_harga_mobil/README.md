# 📊 Day 21 — Car Price Prediction with Random Forest

Project ini berfokus pada penyelesaian masalah **real-world data cleaning**, **feature engineering**, dan implementasi *machine learning* menggunakan **Random Forest Regressor** untuk memprediksi harga jual mobil bekas.

Berbeda dari dataset yang sudah bersih, dataset ini meniru kondisi nyata di mana data memiliki format yang tidak konsisten, nilai yang hilang, dan pencilan (*outliers*) ekstrem yang membutuhkan pra-pemrosesan ekstra sebelum masuk ke tahap pemodelan.

---

## 🎯 Project Objectives

Tujuan utama project ini adalah:

* Melakukan **Data Cleaning** pada kolom dengan tipe data campuran (teks dan angka).
* Menerapkan **Feature Engineering** untuk mengekstrak informasi tersembunyi yang bernilai prediktif.
* Melakukan **Outlier Removal** berbasis persentil untuk menjaga kewajaran distribusi harga.
* Membangun dan mengevaluasi model **Random Forest Regressor** menggunakan metrik R², MAE, dan RMSE.

---

## 📂 Dataset

Dataset yang digunakan adalah **Car Price Prediction Challenge** yang berisi ~19.000 data iklan mobil bekas.

📌 Source: [Kaggle — Car Price Prediction Challenge](https://www.kaggle.com/datasets/deepcontractor/car-price-prediction-challenge?utm_source=gemini)

### Important Features & Preprocessing

| Feature | Description | Preprocessing Strategy |
| --- | --- | --- |
| `Price` | Harga mobil (Target Variable) | Menghapus pencilan ekstrem (hanya mengambil rentang persentil 1% - 99%) |
| `Levy` | Pajak mobil | Mengubah karakter `'-'` menjadi `NaN`, lalu imputasi menggunakan median |
| `Mileage` | Jarak tempuh | Menghapus teks `" km"` dan mengonversinya menjadi tipe numerik |
| `Engine volume` | Kapasitas mesin | Memisahkan indikator `"Turbo"` menjadi kolom biner terpisah |
| `Prod. year` | Tahun produksi | Dikonversi menjadi fitur baru `age_car` (`2020 - Prod. year`) |

---

## 🛠️ Tech Stack

* **Python**
* **Pandas & NumPy** — Data manipulation & cleaning
* **Matplotlib & Seaborn** — Exploratory Data Analysis (EDA)
* **Scikit-Learn** — Machine Learning modeling & evaluation metrics

---

## 🔎 Analysis Workflow

```text
Car Price Prediction Dataset
         │
         ▼
    Data Loading & EDA
         │
         ├── Data Structure Check
         ├── Descriptive Statistics
         └── Missing Value Identification
         │
         ▼
   Real-World Data Cleaning
         │
         ├── Handling 'Levy' (Imputation)
         └── Cleaning 'Mileage' (Text to Numeric)
         │
         ▼
   Feature Engineering
         │
         ├── Extracting 'Turbo' feature
         ├── Calculating 'Age Car'
         └── Outlier Removal (1% - 99%)
         │
         ▼
    Model Building
         │
         ├── Train-Test Split
         └── Random Forest Regressor
         │
         ▼
  Evaluation & Insights (R², MAE, RMSE)

```

---

# 🧹 1. Real-World Data Cleaning

Data mentah seringkali tidak bisa langsung diproses oleh model *machine learning*. Pada project ini, pembersihan difokuskan pada:

* **Mileage**: Data yang awalnya berformat `str` ("150000 km") dipotong karakter teksnya dan diubah menjadi `float`/`int`.
* **Levy**: Karakter non-standar seperti strip (`-`) yang mengindikasikan nilai kosong diganti menjadi `NaN`, kemudian diisi (*imputation*) menggunakan nilai median agar tidak merusak distribusi data.

---

# ⚙️ 2. Feature Engineering & Outliers

Model akan bekerja lebih baik jika diberikan fitur yang lebih representatif.

* **Turbo Extraction**: Kolom `Engine volume` seringkali digabung dengan teks "Turbo". Fitur ini dipisahkan menjadi kolom biner (`1` untuk Turbo, `0` untuk Non-Turbo) karena kehadiran turbo sangat memengaruhi harga mobil.
* **Age Car**: Tahun produksi (`Prod. year`) diubah menjadi variabel absolut `age_car` agar model lebih mudah memahami depresiasi harga berdasarkan umur kendaraan.
* **Outlier Removal**: Rentang harga didapati sangat *skewed* (ada harga $1 dan harga fantastis yang tidak masuk akal). Pemotongan batas bawah 1% dan batas atas 99% ($110 – $84,675) dilakukan agar model belajar dari harga pasar yang realistis.

---

# 🤖 3. Model Training & Evaluation

Algoritma yang digunakan adalah **Random Forest Regressor**, sebuah metode *ensemble* berbasis *decision tree* yang tangguh terhadap hubungan non-linear.

Evaluasi model menggunakan tiga metrik utama:

* **R-Squared (R²)**: Mengukur seberapa baik variansi target (harga) dapat dijelaskan oleh model.
* **Mean Absolute Error (MAE)**: Rata-rata kesalahan absolut prediksi harga.
* **Root Mean Squared Error (RMSE)**: Memberikan penalti lebih besar pada error atau kesalahan prediksi yang besar.

---

# 💡 Key Insights

* **Depresiasi Umur**: Fitur `age_car` menjadi salah satu faktor penentu terkuat; semakin tua mobil, nilainya turun secara eksponensial.
* **Pengaruh Turbo**: Kendaraan dengan fitur `Turbo` secara konsisten memiliki rata-rata harga jual yang lebih tinggi dibandingkan kendaraan *naturally aspirated* di kelas yang sama.
* **Noise Data**: Harga mobil sangat sensitif terhadap pencilan (*outliers*). Tanpa membuang 1% harga terbawah dan teratas, performa regresi akan anjlok karena berusaha menyesuaikan diri dengan *noise*.

---

# ⚠️ Data & Modeling Pitfalls

* **Imputasi Sembarangan**: Mengisi nilai `NaN` pada kolom `Levy` dengan angka 0 atau rata-rata (*mean*) bisa merusak data jika distribusinya *skewed*. Penggunaan **median** lebih aman untuk data seperti ini.
* **Overfitting pada Tree-Based Model**: Random Forest sangat rentan menghafal data *training* jika kedalaman pohon (*max_depth*) tidak dibatasi. Pastikan selalu membandingkan metrik antara data *train* dan *test*.

---

# 📚 Learning Outcomes

Melalui project ini, beberapa konsep yang dipraktikkan:

* Penanganan *Dirty Data* (RegEx / String manipulation di Pandas)
* Imputasi Missing Values
* Thresholding & Percentile untuk Outliers
* Feature Creation
* Ensemble Machine Learning (Random Forest)
* Regression Metrics (R², MAE, RMSE)

---

# 🚀 Next Step

Project ini menjadi dasar yang kuat untuk pemodelan prediktif tabular. Langkah selanjutnya yang bisa dieksplorasi:

```text
Baseline Random Forest
        ↓
Hyperparameter Tuning (GridSearchCV/RandomizedSearchCV)
        ↓
Advanced Algorithms (XGBoost / LightGBM)
        ↓
Model Deployment (Streamlit / Flask API)

```

---

## 📁 Project Structure

```text
Day_21_Car_Price_Prediction_Random_Forest/
│
├── dataset/
│   └── car_price_prediction.csv
├── Day_21_Car_Price_Prediction_RandomForest.ipynb
└── README.md

```

---

## 👤 Author

**Arief Wicaksono**

Aspiring AI Engineer | Data Science Student