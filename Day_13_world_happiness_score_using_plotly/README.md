# 📊 Day 13 — Interactive Visualization with Plotly

Project ini merupakan implementasi **interactive data visualization** menggunakan **Plotly Express** untuk mengeksplorasi **World Happiness Report 2019**.

Berbeda dari visualisasi statis sebelumnya, Plotly memungkinkan pengguna untuk melakukan **hover, zoom, dan eksplorasi data secara langsung** pada grafik.

Project ini digunakan untuk memahami bagaimana visualisasi interaktif dapat membantu menemukan pola, hubungan antarvariabel, serta insight dari dataset multidimensi.

---

## 🎯 Project Objectives

Tujuan utama project ini adalah:

* Mengenal **Plotly Express** untuk membuat visualisasi interaktif.
* Membuat interactive scatter plot.
* Membuat horizontal bar chart.
* Membuat world choropleth map.
* Mengeksplorasi hubungan antar faktor yang berkaitan dengan happiness score.
* Memahami penggunaan hover information pada visualisasi.
* Menghasilkan insight dari data multidimensi.

---

## 📂 Dataset

Dataset yang digunakan adalah:

**World Happiness Report 2019**

📌 Source: [Kaggle — World Happiness Report](https://www.kaggle.com/datasets/unsdsn/world-happiness)

Dataset terdiri dari:

* **156 negara**
* **9 kolom**

### Important Features

| Feature                        | Description                   |
| ------------------------------ | ----------------------------- |
| `Overall rank`                 | Peringkat kebahagiaan         |
| `Country or region`            | Nama negara                   |
| `Score`                        | Happiness Score               |
| `GDP per capita`               | Kontribusi GDP per kapita     |
| `Social support`               | Dukungan sosial               |
| `Healthy life expectancy`      | Harapan hidup sehat           |
| `Freedom to make life choices` | Kebebasan memilih jalan hidup |
| `Generosity`                   | Kedermawanan                  |
| `Perceptions of corruption`    | Persepsi terhadap korupsi     |

---

## 🛠️ Tech Stack

* **Python**
* **Pandas** — Data manipulation & analysis
* **Plotly Express** — Interactive visualization
* **Plotly I/O** — Plot rendering configuration

---

## 🔎 Analysis Workflow

```text
World Happiness Report 2019
          │
          ▼
     Data Loading
          │
          ▼
   Data Exploration
          │
          ├── Data Structure
          ├── Missing Values
          └── Descriptive Statistics
          │
          ▼
 Interactive Visualization
          │
          ├── GDP vs Happiness Score
          ├── Top 10 Countries
          ├── World Happiness Map
          └── Corruption vs Freedom
          │
          ▼
     Insights & Findings
```

---

# 📈 1. Interactive Scatter Plot — GDP vs Happiness Score

Scatter plot digunakan untuk melihat hubungan antara:

* `GDP per capita`
* `Score`

Visualisasi juga menggunakan:

* **Size** → Social Support
* **Color** → Healthy Life Expectancy
* **Hover** → Country Name dan informasi terkait

### Insight

Terlihat adanya **tren positif** antara GDP per kapita dan Happiness Score.

Secara umum, negara dengan GDP per kapita lebih tinggi cenderung memiliki Happiness Score yang lebih tinggi.

Visualisasi interaktif juga memungkinkan pengguna melihat informasi setiap negara secara langsung melalui fitur **hover**.

> ⚠️ Hubungan ini menunjukkan korelasi/pola, bukan bukti hubungan sebab-akibat.

---

# 🏆 2. Bar Chart — Top 10 Happiest Countries

10 negara dengan ranking kebahagiaan tertinggi divisualisasikan menggunakan **horizontal bar chart**.

### Insight

* Negara-negara Eropa, khususnya negara **Nordik**, mendominasi kelompok dengan Happiness Score tertinggi.
* Perbedaan skor antar negara dalam Top 10 relatif kecil.
* Hal ini menunjukkan bahwa kelompok negara tersebut memiliki tingkat Happiness Score yang relatif berdekatan.

---

# 🌍 3. Choropleth — World Happiness Score

**Choropleth** digunakan untuk memvisualisasikan Happiness Score berdasarkan lokasi geografis negara.

Semakin tinggi nilai Happiness Score, semakin tinggi intensitas warna pada peta.

Visualisasi juga menyediakan informasi melalui hover:

* Nama negara
* Happiness Score
* Ranking

### Insight

Peta menunjukkan adanya pola geografis dalam distribusi Happiness Score.

Negara-negara dengan skor tinggi banyak ditemukan di wilayah Eropa, terutama negara-negara Nordik, sedangkan sebagian besar negara di Afrika berada pada kelompok skor yang lebih rendah.

---

# 🔍 4. Corruption vs Freedom

Visualisasi kedua menggunakan scatter plot untuk melihat hubungan antara:

* `Perceptions of corruption`
* `Freedom to make life choices`

Warna menunjukkan **Happiness Score**, sedangkan ukuran titik menunjukkan **Social Support**.

### Insight

Visualisasi menunjukkan bahwa faktor non-ekonomi juga memiliki pola yang berkaitan dengan Happiness Score.

Terdapat beberapa negara yang menjadi perhatian untuk analisis lebih lanjut karena memiliki kombinasi nilai freedom dan perceptions of corruption yang relatif tinggi tetapi Happiness Score yang rendah.

Contohnya adalah **Rwanda dan Somalia**.

Hal ini menunjukkan bahwa Happiness Score tidak dapat dijelaskan hanya oleh satu faktor dan perlu dilihat secara multidimensi.

---

# 💡 Key Insights

Beberapa insight utama dari project:

### 1. GDP & Happiness

Terdapat pola positif antara **GDP per capita** dan Happiness Score.

### 2. Social Support & Health

Social Support dan Healthy Life Expectancy menjadi bagian penting dalam eksplorasi faktor yang berkaitan dengan Happiness Score.

### 3. Geographic Pattern

Negara-negara Nordik mendominasi kelompok Happiness Score tertinggi.

### 4. Multi-dimensional Analysis

Happiness merupakan fenomena multidimensi sehingga perlu dianalisis menggunakan beberapa faktor sekaligus.

### 5. Interactive Visualization

Plotly memungkinkan pengguna mengeksplorasi data secara langsung melalui:

```text
Hover → Zoom → Pan → Explore
```

Hal ini membuat Plotly cocok untuk **exploratory analysis, presentation, dan dashboard**.

---

# ⚠️ Data & Visualization Pitfalls

### Choropleth menggunakan nama negara

Visualisasi peta menggunakan:

```python
locationmode="country names"
```

Karena itu, nama negara yang tidak standar dapat menyebabkan negara tidak muncul pada peta.

Untuk dataset yang lebih kompleks, penggunaan **ISO country code** dapat menjadi alternatif yang lebih robust.

### Korelasi ≠ Kausalitas

Hubungan GDP dan Happiness Score yang terlihat pada scatter plot tidak berarti bahwa GDP secara langsung menyebabkan peningkatan happiness.

Visualisasi menunjukkan **association**, bukan causal relationship.

---

# 📚 Learning Outcomes

Melalui project ini, beberapa konsep yang dipraktikkan:

* Interactive Data Visualization
* Plotly Express
* Scatter Plot
* Horizontal Bar Chart
* Choropleth Map
* Hover Information
* Multi-dimensional Visualization
* Data Exploration
* Correlation Awareness
* Geographic Data Visualization
* Data & Visualization Pitfalls
* Insight Generation

---

# 🚀 Next Step

Project ini menjadi lanjutan dari visualisasi statis menuju **interactive visualization**.

```text
Static Visualization
        ↓
Interactive Visualization
        ↓
Dashboard
        ↓
Data Storytelling
        ↓
Decision Support
```

Kemampuan membuat visualisasi interaktif menjadi fondasi penting sebelum masuk ke tahap **dashboard dan data-driven decision making**.

---

## 📁 Project Structure

```text
Day_13_World_happiness_report_using_plotly/
│
├── Day_13_World_happiness_report_using_plotly.ipynb
└── README.md
```

---

## 👤 Author

**Arief Wicaksono**

Aspiring AI Engineer | Data Science Student

---

⭐ If you find this project useful, feel free to star the repository!
