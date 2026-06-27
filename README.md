# CBR-Narkotika: Case-Based Reasoning untuk Putusan Pidana Narkotika & Psikotropika

**Mata Kuliah:** Penalaran Komputer — Semester Genap 2025/2026  
**Program Studi:** Teknik Informatika, Universitas Muhammadiyah Malang  
**NIM:** 202310370311358  
**SubCPMK-3:** Implementasi siklus Case-Based Reasoning menggunakan dataset putusan Mahkamah Agung RI

---

## Deskripsi Proyek

Sistem **Case-Based Reasoning (CBR)** berbasis Python untuk mendukung analisis putusan pengadilan pidana khusus **Narkotika & Psikotropika**. Data bersumber dari [Direktori Putusan Mahkamah Agung RI](https://putusan3.mahkamahagung.go.id/).

Sistem mengimplementasikan siklus CBR penuh:
1. **Case Base Building** — scraping & preprocessing ≥30 putusan
2. **Case Representation** — ekstraksi metadata & fitur teks
3. **Case Retrieval** — TF-IDF + SVM untuk menemukan kasus serupa
4. **Solution Reuse** — prediksi amar putusan berdasarkan kasus terdekat
5. **Model Evaluation** — Accuracy, Precision, Recall, F1-score

---

## Struktur Repository

```
cbr-narkotika/
│
├── data/
│   ├── raw/                    # Teks putusan hasil scraping (case_001.txt, ...)
│   ├── processed/
│   │   └── cases.csv           # Dataset terstruktur hasil representasi
│   ├── eval/
│   │   ├── queries.json        # Query uji + ground-truth
│   │   ├── retrieval_metrics.csv
│   │   └── prediction_metrics.csv
│   └── results/
│       └── predictions.csv     # Hasil prediksi solusi
│
├── notebooks/
│   ├── 01_case_base.ipynb      # Tahap 1: Scraping & Preprocessing
│   ├── 02_representation.ipynb # Tahap 2: Metadata & Feature Engineering
│   ├── 03_retrieval.ipynb      # Tahap 3: TF-IDF + SVM Retrieval
│   ├── 04_solution_reuse.ipynb # Tahap 4: Prediksi Solusi
│   └── 05_evaluation.ipynb     # Tahap 5: Evaluasi Model
│
├── logs/
│   └── cleaning.log            # Log preprocessing (opsional)
│
├── requirements.txt
└── README.md
```

---

## Instalasi

### 1. Clone repository

```bash
git clone https://github.com/<username>/cbr-narkotika.git
cd cbr-narkotika
```

### 2. Buat virtual environment (opsional tapi disarankan)

```bash
python -m venv venv
source venv/bin/activate        # Linux/Mac
venv\Scripts\activate           # Windows
```

### 3. Install dependencies

```bash
pip install -r requirements.txt
```

### 4. Jalankan di Google Colab (alternatif)

Upload seluruh folder ke Google Drive, lalu buka setiap notebook di Colab.  
Jalankan cell pertama pada setiap notebook untuk install otomatis:

```python
!pip install -r requirements.txt
```

---

## Cara Menjalankan Pipeline End-to-End

Jalankan notebook **secara berurutan**:

```
01 → 02 → 03 → 04 → 05
```

### Tahap 1 — Scraping & Preprocessing
```bash
jupyter notebook notebooks/01_case_base.ipynb
```
Output: `data/raw/case_001.txt` ... `case_030.txt`

### Tahap 2 — Case Representation
```bash
jupyter notebook notebooks/02_representation.ipynb
```
Output: `data/processed/cases.csv`

### Tahap 3 — Case Retrieval
```bash
jupyter notebook notebooks/03_retrieval.ipynb
```
Output: model TF-IDF + SVM, fungsi `retrieve(query, k=5)`

### Tahap 4 — Solution Reuse
```bash
jupyter notebook notebooks/04_solution_reuse.ipynb
```
Output: `data/results/predictions.csv`

### Tahap 5 — Evaluasi Model
```bash
jupyter notebook notebooks/05_evaluation.ipynb
```
Output: `data/eval/retrieval_metrics.csv`, `data/eval/prediction_metrics.csv`

---

## Contoh Penggunaan Fungsi Retrieval

```python
from notebooks.03_retrieval import retrieve, predict_outcome

# Cari 5 kasus paling mirip
query = "terdakwa kedapatan membawa sabu-sabu seberat 1 gram"
top_cases = retrieve(query, k=5)
print(top_cases)
# Output: ['case_012', 'case_007', 'case_023', 'case_018', 'case_003']

# Prediksi amar putusan
solusi = predict_outcome(query)
print(solusi)
# Output: "Terdakwa terbukti melanggar Pasal 112 UU No.35 Tahun 2009 ..."
```

---

## Dependencies Utama

| Library | Versi | Kegunaan |
|---|---|---|
| pandas | ≥2.0 | Manipulasi data |
| scikit-learn | ≥1.3 | TF-IDF, SVM, metrics |
| requests + BeautifulSoup4 | latest | Scraping putusan |
| pdfminer.six | latest | Konversi PDF → teks |
| nltk | ≥3.8 | Tokenisasi, stopwords |
| matplotlib + seaborn | latest | Visualisasi evaluasi |
| jupyter | latest | Notebook environment |

---

## Domain & Data

- **Jenis perkara:** Pidana Khusus — Narkotika & Psikotropika  
- **Sumber data:** [putusan3.mahkamahagung.go.id](https://putusan3.mahkamahagung.go.id/)  
- **Volume:** ≥30 dokumen putusan  
- **Label:** Amar putusan (bebas / bersalah + pasal yang dikenakan)

---

## Hasil Evaluasi (Ringkasan)

| Model | Accuracy | Precision | Recall | F1-Score |
|---|---|---|---|---|
| TF-IDF + Cosine Similarity | - | - | - | - |
| TF-IDF + SVM | - | - | - | - |

*Tabel akan diisi setelah eksperimen selesai (Tahap 5).*

---

## Lisensi

Proyek ini dibuat untuk keperluan akademik. Data putusan bersumber dari domain publik Mahkamah Agung RI.
