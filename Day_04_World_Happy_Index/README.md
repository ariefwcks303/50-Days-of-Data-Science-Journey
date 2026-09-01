# 04 — Menggali World Happiness Report 2019

Analisis eksploratif terhadap **World Happiness Report 2019** (UN Sustainable Development Solutions Network) untuk mengidentifikasi faktor-faktor penentu skor kebahagiaan global seperti ekonomi, dukungan sosial, dan kebebasan.

---

## 🎯 Tujuan Pembelajaran
* Mengurutkan dan menyaring data (filtering & sorting) guna menemukan negara dengan tingkat kebahagiaan tertinggi dan terendah.
* Menghitung koefisien **korelasi** antar variabel numerik dalam dataset.
* Membaca dan menginterpretasikan **heatmap korelasi** untuk mengidentifikasi faktor yang paling berpengaruh terhadap skor kebahagiaan.
* Membangun **scatter plot** untuk menyelidiki pola hubungan linier antara dua variabel.

---

## 📊 Tentang Dataset
* **Sumber:** [Kaggle — unsdsn/world-happiness](https://www.kaggle.com/datasets/unsdsn/world-happiness)
* **File:** `2019.csv` (156 baris × 9 kolom)

| Kolom | Deskripsi |
| :--- | :--- |
| `Overall rank` | Peringkat kebahagiaan global |
| `Country or region` | Nama negara atau wilayah |
| `Score` | Skor kebahagiaan (semakin tinggi, semakin bahagia) |
| `GDP per capita` | Kontribusi Produk Domestik Bruto per kapita |
| `Social support` | Dukungan sosial |
| `Healthy life expectancy` | Angka harapan hidup sehat |
| `Freedom to make life choices` | Kebebasan menentukan pilihan hidup |
| `Generosity` | Tingkat kedermawanan |
| `Perceptions of corruption` | Persepsi terhadap korupsi (kepercayaan pada pemerintah) |

---

## 🛠️ Tech Stack & Libraries
* **Python**
* **Pandas** (manipulasi dan analisis struktur data tabel)
* **NumPy** (operasi numerik)
* **Matplotlib & Seaborn** (visualisasi data dan pembuatan grafik statistik)

---

## 🚀 Ringkasan Alur Analisis
1. **Setup & Data Loading:** Mengunduh dataset langsung via Kaggle API, melakukan ekstrak file zip, dan memuatnya ke dalam Pandas DataFrame.
2. **Data Inspection:** Pengecekan tipe data (`df.info()`), pemeriksaan missing values (`df.isnull().sum()`), dan ringkasan statistik deskriptif (`df.describe()`).
3. **Ranking Analysis:** Memilah dan menampilkan Top 10 negara terbahagia serta Bottom 10 negara tertidak bahagia berdasarkan `Score`.
4. **Data Visualization:** Membuat visualisasi grafik batang (*barplot*) menggunakan Seaborn untuk 10 negara teratas.

---

## 💡 Temuan Utama & Interpretasi
* **Distribusi Global:** Negara-negara di kawasan Eropa mendominasi peringkat atas daftar negara terbahagia (dipimpin oleh **Finlandia**, **Denmark**, dan **Norwegia**), dengan Selandia Baru dan Kanada sebagai perwakilan di luar Eropa. Sebaliknya, kawasan Afrika mendominasi peringkat terbawah.
* **Korelasi Faktor:** Berdasarkan analisis statistik deskriptif, faktor ekonomi (`GDP per capita`) dan `Social support` memiliki rata-rata kontribusi yang jauh lebih tinggi dibandingkan faktor `Generosity` atau `Perceptions of corruption` terhadap skor kebahagiaan secara keseluruhan.

---

## 📁 Struktur File
```text
├── 2019.csv               # Dataset World Happiness Report 2019
├── day_04_happiness.ipynb # Notebook utama berisi kode dan analisis
└── README.md              # Dokumentasi project