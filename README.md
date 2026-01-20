# Analisis Sentimen Investasi di Twitter  
## Menggunakan Support Vector Machine dan K-Nearest Neighbor

## Latar Belakang
Media sosial Twitter menjadi sumber data penting untuk memahami opini publik
terhadap isu investasi. Namun, karakteristik bahasa yang informal serta
ketidakseimbangan kelas sentimen menjadi tantangan dalam proses klasifikasi.

Penelitian ini mengusulkan pendekatan klasifikasi sentimen menggunakan
algoritma **Support Vector Machine (SVM)** dan **K-Nearest Neighbor (K-NN)**.

---

## Tujuan Penelitian
1. Mengklasifikasikan sentimen publik terkait isu investasi di Twitter.
2. Membandingkan performa algoritma SVM dan K-NN.
3. Mengevaluasi dampak preprocessing dan penyeimbangan data.

---

## Metodologi Penelitian
Tahapan penelitian meliputi:
1. Pengumpulan data Twitter berbasis kata kunci investasi.
2. Text preprocessing:
   - Cleaning
   - Case folding
   - Normalisasi Data
   - Stopword removal
   - Translate Bahasa Indonesia - Bahasa Inggris
3. Pelabelan sentimen otomatis menggunakan VADER.
4. Penanganan class imbalance menggunakan SMOTE.
5. Pelatihan dan pengujian model:
   - Support Vector Machine
   - K-Nearest Neighbor
6. Evaluasi menggunakan confusion matrix dan accuracy bar.

Dapat dilihat dengan link dibawah ini:
### [Pipeline](src/pipeline.md)
### [Preprocessing](src/preprocessing.md)
### [Modeling](src/modeling.md)
### [Evaluation](src/evaluation.md)
---

## Ringkasan Hasil
- Kedua algoritma menghasilkan performa klasifikasi yang sangat tinggi.
- SVM menunjukkan stabilitas dan akurasi yang lebih konsisten.
- K-NN tetap kompetitif namun sensitif terhadap distribusi data.

### [Result](result/summary.md)
---

## Struktur Repository
```text
sentiment-analysis-twitter-svm-knn/
├── README.md
├── data/
├── src/
└── result/
