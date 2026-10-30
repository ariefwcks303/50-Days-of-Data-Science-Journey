# Day 43 — Hierarchical Clustering dengan Dendrogram untuk Segmentasi Pelanggan

## 📌 Gambaran Proyek

Proyek **Day 43** mengeksplorasi *unsupervised learning* menggunakan **Agglomerative Hierarchical Clustering** untuk mengelompokkan pelanggan mal berdasarkan **pendapatan tahunan** dan **skor belanja**. Berbeda dari K-Means yang jumlah clusternya ditentukan sebelum pelatihan, hierarchical clustering membangun struktur penggabungan bertingkat yang dapat divisualisasikan melalui **dendrogram**. Jumlah cluster kemudian dipilih dengan mengamati struktur tersebut.

Notebook juga membandingkan hasil Agglomerative Clustering dengan **K-Means** menggunakan **Silhouette Score**, **Adjusted Rand Index (ARI)**, dan visualisasi sebaran cluster.

## 🎯 Tujuan Pembelajaran

- Memahami konsep *agglomerative hierarchical clustering* dan metode **Ward linkage**.
- Membuat serta membaca dendrogram menggunakan `scipy.cluster.hierarchy`.
- Menerapkan `AgglomerativeClustering` dari scikit-learn.
- Melakukan standardisasi fitur sebelum clustering berbasis jarak.
- Membandingkan kualitas dan kesamaan hasil Agglomerative Clustering dengan K-Means.
- Menyusun interpretasi segmentasi pelanggan sebagai dasar hipotesis strategi pemasaran.

## 📂 Dataset

**Nama:** Mall Customer Segmentation  
**Sumber:** [Kaggle — Customer Segmentation Tutorial in Python](https://www.kaggle.com/datasets/vjchoudhary7/customer-segmentation-tutorial-in-python)  
**File:** `Mall_Customers.csv`

| Informasi | Nilai |
|---|---:|
| Jumlah baris | 200 |
| Jumlah kolom | 5 |
| Missing values | 0 |
| Baris duplikat | 0 |

Dataset berisi `CustomerID`, `Gender`, `Age`, `Annual Income (k$)`, dan `Spending Score (1-100)`. Eksperimen menggunakan **dua fitur** untuk clustering:

| Fitur | Keterangan |
|---|---|
| `Annual Income (k$)` | Pendapatan tahunan pelanggan dalam ribuan USD |
| `Spending Score (1-100)` | Skor perilaku belanja pelanggan |

Pendapatan tahunan berada pada rentang **15–137 ribu USD**, sedangkan skor belanja berada pada rentang **1–99**. Tidak ada variabel target karena tugas ini merupakan **unsupervised learning**.

## 🛠️ Teknologi

- Python
- Pandas dan NumPy
- Matplotlib dan Seaborn
- SciPy (`linkage`, `dendrogram`, `fcluster`)
- Scikit-learn (`StandardScaler`, `AgglomerativeClustering`, `KMeans`, `silhouette_score`, `adjusted_rand_score`)
- Jupyter Notebook / Google Colab

## 🔍 Alur Eksperimen

### 1. Eksplorasi Data

Notebook memeriksa struktur data melalui `head()`, `info()`, `describe()`, jumlah missing values, dan duplikasi. Scatter plot awal digunakan untuk melihat sebaran **Annual Income** terhadap **Spending Score** sebelum pembentukan cluster.

### 2. Pemilihan Fitur dan Standardisasi

Fitur yang digunakan:

```python
X = df[['Annual Income (k$)', 'Spending Score (1-100)']].copy()
scaler = StandardScaler()
X_scaled = scaler.fit_transform(X)
```

Hasil transformasi memiliki bentuk **(200, 2)**, dengan rata-rata masing-masing fitur mendekati **0** dan standar deviasi **1**. Standardisasi digunakan agar skala fitur tidak mendominasi perhitungan jarak.

### 3. Membuat Dendrogram dengan Ward Linkage

```python
linked = linkage(X_scaled, 'ward')
label = fcluster(linked, 5, criterion='maxclust')

dendrogram(linked, truncate_mode=None, color_threshold=7)
plt.axhline(y=7, color='red', linestyle='--')
```

**Dendrogram** memperlihatkan urutan penggabungan observasi atau cluster. Ketinggian penggabungan menunjukkan besarnya ketidakmiripan menurut kriteria Ward, bukan sekadar jarak mentah antartitik. Notebook menampilkan garis potong pada **y = 7** dan menggunakan **5 cluster** sebagai konfigurasi eksperimen.

> **Catatan interpretasi:** Pemilihan lima cluster didukung visualisasi dendrogram dalam notebook, tetapi belum dibandingkan secara sistematis dengan seluruh alternatif jumlah cluster. Karena itu, angka lima diperlakukan sebagai pilihan eksperimen, bukan bukti optimum global.

### 4. Agglomerative Clustering

```python
agg = AgglomerativeClustering(
    n_clusters=5,
    metric='euclidean',
    linkage='ward'
)
df['Cluster_agg'] = agg.fit_predict(X_scaled)
```

**Distribusi anggota cluster:**

| Label cluster | Jumlah pelanggan |
|---|---:|
| 0 | 32 |
| 1 | 39 |
| 2 | 85 |
| 3 | 21 |
| 4 | 23 |
| **Total** | **200** |

Semua pelanggan memperoleh satu label cluster; cluster **2** memiliki anggota terbanyak, yaitu **85 pelanggan**.

### 5. Pembanding: K-Means

```python
k_mn = KMeans(n_clusters=5, random_state=42, n_init=10)
df['KMeans_cluster'] = k_mn.fit_predict(X_scaled)
```

Kedua model dievaluasi pada representasi fitur terstandardisasi yang sama. Notebook kemudian membandingkan hasilnya melalui Silhouette Score, ARI, dan dua scatter plot berdampingan.

## 📊 Hasil Evaluasi

| Metrik | Agglomerative | K-Means |
|---|---:|---:|
| Jumlah cluster | 5 | 5 |
| **Silhouette Score** | **0,5538** | **0,5547** |

**Adjusted Rand Index (ARI) antarhasil clustering: `0,9420`.**

### Interpretasi

- **Silhouette Score:** Mengukur seberapa dekat suatu observasi dengan cluster-nya dibandingkan dengan cluster lain. Kedua model memiliki skor sekitar **0,55**; K-Means unggul tipis sebesar **0,0009**.
- **ARI:** Mengukur kesesuaian dua partisi cluster dengan penyesuaian terhadap kesamaan yang terjadi secara kebetulan. Nilai **0,9420** menunjukkan kesamaan segmentasi yang sangat tinggi.
- **Visualisasi:** Scatter plot kedua model menunjukkan pola kelompok yang sangat mirip. Nomor label cluster tidak perlu sama karena label cluster bersifat arbitrer.

**Kesimpulan evaluasi:** Pada dataset dan konfigurasi ini, Agglomerative Clustering dan K-Means menghasilkan pengelompokan yang hampir sama. Hasil tersebut tidak membuktikan bahwa salah satu metode selalu lebih baik untuk dataset lain.

## 💡 Interpretasi Bisnis

Notebook mengusulkan lima **persona segmentasi** dan pendekatan pemasaran berdasarkan kombinasi pendapatan serta skor belanja:

| Segmen konseptual | Karakteristik | Usulan strategi |
|---|---|---|
| **Stars** | Pendapatan tinggi, belanja tinggi | Loyalitas VIP, produk eksklusif, penawaran personal |
| **Targets** | Pendapatan rendah, belanja tinggi | Diskon terarah, poin loyalitas, promo yang sesuai |
| **Carefuls** | Pendapatan tinggi, belanja rendah | Komunikasi kualitas, manfaat produk, dan nilai jangka panjang |
| **Standard** | Pendapatan sedang, belanja sedang | Bundling produk dan penawaran kebutuhan rutin |
| **Sensibles** | Pendapatan rendah, belanja rendah | Produk esensial dan promosi yang sensitif terhadap harga |

**Batasan interpretasi:** Tabel ini mengikuti hipotesis persona bisnis pada bagian kesimpulan notebook. Notebook belum menghitung profil rata-rata tiap label `Cluster_agg` untuk memetakan persona tersebut secara eksplisit ke label **0–4**. Sebelum digunakan dalam kampanye nyata, profil masing-masing cluster perlu diverifikasi melalui agregasi fitur dan pengujian bisnis.

## 🧠 Pembelajaran Utama

1. **Dendrogram memberikan struktur hierarkis**, sehingga hubungan penggabungan antarobservasi dapat diamati sebelum memilih jumlah cluster.
2. **StandardScaler relevan untuk clustering berbasis jarak**, terutama ketika fitur memiliki satuan dan rentang berbeda.
3. **Silhouette Score dan ARI menjawab pertanyaan berbeda:** kualitas pemisahan cluster versus kesamaan dua hasil pengelompokan.
4. **Metode berbeda dapat memberikan segmentasi serupa**, sebagaimana ditunjukkan oleh ARI 0,9420 pada eksperimen ini.
5. **Cluster belum otomatis menjadi segmen bisnis yang siap digunakan.** Profil cluster dan dampak strategi perlu divalidasi lebih lanjut.

## ⚠️ Keterbatasan

- Eksperimen menggunakan **dua fitur**, sehingga aspek lain seperti usia dan gender belum masuk dalam pembentukan cluster.
- Pemilihan **5 cluster** belum diuji sistematis terhadap alternatif jumlah cluster dengan metrik yang sama.
- Evaluasi dilakukan pada satu dataset kecil (**200 pelanggan**) tanpa pengujian kestabilan terhadap perubahan data.
- Persona bisnis masih berupa interpretasi awal, belum dipetakan secara terverifikasi ke ID cluster numerik.
- Tidak ada pengujian dampak kampanye atau metrik bisnis aktual dalam notebook.
- Agglomerative Clustering dan K-Means memiliki karakteristik komputasi yang berbeda; hasil pada dataset kecil ini belum cukup untuk menentukan pilihan pada skala besar.

## 🚀 Pengembangan Selanjutnya

- Bandingkan Silhouette Score untuk beberapa nilai `n_clusters`.
- Buat tabel profil cluster menggunakan `groupby('Cluster_agg')` untuk memeriksa rata-rata pendapatan, skor belanja, dan ukuran cluster.
- Uji metode linkage lain seperti `complete` atau `average`, kemudian bandingkan hasilnya.
- Periksa stabilitas segmentasi dan relevansinya pada data pelanggan yang lebih baru.
- Validasi strategi pemasaran dengan metrik bisnis, misalnya conversion rate, retensi, atau nilai transaksi.

## 📁 Struktur Proyek

```text
Day_43_Unsupervised_Model_Clustering_using_Dendrogram/
├── Day_43_Unsupervised_Model_Clustering_using_Dendrogram.ipynb
└── README.md
```

Dataset `Mall_Customers.csv` diunduh dari Kaggle melalui langkah pada notebook. Untuk menjalankan ulang proses pengunduhan, pengguna perlu mengonfigurasi kredensial Kaggle pada lingkungan masing-masing; **jangan mengunggah `kaggle.json` ke repository publik**.

## 📝 Kesimpulan

Eksperimen Day 43 berhasil menerapkan **Hierarchical Clustering dengan Ward linkage dan dendrogram** untuk segmentasi 200 pelanggan menjadi lima cluster. **Agglomerative Clustering** menghasilkan Silhouette Score **0,5538**, sedangkan **K-Means** memperoleh **0,5547**. Nilai **ARI 0,9420** menunjukkan kesamaan pengelompokan yang sangat tinggi.

Notebook ini memperlihatkan bahwa evaluasi clustering tidak cukup hanya mengamati visualisasi: kualitas pemisahan, konsistensi antarmetode, dan interpretasi segmen bisnis perlu dipertimbangkan bersama sebelum hasil clustering digunakan untuk keputusan nyata.
