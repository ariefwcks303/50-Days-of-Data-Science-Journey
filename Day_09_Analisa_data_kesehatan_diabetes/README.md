# 🩺 Analisis Data Kesehatan Diabetes (EDA)

Exploratory Data Analysis (EDA) terhadap dataset Pima Indians Diabetes untuk memahami pola, distribusi, dan hubungan antar fitur kesehatan yang berkaitan dengan diagnosis diabetes.

## 📋 Deskripsi Project

Data kesehatan sering kali "kotor" dengan cara yang halus — misalnya nilai negatif/nol yang sebenarnya berarti "tidak terukur", bukan nilai valid. Project ini melakukan EDA menyeluruh untuk:
- Membaca data medis dan memahami statistik deskriptifnya
- Mendeteksi dan membersihkan **nilai anomali** (negatif pada Age & Insulin)
- Membandingkan distribusi fitur antara pasien diabetes dan non-diabetes
- Menggali insight dan rekomendasi bisnis dari hasil analisis

## 📊 Tentang Dataset

**Pima Indians Diabetes Dataset** — data medis 1000 pasien wanita keturunan Pima beserta status diabetesnya.

- **Sumber:** [Kaggle — mrsimple07/diabetes-prediction](https://www.kaggle.com/datasets/mrsimple07/diabetes-prediction)
- **Jumlah data:** 1000 baris × 9 kolom

| Kolom | Arti |
|-------|------|
| `Pregnancies` | Jumlah kehamilan |
| `Glucose` | Kadar glukosa darah |
| `BloodPressure` | Tekanan darah diastolik (mm Hg) |
| `SkinThickness` | Ketebalan lipatan kulit trisep |
| `Insulin` | Kadar insulin serum |
| `BMI` | Indeks massa tubuh |
| `DiabetesPedigreeFunction` | Skor riwayat keturunan diabetes |
| `Age` | Usia (tahun) |
| `Diagnosis` | Target: 0 = Non-Diabetes, 1 = Diabetes |

## 🛠️ Tools & Library

- `pandas`, `numpy` — pengolahan data
- `matplotlib`, `seaborn` — visualisasi
- `kaggle` API — unduh dataset

## 🧹 Data Cleaning

Ditemukan anomali medis: nilai negatif pada kolom `Age` dan `Insulin` (tidak mungkin negatif secara logika).

**Langkah pembersihan:**
1. Nilai negatif pada `Age` dan `Insulin` diubah menjadi `NaN`
2. `NaN` diimputasi menggunakan **median** masing-masing kolom
3. `Age` dan `Pregnancies` dibulatkan dan dikonversi ke tipe integer

## 🔍 Alur Exploratory Data Analysis

Analisis disusun berurutan dari yang paling sederhana hingga paling kompleks:

| # | Analisis | Insight Utama |
|---|----------|----------------|
| a | **Proporsi Diabetes vs Non-Diabetes** (Countplot) | Data imbalanced — 694 Non-Diabetes vs 306 Diabetes (~70:30) |
| b | **Distribusi Glucose berdasarkan Diagnosis** (Histogram bertumpuk) | Kelompok Diabetes cenderung punya Glucose lebih tinggi — fitur paling diskriminatif secara visual |
| c | **Perbandingan BMI & Age per Diagnosis** (Boxplot) | Median BMI dan Age relatif mirip antar kelas — kurang diskriminatif sendirian |
| d | **Heatmap Korelasi Antar Fitur** | Korelasi antar fitur (termasuk terhadap Diagnosis) secara umum lemah (< 0.1) dan cenderung non-linear |
| e | **Pairplot Fitur Risiko Utama** (Glucose, BMI, Age, Insulin) | Tidak ada kombinasi 2 fitur yang memisahkan kelas secara jelas — mengindikasikan hubungan fitur-target bersifat multivariat |

## 💡 Kesimpulan & Insight Bisnis

1. **Skrining berbasis Glucose** berpotensi jadi alat *early-warning* murah sebelum tes lanjutan yang lebih mahal (mis. HbA1c)
2. **Data imbalance (~70:30)** mencerminkan prevalensi riil populasi berisiko dan penting untuk estimasi kebutuhan sumber daya kesehatan
3. **Tidak ada fitur tunggal** yang cukup diskriminatif — dibutuhkan pendekatan **skoring risiko multivariat**, bukan aturan sederhana satu fitur
4. Pola hubungan fitur-target bersifat **non-linear**, sehingga model **tree-based** (Random Forest, XGBoost, LightGBM) lebih cocok dibanding model linear
5. Model prediktif sebaiknya diposisikan sebagai **alat bantu triase/risk assessment**, bukan pengganti diagnosis dokter
6. Karena biaya *false negative* (diabetes tidak terdeteksi) lebih mahal daripada *false positive*, metrik **recall/sensitivity** perlu diprioritaskan di atas accuracy pada tahap modeling

## 📁 Struktur File

```
.
├── Day_09_Analisa_Data_Kesehatan_Diabetes_EDA_.ipynb   # Notebook analisis lengkap
└── README.md                                            # Dokumentasi project ini
```

## 🚀 Cara Menjalankan

1. Install dependencies:
   ```bash
   pip install pandas numpy matplotlib seaborn kaggle
   ```
2. Siapkan Kaggle API key (`kaggle.json`) untuk mengunduh dataset
3. Jalankan notebook `Day_09_Analisa_Data_Kesehatan_Diabetes_EDA_.ipynb` secara berurutan dari atas ke bawah

## 🔮 Langkah Selanjutnya

- Feature engineering & feature importance dengan model tree-based
- Penanganan class imbalance (SMOTE, class_weight)
- Modeling klasifikasi (Logistic Regression sebagai baseline, lanjut ke Random Forest/XGBoost)
- Evaluasi model dengan fokus pada recall dan F1-score