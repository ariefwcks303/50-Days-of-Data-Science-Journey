# 📊 Day 16 — Visualisasi Data Kategori dan Perbandingan Grup

Project ini berisi analisis data dan pemodelan visual yang berfokus pada penanganan data kategorikal, seperti jenis kelamin, ras/etnis, dan jenjang pendidikan. Proyek ini mendemonstrasikan cara membandingkan rata-rata dan sebaran performa antar grup menggunakan data nyata hasil ujian siswa.

## 🎯 Project Objectives
Tujuan utama project ini adalah:
- Menganalisis dan memvisualisasikan frekuensi pada data kategorikal.
- Membandingkan rata-rata skor ujian antar berbagai kelompok demografi siswa.
- Melihat distribusi, sebaran, dan *outlier* pada performa siswa menggunakan visualisasi statistik.
- Mencari hubungan persilangan (tabulasi silang) antar variabel kategori.

## 📂 Dataset
Dataset yang digunakan adalah:
**Students Performance in Exams**
- 📌 Source: Kaggle — spscientist/students-performance-in-exams
- Berisi data performa ujian siswa di Amerika Serikat.
- Memiliki 1,000 baris data dan 8 kolom tanpa nilai kosong (bersih).
- Tiap baris mewakili data satu siswa beserta latar belakangnya.

### Important Features
| Kolom | Arti |
|-------|------|
| `gender` | Jenis kelamin siswa |
| `race/ethnicity` | Kelompok ras/etnis (Group A - E) |
| `parental level of education` | Tingkat pendidikan tertinggi orang tua |
| `lunch` | Tipe makan siang (standar atau gratis/reduksi) |
| `test preparation course` | Status penyelesaian kursus persiapan ujian |
| `math score` | Nilai ujian matematika |
| `reading score` | Nilai ujian membaca |
| `writing score` | Nilai ujian menulis |

Metrik baru yang dibuat:
- **Avg_Score** = Rata-rata dari nilai matematika, membaca, dan menulis.

## 🛠️ Tech Stack
- **Python**
- **Pandas** — Data manipulation
- **NumPy** — Numerical computation
- **Matplotlib** — Data visualization
- **Seaborn** — Statistical visualization & heatmap

## 🔎 Analysis Workflow
```text
Students Performance Dataset
        │
        ▼
   Data Exploration
        │
        ▼
 Feature Engineering
        │
        └── Create 'avg_score' metric
        │
        ▼
 Categorical Analysis
        │
        ├── Countplot (Distribusi Kategori)
        ├── Grouped Barplot (Komparasi Grup)
        └── Boxplot (Sebaran & Outliers)
        │
        ▼
 Cross-Tabulation & Heatmap
```

## 🧹 1. Data Preparation & Feature Engineering
Dataset ini sudah dalam keadaan bersih tanpa nilai yang hilang (*missing values*). 
**Create Average Score**
Metrik `avg_score` dibuat untuk merangkum performa siswa secara keseluruhan agar lebih mudah dianalisis secara agregat, alih-alih melihat skor masing-masing mata pelajaran secara terpisah.

## 📈 2. Komponen Visualisasi Utama
Berbagai teknik visualisasi diterapkan untuk menggali informasi dari fitur kategori:
1. **Countplot (Frekuensi Kategori)**: Menunjukkan demografi umum, seperti mayoritas siswa mengonsumsi makan siang tipe standar dan tidak mengikuti kursus persiapan ujian.
2. **Grouped Barplot (Rata-rata Antar Grup)**: Membandingkan performa siswa. Visualisasi ini menunjukkan dengan jelas bahwa siswa yang menyelesaikan kursus persiapan ujian memiliki skor rata-rata yang lebih tinggi di semua mata pelajaran.
3. **Boxplot (Sebaran Skor)**: Mengungkapkan tren kenaikan skor matematika yang selaras dengan jenjang pendidikan orang tua. Boxplot juga memperlihatkan bahwa skor membaca siswa perempuan memiliki median yang lebih tinggi dibandingkan siswa laki-laki.
4. **Heatmap Crosstab (Hubungan 2 Kategori)**: Memvisualisasikan tabel kontingensi (tabulasi silang) untuk melihat korelasi frekuensi antara tipe makan siang dan partisipasi dalam kursus persiapan.

## 💡 Key Insights & Business Recommendations
1. **Dampak Signifikan Kursus Persiapan**: Data menunjukkan bahwa kursus persiapan berbanding lurus dengan peningkatan skor. Pihak sekolah dapat mempertimbangkan untuk mewajibkan atau memberikan subsidi pada kursus ini agar lebih banyak siswa yang ikut.
2. **Pengaruh Latar Belakang Keluarga**: Ada indikasi kuat bahwa tingkat pendidikan orang tua memengaruhi performa akademik anak (terlihat jelas di skor matematika). Program bimbingan khusus mungkin diperlukan untuk siswa dari keluarga dengan latar belakang pendidikan yang lebih rendah.
3. **Perbedaan Gender pada Kemampuan Literasi**: Siswa perempuan secara umum mengungguli laki-laki dalam membaca dan menulis. Pendekatan pengajaran literasi yang berbeda mungkin diperlukan untuk meningkatkan minat dan kemampuan siswa laki-laki.

## 📚 Learning Outcomes
Melalui project ini, konsep-konsep berikut telah dipraktikkan:
- Menangani dan mengeksplorasi data kategorikal menggunakan Pandas.
- Membuat **Countplot** dan **Grouped Barplot** dengan Seaborn untuk komparasi grup.
- Menginterpretasikan **Boxplot** untuk memahami distribusi statistik, median, *quartile*, dan *outliers*.
- Menggunakan `pd.crosstab()` yang digabungkan dengan **Heatmap** untuk menganalisis relasi antar dua variabel non-numerik.

## 📁 Project Structure
```text
Day_16_visualisai_data_kategorikal/
│
├── Day_16_visualisai_data_kategorikal.ipynb
└── README.md
```

👤 **Author**

Arief Wicaksono

Aspiring AI Engineer | Data Science Student

⭐ *If you find this project useful, feel free to star the repository!*