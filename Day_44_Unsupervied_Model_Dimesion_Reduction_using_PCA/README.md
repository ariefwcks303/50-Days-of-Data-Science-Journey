# Day 44 — Reduksi Dimensi Menggunakan Principal Component Analysis (PCA)

## 📌 Gambaran Proyek

Notebook ini mengeksplorasi **Principal Component Analysis (PCA)**, metode *unsupervised learning* untuk merangkum banyak fitur numerik menjadi sejumlah komponen utama yang mempertahankan sebanyak mungkin **variansi data**. Studi kasus menggunakan **Red Wine Quality Dataset**, dengan 11 fitur kimiawi dan skor kualitas anggur.

Fokus eksperimen adalah eksplorasi data, penanganan duplikasi, standardisasi fitur, analisis korelasi, serta pengukuran **explained variance ratio** untuk menentukan jumlah komponen PCA yang sesuai. Notebook belum melatih model prediksi ataupun memvisualisasikan hasil transformasi PCA dalam scatter plot dua dimensi.

## 🎯 Tujuan Pembelajaran

- Memahami prinsip PCA dan konsep komponen utama.
- Menyiapkan fitur numerik menggunakan `StandardScaler`.
- Mengidentifikasi korelasi antarfitur dan hubungannya dengan kualitas anggur.
- Menghitung **explained variance ratio** setiap komponen.
- Menentukan jumlah komponen berdasarkan variansi kumulatif yang ingin dipertahankan.
- Memahami trade-off antara pengurangan dimensi dan kehilangan variansi.

## 📂 Dataset

**Sumber:** [Kaggle — Red Wine Quality (Cortez et al., 2009)](https://www.kaggle.com/datasets/uciml/red-wine-quality-cortez-et-al-2009)

| Informasi | Nilai |
|---|---|
| Dataset | Red Wine Quality |
| Jumlah baris awal | 1.599 |
| Jumlah kolom | 12 |
| Fitur numerik untuk PCA | 11 |
| Kolom kualitas | `quality` (skor 3–8) |
| Missing values | 0 |
| Baris duplikat | 240 |
| Baris setelah penghapusan duplikat | 1.359 |

**Fitur yang digunakan untuk PCA:** `fixed acidity`, `volatile acidity`, `citric acid`, `residual sugar`, `chlorides`, `free sulfur dioxide`, `total sulfur dioxide`, `density`, `pH`, `sulphates`, dan `alcohol`.

Kolom `quality` **tidak digunakan sebagai input PCA**. Notebook juga membuat kategori kualitas (`rendah` untuk ≤5, `sedang` untuk 6, dan `tinggi` untuk >6) sebagai persiapan visualisasi, tetapi belum menampilkan scatter plot PCA berdasarkan kategori tersebut.

## 🛠️ Teknologi

- Python
- Pandas dan NumPy
- Matplotlib dan Seaborn
- Scikit-learn (`StandardScaler`, `PCA`)
- Jupyter Notebook / Google Colab

## 🔬 Alur Eksperimen

### 1. Memuat dan Memahami Data

Dataset diunduh melalui Kaggle CLI, lalu dibaca dari `winequality-red.csv`. Eksplorasi awal menggunakan `head()`, `info()`, `describe()`, serta pemeriksaan distribusi nilai `quality`.

Distribusi skor kualitas sebelum penghapusan duplikasi:

| Skor kualitas | Jumlah |
|---|---:|
| 3 | 10 |
| 4 | 53 |
| 5 | 681 |
| 6 | 638 |
| 7 | 199 |
| 8 | 18 |

Sebagian besar observasi memiliki skor kualitas **5–6**.

### 2. Pembersihan Data

Pemeriksaan awal menemukan **0 missing values** dan **240 baris duplikat**. Notebook menghapus baris yang identik dengan `drop_duplicates()`, sehingga tersisa **1.359 observasi**.

### 3. Standardisasi Fitur

Fitur dan skor kualitas dipisahkan:

```python
X = df.drop('quality', axis=1)
y = df['quality']

scaler = StandardScaler()
X_std = scaler.fit_transform(X)
```

Standardisasi penting karena PCA mencari arah dengan variansi terbesar. Tanpa standardisasi, fitur yang memiliki skala angka lebih besar berpotensi mendominasi hasil komponen utama.

> **Catatan:** Notebook menggunakan seluruh data untuk eksplorasi PCA, tanpa train-test split. Jika PCA nantinya digunakan sebagai tahap preprocessing dalam model prediktif, `StandardScaler` dan PCA harus di-*fit* hanya pada data latih, idealnya melalui `Pipeline`, untuk menghindari *data leakage*.

### 4. Analisis Korelasi

Notebook memvisualisasikan matriks korelasi menggunakan heatmap. Korelasi yang dicatat terhadap `quality` antara lain:

| Fitur | Korelasi dengan `quality` |
|---|---:|
| `alcohol` | +0,48 |
| `sulphates` | +0,25 |
| `citric acid` | +0,23 |
| `volatile acidity` | −0,40 |
| `density` | −0,18 |
| `total sulfur dioxide` | −0,18 |
| `chlorides` | −0,13 |
| `residual sugar` | +0,01 |
| `pH` | −0,06 |

Dalam eksperimen ini, `alcohol` memiliki korelasi positif terkuat dengan kualitas, sedangkan `volatile acidity` memiliki korelasi negatif terkuat. **Korelasi bukan hubungan sebab-akibat.**

### 5. Menjalankan PCA dan Mengukur Explained Variance

Notebook menjalankan PCA penuh terhadap 11 fitur terstandardisasi:

```python
pca_full = PCA()
pca_full.fit(X_std)

evr = pca_full.explained_variance_ratio_
cum = np.cumsum(evr)
```

Hasilnya:

| Komponen | Variansi per komponen | Variansi kumulatif |
|---|---:|---:|
| PC1 | 28,29% | 28,29% |
| PC2 | 17,35% | 45,64% |
| PC3 | 14,07% | 59,71% |
| PC4 | 11,37% | 71,08% |
| PC5 | 8,74% | 79,81% |
| PC6 | 5,80% | 85,61% |
| PC7 | 5,34% | 90,94% |
| PC8 | 3,75% | 94,69% |
| PC9 | 3,13% | 97,82% |
| PC10 | 1,65% | 99,46% |
| PC11 | 0,54% | 100,00% |

*Persentase ditampilkan sesuai pembulatan output notebook.*

### 6. Visualisasi Variansi

Notebook membuat dua grafik:

1. **Bar chart explained variance** untuk melihat kontribusi variansi setiap komponen.
2. **Line chart cumulative explained variance** untuk melihat berapa banyak variansi yang dipertahankan ketika komponen ditambahkan, termasuk garis acuan 80%.

Grafik menunjukkan kontribusi PC1 paling besar dan penambahan komponen berikutnya secara bertahap meningkatkan variansi kumulatif.

## 📊 Hasil dan Interpretasi

| Target retensi variansi | Jumlah komponen minimum berdasarkan output |
|---|---:|
| Sekitar 70% | 4 komponen (71,08%) |
| Mendekati 80% | 5 komponen (79,81%) |
| **Minimal 80%** | **6 komponen (85,61%)** |
| Minimal 90% | 7 komponen (90,94%) |

**Temuan utama:** PCA dapat merangkum 11 fitur kimiawi menjadi 5 komponen yang mempertahankan sekitar **79,81% variansi**. Jika persyaratannya setidaknya 80%, perlu **6 komponen** berdasarkan hasil aktual.

PCA memaksimalkan variansi yang dipertahankan, **bukan secara otomatis mempertahankan seluruh informasi yang relevan untuk memprediksi `quality`**. Dampaknya terhadap performa model perlu diuji secara terpisah.

## 💡 Insight Pembelajaran

- **PCA tidak menggunakan label kualitas saat membentuk komponen.** Kolom `quality` dipisahkan dari input PCA.
- **Standardisasi memengaruhi arah komponen.** Fitur dengan satuan dan rentang berbeda perlu ditangani sebelum PCA.
- **Jumlah komponen merupakan trade-off.** Lebih sedikit komponen menyederhanakan representasi, tetapi membuang sebagian variansi.
- **Ambang retensi harus dibaca secara presisi.** Lima komponen mempertahankan 79,81%, bukan minimal 80%.
- **PCA belum tentu meningkatkan prediksi.** Manfaat terhadap kecepatan, generalisasi, atau akurasi harus dibuktikan melalui eksperimen lanjutan.

## ⚠️ Keterbatasan Eksperimen

- PCA baru di-*fit* untuk menganalisis variansi; notebook belum menjalankan `transform()` atau `fit_transform()` PCA untuk menghasilkan dataset komponen utama.
- Visualisasi scatter PC1–PC2 yang disebut pada tujuan belum diimplementasikan.
- Analisis *component loadings* atau kontribusi fitur asli terhadap setiap komponen belum dilakukan.
- Belum ada perbandingan performa model sebelum dan sesudah PCA.
- Belum ada train-test split, cross-validation, atau evaluasi terhadap tugas prediksi.
- Penghapusan duplikasi dilakukan secara langsung; pengaruhnya terhadap representativitas data belum diuji.

## 🚀 Pengembangan Berikutnya

- Menjalankan `PCA(n_components=2)` dan memvisualisasikan proyeksi PC1–PC2 dengan warna berdasarkan kategori kualitas.
- Memeriksa `pca.components_` untuk menafsirkan *loadings* setiap fitur pada komponen utama.
- Membangun dataset hasil transformasi PCA dengan 5, 6, atau 7 komponen.
- Membandingkan model prediktif menggunakan 11 fitur asli dan fitur hasil PCA.
- Menggunakan `Pipeline` dan cross-validation agar standardisasi dan PCA tidak menyebabkan data leakage dalam eksperimen prediktif.
- Mengukur trade-off performa, interpretabilitas, dan waktu komputasi.

## 📁 Struktur Proyek

```text
Day_44_Unsupervised_Model_Reduction_Dimesion_using_PCA/
├── Day_44_Unsupervised_Model_Reduction_Dimesion_using_PCA.ipynb
└── README.md
```

## 📝 Kesimpulan

Day 44 mempraktikkan konsep dasar **dimensionality reduction** menggunakan PCA pada Red Wine Quality Dataset. Dari **11 fitur numerik**, lima komponen pertama mempertahankan **79,81% variansi**, sedangkan enam komponen mempertahankan **85,61%**.

Eksperimen menunjukkan cara membaca *explained variance ratio* dan menentukan jumlah komponen secara berbasis data. Tahap berikutnya adalah mentransformasikan fitur ke ruang PCA, menafsirkan *loadings*, serta menguji apakah reduksi dimensi benar-benar membantu tugas machine learning yang dituju.


---

## 👨‍💻 Author

**Arief Wicaksono**  
Aspiring AI Engineer | Data Science Student

---

⭐ *Learning Machine Learning one day at a time.*