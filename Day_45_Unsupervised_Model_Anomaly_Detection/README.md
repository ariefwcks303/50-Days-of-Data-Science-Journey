# Day 45 — Unsupervised Model: Anomaly Detection using Isolation Forest

## Overview

Pada project ini, saya mempelajari cara mendeteksi transaksi kartu kredit yang mencurigakan menggunakan pendekatan **Unsupervised Learning**, yaitu **Anomaly Detection dengan Isolation Forest**.

Tujuan eksperimen ini adalah melatih model tanpa memberikan label transaksi sah atau penipuan (`Class`). Setelah model menghasilkan prediksi anomali, hasilnya dibandingkan dengan label asli untuk mengevaluasi kemampuan deteksi.

Isolation Forest menggunakan asumsi bahwa data anomali cenderung jarang dan berbeda dari sebagian besar data, sehingga lebih mudah diisolasi melalui pemisahan acak.

## Learning Objectives

- Memahami konsep Anomaly Detection dalam Unsupervised Learning.
- Memahami cara kerja algoritma `IsolationForest`.
- Melatih model tanpa menggunakan label target.
- Memahami parameter `contamination` untuk menentukan perkiraan proporsi anomali.
- Membandingkan hasil deteksi dengan label asli menggunakan Confusion Matrix dan Classification Report.
- Memahami trade-off antara Precision dan Recall dalam konteks fraud detection.

## Dataset

Dataset yang digunakan adalah **Credit Card Fraud Detection**.

- **Source:** [Kaggle — Credit Card Fraud Detection](https://www.kaggle.com/datasets/mlg-ulb/creditcardfraud)
- **Original data:** 284,807 transactions
- **Total features:** 30 fitur input dan 1 label (`Class`)
- **Fraud transactions:** 492
- **Normal transactions:** 284,315

Dataset berisi transaksi kartu kredit Eropa yang telah dianonimkan. Fitur `V1` sampai `V28` merupakan hasil transformasi PCA, sedangkan `Time` dan `Amount` berisi informasi waktu dan nominal transaksi.

Target `Class`:
- `0` = transaksi sah (normal)
- `1` = transaksi penipuan (fraud)

Dataset asli sangat tidak seimbang karena jumlah transaksi fraud jauh lebih sedikit dibanding transaksi normal.

## Data Sampling

Untuk membuat proses eksperimen lebih ringan, digunakan seluruh transaksi fraud dan sampel 8,000 transaksi normal.

```python
fraudulent = df[df['Class'] == 1]
non_fraudulent = df[df['Class'] == 0].sample(n=8000, random_state=42)

df_sample = pd.concat([fraudulent, non_fraudulent])
df_sample = df_sample.sample(frac=1, random_state=42).reset_index(drop=True)
```

Hasil sampling:

| Information | Result |
|---|---:|
| Total rows | 8,492 |
| Total columns | 31 |
| Fraud transactions | 492 |
| Normal transactions | 8,000 |
| Fraud proportion | 5.79% |
| Missing values | 0 |
| Duplicate rows | 19 |

Pada pemeriksaan dataset asli ditemukan 1,081 baris duplikat. Setelah sampling, terdapat 19 baris duplikat. Notebook mencatat bahwa duplikat tersebut sebaiknya ditinjau, tetapi tidak melakukan penghapusan duplikat pada eksperimen ini.

## Exploratory Data Analysis

EDA dilakukan untuk memahami distribusi transaksi dan melihat pola antara transaksi normal dan fraud.

### 1. Transaction Amount Distribution

Distribusi nominal transaksi divisualisasikan menggunakan histogram dengan membatasi nominal di bawah 500 agar pola lebih mudah diamati.

Dari visualisasi, sebagian besar transaksi normal maupun fraud terkonsentrasi pada nominal yang relatif kecil. Notebook juga mencatat adanya area nominal tertentu, sekitar 100, dengan proporsi transaksi fraud yang tampak lebih tinggi.

### 2. PCA Features: V14 vs V17

Scatter plot menggunakan fitur `V14` dan `V17` untuk melihat sebaran transaksi normal dan fraud.

Pada visualisasi tersebut, transaksi normal cenderung mengelompok, sedangkan transaksi fraud lebih banyak tersebar di area dengan nilai fitur yang lebih rendah atau negatif. Kedua fitur ini memberikan gambaran pemisahan kelas pada ruang dua dimensi.

## Modeling — Isolation Forest

Algoritma yang digunakan adalah `IsolationForest` dari Scikit-learn.

Pada tahap training, kolom `Class` dipisahkan dari fitur dan **tidak digunakan oleh model**.

```python
X = df_sample.drop(columns=["Class"])
y = df_sample["Class"]

kontaminasi = float(y.mean())

iso = IsolationForest(
    contamination=kontaminasi,
    random_state=42,
    n_estimators=200
)

iso.fit(X)
y_pred = iso.predict(X)
```

### Model Parameters

| Parameter | Value | Description |
|---|---:|---|
| `contamination` | ~0.0579 | Perkiraan proporsi anomali dalam data |
| `random_state` | 42 | Menjaga konsistensi hasil |
| `n_estimators` | 200 | Jumlah estimator dalam Isolation Forest |

Output prediksi Isolation Forest:
- `-1` = anomali
- `1` = normal

Prediksi kemudian dikonversi menjadi label biner agar dapat dibandingkan dengan label asli:

```python
df_sample["Anomaly"] = np.where(y_pred == -1, 1, 0)
```

Model mendeteksi **492 transaksi sebagai anomali**, sama dengan jumlah transaksi fraud asli. Namun, kesamaan jumlah ini tidak berarti semua transaksi fraud berhasil ditemukan. Identitas baris yang diprediksi tetap perlu diperiksa melalui evaluasi.

## Model Evaluation

Label `Class` asli digunakan pada tahap validasi untuk membandingkan hasil prediksi anomali dengan kondisi sebenarnya.

### Confusion Matrix

```text
[[7827  173]
 [ 173  319]]
```

| Actual / Predicted | Normal (0) | Fraud (1) |
|---|---:|---:|
| Normal (0) | 7,827 | 173 |
| Fraud (1) | 173 | 319 |

### Classification Report

| Class | Precision | Recall | F1-Score | Support |
|---|---:|---:|---:|---:|
| Normal (0) | 0.98 | 0.98 | 0.98 | 8,000 |
| Fraud (1) | 0.65 | 0.65 | 0.65 | 492 |
| **Accuracy** | | | **0.96** | **8,492** |

### Evaluation Insights

**1. Accuracy sebesar 96%**

Nilai accuracy terlihat tinggi, tetapi perlu diperhatikan bahwa data didominasi transaksi normal. Oleh karena itu, accuracy saja belum cukup untuk menggambarkan kemampuan model dalam mendeteksi fraud.

**2. Performa pada transaksi normal**

Precision dan recall kelas normal mencapai 98%. Model cukup baik dalam mengenali transaksi yang sah.

**3. Performa pada transaksi fraud**

Precision dan recall kelas fraud masing-masing sebesar 65%.
- Recall 65% berarti sekitar 35% transaksi fraud pada sampel tidak terdeteksi.
- Precision 65% berarti sebagian prediksi fraud merupakan transaksi normal yang salah ditandai sebagai anomali.

Dengan demikian, masih terdapat False Positive dan False Negative yang perlu diperhatikan sebelum model digunakan dalam sistem operasional.

## Anomaly Visualization

Notebook memvisualisasikan hasil deteksi pada ruang fitur `V14` dan `V17`.

Terdapat dua pendekatan visualisasi:
- Melatih Isolation Forest menggunakan dua fitur (`V14` dan `V17`) untuk menampilkan decision boundary dua dimensi.
- Menampilkan hasil prediksi model utama yang dilatih menggunakan seluruh fitur, kemudian memproyeksikan titik-titiknya pada plot `V14` dan `V17`.

Visualisasi dua dimensi hanya memperlihatkan sebagian ruang fitur. Hasil prediksi model utama tetap dipengaruhi oleh kombinasi seluruh fitur yang digunakan saat training.

## Key Insights

1. **Unsupervised anomaly detection memiliki potensi untuk fraud detection.** Model dilatih tanpa label `Class`, sedangkan label asli hanya digunakan untuk validasi hasil.
2. **Jumlah anomali yang terdeteksi tidak sama dengan jumlah prediksi yang benar.** Parameter `contamination` menghasilkan jumlah anomali yang mendekati proporsi yang ditentukan, tetapi tidak menjamin seluruh anomali tersebut merupakan fraud asli.
3. **Precision dan Recall perlu diperhatikan bersama.** Nilai keduanya sebesar 65% pada kelas fraud menunjukkan masih ada transaksi fraud yang lolos dan transaksi normal yang salah ditandai.
4. **Kombinasi fitur multidimensi penting.** Model utama menggunakan seluruh fitur, sehingga pola anomali tidak hanya ditentukan oleh dua fitur yang ditampilkan dalam visualisasi.

## Limitations & Next Steps

### Limitations

- Eksperimen menggunakan sampel data, bukan seluruh dataset.
- Terdapat 19 baris duplikat pada sampel dan notebook tidak menghapusnya.
- Model dievaluasi pada data yang sama dengan data training; belum ada pemisahan train-test atau evaluasi pada data baru.
- Nilai `contamination` ditentukan berdasarkan proporsi fraud pada label asli. Dalam penerapan unsupervised yang sesungguhnya, proporsi label fraud biasanya tidak diketahui.
- Performa deteksi fraud masih memiliki Precision dan Recall sebesar 65%.

### Next Steps

- Melakukan feature engineering berbasis waktu, misalnya frekuensi transaksi dalam satu jam terakhir.
- Mengoptimalkan threshold atau skor anomali untuk menyeimbangkan Precision dan Recall.
- Mengembangkan hybrid model dengan Isolation Forest sebagai filter awal, kemudian memvalidasi hasilnya menggunakan model supervised seperti XGBoost atau Random Forest.
- Menguji model pada data terpisah atau transaksi baru untuk memperoleh evaluasi generalisasi yang lebih representatif.

## End-to-End Workflow

1. Import libraries.
2. Load Credit Card Fraud Detection dataset.
3. Inspect dataset dimensions, missing values, and duplicates.
4. Create a sample containing all fraud transactions and 8,000 normal transactions.
5. Perform Exploratory Data Analysis and visualization.
6. Separate features and original labels.
7. Train Isolation Forest without using the `Class` label.
8. Convert anomaly predictions into binary labels.
9. Validate anomaly predictions using the original labels.
10. Evaluate using Confusion Matrix and Classification Report.
11. Visualize anomaly predictions in feature space.
12. Summarize insights, limitations, and possible improvements.

## Tech Stack

- Python
- Pandas
- NumPy
- Matplotlib
- Seaborn
- Scikit-learn
  - `IsolationForest`
  - `confusion_matrix`
  - `classification_report`

## Project Structure

```text
Day_45_Unsupervised_Model_Anomaly_Detection/
│
├── Day_45_Unsupervised_Model_Anomaly_Detection.ipynb
└── README.md
```

## Conclusion

Pada Day 45, saya mempelajari penerapan **Isolation Forest** untuk mendeteksi transaksi mencurigakan menggunakan pendekatan Unsupervised Learning.

Model dilatih tanpa menggunakan label asli dan menghasilkan 492 prediksi anomali. Setelah dibandingkan dengan label fraud yang sebenarnya, model memperoleh accuracy 96%, sedangkan precision dan recall pada kelas fraud masing-masing sebesar 65%.

Eksperimen ini menunjukkan bahwa Isolation Forest dapat digunakan sebagai pendekatan awal untuk anomaly detection. Namun, performa pada kelas fraud masih perlu ditingkatkan dan harus diuji pada data terpisah sebelum dipertimbangkan untuk penggunaan nyata.

---

## 👨‍💻 Author

**Arief Wicaksono**  
Aspiring AI Engineer | Data Science Student

---

⭐ *Learning Machine Learning one day at a time.*
