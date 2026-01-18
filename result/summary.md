# Result Summary

Dokumen ini menyajikan ringkasan hasil penelitian
analisis sentimen menggunakan metode machine learning
dengan fokus pada penanganan ketidakseimbangan kelas data.

---

## Ringkasan Eksperimen

Penelitian ini menggunakan data sentimen media sosial
yang memiliki distribusi kelas tidak seimbang.
Untuk mengatasi permasalahan tersebut,
diterapkan metode **Synthetic Minority Over-sampling Technique (SMOTE)**
sebelum proses pelatihan model.

Model yang digunakan dalam eksperimen:
- Support Vector Machine (SVM)
- K-Nearest Neighbor (K-NN)

---

## Dampak Penerapan SMOTE

Penerapan SMOTE memberikan dampak signifikan terhadap
kualitas data dan performa model klasifikasi.

Manfaat utama yang diperoleh:
- Distribusi kelas sentimen menjadi lebih seimbang
- Model tidak lagi bias terhadap kelas mayoritas
- Peningkatan kemampuan model dalam mengenali kelas minoritas

---

## Performa Model

Berdasarkan hasil evaluasi:
- Model **SVM** menunjukkan performa yang lebih stabil
  dan konsisten pada data hasil oversampling.
- Model **K-NN** tetap memberikan performa yang kompetitif,
  namun sensitif terhadap distribusi dan jumlah data.

Secara keseluruhan, SVM menghasilkan hasil klasifikasi
yang lebih optimal dibandingkan K-NN.

---

## Insight Utama

- Penanganan class imbalance merupakan faktor penting
  dalam analisis sentimen berbasis machine learning.
- SMOTE efektif digunakan sebagai tahap preprocessing lanjutan
  sebelum pelatihan model.
- Pemilihan model yang tepat berpengaruh signifikan
  terhadap hasil akhir klasifikasi.

---

## Kesimpulan

Hasil penelitian menunjukkan bahwa kombinasi
penyeimbangan data menggunakan SMOTE
dan algoritma klasifikasi yang sesuai
mampu meningkatkan performa analisis sentimen secara signifikan.

Pendekatan ini dapat dijadikan referensi
untuk penelitian serupa pada data teks
dengan distribusi kelas yang tidak seimbang.
