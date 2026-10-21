# 🎓 Day 34 — Prediksi Kelulusan Siswa dengan Decision Tree & Random Forest

Project ini merupakan mini project klasifikasi untuk memprediksi status **lulus / tidak lulus** siswa berdasarkan informasi latar belakang dan faktor sosio-demografi.

Berbeda dari pendekatan yang menggunakan nilai ujian sebagai fitur prediksi, project ini sengaja **tidak menggunakan nilai matematika, membaca, dan menulis sebagai input model**. Target `lulus` dibuat terlebih dahulu dari rata-rata ketiga nilai tersebut, kemudian nilai mentah dan rata-ratanya dikeluarkan dari fitur untuk menghindari **target leakage**.

Model yang dibandingkan:

- **Decision Tree**
- **Random Forest**

Hasil eksperimen menunjukkan bahwa **Random Forest** memberikan performa lebih baik dibandingkan Decision Tree.

---

## 🎯 Tujuan Pembelajaran

- Membuat **target buatan** (`lulus`) berdasarkan aturan tertentu.
- Memahami konsep **target leakage** dan cara menghindarinya.
- Melakukan encoding pada fitur kategorikal menggunakan `LabelEncoder`.
- Membagi dataset menggunakan **Train-Test Split dengan Stratification**.
- Membangun model **Decision Tree** dan **Random Forest**.
- Mengevaluasi model menggunakan:
  - Accuracy
  - Precision
  - Recall
  - F1-Score
  - Confusion Matrix
- Membaca **feature importance** dari Random Forest.
- Mengubah hasil model menjadi insight yang relevan untuk konteks pendidikan.

---

## 📊 Tentang Dataset

Dataset yang digunakan adalah **Students Performance in Exams**.

**Sumber:** [Kaggle — spscientist/students-performance-in-exams](https://www.kaggle.com/datasets/spscientist/students-performance-in-exams)

Dataset berisi informasi demografis, latar belakang keluarga, kondisi makan siang, kursus persiapan ujian, serta nilai ujian siswa.

### Fitur Dataset

| Kolom | Keterangan |
|---|---|
| `gender` | Jenis kelamin siswa |
| `race/ethnicity` | Kelompok etnis, group A–E |
| `parental level of education` | Tingkat pendidikan orang tua |
| `lunch` | Tipe makan siang |
| `test preparation course` | Status kursus persiapan ujian |
| `math score` | Nilai matematika |
| `reading score` | Nilai membaca |
| `writing score` | Nilai menulis |

Dataset tidak memiliki kolom target `lulus`, sehingga target dibuat secara manual.

---

## 🏷️ Creating Target

Target `lulus` dibuat berdasarkan rata-rata tiga nilai ujian:

```python
df['rata_rata_nilai'] = (
    df['math score'] +
    df['reading score'] +
    df['writing score']
) / 3

df['lulus'] = np.where(
    df['rata_rata_nilai'] >= 60,
    1,
    0
)
```

Aturannya:

```text
Rata-rata nilai >= 60 → Lulus (1)
Rata-rata nilai < 60  → Tidak Lulus (0)
```

Target ini merupakan **target buatan**, bukan label kelulusan resmi dari sekolah.

---

## 🔎 Exploratory Data Analysis

### 1. Distribusi Lulus vs Tidak Lulus

Dataset memiliki distribusi kelas yang tidak seimbang.

- Kelas **Lulus (1)** lebih banyak.
- Kelas **Tidak Lulus (0)** lebih sedikit.
- Karena terdapat class imbalance, evaluasi tidak cukup hanya menggunakan accuracy.

Khususnya, **recall kelas Tidak Lulus** penting diperhatikan karena tujuan awal project adalah membantu mengidentifikasi siswa yang berisiko tidak lulus.

---

### 2. Pengaruh Test Preparation Course

Hasil eksplorasi menunjukkan perbedaan proporsi kelulusan antara siswa yang menyelesaikan kursus persiapan ujian dan siswa yang tidak mengikuti kursus.

Secara deskriptif:

- Siswa yang **completed** memiliki proporsi lulus sekitar **83–84%**.
- Siswa yang **none** memiliki proporsi lulus sekitar **65%**.

Hal ini menunjukkan adanya hubungan antara status kursus persiapan ujian dan target kelulusan pada dataset.

> **Catatan:** hubungan pada data observasional tidak otomatis berarti hubungan sebab-akibat.

---

### 3. Distribusi Rata-rata Nilai

Rata-rata nilai digunakan untuk membentuk target `lulus`.

Visualisasi boxplot menunjukkan pemisahan yang jelas:

- Kelompok **Tidak Lulus** memiliki median rata-rata nilai sekitar **52**.
- Kelompok **Lulus** memiliki median sekitar **73**.
- Batas target kelulusan berada pada nilai rata-rata **60**.

---

## 🧹 Preprocessing

### 1. Encoding Fitur Kategorikal

Model machine learning membutuhkan input numerik, sehingga lima fitur kategorikal diubah menggunakan `LabelEncoder`:

```python
cat_cols = [
    'gender',
    'race/ethnicity',
    'parental level of education',
    'lunch',
    'test preparation course'
]
```

Setiap kolom di-encode secara terpisah.

### 2. Menghindari Target Leakage

Bagian penting dari preprocessing adalah **tidak menggunakan nilai ujian sebagai fitur model**.

Kolom berikut tidak digunakan sebagai input:

```text
math score
reading score
writing score
rata_rata_nilai
```

Alasannya, target `lulus` dibuat langsung dari kolom-kolom tersebut.

Jika nilai ujian tetap dimasukkan ke model, model dapat "menebak" target dari informasi yang digunakan untuk membentuk target itu sendiri.

### 3. Features & Target

Model hanya menggunakan:

```text
gender
race/ethnicity
parental level of education
lunch
test preparation course
```

Target:

```text
lulus
```

### 4. Train-Test Split

Dataset dibagi menjadi:

- **80% training**
- **20% testing**
- `random_state=42`
- `stratify=y`

Stratification digunakan agar proporsi kelas `lulus` dan `tidak lulus` tetap relatif terjaga pada data training dan testing.

---

## 🌳 Modeling

Dua algoritma berbasis pohon digunakan untuk dibandingkan.

### 1. Decision Tree

Model:

```python
DecisionTreeClassifier(
    max_depth=5,
    random_state=42
)
```

Decision Tree membangun aturan keputusan berbentuk struktur pohon berdasarkan fitur yang tersedia.

---

### 2. Random Forest

Model:

```python
RandomForestClassifier(
    n_estimators=300,
    random_state=42
)
```

Random Forest menggabungkan banyak Decision Tree untuk menghasilkan prediksi yang lebih robust dibandingkan satu pohon.

---

## 📈 Model Evaluation

Hasil evaluasi pada notebook menunjukkan:

| Model | Accuracy | Status |
|---|---:|---|
| Decision Tree | ~65% | Baseline |
| Random Forest | ~71% | **Model Terbaik** |

Pada bagian kesimpulan notebook, performa juga dirangkum sebagai peningkatan accuracy dari sekitar **64.5% menjadi 71.5%**.

### Decision Tree

Classification report menunjukkan:

| Class | Precision | Recall | F1-Score |
|---|---:|---:|---:|
| Tidak Lulus (0) | 0.34 | 0.26 | 0.30 |
| Lulus (1) | 0.73 | 0.80 | 0.76 |

Model cukup baik mengenali siswa yang lulus, tetapi masih lemah dalam mendeteksi siswa yang tidak lulus.

---

### Random Forest

Classification report menunjukkan:

| Class | Precision | Recall | F1-Score |
|---|---:|---:|---:|
| Tidak Lulus (0) | 0.50 | 0.30 | 0.37 |
| Lulus (1) | 0.76 | 0.88 | 0.82 |

Random Forest meningkatkan performa pada kedua kelas dibandingkan Decision Tree.

Namun, **recall kelas Tidak Lulus masih rendah**, sehingga model belum cukup kuat untuk dijadikan sistem tunggal dalam menentukan siswa yang membutuhkan intervensi.

---

## 🧩 Confusion Matrix

### Decision Tree

Confusion matrix:

```text
TN = 15
FP = 42
FN = 29
TP = 114
```

Masalah terbesar adalah **False Positive** yang tinggi.

Artinya, cukup banyak siswa yang sebenarnya tidak lulus tetapi diprediksi sebagai lulus.

### Random Forest

Confusion matrix:

```text
TN = 17
FP = 40
FN = 17
TP = 126
```

Dibandingkan Decision Tree:

- True Positive meningkat dari **114 → 126**.
- False Negative turun dari **29 → 17**.
- True Negative meningkat dari **15 → 17**.
- False Positive turun dari **42 → 40**.

Dengan demikian, Random Forest memberikan prediksi yang lebih baik secara keseluruhan.

---

## 🌲 Feature Importance — Random Forest

Feature importance digunakan untuk melihat fitur mana yang paling banyak digunakan oleh Random Forest dalam membangun keputusan.

Hasil dari notebook:

| Feature | Importance |
|---|---:|
| `parental level of education` | 0.32 / 32% |
| `race/ethnicity` | 0.28 / 28% |
| `lunch` | 0.18 / 18% |
| `test preparation course` | 0.15 / 15% |
| `gender` | 0.08 / 8% |

Fitur dengan importance terbesar adalah:

> **`parental level of education`**

diikuti oleh:

> **`race/ethnicity`**

dan:

> **`lunch`**

### ⚠️ Interpretasi Feature Importance

Feature importance menunjukkan seberapa banyak fitur berkontribusi terhadap keputusan model, **bukan bukti hubungan sebab-akibat**.

Selain itu, karena dataset mengandung fitur sensitif seperti `race/ethnicity` dan `gender`, penggunaan model untuk keputusan pendidikan nyata harus mempertimbangkan aspek **fairness, bias, privacy, dan human oversight**.

---

## 💡 Insight Utama

### 1. Random Forest Mengungguli Decision Tree

Random Forest menghasilkan accuracy sekitar **71%**, lebih tinggi dibandingkan Decision Tree yang berada di sekitar **65%**.

Selain accuracy, Random Forest juga memiliki F1-score yang lebih baik pada kedua kelas.

### 2. Model Masih Lemah Mendeteksi Siswa Tidak Lulus

Recall kelas `Tidak Lulus` pada Random Forest hanya sekitar **30%**.

Ini merupakan masalah penting karena tujuan project adalah mendeteksi siswa yang berisiko tidak lulus.

### 3. Class Imbalance Mempengaruhi Model

Data testing memiliki sekitar:

```text
Tidak Lulus → 57 siswa
Lulus       → 143 siswa
```

Karena kelas lulus lebih dominan, model cenderung lebih baik dalam mengenali kelas mayoritas.

### 4. Test Preparation Memiliki Hubungan dengan Kelulusan

Siswa yang menyelesaikan kursus persiapan memiliki proporsi kelulusan lebih tinggi dibandingkan siswa yang tidak mengikuti kursus.

Insight ini dapat menjadi dasar eksplorasi lebih lanjut, tetapi tidak boleh langsung dianggap sebagai hubungan kausal.

### 5. Model Belum Siap Digunakan sebagai Sistem Keputusan Otomatis

Random Forest cukup baik sebagai eksperimen pembelajaran, tetapi performa pada kelas `Tidak Lulus` masih terlalu rendah untuk digunakan sebagai satu-satunya dasar keputusan terhadap siswa.

---

## 🏫 Rekomendasi dalam Konteks Pendidikan

Berdasarkan hasil eksperimen, beberapa kemungkinan tindakan yang dapat dieksplorasi:

1. **Intervensi dini**  
   Gunakan model sebagai alat bantu screening, bukan sebagai penentu kelulusan.

2. **Program persiapan ujian**  
   Eksplorasi lebih lanjut program tambahan bagi siswa yang membutuhkan dukungan akademik.

3. **Pendampingan siswa**  
   Gunakan hasil prediksi sebagai salah satu sinyal untuk menentukan siswa yang perlu mendapatkan perhatian lebih.

4. **Human-in-the-loop**  
   Keputusan akhir tetap perlu melibatkan guru atau pihak sekolah dan informasi akademik lain yang relevan.

---

## 🧠 Workflow

```text
Students Performance Dataset
            │
            ▼
      Exploratory Data Analysis
            │
            ▼
   Create Target "lulus"
   Average Score >= 60
            │
            ▼
    Remove Score Features
     to Prevent Leakage
            │
            ▼
      Label Encoding
            │
            ▼
 Train-Test Split + Stratify
            │
            ▼
    ┌───────┴────────┐
    ▼                ▼
Decision Tree    Random Forest
    │                │
    └───────┬────────┘
            ▼
     Model Evaluation
            │
            ├── Accuracy
            ├── Precision
            ├── Recall
            ├── F1-Score
            └── Confusion Matrix
            │
            ▼
     Feature Importance
            │
            ▼
       Final Insights
```

---

## 🛠️ Tech Stack

- Python
- Pandas
- NumPy
- Matplotlib
- Seaborn
- Scikit-learn
- Jupyter Notebook

### Library Utama

```python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns

from sklearn.model_selection import train_test_split
from sklearn.preprocessing import LabelEncoder
from sklearn.tree import DecisionTreeClassifier
from sklearn.ensemble import RandomForestClassifier
from sklearn.metrics import (
    confusion_matrix,
    accuracy_score,
    classification_report,
    ConfusionMatrixDisplay
)
```

---

## ⚠️ Limitations

Beberapa keterbatasan project:

- Target `lulus` merupakan **target buatan** berdasarkan threshold rata-rata nilai ≥ 60.
- Dataset memiliki **class imbalance**.
- Fitur model hanya berasal dari latar belakang siswa sehingga informasi akademik langsung tidak digunakan.
- Recall kelas `Tidak Lulus` masih rendah.
- Belum dilakukan **cross-validation**.
- Belum dilakukan **hyperparameter tuning** secara sistematis.
- Belum dilakukan evaluasi dengan metrik yang secara khusus mengoptimalkan deteksi kelas minoritas.
- `LabelEncoder` digunakan pada fitur kategorikal; untuk workflow production, preprocessing kategorikal dapat dirancang menggunakan pipeline yang lebih robust.
- Feature importance tidak dapat diartikan sebagai hubungan kausal.
- Fitur sensitif seperti `race/ethnicity` dan `gender` membutuhkan evaluasi fairness sebelum digunakan dalam konteks nyata.
- Belum dilakukan deployment, monitoring, atau retraining.

---

## 🚀 Next Improvement

Beberapa pengembangan yang dapat dilakukan:

### 1. Cross-Validation

Gunakan K-Fold Cross-Validation untuk mendapatkan estimasi performa yang lebih robust.

### 2. Hyperparameter Tuning

Optimalkan parameter Random Forest seperti:

```text
n_estimators
max_depth
min_samples_split
min_samples_leaf
max_features
```

### 3. Menangani Class Imbalance

Eksperimen dapat dilakukan dengan:

- `class_weight`
- resampling
- SMOTE

Tujuannya bukan sekadar meningkatkan accuracy, tetapi terutama meningkatkan kemampuan mendeteksi kelas `Tidak Lulus`.

### 4. Evaluasi dengan Metric yang Tepat

Selain accuracy, gunakan:

- Recall
- Precision
- F1-Score
- ROC-AUC
- PR-AUC

Terutama fokus pada performa kelas `Tidak Lulus`.

### 5. Fairness Analysis

Karena beberapa fitur berkaitan dengan karakteristik demografis, evaluasi fairness perlu dilakukan sebelum model dipakai untuk keputusan pendidikan.

---

## 📌 Kesimpulan

Day 34 menunjukkan bagaimana **Decision Tree** dan **Random Forest** dapat digunakan untuk memprediksi status kelulusan siswa berdasarkan faktor latar belakang.

Random Forest menjadi model terbaik dalam eksperimen ini dengan accuracy sekitar **71%**, dibandingkan Decision Tree sekitar **65%**.

Namun, model masih memiliki kelemahan penting dalam mendeteksi siswa yang tidak lulus. Recall kelas `Tidak Lulus` hanya sekitar **30%** pada Random Forest.

Karena itu, model lebih tepat diposisikan sebagai **alat bantu screening dan analisis**, bukan sebagai sistem otomatis untuk menentukan kelulusan siswa.

Project ini memperkuat pemahaman mengenai:

> **Target Engineering → Target Leakage → Encoding → Tree-Based Classification → Model Evaluation → Class Imbalance → Feature Importance → Responsible ML**

---

## 👤 Author

**Arief Wicaksono**  
*Aspiring AI Engineer | Data Science Student*
