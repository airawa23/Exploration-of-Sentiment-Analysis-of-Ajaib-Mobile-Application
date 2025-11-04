# 📊 Eksplorasi Analisis Sentimen Ulasan Pengguna Aplikasi Investasi Ajaib

Oleh: Airlangga Bayu Taqwa (NRP: 5026221204)

Eksplorasi ini bertujuan untuk membangun dan membandingkan model klasifikasi sentimen terhadap 2000 ulasan Aplikasi Ajaib yang dikumpulkan dari Google Play Store. Fokus utama adalah mengidentifikasi model Machine Learning terbaik untuk membedakan sentimen Positif dan Negatif pengguna.

# 🛠️ Persyaratan Lingkungan

Proyek ini dibuat menggunakan Python 3.x. Untuk menjalankan notebook, install semua library yang diperlukan:

```bash
 pip install -r requirements.txt
```

File requirements.txt meliputi: pandas, scikit-learn, sastrawi, nltk, seaborn, matplotlib, wordcloud.

# ⚙️ Metodologi dan Alur Kerja
Total ulasan yang diproses: **1898** (setelah menghapus duplikat).

> <img width="599" height="472" alt="image" src="https://github.com/user-attachments/assets/2236a422-3187-408a-9984-8df8ddb60a76" />
https://www.researchgate.net/publication/324626996_Sentiment_Analysis_Using_Lexicon_and_Machine_Learning-Based_Approaches_A_Survey

## 1. **Persiapan Dataset**

- **Pengumpulan Data**: Ulasan aplikasi Ajaib diambil dari Google Play Store.

- **Pelabelan**: Untuk membuat label target (Sentiment_Rating_Label), kami mengadopsi skema klasifikasi Biner dari Rating:

<img width="790" height="490" alt="image" src="https://github.com/user-attachments/assets/4b052eba-4a58-4aa5-9997-b5391dde73e0" />

  - Positif: Rating 4 & 5.
  - Negatif: Rating 1, 2, & 3.
  - Justifikasi: Menggabungkan Rating 3 (Netral) ke Negatif dilakukan untuk menyederhanakan tugas dan mengatasi masalah imbalance pada kelas minoritas, dengan asumsi rating di bawah 4 merefleksikan ketidakpuasan atau masalah.

    | Label  | Frekuensi |
    | :--:   | :------:  |
    |Positif | 1170      |
    |Negatif | 728       |

## 2. **Tahap Pre-processing**

Data diproses melalui langkah-langkah:

 1. **Case Folding & Cleaning**: Menghapus tautan, simbol, angka, dan mengubah teks menjadi huruf kecil (Cleaned Review Text).

 2. **Normalisasi**: Mengubah kata tidak baku menjadi baku menggunakan kamuskatabaku.xlsx (Normalized Review Text).

 3. **Tokenisasi & Stopword Removal**: Memecah kalimat menjadi token dan menghapus stopwords Bahasa Indonesia.

 4. **Stemming**: Mengubah token menjadi kata dasar menggunakan Sastrawi (Stemming Data).

# 🚀 Hasil Pemodelan

Hasil evaluasi menunjukkan bahwa model ensemble unggul dalam menangani fitur TF-IDF, dengan kinerja terbaik dicapai oleh Extra Trees Classifier.

## A. Ekstraksi Fitur dan Baseline Model

- Ekstraksi Fitur: TF-IDF Vectorizer digunakan untuk mengubah kolom Stemming Data menjadi fitur (X), menghasilkan matriks dengan 1898 baris dan 2618 fitur.

- Baseline Model: Multinomial Naïve Bayes (MNB) digunakan sebagai baseline awal.

## B. Perbandingan Algoritma (K-Fold Cross Validation)

  | Algoritma           | Accuracy | precision | recall | F1-Score |
  |  :---               |  :---:   |  :---:    |  :---: | :------: |
  | SVM                 | 0.87     | 0.90      | 0.89   | 0.89     |
  | Naive Bayes         | 0.86     | 0.87      | 0.92   | 0.89     |
  | Logistic Regression | 0.86     | 0.86      | 0.90   | 0.89     |
  | Extra Trees         | 0.85     | 0.86      | 0.86   | 0.88     |
  | Random Forest       | 0.84     | 0.84      | 0.92   | 0.88     |
  | KNN                 | 0.62     | 0.62      | 0.76   | 0.76     |
  
## C. Rencana Peningkatan Kinerja (Future Work)

Untuk mengurangi 40 kesalahan (FN+FP) yang ada pada Extra Trees/SVM, rencana peningkatan difokuskan pada Feature Engineering yang lebih baik:

- Ekstraksi Fitur N-Grams: Menerapkan TF-IDF dengan Bi-grams (ngram_range=(1, 2) atau (1, 3)). N-grams diperlukan untuk menangkap konteks frasa (misalnya, "tidak cepat" $\rightarrow$ "tidak_cepat") yang sangat penting untuk memprediksi Negatif secara akurat.

- Hyperparameter Tuning: Menggunakan Grid Search atau Randomized Search untuk mengoptimalkan parameter kunci model terbaik (C pada SVM atau n_estimators/max_depth pada Extra Trees).
