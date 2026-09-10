
Gemini
Percakapan baru
Telusuri percakapan
Gambar
Koleksi
Notebook baru
Indonesia AI Technology Trends Report 2026
Evaluasi Profil LinkedIn
🤖 AI Engineer Mentor and Learning Framework
🤖 MLOps Lab Assistant | Arief's METC Mentor
🤖 LangChain Academy Guide | Arief's AI Consultant
🤖 McKinsey Forward Program Assistant | Arief Learners
🤖 Hack2Skill Googel APAC Assistant | Arief Consultant
🤖 LLM & Gen AI Lab Assistant | Arief's IDCamp Mentor
Pembuatan README Day 12
Optimasi LinkedIn Bersama HR Manager
Interpretasi Grafik Distribusi Diabetes
Memahami Standar Deviasi Data
Analisis Performa Siswa Python
Pembuatan README.md Day 3
Tawaran Bantuan Teknis AI
Perbandingan Latensi Bare-Metal dan Virtualisasi
Troubleshooting ERR_CONNECTION_TIMED_OUT Error
Mengatasi Situs Naii.co Tidak Bisa Diakses
Tes Turing: Mengukur Kecerdasan Mesin
Benjolan Merah Kelopak Mata: Bintitan atau Kalazion
HP Itel Reset Pabrik Sendiri
Mengembalikan Pemisah Halaman Word
Dada Berkeringat Berlebih: Penyebab dan Solusi
Penanganan Awal Keseleo Pergelangan Kaki
Ubah BPJS PBI ke Mandiri Kelas 3
Tes Darah Pra-Operasi Abses Perianal
Benjolan Dekat Dubur dan Varises Bokong
Meditasi Terpandu Dimulai
Cara Membuka Kemasan Infus NaCl
Perbaiki Daftar Isi Word ke PDF
Varises Bokong: Tanda dan Penanganannya
Huruf Hilang dalam Kutipan Film
Nyeri Anus dan Benjolan: Perlu ke Dokter
Herbal untuk Mengatasi Insomnia
Melihat Hasil Unduh Kagglehub di Colab
Kaki Bengkak, Gatal, Luka: Indikasi Diabetes?
Ubi Rebus untuk Kesehatan Jantung
Laporan Sentimen Publik Regulasi Media Digital
Analisis Sentimen Kebijakan Digital Indonesia
Professional Job Application Response Guidance
Draf Narasi Sertifikat LinkedIn
Integrasi Digital Pengakuan Profesional
Presentasi Beasiswa Pelatihan Gratis Microsoft
Percakapan dengan Gemini
saya punya contoh readme, untuk day ke 11, nah saya butuh kamu buatkan untuk day ke 12, itu notebooknya. tolong ya buatkan readme day 12 dengan mengikuti contoh day 11

📥 1. Data Loading & Inspection
Dataset dimuat menggunakan Pandas dan diperiksa struktur tipe datanya serta kelengkapannya.

Python
df = pd.read_csv('winequality-red.csv')
df.head()
Hasil pemeriksaan menunjukkan seluruh 12 kolom berisi nilai numerik (float64 dan int64) dengan total 1.599 baris dan tidak ada missing value.

📈 2. Histplot & KDEPlot — Distribusi Kadar Alkohol
Histogram digabungkan dengan kurva Estimasi Kepadatan Kernel (kde=True) untuk melihat sebaran kadar alkohol (alcohol) pada red wine.

Python
plt.figure(figsize=(12, 5))
sns.histplot(data=df, x='alcohol', kde=True)
plt.title('Distribusi Kadar Alkohol Red Wine')
plt.xlabel('Alcohol %')
plt.ylabel('Frequency')
plt.show()
Insight
Mayoritas sampel wine memiliki kadar alkohol berkisar di angka 9% hingga 10.5%.

Distribusi bersifat right-skewed (cenderung menceng ke kanan), dengan beberapa sampel memiliki kadar alkohol tinggi hingga 14.9%.

📦 3. Boxplot & Violinplot — Perbandingan Fitur Berdasarkan Kualitas
Boxplot dan violinplot digunakan untuk membandingkan distribusi variabel kimia (seperti volatile acidity dan alcohol) terhadap level quality wine.

Python
plt.figure(figsize=(12, 6))
sns.boxplot(data=df, x='quality', y='alcohol', palette='Set2')
plt.title('Distribusi Alkohol Berdasarkan Skor Kualitas Wine')
plt.show()
Insight
Wine dengan skor kualitas yang lebih tinggi (quality 7 dan 8) cenderung memiliki median kadar alkohol yang lebih tinggi dibandingkan kualitas rendah (quality 5).

Sebaliknya, kadar volatile acidity (keasaman menguap) cenderung lebih rendah pada wine dengan kualitas tinggi.

🌡️ 4. Correlation Heatmap — Hubungan Antar Fitur Kimia
Heatmap digunakan untuk memvisualisasikan matriks korelasi Pearson antar seluruh fitur numerik dalam dataset.

Python
plt.figure(figsize=(10, 8))
corr = df.corr()
sns.heatmap(corr, annot=True, cmap='coolwarm', fmt='.2f', linewidths=0.5)
plt.title('Matriks Korelasi Fitur Kimiawi Red Wine')
plt.show()
Insight
Terdapat korelasi positif yang kuat antara fixed acidity dan density serta citric acid.

pH dan fixed acidity memiliki korelasi negatif yang kuat (secara kimiawi, asam yang tinggi menurunkan pH).

alcohol menunjukkan korelasi positif paling signifikan terhadap skor quality.

👥 5. Pairplot — Eksplorasi Hubungan Multivariate
Pairplot digunakan untuk melihat hubungan sebar pasangan variabel kunci secara simultan.

Python
sns.pairplot(df[['alcohol', 'sulphates', 'volatile acidity', 'quality']], hue='quality', palette='viridis')
plt.show()
Insight
Membantu melihat klaster atau pemisahan antar kelas kualitas berdasarkan kombinasi dua fitur kimia secara cepat.

📊 6. Barplot dengan Estimator — Rata-Rata Sulphates per Kualitas
Barplot digunakan untuk menampilkan estimasi nilai rata-rata (estimator=np.mean) dari fitur kimiawi dikelompokkan berdasarkan tingkat kualitas, lengkap dengan interval kepercayaan (error bar).

Python
plt.figure(figsize=(10, 5))
sns.barplot(data=df, x='quality', y='sulphates', estimator=np.mean, ci='sd', palette='Blues_d')
plt.title('Rata-Rata Kadar Sulphates pada Setiap Tingkat Kualitas Wine')
plt.show()
Insight
Wine dengan kualitas yang lebih tinggi menunjukkan rata-rata kadar sulphates yang cenderung meningkat.

🎨 Statistical Visualization Best Practices
Beberapa prinsip penting dalam menggunakan Seaborn:

1. Memilih Jenis Grafik yang Tepat
Gunakan Histplot/KDE untuk distribusi satu variabel.

Gunakan Boxplot/Violinplot untuk perbandingan kategori vs numerik.

Gunakan Heatmap untuk korelasi banyak fitur sekaligus.

2. Memanfaatkan Fitur hue
Menambahkan dimensi ketiga secara visual tanpa membuat plot baru yang rumit.

💡 Key Insights
Beberapa insight utama dari analisis statistik red wine:

🍷 Kadar alkohol memiliki korelasi positif yang paling nyata dengan peningkatan skor kualitas wine.

📉 Volatile acidity (keasaman menguap) yang tinggi cenderung menurunkan kualitas rasa wine secara signifikan.

⚖️ Fitur-fitur kimia seperti pH, density, dan fixed acidity saling berkaitan erat secara kimiawi.

📊 Grafik statistik Seaborn memudahkan identifikasi pola, sebaran, dan anomali data jauh lebih cepat dibandingkan statistik deskriptif mentah.

📚 Learning Outcomes
Melalui project ini, konsep yang dipraktikkan meliputi:

Seaborn library introduction

Univariate distribution (histplot, kdeplot)

Group comparison (boxplot, violinplot)

Correlation analysis (heatmap)

Multivariate exploration (pairplot)

Aggregation plot (barplot with estimator)

Statistical data visualization best practices

🚀 Next Step
Visualisasi statistik merupakan fondasi penting sebelum membangun model Machine Learning.

Plaintext
Data Loading
 ↓
Data Cleaning
 ↓
Statistical Visualization (Seaborn)
 ↓
Feature Engineering
 ↓
Machine Learning Modeling
Pemahaman ini menjadi jembatan krusial menuju tahap pemodelan prediktif selanjutnya.

📁 Project Structure
Plaintext
Day_12_Statistical_Visualization_Seaborn/
│
├── Day_12_Statistical_Visualization_Seaborn.ipynb
└── README.md
👤 Author
Arief Wicaksono

Aspiring AI Engineer | Data Science Student

⭐ If you find this project useful, feel free to star the repository!
"""

with open("README.md", "w", encoding="utf-8") as f:
f.write(content)
print("README.md created successfully.")


```text?code_stdout&code_event_index=1
README.md created successfully.

File README.md untuk Day 12 (berdasarkan struktur dan format Day 11 serta isi notebook Red Wine Quality dengan Seaborn) sudah berhasil dibuat.

Ikon MD
README
 MD 
Berikut adalah isi dari file README.md Day 12:

Markdown
# 📊 Day 12 — Visualisasi Statistik dengan Seaborn

Notebook ini membahas dasar-dasar **visualisasi statistik menggunakan Seaborn** di atas Matplotlib dengan menggunakan dataset **Red Wine Quality** (UCI Machine Learning).

Fokus utama project adalah memahami bagaimana membuat grafik statistik tingkat tinggi yang langsung menampilkan **distribusi, hubungan antar variabel, perbandingan antar grup, serta matriks korelasi**.

Visualisasi yang dibuat meliputi **histplot & kdeplot, boxplot & violinplot, correlation heatmap, pairplot, serta barplot dengan estimator**.

---

## 🎯 Project Objectives

Tujuan pembelajaran dalam project ini adalah:

* Memahami dasar visualisasi statistik tingkat tinggi menggunakan Seaborn.
* Membuat `histplot` & `kdeplot` untuk melihat **distribusi** variabel numerik.
* Membuat `boxplot` & `violinplot` untuk membandingkan **distribusi antar grup** kualitas anggur.
* Membuat **heatmap korelasi** untuk melihat hubungan linier antar fitur kimiawi.
* Membuat `pairplot` untuk mengeksplorasi banyak hubungan variabel sekaligus.
* Membuat `barplot` dengan **estimator** (rata-rata) lengkap dengan interval kepercayaan.
* Menghasilkan insight statistik dan domain knowledge dari data kimiawi red wine.

---

## 📂 Dataset

Dataset yang digunakan adalah:

**Red Wine Quality**

📌 Source: [Kaggle — Red Wine Quality Dataset](https://www.kaggle.com/datasets/uciml/red-wine-quality-cortez-et-al-2009)

Dataset ini berisi hasil uji laboratorium sampel anggur merah Portugal (Vinho Verde), mencakup berbagai sifat fisikokimia dan skor kualitas sensorik.

### Dataset Overview

* **1.599 baris**
* **12 kolom**
* Tidak terdapat missing value pada dataset.

### Important Features

| Feature               | Description                                           |
| --------------------- | ----------------------------------------------------- |
| `fixed acidity`       | Keasaman tetap (asam tartarat)                        |
| `volatile acidity`    | Keasaman menguap (asam asetat — bila tinggi terasa cuka) |
| `citric acid`         | Asam sitrat (menambah kesegaran)                      |
| `residual sugar`      | Sisa gula dalam wine                                  |
| `chlorides`           | Kadar garam / klorida                                 |
| `free sulfur dioxide` | Sulfur dioksida bebas (pengawet)                      |
| `total sulfur dioxide`| Total sulfur dioksida                                 |
| `density`             | Densitas / massa jenis wine                           |
| `pH`                  | Tingkat keasaman (pH)                                 |
| `sulphates`           | Kadar sulfat (senyawa sulfur)                         |
| `alcohol`             | Kadar alkohol (% volume)                              |
| `quality`             | Skor kualitas (skala 0–10, penilaian ahli)            |

---

## 🛠️ Tech Stack

Project ini menggunakan:

* **Python**
* **Pandas** — Data manipulation & aggregation
* **NumPy** — Numerical computation
* **Matplotlib** — Dasar kanvas & kustomisasi plot
* **Seaborn** — Visualisasi statistik tingkat tinggi

---

# 🔎 Analysis Workflow

```text
Dataset
   │
   ▼
Data Loading & Inspection
   │
   ▼
Data Verification (Missing Value & Outliers)
   │
   ▼
Statistical Visualization
   │
   ├── Histplot & KDEPlot (Distribusi Variabel Tunggal)
   ├── Boxplot & ViolinPlot (Perbandingan Grup Kualitas)
   ├── Correlation Heatmap (Hubungan Antar Fitur)
   ├── Pairplot (Multivariate Relationship)
   └── Barplot with Estimator (Agregasi & Confidence Interval)
   │
   ▼
Domain & Business Insights
📥 1. Data Loading & Inspection
Dataset dimuat menggunakan Pandas dan diperiksa struktur tipe datanya serta kelengkapannya.

Python
df = pd.read_csv('winequality-red.csv')
df.head()
Hasil pemeriksaan menunjukkan seluruh 12 kolom berisi nilai numerik (float64 dan int64) dengan total 1.599 baris dan tidak ada missing value.

📈 2. Histplot & KDEPlot — Distribusi Kadar Alkohol
Histogram digabungkan dengan kurva Estimasi Kepadatan Kernel (kde=True) untuk melihat sebaran kadar alkohol (alcohol) pada red wine.

Python
plt.figure(figsize=(12, 5))
sns.histplot(data=df, x='alcohol', kde=True)
plt.title('Distribusi Kadar Alkohol Red Wine')
plt.xlabel('Alcohol %')
plt.ylabel('Frequency')
plt.show()
Insight
Mayoritas sampel wine memiliki kadar alkohol berkisar di angka 9% hingga 10.5%.

Distribusi bersifat right-skewed (cenderung menceng ke kanan), dengan beberapa sampel memiliki kadar alkohol tinggi hingga 14.9%.

📦 3. Boxplot & Violinplot — Perbandingan Fitur Berdasarkan Kualitas
Boxplot dan violinplot digunakan untuk membandingkan distribusi variabel kimia (seperti volatile acidity dan alcohol) terhadap level quality wine.

Python
plt.figure(figsize=(12, 6))
sns.boxplot(data=df, x='quality', y='alcohol', palette='Set2')
plt.title('Distribusi Alkohol Berdasarkan Skor Kualitas Wine')
plt.show()
Insight
Wine dengan skor kualitas yang lebih tinggi (quality 7 dan 8) cenderung memiliki median kadar alkohol yang lebih tinggi dibandingkan kualitas rendah (quality 5).

Sebaliknya, kadar volatile acidity (keasaman menguap) cenderung lebih rendah pada wine dengan kualitas tinggi.

🌡️ 4. Correlation Heatmap — Hubungan Antar Fitur Kimia
Heatmap digunakan untuk memvisualisasikan matriks korelasi Pearson antar seluruh fitur numerik dalam dataset.

Python
plt.figure(figsize=(10, 8))
corr = df.corr()
sns.heatmap(corr, annot=True, cmap='coolwarm', fmt='.2f', linewidths=0.5)
plt.title('Matriks Korelasi Fitur Kimiawi Red Wine')
plt.show()
Insight
Terdapat korelasi positif yang kuat antara fixed acidity dan density serta citric acid.

pH dan fixed acidity memiliki korelasi negatif yang kuat (secara kimiawi, asam yang tinggi menurunkan pH).

alcohol menunjukkan korelasi positif paling signifikan terhadap skor quality.

👥 5. Pairplot — Eksplorasi Hubungan Multivariate
Pairplot digunakan untuk melihat hubungan sebar pasangan variabel kunci secara simultan.

Python
sns.pairplot(df[['alcohol', 'sulphates', 'volatile acidity', 'quality']], hue='quality', palette='viridis')
plt.show()
Insight
Membantu melihat klaster atau pemisahan antar kelas kualitas berdasarkan kombinasi dua fitur kimia secara cepat.

📊 6. Barplot dengan Estimator — Rata-Rata Sulphates per Kualitas
Barplot digunakan untuk menampilkan estimasi nilai rata-rata (estimator=np.mean) dari fitur kimiawi dikelompokkan berdasarkan tingkat kualitas, lengkap dengan interval kepercayaan (error bar).

Python
plt.figure(figsize=(10, 5))
sns.barplot(data=df, x='quality', y='sulphates', estimator=np.mean, ci='sd', palette='Blues_d')
plt.title('Rata-Rata Kadar Sulphates pada Setiap Tingkat Kualitas Wine')
plt.show()
Insight
Wine dengan kualitas yang lebih tinggi menunjukkan rata-rata kadar sulphates yang cenderung meningkat.

🎨 Statistical Visualization Best Practices
Beberapa prinsip penting dalam menggunakan Seaborn:

1. Memilih Jenis Grafik yang Tepat
Gunakan Histplot/KDE untuk distribusi satu variabel.

Gunakan Boxplot/Violinplot untuk perbandingan kategori vs numerik.

Gunakan Heatmap untuk korelasi banyak fitur sekaligus.

2. Memanfaatkan Fitur hue
Menambahkan dimensi ketiga secara visual tanpa membuat plot baru yang rumit.

💡 Key Insights
Beberapa insight utama dari analisis statistik red wine:

🍷 Kadar alkohol memiliki korelasi positif yang paling nyata dengan peningkatan skor kualitas wine.

📉 Volatile acidity (keasaman menguap) yang tinggi cenderung menurunkan kualitas rasa wine secara signifikan.

⚖️ Fitur-fitur kimia seperti pH, density, dan fixed acidity saling berkaitan erat secara kimiawi.

📊 Grafik statistik Seaborn memudahkan identifikasi pola, sebaran, dan anomali data jauh lebih cepat dibandingkan statistik deskriptif mentah.

📚 Learning Outcomes
Melalui project ini, konsep yang dipraktikkan meliputi:

Seaborn library introduction

Univariate distribution (histplot, kdeplot)

Group comparison (boxplot, violinplot)

Correlation analysis (heatmap)

Multivariate exploration (pairplot)

Aggregation plot (barplot with estimator)

Statistical data visualization best practices

🚀 Next Step
Visualisasi statistik merupakan fondasi penting sebelum membangun model Machine Learning.

Plaintext
Data Loading
 ↓
Data Cleaning
 ↓
Statistical Visualization (Seaborn)
 ↓
Feature Engineering
 ↓
Machine Learning Modeling
Pemahaman ini menjadi jembatan krusial menuju tahap pemodelan prediktif selanjutnya.

📁 Project Structure
Plaintext
Day_12_Statistical_Visualization_Seaborn/
│
├── Day_12_Statistical_Visualization_Seaborn.ipynb
└── README.md
👤 Author
Arief Wicaksono

Aspiring AI Engineer | Data Science Student

⭐ If you find this project useful, feel free to star the repository!




Gemini adalah AI dan dapat membuat kesalahan.

# 📊 Day 12 — Visualisasi Statistik dengan Seaborn

Notebook ini membahas dasar-dasar **visualisasi statistik menggunakan Seaborn** di atas Matplotlib dengan menggunakan dataset **Red Wine Quality** (UCI Machine Learning).

Fokus utama project adalah memahami bagaimana membuat grafik statistik tingkat tinggi yang langsung menampilkan **distribusi, hubungan antar variabel, perbandingan antar grup, serta matriks korelasi**.

Visualisasi yang dibuat meliputi **histplot & kdeplot, boxplot & violinplot, correlation heatmap, pairplot, serta barplot dengan estimator**.

---

## 🎯 Project Objectives

Tujuan pembelajaran dalam project ini adalah:

* Memahami dasar visualisasi statistik tingkat tinggi menggunakan Seaborn.
* Membuat `histplot` & `kdeplot` untuk melihat **distribusi** variabel numerik.
* Membuat `boxplot` & `violinplot` untuk membandingkan **distribusi antar grup** kualitas anggur.
* Membuat **heatmap korelasi** untuk melihat hubungan linier antar fitur kimiawi.
* Membuat `pairplot` untuk mengeksplorasi banyak hubungan variabel sekaligus.
* Membuat `barplot` dengan **estimator** (rata-rata) lengkap dengan interval kepercayaan.
* Menghasilkan insight statistik dan domain knowledge dari data kimiawi red wine.

---

## 📂 Dataset

Dataset yang digunakan adalah:

**Red Wine Quality**

📌 Source: [Kaggle — Red Wine Quality Dataset](https://www.kaggle.com/datasets/uciml/red-wine-quality-cortez-et-al-2009)

Dataset ini berisi hasil uji laboratorium sampel anggur merah Portugal (Vinho Verde), mencakup berbagai sifat fisikokimia dan skor kualitas sensorik.

### Dataset Overview

* **1.599 baris**
* **12 kolom**
* Tidak terdapat missing value pada dataset.

### Important Features

| Feature               | Description                                           |
| --------------------- | ----------------------------------------------------- |
| `fixed acidity`       | Keasaman tetap (asam tartarat)                        |
| `volatile acidity`    | Keasaman menguap (asam asetat — bila tinggi terasa cuka) |
| `citric acid`         | Asam sitrat (menambah kesegaran)                      |
| `residual sugar`      | Sisa gula dalam wine                                  |
| `chlorides`           | Kadar garam / klorida                                 |
| `free sulfur dioxide` | Sulfur dioksida bebas (pengawet)                      |
| `total sulfur dioxide`| Total sulfur dioksida                                 |
| `density`             | Densitas / massa jenis wine                           |
| `pH`                  | Tingkat keasaman (pH)                                 |
| `sulphates`           | Kadar sulfat (senyawa sulfur)                         |
| `alcohol`             | Kadar alkohol (% volume)                              |
| `quality`             | Skor kualitas (skala 0–10, penilaian ahli)            |

---

## 🛠️ Tech Stack

Project ini menggunakan:

* **Python**
* **Pandas** — Data manipulation & aggregation
* **NumPy** — Numerical computation
* **Matplotlib** — Dasar kanvas & kustomisasi plot
* **Seaborn** — Visualisasi statistik tingkat tinggi

---

# 🔎 Analysis Workflow

```text
Dataset
   │
   ▼
Data Loading & Inspection
   │
   ▼
Data Verification (Missing Value & Outliers)
   │
   ▼
Statistical Visualization
   │
   ├── Histplot & KDEPlot (Distribusi Variabel Tunggal)
   ├── Boxplot & ViolinPlot (Perbandingan Grup Kualitas)
   ├── Correlation Heatmap (Hubungan Antar Fitur)
   ├── Pairplot (Multivariate Relationship)
   └── Barplot with Estimator (Agregasi & Confidence Interval)
   │
   ▼
Domain & Business Insights
```

---

# 📥 1. Data Loading & Inspection

Dataset dimuat menggunakan Pandas dan diperiksa struktur tipe datanya serta kelengkapannya.

```python
df = pd.read_csv('winequality-red.csv')
df.head()
```

Hasil pemeriksaan menunjukkan seluruh 12 kolom berisi nilai numerik (`float64` dan `int64`) dengan total 1.599 baris dan tidak ada *missing value*.

---

# 📈 2. Histplot & KDEPlot — Distribusi Kadar Alkohol

Histogram digabungkan dengan kurva Estimasi Kepadatan Kernel (`kde=True`) untuk melihat sebaran kadar alkohol (`alcohol`) pada red wine.

```python
plt.figure(figsize=(12, 5))
sns.histplot(data=df, x='alcohol', kde=True)
plt.title('Distribusi Kadar Alkohol Red Wine')
plt.xlabel('Alcohol %')
plt.ylabel('Frequency')
plt.show()
```

### Insight

* Mayoritas sampel wine memiliki kadar alkohol berkisar di angka 9% hingga 10.5%.
* Distribusi bersifat *right-skewed* (cenderung menceng ke kanan), dengan beberapa sampel memiliki kadar alkohol tinggi hingga 14.9%.

---

# 📦 3. Boxplot & Violinplot — Perbandingan Fitur Berdasarkan Kualitas

Boxplot dan violinplot digunakan untuk membandingkan distribusi variabel kimia (seperti `volatile acidity` dan `alcohol`) terhadap level `quality` wine.

```python
plt.figure(figsize=(12, 6))
sns.boxplot(data=df, x='quality', y='alcohol', palette='Set2')
plt.title('Distribusi Alkohol Berdasarkan Skor Kualitas Wine')
plt.show()
```

### Insight

* Wine dengan skor kualitas yang lebih tinggi (`quality` 7 dan 8) cenderung memiliki median kadar alkohol yang **lebih tinggi** dibandingkan kualitas rendah (`quality` 5).
* Sebaliknya, kadar *volatile acidity* (keasaman menguap) cenderung **lebih rendah** pada wine dengan kualitas tinggi.

---

# 🌡️ 4. Correlation Heatmap — Hubungan Antar Fitur Kimia

Heatmap digunakan untuk memvisualisasikan matriks korelasi Pearson antar seluruh fitur numerik dalam dataset.

```python
plt.figure(figsize=(10, 8))
corr = df.corr()
sns.heatmap(corr, annot=True, cmap='coolwarm', fmt='.2f', linewidths=0.5)
plt.title('Matriks Korelasi Fitur Kimiawi Red Wine')
plt.show()
```

### Insight

* Terdapat korelasi positif yang kuat antara `fixed acidity` dan `density` serta `citric acid`.
* `pH` dan `fixed acidity` memiliki korelasi negatif yang kuat (secara kimiawi, asam yang tinggi menurunkan pH).
* `alcohol` menunjukkan korelasi positif paling signifikan terhadap skor `quality`.

---

# 👥 5. Pairplot — Eksplorasi Hubungan Multivariate

Pairplot digunakan untuk melihat hubungan sebar pasangan variabel kunci secara simultan.

```python
sns.pairplot(df[['alcohol', 'sulphates', 'volatile acidity', 'quality']], hue='quality', palette='viridis')
plt.show()
```

### Insight

* Membantu melihat klaster atau pemisahan antar kelas kualitas berdasarkan kombinasi dua fitur kimia secara cepat.

---

# 📊 6. Barplot dengan Estimator — Rata-Rata Sulphates per Kualitas

Barplot digunakan untuk menampilkan estimasi nilai rata-rata (`estimator=np.mean`) dari fitur kimiawi dikelompokkan berdasarkan tingkat kualitas, lengkap dengan interval kepercayaan (*error bar*).

```python
plt.figure(figsize=(10, 5))
sns.barplot(data=df, x='quality', y='sulphates', estimator=np.mean, ci='sd', palette='Blues_d')
plt.title('Rata-Rata Kadar Sulphates pada Setiap Tingkat Kualitas Wine')
plt.show()
```

### Insight

* Wine dengan kualitas yang lebih tinggi menunjukkan rata-rata kadar `sulphates` yang cenderung meningkat.

---

# 🎨 Statistical Visualization Best Practices

Beberapa prinsip penting dalam menggunakan Seaborn:

### 1. Memilih Jenis Grafik yang Tepat
* Gunakan **Histplot/KDE** untuk distribusi satu variabel.
* Gunakan **Boxplot/Violinplot** untuk perbandingan kategori vs numerik.
* Gunakan **Heatmap** untuk korelasi banyak fitur sekaligus.

### 2. Memanfaatkan Fitur `hue`
* Menambahkan dimensi ketiga secara visual tanpa membuat plot baru yang rumit.

---

# 💡 Key Insights

Beberapa insight utama dari analisis statistik red wine:

1. 🍷 Kadar **alkohol** memiliki korelasi positif yang paling nyata dengan peningkatan skor kualitas wine.
2. 📉 **Volatile acidity** (keasaman menguap) yang tinggi cenderung menurunkan kualitas rasa wine secara signifikan.
3. ⚖️ Fitur-fitur kimia seperti `pH`, `density`, dan `fixed acidity` saling berkaitan erat secara kimiawi.
4. 📊 Grafik statistik Seaborn memudahkan identifikasi pola, sebaran, dan anomali data jauh lebih cepat dibandingkan statistik deskriptif mentah.

---

# 📚 Learning Outcomes

Melalui project ini, konsep yang dipraktikkan meliputi:

* Seaborn library introduction
* Univariate distribution (`histplot`, `kdeplot`)
* Group comparison (`boxplot`, `violinplot`)
* Correlation analysis (`heatmap`)
* Multivariate exploration (`pairplot`)
* Aggregation plot (`barplot` with estimator)
* Statistical data visualization best practices

---

# 🚀 Next Step

Visualisasi statistik merupakan fondasi penting sebelum membangun model Machine Learning.

```text
Data Loading
 ↓
Data Cleaning
 ↓
Statistical Visualization (Seaborn)
 ↓
Feature Engineering
 ↓
Machine Learning Modeling
```

Pemahaman ini menjadi jembatan krusial menuju tahap pemodelan prediktif selanjutnya.

---

## 📁 Project Structure

```text
Day_12_Statistical_Visualization_Seaborn/
│
├── Day_12_Statistical_Visualization_Seaborn.ipynb
└── README.md
```

---

## 👤 Author

**Arief Wicaksono**

Aspiring AI Engineer | Data Science Student

---

⭐ If you find this project useful, feel free to star the repository!