# Day 35 — Klasifikasi Korban Titanic dengan Decision Tree & Visualisasinya

## 🎯 Objective

Project ini merupakan latihan **Binary Classification** menggunakan **Decision Tree** pada Titanic Dataset.

Fokus utama:
- Melatih `DecisionTreeClassifier`.
- Memvisualisasikan struktur Decision Tree menggunakan `plot_tree`.
- Membandingkan akurasi train vs test untuk memahami overfitting.
- Menganalisis pengaruh `max_depth` terhadap performa.
- Mengevaluasi model dengan accuracy, classification report, dan confusion matrix.
- Menganalisis feature importance.

## 📊 Dataset

- **Source:** [Kaggle — yasserh/titanic-dataset](https://www.kaggle.com/datasets/yasserh/titanic-dataset)
- **Rows:** 891
- **Columns awal:** 12
- **Target:** `Survived` — `0` = tidak selamat, `1` = selamat.

### Fitur yang digunakan

| Feature | Keterangan |
|---|---|
| `Pclass` | Kelas tiket (1/2/3) |
| `Sex` | Jenis kelamin |
| `Age` | Usia |
| `SibSp` | Jumlah saudara/pasangan di kapal |
| `Parch` | Jumlah orang tua/anak di kapal |
| `Fare` | Harga tiket |
| `Embarked` | Pelabuhan naik (C/Q/S) |

`PassengerId`, `Name`, `Ticket`, dan `Cabin` tidak digunakan sebagai fitur model.

## 🔎 Exploratory Data Analysis

Dataset memiliki:
- `Age`: 177 missing values
- `Cabin`: 687 missing values
- `Embarked`: 2 missing values
- Total missing values: 866

Notebook mengeksplorasi tingkat keselamatan berdasarkan jenis kelamin dan kelas tiket.

Salah satu temuan utama: sekitar **74% penumpang perempuan selamat**, sedangkan sekitar **81% penumpang laki-laki tidak selamat**.

## 🧹 Data Cleaning & Feature Engineering

### Missing Value Imputation

- `Age` → median
- `Embarked` → modus
- `Fare` → median

`Cabin` tidak digunakan karena memiliki missing value yang sangat banyak.

### Encoding

```text
Sex:
male → 0
female → 1

Embarked:
S → 0
C → 1
Q → 2
```

Setelah preprocessing:
- **891 rows**
- **7 features + 1 target**
- **0 missing values**

## ✂️ Train-Test Split

```python
train_test_split(
    X,
    y,
    test_size=0.2,
    random_state=42,
    stratify=y
)
```

| Dataset | Shape |
|---|---:|
| Train | `(712, 7)` |
| Test | `(179, 7)` |

## 🌳 Decision Tree Modeling

Model awal:

```python
DecisionTreeClassifier(
    max_depth=3,
    random_state=42
)
```

### Initial Performance

| Metric | Score |
|---|---:|
| Train Accuracy | 83% |
| Test Accuracy | 79% |

Decision Tree kemudian divisualisasikan menggunakan `plot_tree` untuk melihat aturan keputusan yang terbentuk.

## 📈 Analisis `max_depth` & Overfitting

Notebook menguji `max_depth` dari 1 hingga 20.

| max_depth | Train Accuracy | Test Accuracy |
|---:|---:|---:|
| 1 | 0.79 | 0.78 |
| 2 | 0.80 | 0.76 |
| 3 | 0.83 | 0.79 |
| 4 | 0.84 | 0.79 |
| 5 | 0.87 | 0.78 |
| 6 | 0.89 | 0.80 |
| 7 | 0.89 | 0.81 |
| 8 | 0.92 | 0.81 |
| 9 | 0.93 | 0.82 |
| 10 | 0.94 | 0.80 |
| 14 | 0.96 | 0.83 |
| 15 | 0.96 | 0.83 |
| 16 | 0.97 | **0.84** |
| 17 | 0.97 | 0.83 |
| 18 | 0.97 | 0.82 |
| 19 | 0.98 | 0.83 |
| 20 | 0.98 | 0.83 |

`max_depth=16` menghasilkan **akurasi test terbaik sebesar 84%**.

Train accuracy terus meningkat hingga sekitar **98%**, sementara test accuracy cenderung mendatar atau menurun setelah titik tertentu. Ini menunjukkan indikasi **overfitting**.

## 🏆 Best Model

```python
DecisionTreeClassifier(
    max_depth=16,
    random_state=42
)
```

| Metric | Score |
|---|---:|
| Train Accuracy | **97%** |
| Test Accuracy | **84%** |

Terdapat gap sekitar **13 percentage points** antara train dan test accuracy.

## 📋 Classification Report

| Class | Precision | Recall | F1-Score | Support |
|---|---:|---:|---:|---:|
| Tidak (0) | 0.85 | 0.89 | 0.87 | 110 |
| Selamat (1) | 0.81 | 0.75 | 0.78 | 69 |
| **Accuracy** | | | **0.84** | **179** |
| Macro Avg | 0.83 | 0.82 | 0.83 | 179 |
| Weighted Avg | 0.84 | 0.84 | 0.84 | 179 |

Model lebih baik dalam mengenali kelas **Tidak Selamat (0)** dengan recall **89%**, dibandingkan kelas **Selamat (1)** dengan recall **75%**.

## 🧮 Confusion Matrix

| | Predicted Tidak | Predicted Selamat |
|---|---:|---:|
| **Actual Tidak** | 98 | 12 |
| **Actual Selamat** | 17 | 52 |

- **TN = 98**
- **FP = 12**
- **FN = 17**
- **TP = 52**

## ⭐ Feature Importance

Notebook juga menggunakan `feature_importances_` dari Decision Tree dan memvisualisasikannya dengan horizontal bar chart untuk melihat kontribusi relatif setiap fitur terhadap keputusan model.

> Feature importance menunjukkan kontribusi relatif fitur terhadap keputusan model, bukan hubungan sebab-akibat.

## 🔄 Machine Learning Workflow

```text
Titanic Dataset
      ↓
Initial Exploration
      ↓
Missing Value Analysis
      ↓
Data Cleaning
      ↓
Feature Selection
      ↓
Categorical Encoding
      ↓
Train-Test Split (80:20)
      ↓
Decision Tree (max_depth=3)
      ↓
Decision Tree Visualization
      ↓
Experiment max_depth 1–20
      ↓
Select Best max_depth = 16
      ↓
Final Evaluation
      ↓
Classification Report
      ↓
Confusion Matrix
      ↓
Feature Importance
      ↓
Overfitting Analysis
```

## 🛠️ Tech Stack

- Python
- Pandas
- NumPy
- Matplotlib
- Seaborn
- Scikit-learn
- Google Colab
- Jupyter Notebook

## 💡 Key Takeaways

1. **Decision Tree mudah divisualisasikan**, sehingga aturan keputusan model lebih mudah dipahami.
2. `max_depth` sangat berpengaruh terhadap kompleksitas dan generalisasi model.
3. `max_depth=16` memberikan **84% test accuracy**, tertinggi pada eksperimen depth 1–20.
4. Gap **97% train vs 84% test** menunjukkan indikasi overfitting.
5. Model lebih baik mendeteksi kelas **Tidak Selamat** dibanding kelas **Selamat**.
6. Confusion matrix membantu memahami jenis kesalahan prediksi.
7. Feature importance membantu memahami fitur yang paling banyak digunakan model dalam pengambilan keputusan.

## ⚠️ Limitations & Possible Improvements

- Melakukan **hyperparameter tuning** yang lebih sistematis.
- Menggunakan **Cross-Validation**.
- Membandingkan dengan **Random Forest**, **Gradient Boosting**, atau **Logistic Regression**.
- Menambahkan feature engineering.
- Menggunakan metrik tambahan seperti ROC-AUC.
- Mengontrol kompleksitas model untuk mengurangi overfitting.

## 📌 Conclusion

Project Day 35 menunjukkan bagaimana **Decision Tree** dapat digunakan untuk memprediksi keselamatan penumpang Titanic sekaligus memberikan visualisasi aturan keputusan model.

Model awal dengan `max_depth=3` memperoleh **79% test accuracy**. Setelah menguji `max_depth` 1–20, `max_depth=16` memberikan performa test terbaik sebesar **84%**.

Walaupun performanya cukup baik, gap antara **97% train accuracy** dan **84% test accuracy** menunjukkan adanya indikasi overfitting. Eksperimen ini memperkuat pemahaman mengenai hubungan antara **model complexity, visualization, feature importance, accuracy, dan overfitting** pada Decision Tree.

## 👨‍💻 Author

**Arief Wicaksono**  
Aspiring AI Engineer | Data Science Student
