# 🌸 Day 31 — Klasifikasi Bunga Iris dengan K-Nearest Neighbor

Project ini merupakan lanjutan dari pembelajaran klasifikasi biner pada Day 29 dan klasifikasi Titanic pada Day 30. Pada Day 31, fokus pembelajaran bergeser ke **klasifikasi multi-class** menggunakan algoritma **K-Nearest Neighbors (KNN)** pada **Iris Dataset**.

KNN mengklasifikasikan data baru berdasarkan tetangga terdekatnya. Karena KNN merupakan algoritma berbasis jarak, proses **feature scaling** menjadi bagian penting dari preprocessing.

---

## 🎯 Tujuan Pembelajaran

- Memahami cara kerja **K-Nearest Neighbors (KNN)** untuk klasifikasi multi-class.
- Memahami pentingnya **scaling** pada algoritma berbasis jarak.
- Mencari nilai **k terbaik** melalui percobaan beberapa nilai k.
- Memahami penggunaan **LabelEncoder** untuk target kategorikal.
- Mengevaluasi model menggunakan **Confusion Matrix** dan **Classification Report**.
- Mengidentifikasi fitur yang paling membantu membedakan spesies bunga Iris melalui EDA.

---

## 📊 Tentang Dataset

Dataset yang digunakan adalah **Iris Dataset** yang berisi pengukuran bunga dari tiga spesies:

- `Iris-setosa`
- `Iris-versicolor`
- `Iris-virginica`

**Sumber:** Kaggle — uciml/iris

Dataset memiliki:

- **150 baris**
- **6 kolom**
- **4 fitur numerik**
- **1 target kategorikal**
- **1 kolom ID**

### Fitur

| Kolom | Keterangan |
|---|---|
| `SepalLengthCm` | Panjang sepal |
| `SepalWidthCm` | Lebar sepal |
| `PetalLengthCm` | Panjang petal |
| `PetalWidthCm` | Lebar petal |
| `Species` | Target / spesies bunga |
| `Id` | Identifier, tidak digunakan dalam modeling |

Dataset memiliki distribusi kelas yang seimbang, yaitu **50 sampel untuk setiap spesies**, dan tidak ditemukan missing value.

---

## 🔎 Exploratory Data Analysis

Beberapa analisis dilakukan sebelum modeling.

### 1. Distribusi Spesies

Ketiga kelas memiliki jumlah data yang sama. Kondisi ini menunjukkan bahwa dataset balanced dan tidak terdapat dominasi salah satu kelas.

### 2. Petal Length vs Petal Width

Visualisasi menunjukkan bahwa `Iris-setosa` memiliki pemisahan yang sangat jelas dari dua spesies lainnya.

Sementara itu, `Iris-versicolor` dan `Iris-virginica` memiliki sedikit overlap sehingga keduanya relatif lebih sulit dibedakan.

### 3. Distribusi Fitur

Dari boxplot terlihat bahwa:

- Fitur **petal** merupakan pembeda yang sangat kuat.
- `PetalLengthCm` dan `PetalWidthCm` membantu memisahkan `Iris-setosa` dengan sangat jelas.
- Fitur **sepal** memiliki overlap lebih besar, khususnya antara `Iris-versicolor` dan `Iris-virginica`.

---

## 🧹 Preprocessing

Tahapan preprocessing yang dilakukan:

### 1. Menghapus Kolom ID

Kolom `Id` tidak memiliki nilai informatif untuk proses klasifikasi sehingga tidak digunakan sebagai fitur.

### 2. Encoding Target

Target `Species` yang berbentuk teks diubah menjadi angka menggunakan `LabelEncoder`.

Target kemudian direpresentasikan menjadi tiga kelas:

```text
0 → Iris-setosa
1 → Iris-versicolor
2 → Iris-virginica
```

### 3. Train-Test Split

Dataset dibagi menjadi:

- **80% data training**
- **20% data testing**
- `random_state=42`
- menggunakan `stratify=y`

Penggunaan stratification bertujuan mempertahankan proporsi ketiga kelas pada data training dan testing.

### 4. Feature Scaling

Digunakan `StandardScaler`.

Scaling menjadi sangat penting karena KNN menentukan kedekatan berdasarkan **jarak**. Tanpa scaling, fitur dengan skala atau rentang nilai lebih besar dapat mendominasi perhitungan jarak.

Scaling dilakukan dengan:

```python
scaler = StandardScaler()

X_train_scaled = scaler.fit_transform(X_train)
X_test_scaled = scaler.transform(X_test)
```

Scaler hanya di-fit pada data training untuk menghindari data leakage.

---

## 🤖 Modeling — K-Nearest Neighbors

Model yang digunakan:

```python
KNeighborsClassifier()
```

Parameter utama KNN adalah `n_neighbors` atau **k**.

Konsep sederhananya:

- **k terlalu kecil** → model lebih sensitif terhadap noise dan berpotensi overfitting.
- **k terlalu besar** → model menjadi terlalu halus dan berpotensi underfitting.

Pada project ini dilakukan pencarian nilai:

```text
k = 1 sampai 20
```

Setiap nilai k diuji menggunakan data testing dan dibandingkan berdasarkan accuracy.

---

## 🔍 Pemilihan Nilai K

Hasil eksperimen menunjukkan bahwa model memperoleh performa terbaik pada:

```text
k = 1
```

Dengan akurasi:

```text
97%
```

Nilai k terbaik kemudian digunakan untuk membangun model KNN final.

---

## 📈 Model Evaluation

Model dievaluasi menggunakan:

- Accuracy
- Confusion Matrix
- Classification Report
- Precision
- Recall
- F1-Score

### Accuracy

Model menghasilkan:

> **97% accuracy**

Artinya model berhasil mengklasifikasikan **29 dari 30 data testing** dengan benar.

### Confusion Matrix

Hasil evaluasi menunjukkan:

- **Iris-setosa:** 10/10 benar
- **Iris-versicolor:** 10/10 benar
- **Iris-virginica:** 9/10 benar

Terdapat **1 Iris-virginica** yang diprediksi sebagai **Iris-versicolor**.

Hal ini menunjukkan bahwa kedua spesies tersebut memiliki karakteristik yang lebih mirip dibandingkan `Iris-setosa`.

### Classification Report

Ringkasan performa:

| Class | Precision | Recall | F1-Score |
|---|---:|---:|---:|
| Iris-setosa | 1.00 | 1.00 | 1.00 |
| Iris-versicolor | 0.91 | 1.00 | 0.95 |
| Iris-virginica | 1.00 | 0.90 | 0.95 |
| **Accuracy** | | | **0.97** |

---

## 💡 Insight Utama

### 1. Dataset Sangat Seimbang

Masing-masing spesies memiliki **50 sampel**, sehingga tidak terdapat ketidakseimbangan kelas yang signifikan.

### 2. Petal Merupakan Fitur yang Sangat Informatif

`PetalLengthCm` dan `PetalWidthCm` memberikan pemisahan yang sangat jelas, terutama untuk mengenali `Iris-setosa`.

### 3. Scaling Sangat Penting untuk KNN

Karena KNN menggunakan jarak untuk menentukan tetangga terdekat, standardisasi fitur membantu membuat kontribusi antar fitur menjadi lebih sebanding.

### 4. Versicolor dan Virginica Lebih Sulit Dibedakan

Kesalahan klasifikasi yang ditemukan terjadi antara `Iris-versicolor` dan `Iris-virginica`. Hal ini sejalan dengan hasil EDA yang menunjukkan adanya overlap karakteristik kedua spesies tersebut.

### 5. KNN Memberikan Performa Tinggi

Dengan pemilihan nilai k terbaik, KNN menghasilkan akurasi **97%** pada data testing.

---

## 🧠 Workflow

```text
Iris Dataset
      │
      ▼
Exploratory Data Analysis
      │
      ▼
Remove ID Column
      │
      ▼
Label Encoding Target
      │
      ▼
Train-Test Split (80/20)
      │
      ▼
StandardScaler
      │
      ▼
Search k = 1..20
      │
      ▼
Select Best k
      │
      ▼
K-Nearest Neighbors
      │
      ▼
Model Evaluation
      │
      ├── Accuracy → 97%
      ├── Confusion Matrix
      └── Classification Report
```

---

## 🛠️ Tech Stack

- Python
- Pandas
- NumPy
- Matplotlib
- Seaborn
- Scikit-learn
- Jupyter Notebook

### Library Utama

```python
from sklearn.model_selection import train_test_split
from sklearn.preprocessing import StandardScaler, LabelEncoder
from sklearn.neighbors import KNeighborsClassifier
from sklearn.metrics import (
    confusion_matrix,
    classification_report,
    ConfusionMatrixDisplay,
    accuracy_score
)
```

---

## ⚠️ Limitations

Beberapa keterbatasan dari eksperimen ini:

- Dataset Iris relatif kecil dan sederhana.
- Evaluasi dilakukan menggunakan satu train-test split.
- Pemilihan k dilakukan berdasarkan accuracy pada test set, sehingga pendekatan ini belum menggunakan cross-validation untuk pemilihan hyperparameter.
- Belum dilakukan perbandingan KNN dengan beberapa algoritma klasifikasi lainnya.
- Belum dilakukan deployment atau monitoring model.

Untuk workflow machine learning yang lebih production-ready, eksperimen berikutnya dapat menggunakan **Cross-Validation**, pipeline preprocessing + model, hyperparameter tuning, dan evaluasi pada data yang benar-benar unseen.

---

## 📌 Kesimpulan

Pada Day 31, berhasil dibangun model **K-Nearest Neighbors** untuk melakukan klasifikasi multi-class pada Iris Dataset.

Model menggunakan preprocessing berupa **LabelEncoder**, **Train-Test Split dengan Stratification**, dan **StandardScaler** sebelum proses training.

Setelah melakukan pencarian nilai k dari 1 hingga 20, diperoleh **k terbaik = 1** dengan akurasi **97%** pada data testing.

Hasil evaluasi menunjukkan bahwa `Iris-setosa` dapat diklasifikasikan dengan sempurna, sementara sebagian kecil kesalahan terjadi antara `Iris-versicolor` dan `Iris-virginica`.

Project ini memperkuat pemahaman mengenai:

> **Multi-Class Classification → Distance-Based Algorithm → Scaling → Hyperparameter Selection → Model Evaluation**

---

## 👤 Author

**Arief Wicaksono**  

*Aspiring AI Engineer | Data Science Student*

---

⭐ If you find this project useful, feel free to star the repository!

