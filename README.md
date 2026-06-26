# Tugas Besar Natural Language Processing (NLP)

Repositori ini berisikan keseluruhan alur kerja untuk proyek Tugas Besar Mata Kuliah Natural Language Processing (NLP). Adapun proyek ini bertujuan untuk membangun sistem klasifikasi teks berbasis *Machine Learning* dan *Deep Learning* yang dapat mengategorikan unggahan dari platform X ke dalam tiga kategori utama yaitu **Ekonomi, Teknologi, dan Entertainment**.

## 👥 Anggota Kelompok (Kelompok 6)
1. **Fadlullah Hasan** - F1D02310008
2. **Datu Reksa Hamza Putra** - F1D02310045
3. **Kanda Rifqi Alfaz** - F1D02310064

---

## 📂 Informasi Dataset & Proses Translasi (Penting)

Sebelum masuk ke tahap eksplorasi dan prapemrosesan, **seluruh dataset yang digunakan dalam proyek diterjemahkan terlebih dahulu ke dalam Bahasa Indonesia** menggunakan *Machine Translation*. Hal ini bertujuan agar model difokuskan untuk memahami dan mengklasifikasikan konteks dengan teks berbahasa Indonesia, khususnya menggunakan model pra-latih *IndoBERT*.

Kami menggunakan dua sumber dataset utama (*Corpus*):

### 1. Corpus 1: Dataset Kaggle (Training & Validation)
* **Sumber:** Kaggle
* **Deskripsi:** Dataset skala besar yang awalnya berbahasa Inggris. Kami membuang (drop) kategori 'Medical' yang ada dalam dataset sehingga tersisa 3 kategori utama yaitu Ekonomi, Teknologi, Entertainment.
* **Jumlah Data:** ~48.704 data (setelah dibersihkan dari total awal 65.535 data).
* **Penggunaan:** Digunakan murni untuk *training* dan memvalidasi model.

### 2. Corpus 2: Dataset Hasil Scraping X / Twitter (Testing Data)
* **Sumber:** Hasil *scraping* manual menggunakan *Instant Data Scraper* dari platform X.
* **Jumlah Data:** 300 data.
* **Pembagian Tugas Scraping:** Dataset ini dikumpulkan secara merata oleh masing-masing anggota kelompok dengan rincian:
  * **Kanda Rifqi Alfaz (100 data):** Kategori **Ekonomi**.
  * **Fadlullah Hasan (100 data):** Kategori **Teknologi**.
  * **Datu Reksa Hamza Putra (100 data):** Kategori **Entertainment**.
* **Penggunaan:** Digunakan murni sebagai *testing data* untuk menguji seberapa baik model memprediksi unggahan dunia nyata. Data ini juga melalui tahap translasi jika terdapat unggahan dengan bahasa Inggris.

---

## ⚙️ Skenario Pemodelan & Variasi Ukuran Korpus

Untuk membandingkan performa pendekatan NLP dari algoritma klasik hingga *Large Language Models*, kami menggunakan 4 skenario pemodelan. **Setiap skenario diuji dan dijalankan secara menyeluruh menggunakan berbagai variasi ukuran korpus latih yaitu 10k, 20k, 30k, 40k, hingga Full Data** untuk mengamati pengaruh skala data terhadap performa metrik model:

1. **Skenario 1:** TF-IDF + Naive Bayes (Klasik probabilistik)
2. **Skenario 2:** TF-IDF + Support Vector Machine (SVM) (Margin-classifier)
3. **Skenario 3:** FastText + SVM (Word Embedding + Margin-classifier)
4. **Skenario 4:** IndoBERT (Contextual Embedding / *Deep Learning*)

---

## 🗂️ Struktur Repositori & Alur Kerja (*Notebooks*)

Berikut adalah penjelasan fungsi dari masing-masing *notebook* Jupyter yang ada dalam repositori ini, diurutkan berdasarkan alur eksekusi proyek:

* **`translate.ipynb`** : *Script* awal untuk menerjemahkan kumpulan teks (*news/tweets*) dari bahasa asing ke dalam Bahasa Indonesia secara otomatis menggunakan pustaka translasi.
* **`00_EDA_V2.ipynb` & `00_EDA_V2_scrapping.ipynb`** : *Exploratory Data Analysis* (EDA). Melakukan analisis sebaran kategori, visualisasi *WordCloud*, dan menghitung *Top Words* untuk dataset Kaggle dan data *scraping*.
* **`01_preprocessing.ipynb` & `01_preprocessing_chunk.ipynb`** : Tahap pembersihan teks (hapus *URL, mention, hashtag*, tanda baca), *case folding*, tokenisasi, *stopword removal*, dan *stemming* (menggunakan NLTK & Sastrawi). File *chunk* berfungsi membagi data latih menjadi beberapa ukuran korpus (10k, 20k, 30k, 40k, Full).
* **`02_modeling1.ipynb`** : Pelatihan Skenario 1 (TF-IDF + Naive Bayes) pada setiap variasi ukuran korpus (10k - Full).
* **`02_modeling2.ipynb`** : Pelatihan Skenario 2 (TF-IDF + SVM) pada setiap variasi ukuran korpus (10k - Full).
* **`02_modeling3.ipynb`** : Pelatihan Skenario 3 (FastText + SVM) pada setiap variasi ukuran korpus (10k - Full).
* **`02_modeling4.ipynb`** : Alur utama proses *fine-tuning* model IndoBERT (Skenario 4) untuk berbagai ukuran korpus.
* **`indobert_10k.ipynb` s/d `indobert_40k.ipynb` & `indobert.ipynb`** : Berkas pendukung yang merupakan hasil pemisahan/pembelahan eksekusi kode dari `02_modeling4.ipynb` berdasarkan ukuran korpus tertentu demi efisiensi waktu pengujian komputasi.
* **`03_training_comparison.ipynb`** : Kompilasi dan perbandingan metrik evaluasi (*Accuracy, Precision, Recall, F1-Score*) dari seluruh skenario pemodelan di semua variasi ukuran korpus.
* **`04.predict.ipynb`** : *Script* inferensi akhir. Menjalankan model terbaik yang telah disimpan untuk memprediksi 300 data *scraping* X/Twitter (*unseen data*).

---

## 📌 Kesimpulan Singkat
Dataset Kaggle memiliki ketidakseimbangan kelas kategori *Entertainment* hingga ~41%, yang menyebabkan sedikit bias prior pada algoritma klasik seperti Naive Bayes. Namun, seiring bertambahnya ukuran korpus dari 10k hingga *Full Data*, performa model mengalami perubahan. Model berbasis *deep learning* kontekstual seperti **IndoBERT** terbukti menunjukkan ketangguhan yang lebih baik dalam menangkap konteks pada *unseen data* dari Twitter dibandingkan metode ekstraksi fitur tradisional.