# 📊 Mini Proyek: Story Telling with Data

Grafik yang estetis belum tentu mampu menceritakan kondisi bisnis yang sebenarnya. Dalam mini-proyek ini, kita berfokus pada **Data Storytelling**—seni merangkai sekumpulan visualisasi data menjadi sebuah narasi bisnis yang kohesif, logis, dan mudah dipahami oleh audiens non-teknis (seperti manajemen atau *stakeholder*). 

Kita akan membedah data penjualan ritel dari Superstore, bergerak dari gambaran besar (makro) hingga menemukan akar masalah (mikro), lalu mengubah wawasan tersebut menjadi rekomendasi yang bisa langsung dieksekusi.

---

## 📑 Daftar Isi
1. [Tujuan Pembelajaran](#-tujuan-pembelajaran)
2. [Tentang Dataset](#-tentang-dataset)
3. [Alat & Teknologi](#-alat--teknologi)
4. [Alur Cerita Analisis](#-alur-cerita-analisis)
5. [Rekomendasi Bisnis](#-rekomendasi-bisnis)
6. [Struktur Proyek](#-struktur-proyek)
7. [Cara Menjalankan Proyek](#-cara-menjalankan-proyek)

---

## 🎯 Tujuan Pembelajaran
Melalui proyek ini, fokus utama yang ingin dicapai adalah:
* **Memahami Framework Storytelling:** Menggunakan pola narasi bisnis klasik: **Situasi (Situation) → Komplikasi (Complication) → Temuan (Resolution) → Rekomendasi (Action)**.
* **Menyusun Tata Letak Visual:** Membuat urutan grafik yang saling mendukung dan tidak tumpang tindih.
* **Menerjemahkan Data Teknis:** Menulis narasi dan memberikan anotasi pada grafik agar ramah bagi audiens bisnis.
* **Melahirkan *Actionable Insights*:** Tidak sekadar bilang "penjualan naik turun", tapi memberikan saran konkret apa yang harus dilakukan perusahaan.

---

## 📦 Tentang Dataset
Dataset yang digunakan adalah **Sample - Superstore**, yang mencatat rekam jejak transaksi penjualan ritel sebuah jaringan toko serba ada fiktif di Amerika Serikat (AS).

* **Konteks Data:** Penjualan B2C (Business to Consumer) dan B2B (Business to Business) dari berbagai segmen produk.
* **Sumber Data:** [Kaggle — vivek468/superstore-dataset-final](https://www.kaggle.com/datasets/vivek468/superstore-dataset-final)
* **Format & *Encoding*:** File berekstensi `.csv` dan wajib dibaca menggunakan *encoding* `latin-1` karena mengandung karakter khusus yang tidak didukung oleh UTF-8 standar.

---

## 🛠️ Alat & Teknologi
Proyek ini sepenuhnya dibangun menggunakan ekosistem Python untuk Data Science:
* **Python 3.x**
* **Pandas:** Untuk *data wrangling*, manipulasi tabel, agregasi, dan pembersihan data.
* **Matplotlib & Seaborn:** Digunakan sebagai mesin utama untuk menghasilkan visualisasi data yang tajam, kustomisasi palet warna, dan tata letak multi-grafik.
* **Jupyter Notebook:** Sebagai *environment* pengembangan interaktif.

---

## 📈 Alur Cerita Analisis (The Data Story)
Proyek ini membedah data melalui tiga babak narasi utama yang saling berkaitan:

### 📖 Babak 1: Ilusi Pertumbuhan (Omzet Tumbuh dari Waktu ke Waktu)
Di babak pertama, kita melihat kondisi *top-line* bisnis (pendapatan kotor). 
* **Situasi:** Manajemen merasa optimis karena grafik tren bulanan menunjukkan omzet terus bertumbuh dari tahun ke tahun.
* **Temuan:** Ada lonjakan musiman (*seasonality*) yang sangat konsisten dan tajam di kuartal keempat (Q4) setiap tahunnya akibat musim liburan.
* **Komplikasi:** Omzet yang tinggi adalah ilusi jika tidak diimbangi dengan efisiensi. Pertanyaannya: *Apakah tingginya angka penjualan ini sejalan dengan laba yang masuk ke kas perusahaan?*

### 📖 Babak 2: Margin yang Timpang (Laba Tidak Merata per Kategori)
Untuk menjawab komplikasi di Babak 1, kita membandingkan *Sales* (Penjualan) vs *Profit* (Laba) pada tiga kategori utama.
* **Temuan:** Kategori *Technology* dan *Office Supplies* tampil sebagai bintang utama. Mereka tidak hanya mencetak penjualan yang tinggi, tetapi margin labanya sangat tebal dan sehat.
* **Komplikasi:** Kategori *Furniture* menjadi masalah. Penjualannya sangat masif dan berkontribusi besar pada omzet, tetapi laba yang dihasilkan sangat tipis, hampir menyentuh garis nol.

### 📖 Babak 3: Mencari Biang Keladi (Laba Merugi di Sub-Kategori)
Kita melakukan *drill-down* khusus ke dalam kategori *Furniture* dan sub-kategori lainnya untuk mencari tahu apa yang menggerus profitabilitas perusahaan.
* **Temuan:** Visualisasi batang (*bar chart*) dengan jelas mengekspos bahwa sub-kategori **Tables** dan **Bookcases** (dari kategori *Furniture*), serta **Supplies** mencetak angka minus (rugi). Mereka adalah beban atau *bleeding point* perusahaan. Menjual barang-barang ini ternyata lebih memakan biaya operasional/diskon daripada mendatangkan untung.

---

## 💡 Rekomendasi Bisnis
Dari cerita data di atas, berikut adalah langkah strategis yang direkomendasikan untuk manajemen:
1. **Evaluasi Ulang Sub-Kategori *Tables* dan *Bookcases*:** Karena barang-barang ini besar dan berat, kemungkinan besar biaya logistik dan penyimpanannya menggerus margin. Pertimbangkan untuk menaikkan harga jual, mengurangi diskon, atau merenegosiasi ongkos kirim dengan pihak ketiga.
2. **Strategi *Bundling*:** Jika *Tables* tidak bisa dihapus dari katalog karena merupakan daya tarik toko, lakukan *bundling* silang. Jual meja satu paket dengan produk dari kategori *Technology* atau *Office Supplies* yang memiliki margin tinggi untuk mensubsidi kerugian.
3. **Fokus Promosi Q4 di Kategori Unggulan:** Menjelang musim liburan akhir tahun, alokasikan *budget* pemasaran untuk mendorong produk *Technology* alih-alih *Furniture*, guna memaksimalkan laba bersih.

---

## 📂 Struktur Proyek
```text
📦 data-storytelling-superstore
 ┣ 📜 data_storytelling.ipynb        # Jupyter Notebook utama berisi analisis
 ┗ 📜 README.md                      # Dokumentasi proyek ini

 