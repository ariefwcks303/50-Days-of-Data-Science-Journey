# 🎬 Day 48 --- Capstone Project: Sistem Rekomendasi Film dengan Content-Based Filtering


<p align="center">
  <strong>Movie Recommendation System</strong><br>
  Sistem rekomendasi film berbasis kemiripan genre, keywords, dan sinopsis.
</p>

<p align="center">
  <img src="https://img.shields.io/badge/Project-Capstone-blue" alt="Project: Capstone">
  <img src="https://img.shields.io/badge/Type-Recommendation%20System-purple" alt="Type: Recommendation System">
  <img src="https://img.shields.io/badge/Method-Content--Based%20Filtering-green" alt="Method: Content-Based Filtering">
  <img src="https://img.shields.io/badge/Python-3.x-yellow" alt="Python">
</p>

## 📌 1. Gambaran Proyek

Proyek ini membangun sistem rekomendasi film sederhana menggunakan
pendekatan **Content-Based Filtering**. Sistem merekomendasikan film
berdasarkan kemiripan konten, seperti genre, kata kunci, dan sinopsis,
bukan berdasarkan riwayat interaksi atau rating pengguna lain.

Tujuan utamanya adalah memahami bagaimana metadata film dapat diolah
menjadi representasi numerik, dibandingkan tingkat kemiripannya, lalu
digunakan untuk menghasilkan rekomendasi secara otomatis.

## 🎯 2. Tujuan Pembelajaran

-   Memahami konsep Content-Based Filtering.
-   Mem-parse kolom metadata berformat string yang berisi struktur JSON
    menggunakan `ast.literal_eval`.
-   Menggabungkan genre, keywords, dan overview menjadi satu profil
    konten (`tags`).
-   Mengubah teks menjadi representasi numerik menggunakan TF-IDF.
-   Mengukur kemiripan antarfilm menggunakan Cosine Similarity.
-   Membuat fungsi rekomendasi berdasarkan judul film.
-   Menginterpretasikan hasil rekomendasi dan keterbatasan pendekatan
    content-based.

## 📂 3. Dataset

**TMDB 5000 Movie Metadata**

  ---------------------------------------------------------------------------------------------------------
  Informasi                           Detail
  ----------------------------------- ---------------------------------------------------------------------
  Sumber                              [Kaggle --- TMDB 5000 Movie
                                      Metadata](https://www.kaggle.com/datasets/tmdb/tmdb-movie-metadata)

  File utama                          `tmdb_5000_movies.csv`

  Jumlah data                         4.803 film

  Kolom yang digunakan                6
  ---------------------------------------------------------------------------------------------------------

### Kolom yang digunakan

  -----------------------------------------------------------------------
  Kolom                               Deskripsi
  ----------------------------------- -----------------------------------
  `title`                             Judul film

  `genres`                            Genre film dalam representasi
                                      string berisi struktur JSON

  `keywords`                          Kata kunci atau tema film dalam
                                      representasi string berisi struktur
                                      JSON

  `overview`                          Sinopsis film

  `vote_average`                      Rata-rata rating penonton

  `popularity`                        Skor popularitas TMDB
  -----------------------------------------------------------------------

Kolom `genres` dan `keywords` bukan list Python secara langsung,
melainkan string yang merepresentasikan list of dictionaries. Karena
itu, keduanya perlu di-parse sebelum digunakan.

## 🧰 4. Teknologi dan Library

  Teknologi / Library                Peran
  ---------------------------------- ----------------------------------------
  Python                             Bahasa pemrograman
  Pandas & NumPy                     Manipulasi data
  `ast`                              Parsing metadata dengan `literal_eval`
  Matplotlib & Seaborn               Visualisasi data
  Scikit-learn `TfidfVectorizer`     Mengubah teks menjadi vektor TF-IDF
  Scikit-learn `cosine_similarity`   Mengukur kemiripan antarvektor

## ⚙️ 5. Alur Pengerjaan

### 🔄 Ringkasan pipeline

``` mermaid
flowchart TD
    A["Dataset TMDB<br/>4.803 film"] --> B["Seleksi kolom & pemeriksaan data"]
    B --> C["Parsing genres dan keywords"]
    C --> D["Feature Engineering<br/>Membuat tags"]
    D --> E["TF-IDF Vectorization<br/>4.803 × 5.000"]
    E --> F["Cosine Similarity<br/>4.803 × 4.803"]
    F --> G["Fungsi rekomendasi(judul, n=5)"]
    G --> H["Top-N film rekomendasi"]
```

### 5.1 Pemilihan Kolom dan Pemeriksaan Awal

Dari dataset awal, notebook memilih enam kolom yang relevan untuk
rekomendasi berbasis konten: `title`, `genres`, `keywords`, `overview`,
`vote_average`, dan `popularity`.

  Pemeriksaan               Hasil
  ----------------------- -------
  Jumlah baris              4.803
  Jumlah kolom terpilih         6
  Missing values                3
  Baris duplikat                0

Ketiga missing values terdapat pada kolom `overview`. Nilai tersebut
diisi dengan string kosong agar proses penggabungan teks dapat
dilakukan.

### 5.2 Parsing Genre dan Keywords

Fungsi `take_name()` menggunakan `ast.literal_eval()` untuk mengubah
string metadata menjadi objek Python, kemudian mengambil nilai `name`
dari setiap dictionary.

Hasil parsing disimpan pada kolom baru:

-   `genre_list`
-   `keyword_list`

Jika parsing gagal karena format tidak valid atau tipe data tidak
sesuai, fungsi mengembalikan list kosong.

### 5.3 Feature Engineering: Membuat `tags`

Genre, keywords, dan overview digabungkan menjadi satu dokumen teks pada
kolom `tags`.

Sebelum penggabungan, spasi pada item genre dan keyword yang terdiri
dari beberapa kata dihapus. Contohnya, `Science Fiction` menjadi
`ScienceFiction`, agar diperlakukan sebagai satu token.

Seluruh teks kemudian diubah menjadi huruf kecil. Kolom `tags` menjadi
representasi tekstual gabungan atau profil konten setiap film yang
digunakan dalam proses vektorisasi.

### 5.4 Exploratory Data Analysis

Notebook melakukan beberapa eksplorasi:

  -----------------------------------------------------------------------
  Analisis                            Temuan
  ----------------------------------- -----------------------------------
  Genre populer                       Drama (2.297), Comedy (1.722),
                                      Thriller (1.274)

  Distribusi rating                   Mayoritas rating berada di sekitar
                                      6--7

  Panjang sinopsis                    Sebagian besar overview memiliki
                                      panjang sekitar 20--50 kata

  Film dengan rating tertinggi        Menampilkan 12 film setelah
                                      membatasi data pada minimal 1.000
                                      vote
  -----------------------------------------------------------------------

Analisis ini membantu memahami karakteristik metadata yang menjadi dasar
rekomendasi. Film dengan sinopsis pendek akan lebih bergantung pada
informasi genre dan keywords.

### 5.5 TF-IDF: Mengubah Teks Menjadi Vektor

`TfidfVectorizer` digunakan untuk mengubah kolom `tags` menjadi
representasi numerik.

``` python
TfidfVectorizer(
    stop_words="english",
    max_features=5000
)
```

  Komponen                              Ukuran
  ---------------------------- ---------------
  Jumlah film                            4.803
  Jumlah fitur/kata terpilih             5.000
  Matriks TF-IDF                 4.803 × 5.000

TF-IDF memberikan bobot lebih tinggi pada kata yang relatif khas untuk
suatu dokumen dan menurunkan bobot kata yang terlalu umum di banyak
dokumen. Dengan demikian, setiap film direpresentasikan sebagai vektor
dalam ruang fitur teks.

### 5.6 Cosine Similarity: Mengukur Kemiripan Film

Cosine Similarity digunakan untuk menghitung kemiripan antarvektor
TF-IDF. Hasilnya adalah matriks kemiripan berukuran:

``` text
4.803 × 4.803
```

Setiap elemen `(i, j)` menyatakan tingkat kemiripan konten film ke-i
dengan film ke-j. Nilai diagonal bernilai 1 karena setiap film
dibandingkan dengan dirinya sendiri.

Dalam implementasi ini, matriks kemiripan dihitung untuk seluruh
pasangan film dan disimpan dalam variabel `similarity`.

### 5.7 Fungsi Rekomendasi

Fungsi berikut digunakan untuk menghasilkan rekomendasi:

``` python
rekomendasi(judul, n=5)
```

**Alur kerja fungsi:**

1.  Mengubah input judul menjadi huruf kecil dan menghapus spasi di
    awal/akhir.
2.  Mencari indeks film berdasarkan judul.
3.  Jika judul persis tidak ditemukan, sistem mencoba pencarian
    substring.
4.  Mengambil skor kemiripan film terhadap seluruh film lain.
5.  Mengurutkan skor dari tertinggi ke terendah.
6.  Mengabaikan film yang sedang dicari.
7.  Mengembalikan sejumlah rekomendasi teratas.

**Informasi pada output:**

  Output       Keterangan
  ------------ --------------------------------------------
  Judul film   Nama film yang direkomendasikan
  Genre        Genre film
  Rating       `vote_average`
  Kemiripan    Skor cosine similarity terhadap film input

Jika judul tidak ditemukan, fungsi mengembalikan pesan bahwa film tidak
tersedia di dataset.

## 📊 6. Hasil Eksperimen

Sistem diuji menggunakan tiga judul film. Berikut lima rekomendasi
teratas untuk setiap judul.

### 6.1 🎥 Input: Avatar

  Film Rekomendasi      Genre                                  Rating   Kemiripan
  --------------------- ------------------------------------ -------- -----------
  Falcon Rising         Adventure, Action                         5.5       0.207
  Battle: Los Angeles   Action, Science Fiction                   5.5       0.193
  Apollo 18             Horror, Thriller, Science Fiction         5.0       0.187
  Titan A.E.            Animation, Action, Science Fiction        6.3       0.182
  Predators             Action, Science Fiction, Adventure        6.0       0.168

Hasil rekomendasi didominasi film bertema fiksi ilmiah, petualangan,
luar angkasa, dan dunia asing.

### 6.2 🦇 Input: The Dark Knight

  -------------------------------------------------------------------------
  Film            Genre                         Rating            Kemiripan
  Rekomendasi                                          
  --------------- --------------- -------------------- --------------------
  The Dark Knight Action, Crime,                   7.6                0.468
  Rises           Drama                                

  Batman Returns  Action, Fantasy                  6.6                0.398

  Batman Forever  Action, Crime,                   5.2                0.337
                  Fantasy                              

  Batman Begins   Action, Crime,                   7.5                0.324
                  Drama                                

  Batman: The     Action,                          7.9                0.296
  Dark Knight     Animation                            
  Returns, Part 2                                      
  -------------------------------------------------------------------------

Sistem merekomendasikan film Batman dan film bertema superhero,
kriminal, serta aksi. Kemiripan keyword dan tema membantu menghasilkan
rekomendasi yang relevan.

### 6.3 🦸 Input: The Avengers

  -------------------------------------------------------------------------
  Film            Genre                         Rating            Kemiripan
  Rekomendasi                                          
  --------------- --------------- -------------------- --------------------
  Avengers: Age   Action,                          7.3                0.277
  of Ultron       Adventure,                           
                  Science Fiction                      

  Ant-Man         Science                          7.0                0.222
                  Fiction,                             
                  Action,                              
                  Adventure                            

  Captain         Action,                          6.6                0.219
  America: The    Adventure,                           
  First Avenger   Science Fiction                      

  Thor: The Dark  Action,                          6.8                0.214
  World           Adventure,                           
                  Fantasy                              

  Iron Man 3      Action,                          6.8                0.205
                  Adventure,                           
                  Science Fiction                      
  -------------------------------------------------------------------------

Hasil didominasi film superhero Marvel dan film aksi-petualangan dengan
karakter serta tema yang beririsan.

### 📈 Ringkasan hasil

  Film input        Pola rekomendasi yang terlihat         Kemiripan tertinggi
  ----------------- ------------------------------------ ---------------------
  Avatar            Sci-Fi, petualangan, luar angkasa                    0.207
  The Dark Knight   Batman, superhero, kriminal, aksi                    0.468
  The Avengers      Superhero Marvel, aksi-petualangan                   0.277

> **Catatan:** Skor kemiripan menunjukkan kedekatan representasi teks
> berdasarkan fitur yang digunakan. Skor ini bukan probabilitas bahwa
> pengguna akan menyukai film tersebut.

## 💡 7. Insight dan Interpretasi

1.  **Metadata dapat menjadi dasar rekomendasi.** Genre, keywords, dan
    sinopsis menyediakan informasi untuk membangun profil konten film.
2.  **TF-IDF membantu membedakan konten.** Kata yang terlalu umum
    mendapat bobot lebih rendah, sedangkan kata yang lebih khas dapat
    menjadi pembeda antarfilm.
3.  **Cosine Similarity menghasilkan urutan rekomendasi.** Sistem
    memilih film dengan skor kemiripan tertinggi terhadap film input.
4.  **Content-Based Filtering tidak membutuhkan data interaksi
    pengguna.** Rekomendasi dapat dibuat hanya dari metadata item.
5.  **Rekomendasi dapat terlalu serupa.** Sistem cenderung menyajikan
    film dengan genre, tema, atau franchise yang mirip sehingga variasi
    rekomendasi dapat terbatas.

## ⚠️ 8. Keterbatasan Sistem

  -----------------------------------------------------------------------
  Area                    Keterbatasan            Dampak
  ----------------------- ----------------------- -----------------------
  Data pengguna           Belum mempelajari       Belum personal
                          preferensi dari         berdasarkan interaksi
                          perilaku pengguna       

  Metadata                Bergantung pada         Metadata yang tidak
                          kualitas genre,         lengkap dapat
                          keywords, dan overview  mengurangi kualitas
                                                  representasi

  Sinopsis                Film dengan sinopsis    Rekomendasi lebih
                          pendek memiliki sinyal  bergantung pada genre
                          konten lebih sedikit    dan keywords

  Pencarian judul         Pencarian substring     Potensi hasil pencarian
                          dapat memilih kecocokan ambigu
                          pertama saat ada judul  
                          mirip                   

  Skoring                 Rating dan popularitas  Ranking hanya
                          belum digabungkan ke    berdasarkan kemiripan
                          skor rekomendasi        konten

  Skalabilitas            Matriks cosine          Kebutuhan memori dan
                          similarity penuh        komputasi meningkat
                          berukuran 4.803 × 4.803 seiring jumlah film

  Evaluasi                Evaluasi masih          Belum ada metrik
                          kualitatif melalui      rekomendasi formal atau
                          beberapa contoh         pengujian pengguna
  -----------------------------------------------------------------------

## 🚀 9. Pengembangan Selanjutnya

  -----------------------------------------------------------------------
  Prioritas pengembangan  Ide                     Tujuan
  ----------------------- ----------------------- -----------------------
  Hybrid Rating           Menggabungkan kemiripan Mempertimbangkan
                          konten dengan rating    kemiripan sekaligus
                          dan jumlah vote,        kualitas penilaian
                          misalnya Bayesian       
                          Average                 

  Collaborative Filtering Mengintegrasikan data   Menangkap preferensi
                          rating atau interaksi   kolektif pengguna
                          pengguna                

  Metadata Expansion      Menambahkan informasi   Memperkaya profil
                          sutradara dan pemeran   konten film
                          utama                   

  Evaluasi Sistem         Menambahkan evaluasi    Mengukur kualitas
                          relevansi rekomendasi   rekomendasi secara
                          dan pengujian pengguna  sistematis

  Skalabilitas            Mengevaluasi            Menangani katalog film
                          nearest-neighbor search yang lebih besar tanpa
                          atau indexing           menyimpan seluruh
                                                  matriks kemiripan
  -----------------------------------------------------------------------

## 🗂️ 10. Struktur Proyek

``` text
Day_48_Capstone_Project_System_Recommendation/
├── Day_48_Capstone_Project_System_Recommendation.ipynb
└── README.md
```

Dataset `tmdb_5000_movies.csv` diunduh dari Kaggle dan digunakan sebagai
input notebook.

## 🏁 11. Kesimpulan

Pada Day 48, dibangun sistem rekomendasi film berbasis **Content-Based
Filtering** menggunakan metadata TMDB 5000 Movie Metadata.

Genre, keywords, dan overview diproses menjadi satu profil konten
(`tags`), kemudian diubah menjadi vektor TF-IDF. Cosine Similarity
digunakan untuk menghitung kemiripan antarfilm, dan fungsi
`rekomendasi()` mengembalikan film dengan skor kemiripan tertinggi.

Pengujian pada Avatar, The Dark Knight, dan The Avengers menunjukkan
bahwa sistem dapat menghasilkan rekomendasi yang relevan secara tematik
berdasarkan metadata film.

Proyek ini menjadi implementasi dasar sistem rekomendasi end-to-end.
Pengembangan berikutnya dapat menggabungkan sinyal rating dan perilaku
pengguna, memperluas metadata, serta mengevaluasi relevansi rekomendasi
secara lebih formal.

------------------------------------------------------------------------


<p align="center">

`<sub>Day 48 --- 50 Days of Data Science Journey`</sub>`
</p>

