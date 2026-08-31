# 04 — Menggali World Happiness Report 2019

Analisis eksploratif terhadap **World Happiness Report 2019** (UN Sustainable Development Solutions Network) untuk mengidentifikasi faktor-faktor penentu skor kebahagiaan global seperti ekonomi, dukungan sosial, dan kebebasan[cite: 3].

---

## 🎯 Tujuan Pembelajaran
* Mengurutkan dan menyaring data (filtering & sorting) guna menemukan negara dengan tingkat kebahagiaan tertinggi dan terendah[cite: 3].
* Menghitung koefisien **korelasi** antar variabel numerik dalam dataset[cite: 3].
* Membaca dan menginterpretasikan **heatmap korelasi** untuk mengidentifikasi faktor yang paling berpengaruh terhadap skor kebahagiaan[cite: 3].
* Membangun **scatter plot** untuk menyelidiki pola hubungan linier antara dua variabel[cite: 3].

---

## 📊 Tentang Dataset
* **Sumber:** [Kaggle — unsdsn/world-happiness](https://www.kaggle.com/datasets/unsdsn/world-happiness)[cite: 3]
* **File:** `2019.csv` (156 baris × 9 kolom)[cite: 3]

| Kolom | Deskripsi |
| :--- | :--- |
| `Overall rank` | Peringkat kebahagiaan global[cite: 3] |
| `Country or region` | Nama negara atau wilayah[cite: 3] |
| `Score` | Skor kebahagiaan (semakin tinggi, semakin bahagia)[cite: 3] |
| `GDP per capita` | Kontribusi Produk Domestik Bruto per kapita[cite: 3] |
| `Social support` | Dukungan sosial[cite: 3] |
| `Healthy life expectancy` | Angka harapan hidup sehat[cite: 3] |
| `Freedom to make life choices` | Kebebasan menentukan pilihan hidup[cite: 3] |
| `Generosity` | Tingkat kedermawanan[cite: 3] |
| `Perceptions of corruption` | Persepsi terhadap korupsi (kepercayaan pada pemerintah)[cite: 3] |

---

## 🛠️ Tech Stack & Libraries
* **Python**[cite: 3]
* **Pandas** (manipulasi dan analisis struktur data tabel)[cite: 3]
* **NumPy** (operasi numerik)[cite: 3]
* **Matplotlib & Seaborn** (visualisasi data dan pembuatan grafik statistik)[cite: 3]

---

## 🚀 Ringkasan Alur Analisis
1. **Setup & Data Loading:** Mengunduh dataset langsung via Kaggle API, melakukan ekstrak file zip, dan memuatnya ke dalam Pandas DataFrame[cite: 3].
2. **Data Inspection:** Pengecekan tipe data (`df.info()`), pemeriksaan missing values (`df.isnull().sum()`), dan ringkasan statistik deskriptif (`df.describe()`)[cite: 3].
3. **Ranking Analysis:** Memilah dan menampilkan Top 10 negara terbahagia serta Bottom 10 negara tertidak bahagia berdasarkan `Score`[cite: 3].
4. **Data Visualization:** Membuat visualisasi grafik batang (*barplot*) menggunakan Seaborn untuk 10 negara teratas[cite: 3].

---

## 💡 Temuan Utama & Interpretasi
* **Distribusi Global:** Negara-negara di kawasan Eropa mendominasi peringkat atas daftar negara terbahagia (dipimpin oleh **Finlandia**, **Denmark**, dan **Norwegia**), dengan Selandia Baru dan Kanada sebagai perwakilan di luar Eropa[cite: 3]. Sebaliknya, kawasan Afrika mendominasi peringkat terbawah[cite: 3].
* **Korelasi Faktor:** Berdasarkan analisis statistik deskriptif, faktor ekonomi (`GDP per capita`) dan `Social support` memiliki rata-rata kontribusi yang jauh lebih tinggi dibandingkan faktor `Generosity` atau `Perceptions of corruption` terhadap skor kebahagiaan secara keseluruhan[cite: 3].

---

## 📁 Struktur File
```text
├── 2019.csv               # Dataset World Happiness Report 2019
├── day_04_happiness.ipynb # Notebook utama berisi kode dan analisis
└── README.md              # Dokumentasi project