```python
complete_readme = """# 08 — Eksplorasi Kualitas Anggur Merah (Red Wine Quality Exploration)

Repositori ini berisi dokumentasi lengkap dan kode untuk melakukan analisis eksploratif (*Exploratory Data Analysis* / EDA) mendalam terhadap dataset **Red Wine Quality** (Cortez et al., 2009). Analisis ini meneliti hubungan antara 11 properti fisiko-kimia dengan skor kualitas sensorik dari anggur merah Portugis ("Vinho Verde").

---

## 📋 Daftar Isi
1. [Tentang Dataset](#-tentang-dataset)
2. [Struktur & Deskripsi Fitur](#-struktur--deskripsi-fitur)
3. [Tech Stack & Prasyarat Library](#-tech-stack--prasyarat-library)
4. [Langkah-Langkah Analisis (Workflow)](#-langkah-langkah-analisis-workflow)
5. [Ringkasan Temuan & Wawasan (*Insights*)](#-ringkasan-temuan--wawasan-insights)
6. [Cara Menjalankan Notebook](#-cara-menjalankan-notebook)
7. [Referensi](#-referensi)

---

## 📊 Tentang Dataset
* **Sumber Data:** [Kaggle — Red Wine Quality Dataset](https://www.kaggle.com/datasets/uciml/red-wine-quality-cortez-et-al-2009) / UCI Machine Learning Repository.
* **Jumlah Sampel:** 1.599 baris data observasi anggur merah.
* **Jumlah Fitur:** 12 kolom (11 fitur prediktor fisikokimia dan 1 variabel target sensorik).
* **Kualitas Data:** Dataset bersih dengan tipe data numerik (`float64` dan `int64`) serta bebas dari *missing values*.

---

## 🗂️ Struktur & Deskripsi Fitur

Berikut adalah rincian dari seluruh variabel yang dianalisis dalam dataset:

| No. | Kolom | Tipe Data | Deskripsi / Penjelasan Fisiologis-Kimia |
|:---|:---|:---|:---|
| 1 | `fixed acidity` | Float | Sebagian besar asam yang terkandung dalam anggur bersifat tetap atau tidak mudah menguap (sebagian besar asam tartarat). |
| 2 | `volatile acidity` | Float | Kadar asam asetat dalam anggur. Kadar yang terlalu tinggi dapat menimbulkan rasa dan aroma seperti cuka (*vinegar-like*). |
| 3 | `citric acid` | Float | Ditemukan dalam jumlah kecil, asam sitrat dapat menambahkan rasa segar (*freshness*) dan kekhasan pada rasa anggur. |
| 4 | `residual sugar` | Float | Jumlah sisa gula setelah proses fermentasi selesai. Anggur kering (*dry wines*) biasanya memiliki kadar gula sisa yang sangat rendah. |
| 5 | `chlorides` | Float | Jumlah kandungan garam atau klorida di dalam anggur. |
| 6 | `free sulfur dioxide` | Float | Bentuk sulfur dioksida bebas ($SO_2$) yang berperan mencegah pertumbuhan mikroorganisme dan oksidasi pada anggur. |
| 7 | `total sulfur dioxide` | Float | Jumlah total bentuk sulfur dioksida bebas dan terikat; konsentrasi tinggi dapat tercium bau menyengat. |
| 8 | `density` | Float | Massa jenis anggur, yang sangat bergantung pada persentase kandungan alkohol dan kadar gula. |
| 9 | `pH` | Float | Menunjukkan tingkat keasaman atau kebasaan skala 0 (sangat asam) hingga 14 (sangat basa); sebagian besar anggur berada pada kisaran pH 3–4. |
| 10 | `sulphates` | Float | Aditif anggur yang dapat berkontribusi pada tingkat gas sulfur dioksida ($SO_2$), bertindak sebagai antimikroba dan antioksidan. |
| 11 | `alcohol` | Float | Persentase kadar alkohol (%) yang terkandung di dalam produk akhir anggur. |
| 12 | `quality` | Integer | Skor target kualitas penilaian subjektif dari panel pakar/sensorik dengan rentang skala nilai 0 hingga 10. |

---

## 🛠️ Tech Stack & Prasyarat Library
Proyek ini dikembangkan menggunakan bahasa pemrograman Python serta memanfaatkan pustaka analisis data standar industri:
* **Python 3.x**
* **Pandas**: Manipulasi, pembersihan, dan pengelompokan struktur data tabular.
* **NumPy**: Operasi matematis dan array berkinerja tinggi.
* **Matplotlib**: Pembuatan visualisasi dasar grafik statistik dua dimensi.
* **Seaborn**: Pustaka visualisasi tingkat tinggi untuk grafik statistik estetis (seperti *heatmap*, *boxplot*, dan *distribution plot*).

---

## 🔍 Langkah-Langkah Analisis (Workflow)
Dalam notebook ini, proses EDA dilakukan melalui beberapa tahapan sistematis:
1. **Inspeksi Awal Data**: Memuat dataset, melihat lima baris pertama (`head()`), memeriksa dimensi data, tipe data, serta mengecek ada atau tidaknya nilai kosong (*missing values*).
2. **Statistik Deskriptif**: Menghitung parameter ringkasan statistik (rata-rata, nilai tengah/median, deviasi standar, nilai minimum, maksimum, serta kuartil) untuk setiap kolom fitur kimia.
3. **Analisis Distribusi Target (`quality`)**: Memeriksa sebaran skor kualitas anggur menggunakan *countplot* untuk melihat ketimpangan kelas (*class imbalance*).
4. **Analisis Univariat & Bivariat**: 
   * Menggunakan *boxplot* untuk mendeteksi *outlier* (pencilan) pada masing-masing parameter kimia.
   * Membuat *histogram* dan *KDE plot* guna melihat bentuk distribusi data (normal atau *skewed*).
5. **Analisis Korelasi Multivariat**: Membangun matriks korelasi Pearson dan memvisualisasikannya ke dalam bentuk **Correlation Heatmap** untuk mengidentifikasi fitur yang memiliki hubungan terkuat terhadap kualitas anggur.

---

## 📈 Ringkasan Temuan & Wawasan (*Insights*)
* **Konsentrasi Kualitas (Target):** Penilaian kualitas para pakar terpusat pada skor menengah, yaitu **5 dan 6**. Sangat sedikit sampel anggur yang mendapatkan skor ekstrem rendah (3 dan 4) maupun skor tinggi (7 dan 8), menunjukkan distribusi normal yang cenderung terkonsentrasi di tengah.
* **Pengaruh Alkohol (`alcohol`):** Kadar alkohol terbukti memiliki korelasi positif terkuat terhadap peningkatan skor kualitas anggur. Sampel dengan kualitas lebih tinggi umumnya memiliki persentase alkohol yang lebih tinggi.
* **Pengaruh Keasaman Menguap (`volatile acidity`):** Sebaliknya, variabel keasaman menguap menunjukkan korelasi negatif yang kuat terhadap kualitas. Semakin tinggi kadar asam asetat (cuka), semakin rendah penilaian kualitas yang diberikan oleh panelis.
* **Peran Sulfat & Asam Sitrat:** Kadar sulfat (`sulphates`) dan asam sitrat (`citric acid`) juga menunjukkan korelasi positif moderat terhadap peningkatan skor kualitas.

---

## 🚀 Cara Menjalankan Notebook
1. Pastikan Anda telah menginstal lingkungan Python serta pustaka yang diperlukan (`pandas`, `numpy`, `matplotlib`, `seaborn`).
2. Unduh dataset Red Wine Quality dari [Kaggle](https://www.kaggle.com/datasets/uciml/red-wine-quality-cortez-et-al-2009) atau pastikan skrip *download* otomatis via Kaggle API aktif.
3. Buka berkas notebook (`.ipynb`) melalui **Jupyter Notebook**, **JupyterLab**, atau unggah ke **Google Colab**.
4. Jalankan setiap sel secara berurutan (*Run All*) untuk mereproduksi seluruh tahapan EDA dan grafik visualisasi.

---

## 📚 Referensi
* Cortez, P., Cerdeira, A., Almeida, F., Matos, T., & Reis, J. (2009). *Modeling wine preferences by data mining from physicochemical properties*. Decision Support Systems, 47(4), 547-553.
"""

with open("README.md", "w", encoding="utf-8") as f:
    f.write(complete_readme)

print("Comprehensive README.md created successfully.")


```

```text
Comprehensive README.md created successfully.


```

File `README.md` telah diperbarui menjadi jauh lebih lengkap, terstruktur, dan mendalam. Berikut adalah versi lengkap dari dokumentasi proyek analisis kualitas anggur merah tersebut:

```markdown
# 08 — Eksplorasi Kualitas Anggur Merah (Red Wine Quality Exploration)

Repositori ini berisi dokumentasi lengkap dan kode untuk melakukan analisis eksploratif (*Exploratory Data Analysis* / EDA) mendalam terhadap dataset **Red Wine Quality** (Cortez et al., 2009). Analisis ini meneliti hubungan antara 11 properti fisiko-kimia dengan skor kualitas sensorik dari anggur merah Portugis ("Vinho Verde").

---

## 📋 Daftar Isi
1. [Tentang Dataset](#-tentang-dataset)
2. [Struktur & Deskripsi Fitur](#-struktur--deskripsi-fitur)
3. [Tech Stack & Prasyarat Library](#-tech-stack--prasyarat-library)
4. [Langkah-Langkah Analisis (Workflow)](#-langkah-langkah-analisis-workflow)
5. [Ringkasan Temuan & Wawasan (*Insights*)](#-ringkasan-temuan--wawasan-insights)
6. [Cara Menjalankan Notebook](#-cara-menjalankan-notebook)
7. [Referensi](#-referensi)

---

## 📊 Tentang Dataset
* **Sumber Data:** [Kaggle — Red Wine Quality Dataset](https://www.kaggle.com/datasets/uciml/red-wine-quality-cortez-et-al-2009) / UCI Machine Learning Repository.
* **Jumlah Sampel:** 1.599 baris data observasi anggur merah.
* **Jumlah Fitur:** 12 kolom (11 fitur prediktor fisikokimia dan 1 variabel target sensorik).
* **Kualitas Data:** Dataset bersih dengan tipe data numerik (`float64` dan `int64`) serta bebas dari *missing values*.

---

## 🗂️ Struktur & Deskripsi Fitur

Berikut adalah rincian dari seluruh variabel yang dianalisis dalam dataset:

| No. | Kolom | Tipe Data | Deskripsi / Penjelasan Fisiologis-Kimia |
|:---|:---|:---|:---|
| 1 | `fixed acidity` | Float | Sebagian besar asam yang terkandung dalam anggur bersifat tetap atau tidak mudah menguap (sebagian besar asam tartarat). |
| 2 | `volatile acidity` | Float | Kadar asam asetat dalam anggur. Kadar yang terlalu tinggi dapat menimbulkan rasa dan aroma seperti cuka (*vinegar-like*). |
| 3 | `citric acid` | Float | Ditemukan dalam jumlah kecil, asam sitrat dapat menambahkan rasa segar (*freshness*) dan kekhasan pada rasa anggur. |
| 4 | `residual sugar` | Float | Jumlah sisa gula setelah proses fermentasi selesai. Anggur kering (*dry wines*) biasanya memiliki kadar gula sisa yang sangat rendah. |
| 5 | `chlorides` | Float | Jumlah kandungan garam atau klorida di dalam anggur. |
| 6 | `free sulfur dioxide` | Float | Bentuk sulfur dioksida bebas ($SO_2$) yang berperan mencegah pertumbuhan mikroorganisme dan oksidasi pada anggur. |
| 7 | `total sulfur dioxide` | Float | Jumlah total bentuk sulfur dioksida bebas dan terikat; konsentrasi tinggi dapat tercium bau menyengat. |
| 8 | `density` | Float | Massa jenis anggur, yang sangat bergantung pada persentase kandungan alkohol dan kadar gula. |
| 9 | `pH` | Float | Menunjukkan tingkat keasaman atau kebasaan skala 0 (sangat asam) hingga 14 (sangat basa); sebagian besar anggur berada pada kisaran pH 3–4. |
| 10 | `sulphates` | Float | Aditif anggur yang dapat berkontribusi pada tingkat gas sulfur dioksida ($SO_2$), bertindak sebagai antimikroba dan antioksidan. |
| 11 | `alcohol` | Float | Persentase kadar alkohol (%) yang terkandung di dalam produk akhir anggur. |
| 12 | `quality` | Integer | Skor target kualitas penilaian subjektif dari panel pakar/sensorik dengan rentang skala nilai 0 hingga 10. |

---

## 🛠️ Tech Stack & Prasyarat Library
Proyek ini dikembangkan menggunakan bahasa pemrograman Python serta memanfaatkan pustaka analisis data standar industri:
* **Python 3.x**
* **Pandas**: Manipulasi, pembersihan, dan pengelompokan struktur data tabular.
* **NumPy**: Operasi matematis dan array berkinerja tinggi.
* **Matplotlib**: Pembuatan visualisasi dasar grafik statistik dua dimensi.
* **Seaborn**: Pustaka visualisasi tingkat tinggi untuk grafik statistik estetis (seperti *heatmap*, *boxplot*, dan *distribution plot*).

---

## 🔍 Langkah-Langkah Analisis (Workflow)
Dalam notebook ini, proses EDA dilakukan melalui beberapa tahapan sistematis:
1. **Inspeksi Awal Data**: Memuat dataset, melihat lima baris pertama (`head()`), memeriksa dimensi data, tipe data, serta mengecek ada atau tidaknya nilai kosong (*missing values*).
2. **Statistik Deskriptif**: Menghitung parameter ringkasan statistik (rata-rata, nilai tengah/median, deviasi standar, nilai minimum, maksimum, serta kuartil) untuk setiap kolom fitur kimia.
3. **Analisis Distribusi Target (`quality`)**: Memeriksa sebaran skor kualitas anggur menggunakan *countplot* untuk melihat ketimpangan kelas (*class imbalance*).
4. **Analisis Univariat & Bivariat**: 
   * Menggunakan *boxplot* untuk mendeteksi *outlier* (pencilan) pada masing-masing parameter kimia.
   * Membuat *histogram* dan *KDE plot* guna melihat bentuk distribusi data (normal atau *skewed*).
5. **Analisis Korelasi Multivariat**: Membangun matriks korelasi Pearson dan memvisualisasikannya ke dalam bentuk **Correlation Heatmap** untuk mengidentifikasi fitur yang memiliki hubungan terkuat terhadap kualitas anggur.

---

## 📈 Ringkasan Temuan & Wawasan (*Insights*)
* **Konsentrasi Kualitas (Target):** Penilaian kualitas para pakar terpusat pada skor menengah, yaitu **5 dan 6**. Sangat sedikit sampel anggur yang mendapatkan skor ekstrem rendah (3 dan 4) maupun skor tinggi (7 dan 8), menunjukkan distribusi normal yang cenderung terkonsentrasi di tengah.
* **Pengaruh Alkohol (`alcohol`):** Kadar alkohol terbukti memiliki korelasi positif terkuat terhadap peningkatan skor kualitas anggur. Sampel dengan kualitas lebih tinggi umumnya memiliki persentase alkohol yang lebih tinggi.
* **Pengaruh Keasaman Menguap (`volatile acidity`):** Sebaliknya, variabel keasaman menguap menunjukkan korelasi negatif yang kuat terhadap kualitas. Semakin tinggi kadar asam asetat (cuka), semakin rendah penilaian kualitas yang diberikan oleh panelis.
* **Peran Sulfat & Asam Sitrat:** Kadar sulfat (`sulphates`) dan asam sitrat (`citric acid`) juga menunjukkan korelasi positif moderat terhadap peningkatan skor kualitas.

---

## 🚀 Cara Menjalankan Notebook
1. Pastikan Anda telah menginstal lingkungan Python serta pustaka yang diperlukan (`pandas`, `numpy`, `matplotlib`, `seaborn`).
2. Unduh dataset Red Wine Quality dari [Kaggle](https://www.kaggle.com/datasets/uciml/red-wine-quality-cortez-et-al-2009) atau pastikan skrip *download* otomatis via Kaggle API aktif.
3. Buka berkas notebook (`.ipynb`) melalui **Jupyter Notebook**, **JupyterLab**, atau unggah ke **Google Colab**.
4. Jalankan setiap sel secara berurutan (*Run All*) untuk mereproduksi seluruh tahapan EDA dan grafik visualisasi.

---

## 📚 Referensi
* Cortez, P., Cerdeira, A., Almeida, F., Matos, T., & Reis, J. (2009). *Modeling wine preferences by data mining from physicochemical properties*. Decision Support Systems, 47(4), 547-553.

```