# Modeling

Tahap modeling bertujuan untuk membangun model klasifikasi sentimen
berdasarkan data yang telah melalui proses preprocessing dan pelabelan
sentimen otomatis.

Fokus utama pada tahap ini adalah penanganan ketidakseimbangan kelas
(class imbalance) menggunakan metode **Synthetic Minority Over-sampling
Technique (SMOTE)** sebelum proses pelatihan model.

Model yang digunakan dalam penelitian ini meliputi:
- Support Vector Machine (SVM)
- K-Nearest Neighbor (K-NN)

---

## Permasalahan Class Imbalance

Distribusi kelas sentimen pada data awal tidak seimbang,
di mana kelas mayoritas mendominasi dataset.
Kondisi ini dapat menyebabkan model bias dan menurunkan
kemampuan dalam mengenali kelas minoritas.

Untuk mengatasi permasalahan tersebut,
diterapkan metode oversampling SMOTE.

---

## Penerapan SMOTE

SMOTE digunakan untuk menghasilkan sampel sintetis
pada kelas minoritas sehingga distribusi kelas menjadi lebih seimbang.

```python
from imblearn.over_sampling import SMOTE

smote = SMOTE(random_state=42)
X_resampled, y_resampled = smote.fit_resample(X, y)
```
