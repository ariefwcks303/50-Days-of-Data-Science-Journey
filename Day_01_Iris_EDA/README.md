# Day 01: Exploratory Data Analysis (EDA) - Iris Dataset

Proyek ini merupakan langkah pertama saya dalam marathon **50-Days-of-Data-Science-Journey**. Fokus hari ini adalah memahami struktur data tabular menggunakan pustaka `pandas` dan mengekstraksi insight dari karakteristik fisik bunga Iris.

## 🎯 Tujuan Pembelajaran
* **Data Handling:** Membaca dan memahami struktur `DataFrame` dengan *pandas*.
* **Data Summarization:** Menggunakan fungsi `head()`, `shape`, `info()`, dan `describe()` untuk mendapatkan ringkasan statistik.
* **Data Visualization:** Melakukan visualisasi dasar untuk melihat korelasi antar fitur.
* **Critical Thinking:** Mengidentifikasi jebakan data (data leakage) pada kolom yang bersifat unik/indeks.

## 🔍 Key Findings (Insight)
Berdasarkan analisis yang telah saya lakukan:
1.  **Fitur Informatif:** Ukuran *petal* (panjang & lebar) terbukti menjadi pembeda paling kuat antar spesies bunga.
2.  **Klasifikasi:** Spesies *Iris-setosa* memiliki pola yang sangat jelas dan mudah dipisahkan dibandingkan dengan *Iris-virginica* dan *Iris-versicolor*.
3.  **Data Hygiene:** Saya mencatat bahwa kolom `Id` hanyalah nomor urut sampel dan tidak memberikan nilai prediktif. Saya melakukan eliminasi/pengabaian pada kolom ini agar model tidak melakukan "kecurangan" (*data leakage*) saat tahap pelatihan nanti.

## 🛠 Tech Stack
* **Language:** Python
* **Libraries:** Pandas, Matplotlib, Seaborn
* **Platform:** Google Colab

## 📈 Alur Kerja (Pipeline)
1.  **Import Library:** Menyiapkan alat analisis.
2.  **Load Dataset:** Mengimpor data dari sumber (Kaggle).
3.  **Statistic View:** Observasi awal terhadap distribusi dan tipe data.
4.  **EDA:** Eksplorasi mendalam mengenai korelasi antar variabel.
5.  **Visualization:** Membuat grafik untuk memvalidasi temuan.
6.  **Kesimpulan:** Merangkum temuan untuk kebutuhan *modeling* di masa depan.

---
*Proyek ini adalah bagian dari rangkaian 50 hari perjalanan belajar Data Science saya.*