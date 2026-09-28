# 💵 Day 22 — Prediksi Tip Restoran dengan Linear Regression

Project ini membahas penerapan **Linear Regression** untuk memprediksi besarnya tip yang diberikan pelanggan restoran berdasarkan informasi transaksi dan karakteristik pelanggan.

Fokus utama project adalah memahami bagaimana **feature engineering, categorical encoding, model regression, evaluasi metrik, dan interpretasi koefisien** digunakan dalam workflow machine learning sederhana.

---

## 🎯 Project Objectives

Tujuan pembelajaran dalam project ini:

* Membuat fitur baru `tip_pctg` sebagai persentase tip terhadap total tagihan.
* Melakukan exploratory data analysis terhadap faktor yang berkaitan dengan tip.
* Melakukan **One-Hot Encoding** terhadap fitur kategorikal.
* Mencegah **target leakage** dengan mengeluarkan `tip_pctg` dari fitur training.
* Membagi dataset menjadi data training dan testing.
* Melatih model **Linear Regression**.
* Mengevaluasi model menggunakan:

  * R²
  * MAE
  * RMSE
* Menginterpretasikan koefisien model.
* Membandingkan nilai tip aktual dengan hasil prediksi.

---

# 📂 Dataset

Dataset yang digunakan adalah **Tips Dataset**, yaitu dataset klasik mengenai transaksi restoran.

📌 Source: [Kaggle — Seaborn Tips Dataset](https://www.kaggle.com/datasets/ranjeetjain3/seaborn-tips-dataset)

Dataset terdiri dari:

* **244 baris**
* **7 kolom**

### Dataset Features

| Feature      | Description                             |
| ------------ | --------------------------------------- |
| `total_bill` | Total tagihan restoran dalam USD        |
| `tip`        | Besarnya tip dalam USD — **target**     |
| `sex`        | Jenis kelamin pembayar                  |
| `smoker`     | Apakah pelanggan berada di area perokok |
| `day`        | Hari transaksi                          |
| `time`       | Waktu makan                             |
| `size`       | Jumlah orang dalam satu meja            |

### Target

```text
tip
```

Model bertujuan memprediksi nilai `tip` berdasarkan fitur transaksi dan kategori pelanggan.

---

# 🛠️ Tech Stack

* **Python**
* **Pandas** — Data manipulation
* **NumPy** — Numerical computation
* **Matplotlib** — Data visualization
* **Seaborn** — Statistical visualization
* **Scikit-learn** — Machine learning & model evaluation

Model yang digunakan:

```text
Linear Regression
```

---

# 🔎 Machine Learning Workflow

```text
Tips Dataset
     │
     ▼
Data Exploration
     │
     ▼
Feature Engineering
     │
     └── Create tip_pctg
     │
     ▼
EDA & Visualization
     │
     ├── Bill vs Tip
     ├── Tip Percentage Distribution
     └── Tip by Day & Time
     │
     ▼
Preprocessing
     │
     ├── One-Hot Encoding
     ├── Remove Target Leakage
     └── Train/Test Split
     │
     ▼
Linear Regression
     │
     ▼
Prediction
     │
     ▼
Model Evaluation
     │
     ├── R²
     ├── MAE
     └── RMSE
     │
     ▼
Coefficient Interpretation
     │
     ▼
Business Insight
```

---

# 🔍 1. Data Exploration

Dataset memiliki **244 baris dan 7 kolom**.

Tidak ditemukan missing value pada dataset.

Beberapa fitur bertipe kategorikal:

```text
sex
smoker
day
time
```

Fitur-fitur tersebut perlu diubah menjadi representasi numerik sebelum digunakan oleh Linear Regression.

Secara deskriptif:

* Rata-rata `total_bill` sekitar **19.8 USD**.
* Rata-rata `tip` sekitar **3 USD**.
* Ukuran meja umumnya sekitar **2–3 orang**.

---

# ⚙️ 2. Feature Engineering — Tip Percentage

Dibuat fitur baru:

```python
df["tip_pctg"] = df["tip"] / df["total_bill"] * 100
```

Fitur ini menunjukkan persentase tip terhadap total tagihan.

### Insight

Rata-rata pelanggan memberikan tip sekitar:

> **16% dari total tagihan**

Sebagian besar pelanggan berada pada rentang sekitar **10–20%**, meskipun terdapat beberapa observasi dengan persentase tip yang jauh lebih tinggi.

---

# 📊 3. Exploratory Data Analysis

## A. Total Bill vs Tip

Scatter plot dengan regression line digunakan untuk melihat hubungan antara:

```text
total_bill → tip
```

### Insight

Terlihat hubungan positif antara `total_bill` dan `tip`.

Secara umum:

> semakin besar total tagihan, semakin besar pula nominal tip.

Hal ini membuat `total_bill` menjadi salah satu fitur penting dalam memprediksi nilai tip.

> ⚠️ Hubungan yang terlihat pada visualisasi menunjukkan association, bukan bukti hubungan sebab-akibat.

---

## B. Distribusi Tip Percentage

Distribusi `tip_pctg` menunjukkan bahwa mayoritas pelanggan memberikan tip sekitar:

```text
10% – 20%
```

Terdapat beberapa observasi dengan tip percentage di atas **40%**, yang terlihat sebagai outlier.

---

## C. Tip berdasarkan Hari dan Waktu

Rata-rata tip dibandingkan berdasarkan:

* Hari
* Waktu makan

### Insight

Tip cenderung lebih tinggi pada:

> **Dinner**

dibandingkan:

> **Lunch**

Selain itu, tip pada akhir pekan terlihat sedikit lebih tinggi.

Namun, perbedaan tersebut perlu dibaca bersama dengan ukuran tagihan karena tip nominal sangat berkaitan dengan `total_bill`.

---

# 🧹 4. Preprocessing

Model machine learning membutuhkan input numerik.

Oleh karena itu, fitur kategorikal:

```text
sex
smoker
day
time
```

diubah menggunakan **One-Hot Encoding**:

```python
pd.get_dummies(
    df.drop(columns=["tip", "tip_pctg"]),
    columns=feat_cat,
    drop_first=True
)
```

Parameter:

```text
drop_first=True
```

digunakan untuk menghilangkan satu kategori sebagai baseline/reference category.

---

# ⚠️ Target Leakage Prevention

Fitur:

```text
tip_pctg
```

**tidak digunakan sebagai input model.**

Alasannya:

```text
tip_pctg = tip / total_bill × 100
```

Sementara:

```text
tip
```

adalah target yang ingin diprediksi.

Jika `tip_pctg` dimasukkan sebagai fitur, model akan mendapatkan informasi yang secara langsung berasal dari target.

```text
tip
 ↑
tip_pctg
```

Hal tersebut merupakan bentuk **target leakage**.

Karena itu, `tip_pctg` hanya digunakan untuk analisis/EDA dan dikeluarkan sebelum training model.

---

# ✂️ 5. Train/Test Split

Dataset dibagi menjadi:

```text
80% → Training Data
20% → Testing Data
```

Menggunakan:

```python
train_test_split(
    X,
    y,
    test_size=0.2,
    random_state=42
)
```

Data training digunakan untuk mempelajari hubungan antara fitur dan target.

Data testing digunakan untuk mengevaluasi kemampuan model terhadap data yang tidak digunakan selama training.

---

# 🤖 6. Linear Regression

Model yang digunakan:

```python
model = LinearRegression()
```

Model kemudian dilatih menggunakan:

```python
model.fit(X_train, y_train)
```

Prediksi dilakukan menggunakan:

```python
y_pred = model.predict(X_test)
```

Secara konseptual, Linear Regression mencoba mempelajari hubungan:

```text
Features
   ↓
Linear Combination
   ↓
Predicted Tip
```

---

# 📏 7. Model Evaluation

Model dievaluasi menggunakan tiga metrik:

## R² — Coefficient of Determination

Hasil:

> **R² ≈ 0.44**

Artinya model menjelaskan sekitar **44% variasi nilai tip** pada data pengujian.

Masih terdapat variasi tip yang tidak dijelaskan oleh fitur-fitur dalam model.

---

## MAE — Mean Absolute Error

Hasil:

> **MAE ≈ $0.67**

Secara rata-rata, prediksi model memiliki selisih sekitar **0.67 USD** dari nilai tip aktual.

---

## RMSE — Root Mean Squared Error

Hasil:

> **RMSE ≈ $0.84**

RMSE memberikan penalti lebih besar terhadap error yang besar dibandingkan MAE.

Karena:

```text
RMSE > MAE
```

terdapat beberapa prediction error yang relatif lebih besar dan mendapatkan penalti tambahan dari RMSE.

---

# 📊 8. Model Coefficient Interpretation

Koefisien Linear Regression digunakan untuk memahami arah dan besarnya hubungan masing-masing fitur terhadap prediksi tip, dengan fitur lain dianggap konstan.

## Positive Coefficients

### `size` → +0.23

Merupakan salah satu koefisien positif terbesar.

Secara model, setiap tambahan satu orang pada ukuran meja berkaitan dengan peningkatan prediksi tip sekitar **0.23 USD**, dengan asumsi fitur lain konstan.

### `time_Lunch` → +0.09

Memiliki hubungan positif terhadap prediksi tip dibandingkan kategori referensinya.

### `total_bill` → +0.09

Memiliki hubungan positif dengan nilai tip.

Secara intuitif, tagihan yang lebih besar cenderung diikuti oleh tip nominal yang lebih besar.

### `sex_Male` → +0.03

Koefisiennya relatif kecil sehingga pengaruh fitur ini dalam model terlihat terbatas dibandingkan fitur lainnya.

---

# 📉 Negative Coefficients

### `smoker_Yes` → -0.19

Model menghasilkan koefisien negatif dibandingkan kategori referensinya.

Dengan fitur lain konstan, kategori tersebut berkaitan dengan prediksi tip yang lebih rendah sekitar **0.19 USD**.

### `day_Sat` → -0.19

Memiliki koefisien negatif terhadap kategori hari referensi.

### `day_Thur` → -0.18

Juga menunjukkan koefisien negatif dibandingkan kategori referensi.

### `day_Sun` → -0.05

Memiliki pengaruh negatif yang relatif kecil dibandingkan kategori referensinya.

> **Catatan:** Koefisien kategorikal harus dibaca relatif terhadap kategori referensi yang dihasilkan oleh `drop_first=True`, bukan sebagai efek absolut dari kategori tersebut.

---

# 🎯 9. Actual vs Predicted

Visualisasi terakhir membandingkan:

```text
Actual Tip
    vs
Predicted Tip
```

Garis diagonal menunjukkan kondisi:

```text
Prediction = Actual
```

### Insight

Sebagian besar titik berada di sekitar garis prediksi sempurna.

Hal tersebut menunjukkan bahwa model mampu menangkap **tren umum nilai tip**, meskipun masih terdapat error pada prediksi individual.

---

# 💡 Key Insights

Beberapa insight utama dari project:

### 1. `total_bill` merupakan predictor penting

Terdapat hubungan positif antara total tagihan dan nominal tip.

### 2. Tip percentage relatif terkonsentrasi

Rata-rata tip berada di sekitar **16%** dari total tagihan, dengan mayoritas observasi berada sekitar **10–20%**.

### 3. Model menjelaskan sebagian variasi tip

Linear Regression menghasilkan:

```text
R²   ≈ 0.44
MAE  ≈ $0.67
RMSE ≈ $0.84
```

Model menangkap sebagian pola, tetapi masih terdapat faktor lain yang tidak direpresentasikan dalam dataset.

### 4. `size` memiliki koefisien positif terbesar

Jumlah orang dalam satu meja memiliki hubungan positif yang cukup kuat dengan prediksi nominal tip.

### 5. Categorical features memberikan tambahan informasi

Variabel seperti `smoker`, `day`, `time`, dan `sex` memberikan kontribusi terhadap model, tetapi koefisiennya perlu dibaca relatif terhadap kategori baseline.

---

# 💼 Business Insight

Jika tujuan bisnis adalah memperkirakan pendapatan tip, model menunjukkan bahwa informasi transaksi seperti:

```text
Total Bill
+
Table Size
```

sudah memberikan informasi yang cukup berarti.

Informasi mengenai:

```text
Day
Time
Smoker
Sex
```

memberikan tambahan informasi, tetapi kontribusinya dalam model ini relatif lebih kecil dibandingkan faktor transaksi utama.

Secara sederhana:

```text
Transaction Value
       +
   Table Size
       ↓
 Tip Prediction
```

Model sederhana seperti ini dapat menjadi baseline sebelum mencoba model regression yang lebih kompleks.

---

# ⚠️ Modeling Pitfalls

### 1. Target Leakage

`tip_pctg` tidak boleh digunakan sebagai feature karena dihitung menggunakan target `tip`.

### 2. Small Dataset

Dataset hanya memiliki **244 observasi**, sehingga hasil
