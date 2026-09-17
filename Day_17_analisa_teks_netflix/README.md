# 🎬 Day 17 — Text Analysis: Netflix Genre & Description

Project ini merupakan implementasi **dasar analisis teks (text mining)** untuk mengeksplorasi katalog tayangan pada dataset **Netflix Movies and TV Shows**.

Berbeda dengan NLP tingkat lanjut, project ini murni menggunakan library standar bawaan Python tanpa alat berat, guna memahami fundamental pemrosesan teks dari awal.

Project ini digunakan untuk memahami bagaimana mengekstrak informasi, menghitung frekuensi kata, dan menemukan pola tema dari teks mentah.

---

## 🎯 Project Objectives

Tujuan utama project ini adalah:

* Memahami konsep **Tokenisasi** menggunakan Regex.
* Mengenal fungsi **Stopwords** untuk menyaring kata umum.
* Menggunakan `collections.Counter` untuk menghitung frekuensi teks.
* Menganalisis distribusi genre populer dari data string.
* Memvisualisasikan sebaran panjang karakter pada judul dan deskripsi.

---

## 📂 Dataset

Dataset yang digunakan adalah:

**Netflix Movies and TV Shows**

📌 Source: [Kaggle — Netflix Shows](https://www.kaggle.com/datasets/shivamb/netflix-shows)

Dataset terdiri dari:

* **~8.800 baris data**
* **12 kolom**

### Important Features

| Feature         | Description                               |
| --------------- | ----------------------------------------- |
| `type`          | Kategori tayangan (Movie / TV Show)       |
| `title`         | Judul tayangan                            |
| `listed_in`     | Genre/kategori (dipisahkan koma)          |
| `description`   | Sinopsis singkat tayangan                 |
| `release_year`  | Tahun rilis tayangan                      |
| `rating`        | Rating usia penonton (TV-MA, PG-13, dll)  |

---

## 🛠️ Tech Stack

* **Python**
* **Pandas** — Data manipulation & analysis
* **Re (Regex)** — String extraction & tokenization
* **Collections** — Counting word frequency
* **Matplotlib** — Static data visualization

---

## 🔎 Analysis Workflow

```text
Netflix Catalog Dataset
          │
          ▼
     Data Loading
          │
          ▼
   Data Exploration
          │
          ├── Check Missing Values
          ├── Drop Nulls (Description/Genre)
          └── Remove Duplicates
          │
          ▼
    Text Processing
          │
          ├── Proporsi Movie vs TV Show
          ├── Split String (Genre)
          ├── Regex Tokenization (Description)
          └── Remove Stopwords
          │
          ▼
   Insights & Findings
```

---

# 📈 1. Proportion & Popular Genres

Eksplorasi pertama melihat dominasi tipe konten dan genre apa yang paling banyak diproduksi oleh Netflix menggunakan **Horizontal Bar Chart**.

### Insight

Katalog didominasi oleh kategori **Movie**. Genre tayangan memiliki cakupan global dengan urutan teratas diduduki oleh *International Movies*, *Dramas*, dan *Comedies*.

---

# 📝 2. Word Frequency — Description Analysis

Analisis menggunakan teknik NLP manual untuk menggabungkan seluruh teks deskripsi, mengambil kata dasar (huruf kecil), membuang *stopwords*, dan menghitung 20 kata terbanyak.

### Insight

Tema yang diangkat Netflix sangat universal. Kata-kata seperti *life*, *family*, *young*, *love*, dan *world* mendominasi sinopsis. Hal ini menunjukkan fokus pada cerita kehidupan, dinamika keluarga, dan percintaan yang *relatable*.

---

# 📊 3. Histogram — Text Length Distribution

Menganalisis karakteristik penulisan judul dan sinopsis dengan mengukur jumlah karakter (pada `title`) dan jumlah kata (pada `description`).

### Insight

Netflix menjaga agar judul tetap *catchy* (rata-rata di bawah 30 karakter) dan deskripsi sangat *to the point* (hanya berkisar 23–24 kata). Format ringkas ini mempermudah pengguna membuat keputusan menonton.

---

# 💡 Key Insights

Beberapa insight utama dari project:

### 1. Content Domination
Netflix lebih berfokus pada penyediaan film layar lebar (*Movies*) dibandingkan serial (*TV Shows*).

### 2. Universal Themes
Sinopsis tayangan selalu berpusat pada unsur humanis (hidup, keluarga, cinta).

### 3. Concise Copywriting
Teks di platform didesain agar sangat ringkas, cepat dibaca, dan memancing rasa penasaran tanpa *spoiler* berlebihan.

---

# ⚠️ Data Pitfalls

### Keakuratan Stopwords
Hasil frekuensi kata sangat sensitif terhadap *stopwords* yang ditentukan secara manual. Jika kurang lengkap, kata sambung yang tidak bermakna akan mendominasi hasil analisis.

### Keterbatasan Frekuensi Mentah
Fungsi `Counter` hanya menghitung kuantitas (frekuensi kemunculan), bukan kualitas (makna). Kata yang sering muncul belum tentu *meaningful* untuk pemodelan di masa depan.

---

# 📚 Learning Outcomes

Melalui project ini, beberapa konsep yang dipraktikkan:

* Basic Text Mining
* String Manipulation
* Stopwords Filtering
* Regex Match & Extract
* Histogram & Bar Chart Visualization
* Insight Generation dari Unstructured Data

---

# 🚀 Next Step

Project ini menjadi fondasi awal analisis teks sebelum masuk ke teknik *Natural Language Processing* (NLP) yang lebih kompleks.

```text
Basic Text Mining (Regex & Counter)
        ↓
Text Weighting (TF-IDF)
        ↓
Sentiment Analysis
        ↓
Topic Modeling
        ↓
Machine Learning NLP
```

---

## 📁 Project Structure

```text
Day_17_Netflix_Text_Analysis/
│
├── Day_17_Netflix_Text_Analysis.ipynb
└── README.md
```

---

## 👤 Author

**Arief Wicaksono**

Aspiring AI Engineer | Data Science Student

---

⭐ If you find this project useful, feel free to star the repository!