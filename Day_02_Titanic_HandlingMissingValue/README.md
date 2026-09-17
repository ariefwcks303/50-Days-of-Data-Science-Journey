# Day 02: Data Cleaning - Titanic Dataset

Selamat datang di proyek hari ke-2 dari perjalanan **50-Days-of-Data-Science-Journey** saya! Setelah kemarin memahami dataset yang "bersih", hari ini saya belajar menghadapi kenyataan di lapangan: **Data Kotor**.

Dalam proyek ini, saya mengeksplorasi dataset Titanic yang legendaris untuk mempelajari teknik dasar *Data Cleaning* agar data siap digunakan untuk analisis lebih lanjut.

## 🎯 Mengapa Proyek Ini Penting?
Dalam dunia nyata, data tidak pernah sempurna. Seringkali kita menemukan:
* **Missing Values:** Data yang kosong atau tidak tercatat.
* **Duplikat:** Data ganda yang bisa merusak akurasi perhitungan.
* **Tipe Data Salah:** Format angka yang terbaca sebagai teks, dsb.

Jika kita membiarkan data ini kotor, hasil analisis (dan model AI) kita akan menyesatkan.

## 🛠 Apa yang Saya Pelajari?
1. **Deteksi Data:** Menemukan kolom mana saja yang "bolong" (kosong).
2. **Strategi Imputasi:**
   - Mengisi data yang kosong menggunakan nilai tengah (**Median**) atau nilai yang paling sering muncul (**Modus**).
   - Menghapus kolom yang terlalu banyak kosongnya (karena tidak informatif).
3. **Data Integrity:** Menghapus entri ganda dan memastikan tipe data sesuai.

## 📊 Insight & Temuan


Dari analisis ini, saya menemukan pola yang menarik:
- **Prioritas Penyelamatan:** Data menunjukkan bahwa penumpang wanita dan penumpang kelas atas memiliki tingkat keselamatan yang lebih tinggi.
- **Kejujuran Data:** Daripada "mengarang" isi kolom `Cabin` yang 77% kosong, saya memilih untuk menghapus kolom tersebut agar analisis tetap objektif.
- **Pentingnya Konteks:** Mengisi umur dengan nilai rata-rata global itu berisiko. Saya belajar bahwa menggunakan **median per kelas** jauh lebih akurat daripada sekadar angka rata-rata tunggal.

## 🚀 Cara Menjalankan
1. Anda bisa melihat alur pengerjaan saya di file: `Day_2_Cleaning_Data_Studi_Kasus_Titanic_Dataset.ipynb`
2. Jalankan notebook tersebut di [Google Colab](https://colab.research.google.com/).

---
*Proyek ini adalah bagian dari rangkaian 50 hari perjalanan belajar Data Science saya. Mari terhubung di [LinkedIn Anda].*