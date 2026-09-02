# 📊 Analisis Performa Siswa

Notebook ini berisi Exploratory Data Analysis (EDA) terhadap dataset **Students Performance in Exams** untuk memahami faktor-faktor yang berkaitan dengan performa akademik siswa.

Analisis dilakukan terhadap **1.000 data siswa** dengan berbagai atribut demografis dan latar belakang, seperti gender, pendidikan orang tua, jenis makan siang, serta partisipasi dalam kursus persiapan ujian.

---

## 🎯 Tujuan Analisis

Tujuan dari project ini adalah untuk:

* Memahami struktur dan karakteristik dataset.
* Melakukan eksplorasi awal terhadap data siswa.
* Membandingkan performa akademik berdasarkan beberapa kelompok.
* Menganalisis hubungan antara nilai matematika, membaca, dan menulis.
* Mengidentifikasi pengaruh **test preparation course** terhadap performa siswa.
* Menganalisis hubungan antara jenis **lunch** dan performa akademik.
* Menggunakan **crosstab** untuk melihat distribusi antar variabel kategorikal.
* Melakukan **independent t-test** untuk menguji signifikansi perbedaan performa siswa.

---

## 📂 Dataset

Dataset yang digunakan adalah:

**Students Performance in Exams**

📌 Sumber:
[Kaggle — Students Performance in Exams](https://www.kaggle.com/datasets/spscientist/students-performance-in-exams)

### Ukuran Dataset

* **1.000 baris**
* **8 kolom**

### Deskripsi Kolom

| Kolom                         | Deskripsi                               |
| ----------------------------- | --------------------------------------- |
| `gender`                      | Jenis kelamin siswa                     |
| `race/ethnicity`              | Kelompok etnis siswa                    |
| `parental level of education` | Tingkat pendidikan orang tua            |
| `lunch`                       | Jenis makan siang yang diterima siswa   |
| `test preparation course`     | Status mengikuti kursus persiapan ujian |
| `math score`                  | Nilai matematika                        |
| `reading score`               | Nilai membaca                           |
| `writing score`               | Nilai menulis                           |

Dalam proses analisis, ditambahkan kolom baru:

* `average_score` → rata-rata dari nilai matematika, membaca, dan menulis.

---

## 🛠️ Tech Stack

Project ini menggunakan Python dengan beberapa library berikut:

* **Pandas** → Data manipulation dan analysis
* **NumPy** → Numerical computation
* **Matplotlib** → Data visualization
* **Seaborn** → Statistical data visualization
* **SciPy** → Statistical testing

---

## 🔍 Tahapan Analisis

### 1. Import Library

Library yang digunakan di-import untuk mendukung proses:

* Data manipulation
* Exploratory Data Analysis
* Statistical analysis
* Data visualization

---

### 2. Data Understanding

Pada tahap awal dilakukan eksplorasi terhadap dataset menggunakan:

* `head()`
* `info()`
* `shape`
* `describe()`
* Missing value checking

### Hasil Awal

Dataset terdiri dari:

* 1.000 observasi siswa
* 8 kolom
* 3 fitur numerik berupa nilai ujian
* 5 fitur kategorikal
* Tidak ditemukan missing values

---

### 3. Data Cleaning & Feature Engineering

Dataset relatif sudah bersih, namun dilakukan penyesuaian nama kolom agar lebih mudah digunakan dalam proses analisis.

Selain itu, dibuat fitur baru:

```python
average_score
```

Fitur ini merupakan rata-rata dari:

* Math Score
* Reading Score
* Writing Score

Kolom ini digunakan untuk memberikan gambaran umum mengenai performa akademik setiap siswa.

---

## 📈 Exploratory Data Analysis

### A. Distribusi Rata-Rata Nilai

Visualisasi histogram digunakan untuk melihat distribusi `average_score`.

**Insight:**

* Distribusi nilai mendekati distribusi normal.
* Mayoritas siswa memiliki rata-rata nilai sekitar **68–70**.
* Relatif sedikit siswa yang memiliki nilai sangat rendah maupun sangat tinggi.

---

### B. Perbandingan Nilai Matematika Berdasarkan Gender

Boxplot digunakan untuk membandingkan distribusi nilai matematika berdasarkan gender.

Analisis ini membantu melihat:

* Median nilai
* Rentang distribusi
* Variasi nilai
* Perbedaan performa antar kelompok

---

### C. Pengaruh Test Preparation Course

Bar chart digunakan untuk membandingkan rata-rata performa antara:

* Siswa yang mengikuti kursus persiapan ujian
* Siswa yang tidak mengikuti kursus persiapan

### Insight

Siswa yang mengikuti **test preparation course** memiliki rata-rata nilai yang lebih tinggi dibandingkan siswa yang tidak mengikuti kursus.

Perbedaan rata-rata performa sekitar:

> **7,63 poin**

Hal ini kemudian diuji lebih lanjut menggunakan analisis statistik.

---

### D. Pengaruh Jenis Lunch

Analisis dilakukan untuk membandingkan performa siswa berdasarkan jenis makan siang.

Jenis lunch yang dianalisis:

* Standard
* Free/Reduced

### Insight

Siswa dengan kategori **standard lunch** menunjukkan rata-rata nilai yang lebih tinggi secara konsisten pada:

* Matematika
* Membaca
* Menulis

Jenis lunch dalam dataset dapat digunakan sebagai salah satu indikator kondisi sosial atau ekonomi, meskipun hubungan tersebut tidak dapat langsung diinterpretasikan sebagai hubungan sebab-akibat.

---

### E. Korelasi Antar Mata Pelajaran

Heatmap digunakan untuk melihat hubungan antara:

* Math Score
* Reading Score
* Writing Score

### Insight

Terdapat korelasi yang kuat, terutama antara:

> **Reading Score dan Writing Score**

Hal ini menunjukkan bahwa performa membaca dan menulis memiliki hubungan yang cukup erat dalam dataset.

---

### F. Crosstab Analysis

Crosstab digunakan untuk menganalisis hubungan antara:

```text
lunch × test preparation course
```

Analisis ini membantu melihat distribusi jumlah siswa berdasarkan kombinasi kedua kategori tersebut.

---

## 🧪 Statistical Testing

### Independent T-Test

Setelah visualisasi menunjukkan bahwa siswa yang mengikuti kursus persiapan memiliki rata-rata nilai lebih tinggi, dilakukan **independent t-test** untuk mengetahui apakah perbedaan tersebut signifikan secara statistik.

Hipotesis yang digunakan:

### H₀ — Null Hypothesis

Tidak terdapat perbedaan rata-rata performa yang signifikan antara siswa yang mengikuti kursus persiapan dan yang tidak.

### H₁ — Alternative Hypothesis

Terdapat perbedaan rata-rata performa yang signifikan antara siswa yang mengikuti kursus persiapan dan yang tidak.

### Aturan Keputusan

```text
p-value < 0.05 → Perbedaan signifikan
p-value ≥ 0.05 → Perbedaan tidak signifikan
```

### Hasil Analisis

| Kelompok               | Rata-Rata Skor |
| ---------------------- | -------------: |
| Tidak mengikuti kursus |          65.04 |
| Mengikuti kursus       |          72.67 |
| Selisih                |           7.63 |

Hasil pengujian menunjukkan bahwa perbedaan performa antara kedua kelompok **signifikan secara statistik**.

---

# 💡 Key Insights

Beberapa insight utama dari analisis ini:

1. 📊 Distribusi performa siswa cenderung mendekati distribusi normal dengan rata-rata sekitar **68**.

2. 🎓 Siswa yang mengikuti **test preparation course** memiliki performa rata-rata lebih tinggi.

3. 📈 Perbedaan performa antara siswa yang mengikuti kursus dan yang tidak mengikuti kursus terbukti signifikan secara statistik.

4. 🍽️ Terdapat perbedaan performa berdasarkan jenis **lunch**, di mana kelompok dengan standard lunch memiliki rata-rata nilai lebih tinggi dalam dataset ini.

5. 📚 Nilai membaca dan menulis memiliki hubungan yang kuat.

---

# 💼 Rekomendasi

Berdasarkan hasil analisis, beberapa rekomendasi yang dapat dipertimbangkan adalah:

### 1. Memperluas Akses Kursus Persiapan

Karena siswa yang mengikuti kursus persiapan menunjukkan performa lebih tinggi, sekolah dapat mempertimbangkan:

* Menyediakan program persiapan ujian tambahan.
* Memberikan akses kursus bagi siswa yang membutuhkan.
* Mengembangkan program belajar tambahan.

---

### 2. Memberikan Dukungan Akademik Tambahan

Kelompok siswa dengan performa lebih rendah dapat diberikan:

* Program mentoring
* Kelas tambahan
* Learning support
* Academic intervention

---

### 3. Melakukan Analisis Lanjutan

Analisis selanjutnya dapat mengeksplorasi:

* Pengaruh pendidikan orang tua terhadap performa siswa.
* Hubungan antar variabel demografis.
* Feature importance menggunakan machine learning.
* Student performance prediction.
* Analisis multivariat untuk memahami faktor-faktor yang paling berkaitan dengan performa.

---

# 📁 Project Structure

```text
.
├── Day_07_Analisa_performa_siswa.ipynb
└── README.md
```

---

# 🚀 How to Run

Clone repository:

```bash
git clone <repository-url>
```

Install dependencies:

```bash
pip install pandas numpy matplotlib seaborn scipy kaggle
```

Kemudian jalankan notebook:

```text
Day_07_Analisa_performa_siswa.ipynb
```

Notebook dapat dijalankan menggunakan:

* Jupyter Notebook
* JupyterLab
* Google Colab
* VS Code

---

# 📚 Learning Outcomes

Melalui project ini, konsep yang dipraktikkan meliputi:

* Data loading
* Data inspection
* Data cleaning
* Feature engineering
* Exploratory Data Analysis
* Data visualization
* Group comparison
* Correlation analysis
* Crosstab analysis
* Statistical hypothesis testing
* Independent t-test
* Data-driven insight generation

---

## 👤 Author

**Arief Wicaksono**

Aspiring AI Engineer | Data Science Student

---

⭐ Jika repository ini bermanfaat, jangan lupa untuk memberikan **Star**!
