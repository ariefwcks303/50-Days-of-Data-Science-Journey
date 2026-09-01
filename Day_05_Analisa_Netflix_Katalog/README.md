# 05 — Menggali Katalog Netflix

Analisis eksploratif terhadap dataset **Netflix Movies and TV Shows** dari Kaggle untuk mengidentifikasi pola distribusi konten, perbandingan film vs serial TV, tren rilis, negara produsen utama, serta analisis genre dan rating usia.

---

## 🎯 Tujuan Pembelajaran
* Melakukan *data cleaning* dan penanganan nilai kosong (*missing values*) pada dataset dunia nyata.
* Memparsing dan mengekstrak informasi waktu/tanggal (*datetime*) dari kolom `date_added`.
* Menerapkan teknik *string manipulation* dan pemisahan data teks (menggunakan fungsi string Pandas dan `Counter`) untuk menganalisis kolom multivariat seperti genre (`listed_in`) dan negara (`country`).
* Membangun visualisasi data yang informatif menggunakan `Matplotlib` dan `Seaborn` untuk menyajikan temuan tren katalog global.

---

## 📊 Tentang Dataset
* **Sumber:** [Kaggle — shivamb/netflix-shows](https://www.kaggle.com/datasets/shivamb/netflix-shows)
* **File:** `netflix_titles.csv` (8.807 baris × 12 kolom)

| Kolom | Deskripsi |
| :--- | :--- |
| `show_id` | Identifikasi unik setiap baris (e.g., s1, s2) |
| `type` | Kategori konten (`Movie` atau `TV Show`) |
| `title` | Judul film atau serial TV |
| `director` | Nama sutradara konten |
| `cast` | Daftar aktor/aktris yang membintangi |
| `country` | Negara asal produksi konten |
| `date_added` | Tanggal konten resmi ditambahkan ke platform Netflix |
| `release_year` | Tahun rilis asli konten tersebut |
| `rating` | Rating usia/kategori penonton (e.g., TV-MA, PG-13, R) |
| `duration` | Durasi tayang (menit untuk film atau musim untuk TV Show) |
| `listed_in` | Genre atau kategori konten |
| `description` | Sinopsis singkat atau deskripsi konten |

---

## 🛠️ Tech Stack & Libraries
* **Python**
* **Pandas** (manipulasi struktur tabel, pembersihan data, dan *string operations*)
* **NumPy** (operasi numerik dasar)
* **Matplotlib & Seaborn** (visualisasi data statistik dan grafik eksploratif)
* **Collections (Counter)** (menghitung frekuensi kemunculan elemen string terurai)

---

## 🚀 Ringkasan Alur Analisis
1. **Setup & Data Loading:** Mengimpor pustaka yang diperlukan dan memuat dataset `netflix_titles.csv` ke dalam Pandas DataFrame.
2. **Data Inspection:** Memeriksa struktur DataFrame (`df.info()`), dimensi data, serta mendeteksi keberadaan *missing values* pada setiap kolom.
3. **Data Cleaning & Preprocessing:** Menangani nilai kosong serta melakukan konversi format teks dan parsing tanggal pada kolom `date_added` menjadi tipe *datetime*.
4. **Exploratory Data Analysis (EDA):** 
   - Membandingkan proporsi antara jumlah Film dan Serial TV.
   - Menganalisis tren penambahan konten dari tahun ke tahun.
   - Mengidentifikasi negara-negara produsen konten terbanyak di platform Netflix.
5. **Advanced String Analysis:** Memecah string majemuk pada kolom genre (`listed_in`) dan negara (`country`) untuk mendapatkan distribusi kategori yang akurat.

---

## 💡 Temuan Utama & Interpretasi
* **Dominasi Film:** Katalog global Netflix didominasi secara signifikan oleh konten Film (*Movies*) dibandingkan Serial TV (*TV Shows*).
* **Lonjakan Platform:** Penambahan konten baru mengalami pertumbuhan yang sangat pesat dalam beberapa tahun terakhir sebelum mencapai titik stabil ekspansi global.
* **Geografi Konten:** Amerika Serikat dan India menempati posisi teratas sebagai kontributor utama produsen film dan serial TV di katalog Netflix.
* **Kategori Genre:** Genre drama, komedi, dan internasional mendominasi jajaran kategori terbanyak yang dicatat dalam platform.

---

## 📁 Struktur File
```text
├── netflix_titles.csv             # Dataset Netflix Movies and TV Shows
├── Day_5_analisa_netflix_katalog.ipynb # Notebook utama analisis katalog Netflix
└── README.md                      # Dokumentasi project
```