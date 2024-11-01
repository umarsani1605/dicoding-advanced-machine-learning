# Laporan Proyek Machine Learning Terapan - Umar Sani

![Kurma](/images/kurma.jpg)

## Domain Proyek

Buah kurma (Phoenix dactylifera) merupakan buah yang kaya nutrisi dan memiliki beragam varietas. Di seluruh dunia, terdapat 200 jenis dan lebih dari 2500 spesies, masing-masing memiliki perbedaan dalam hal ukuran, bentuk, dan warna. Di industri pertanian, klasifikasi buah kurma diperlukan untuk membantu petani dalam memilah dan mengklasifikasikan hasil panen. Di industri makanan, klasifikasi buah kurma dapat membantu mengelompokkan produk dan memastikan konsumen mendapatkan produk kurma dengan varietas yang sesuai. Namun, identifikasi varietas kurma secara manual membutuhkan keahlian khusus dan waktu yang cukup lama. Oleh karena itu diperlukan metode otomatis untuk mengklasifikasikan varietas kurma. Salah satu metode yang dapat digunakan adalah metode machine learning. 

Penelitian Koklu *et al.* (2021) mengklasifikasikan varietas kurma dengan menggunakan logistic regression (LR) dan artificial neural network (ANN). Penelitian tersebut berhasil mendapatkan hasil akurasi 91.0% and 92.2%. 

Percobaan pada proyek ini bertujuan untuk mengeksplorasi penggunaan metode machine learning dalam klasifikasi varietas kurma. Metode machine learning yang digunakan dalam penelitian ini yaitu Support Vector Machine (SVM), K-Nearest Neighbors (KNN), Random Forest, Logistic Regression, Naive Bayes, Multi-Layer Perceptron (MLP), dan XGBoost.


Referensi:

KOKLU, M., KURSUN, R., TASPINAR, Y. S., and CINAR, I. (2021). Classification of Date Fruits into Genetic Varieties Using Image Analysis. Mathematical Problems in Engineering, Vol.2021, Article ID: 4793293, DOI:10.1155/2021/4793293


## Business Understanding

### Problem Statements

- Klasifikasi jenis kurma secara manual memerlukan keahlian khusus dan waktu yang cukup lama.
- Klasifikasi yang tidak akurat mempengaruhi kualitas dan varietas yang tidak sesuai.

### Goals

- Mengembangkan model machine learning yang dapat mengklasifikasikan buah kurma secara otomatis.
- Meningkatkan akurasi klasifikasi buah kurma untuk memastikan kualitas dan varietas produk yang sesuai.

### Solutions

- Menggunakan beberapa model machine learning untuk mengembangkan model klasifikasi.
- Melakukan evaluasi model untuk memilih model dengan performa yang terbaik.

## Data Understanding

Data yang digunakan adalah Date Fruit Datasets yang dihimpun oleh Koklu *et al.* (2021). Dataset memiliki 898 baris yang berisi fitur-fitur numerik  dari tujuh jenis kurma yang berbeda. Dataset dikumpulkan dari gambar-gambar kurma yang diperoleh melalui sistem computer vision dan diolah lebih lanjut untuk mengekstraksi 34 fitur buah kurma, meliputi fitur-fitur morfologi, bentuk, dan warna.

![Kurma](/images/kurma-dataset.png)

Proses pengambilan dataset kurma oleh Koklu *et al.* (2021)

URL Dataset: https://www.kaggle.com/datasets/muratkokludataset/date-fruit-datasets/data

### Fitur-Fitur Dataset

**Fitur morfologi:**
- Area: Luas permukaan kurma.
- Perimeter: Keliling kurma.
- Major axis: Sumbu utama kurma.
- Minor axis: Sumbu minor kurma.
- Eccentricity: Eksentrisitas kurma.
- Roundness: Kebulatan kurma.
- Equivalent diameter: Diameter ekuivalen kurma.
- Solidity: Kekompakan kurma.
- Convex_area: Luas konveks kurma.
- Extent: Ekstensi kurma.
- Aspect ratio: Rasio aspek kurma.
- Compactness: Kekompakan kurma.

**Fitur bentuk:**

- Shapefactor_1 hingga Shapefactor_4: Faktor bentuk kurma.

**Fitur warna:**

- Mean RR, Std. dev RR, Skew RR, Kurtosis RR, Entropy RR, All Daub4 RR: Fitur warna kurma berdasarkan channel RGB.

## Data Analysis

Pada tahapan ini, proses data analysis dilakukan untuk memahami struktur dan karakteristik dataset.

1. Memeriksa Informasi Dataset

    Pertama, kita memeriksa informasi dataset dengan fungsi `df.info()`. Dari fungsi tersebut kita dapat melihat bahwa dataset memiliki 897 baris dan 35 kolom, sendang semua fitur berupa data numerik, kecuali kolom `Class`  yang merupakan kolom target. Tidak ada nilai null yang ditemukan dalam dataset, yang berarti data sudah lengkap dan siap untuk dianalisis lebih lanjut.

    ![Dataset Info](/images/df_info.png)

 2. Memeriksa Statistik Deskriptif

    Statistik deskriptif memberikan gambaran tentang rentang nilai, mean, dan standar deviasi dari setiap fitur. Ini membantu kita memahami distribusi data dan mengidentifikasi fitur yang mungkin memerlukan normalisasi. Misalnya, kita bisa melihat apakah ada fitur yang memiliki rentang nilai yang sangat berbeda dari yang lain.

    ![Dataset Describe](/images/df_describe.png)

 3. Perbandingan Jumlah Data untuk Setiap Kelas

    Berikutnya kita visualisasikan distribusi jumlah data untuk setiap kelas dalam dataset. Ini berguna untuk memastikan bahwa dataset tidak memiliki ketidakseimbangan kelas yang signifikan, yang dapat mempengaruhi kinerja model machine learning.

    ![Bar Plot Perbandingan Kelas](/images/barplot-classes.png)


 4. Scatterplot dan Heatmap untuk Korelasi Fitur:

    Scatterplot dan heatmap membantu kita melihat korelasi antara fitur-fitur dalam dataset. Korelasi yang tinggi antara dua fitur dapat menunjukkan bahwa salah satu fitur mungkin redundant dan bisa dihapus untuk menyederhanakan model.

    ![Scatter Plot](/images/scatterplot.png)
    ![Heatmap](/images/correlation-matrix.png)

    Berdasarkan scatterplot dan correlation matrix diatas, setiap fitur cenderung memiliki korelasi dengan fitur-fitur yang lain. Walaupun ada fitur yang tidak berkolerasi dengan beberapa fitur lainnya, tetap ada pasangan fitur yang berkolerasi. Oleh karena itu, kita tidak melakukan pengurangan fitur.

 5. Histogram untuk Distribusi Fitur

    Histogram menunjukkan distribusi jumlah baris untuk setiap fitur dalam dataset. Ini membantu kita memahami apakah data memiliki skewness atau kesimetrisan, yang dapat mempengaruhi kinerja model machine learning.

    ![Histogram](/images/histogram.png)

    Berdasarkan gambar diatas, setiap fitur dataset memiliki skewness atau kesimetrisan yang beragam.

 6. KDE Plot untuk Distribusi Data per Kelas

    KDE plot menampilkan perbandingan distribusi antar kelas dalam dataset dengan visualisasi yang lebih jelas. Ini membantu kita memahami bagaimana fitur-fitur berbeda antara kelas-kelas yang berbeda.

    ![KDE Plot](/images/kde-plot.png)

    KDE plot sebenarnya tidak jauh berbeda dengan histogram, namun dengan grafik yang lebih halus dan perbandingan antar kelas yang lebih jelas.

## Data Preparation

Pada bagian ini, kita menerapkan beberapa teknik data preparation sebelum dataset diinputkan ke model machine learning.

1. Pembagian Dataset
   
   Dataset dibagi menjadi data training dan testing dengan perbandingan 80:20 menggunakan `train_test_split` dari `sklearn.model_selection`.

2. Label Encoding
   
   Kolom target Class diubah menjadi nilai numerik menggunakan `LabelEncoder` dari `sklearn.preprocessing`. Label encoding diperlukan karena model machine learning tidak dapat memproses data kategorikal secara langsung, sehingga perlu diubah menjadi data numerik terlebih dahulu.

3. Normalisasi
   
   Fitur-fitur dinormalisasi menggunakan `StandardScaler` agar semua fitur berada pada skala nilai yang sama, yang akan mempercepat dan mengefisiensi kinerja model machine learning.

## Modeling

Pada tahap ini, beberapa model machine learning digunakan untuk mengklasifikasi varietas buah kurma. Model yang digunakan yaitu Support Vector Machine (SVM), K-Nearest Neighbors (KNN), Random Forest, Logistic Regression, Naive Bayes, Multi-Layer Perceptron (MLP), dan XGBoost. 

Setiap model akan dilakukan proses training dan testing. Metrik evaluasi  yang digunakan adalah akurasi. Hasil akurasi dari setiap model akan disimpan untuk dilakukan evaluasi lebih lanjut.

1. **Support Vector Machine (SVM)**

    SVM adalah model machine learning yang  bekerja dengan mencari hyperplane terbaik yang memisahkan data ke dalam setiap kelas yang berbeda. Model yang digunakan SVC (Support Vector Classifier), jenis SVM yang khusus digunakan untuk klasifikasi.

    **Parameter:** Kernel `linear` dengan `C=1`. Parameter terbaik didapatkan dari beberapa kali percobaan.
  
    **Kelebihan:** Efektif pada data berdimensi tinggi dengan generalisasi yang baik.
  
    **Kekurangan:** Tidak bekerja dengan baik pada dataset yang sangat besar.

    **Hasil:**

    ![Hasil Model](/images/1_svm.png)

2. **K-Nearest Neighbors (KNN)**

    K-Nearest Neighbors (KNN) adalah algoritma klasifikasi yang bekerja dengan mencari sejumlah (k) tetangga terdekat dari data baru berdasarkan jarak tertentu.

    **Parameter:** Berdasarkan beberapa kali running, parameter terbaik yang didapatkan yaitu  `n_neighbors=3`.

    **Kelebihan:** Sederhana, mudah diimplementasikan, dan  tidak memerlukan training model yang lama.

    **Kekurangan:** Tidak efisien pada dataset besar karena harus menghitung jarak ke semua titik data.

    **Hasil:**

    ![Hasil Model](/images/2_knn.png)

3. **Random Forest**

    Random Forest adalah model machine learning berbasis ensemble yang menggabungkan hasil dari banyak pohon keputusan (decision tree).

    **Parameter:** Parameter yang digunakan yaitu `n_estimators=100`, `max_depth=12`.

    **Kelebihan:** Dapat menangani dataset dengan fitur yang banyak dan beragam.

    **Kekurangan:** Model yang kompleks dan membutuhkan banyak sumber daya komputasi.

    **Hasil:**

    ![Hasil Model](/images/3_rfc.png)

4.  **Logistic Regression**

    Logistic Regression adalah metode statistik yang digunakan untuk memprediksi probabilitas kejadian berdasarkan satu atau lebih variabel independen.

    **Parameter:** `max_iter=1000`.

    **Kelebihan:** Cepat, sederhana, dan mudah diinterpretasikan.

    **Kekurangan:** Tidak bekerja dengan baik pada data yang tidak linear.

    **Hasil:**

    ![Hasil Model](/images/4_lr.png)

5. **Naive Bayes**

    Naive Bayes adalah algoritma klasifikasi yang didasarkan pada Teorema Bayes dengan asumsi semua fitur independen satu sama lain.

    **Kelebihan:** Sederhana dan efisien.

    **Kekurangan:** Asumsi independensi fitur yang jarang terpenuhi dalam dunia nyata.

    **Hasil:**

    ![Hasil Model](/images/5_nbc.png)

6. **Multi-Layer Perceptron (MLP)**

    Multi-Layer Perceptron (MLP) adalah jenis jaringan saraf tiruan yang terdiri dari beberapa lapisan neuron. Model ini bekerja dengan mengirimkan data melalui beberapa lapisan tersembunyi, yang memungkinkan jaringan untuk belajar representasi yang lebih kompleks dari data.

    **Parameter:** `hidden_layer_sizes=(100,)`, `max_iter=500`.

    **Kelebihan:** Dapat menangkap hubungan non-linear dalam data.

    **Kekurangan:** Membutuhkan banyak data dan waktu yang cukup lama untuk training.

    **Hasil:**

    ![Hasil Model](/images/6_mlp.png)

7. **Extreme Gradient Boosting (XGBoost)**

    XGBoost merupakan model machine learning yang bekerja dengan membangun model prediktif secara berlapis (ensemble), di mana setiap lapisan (atau pohon keputusan) belajar dari kesalahan lapisan sebelumnya.

    **Parameter:** `objective='multi:softmax'`

    **Kelebihan:** Akurasi cenderung tinggi dan fleksibel untuk berbagai  masalah.

    **Kekurangan:** Model kompleks dan membutuhkan komputasi yang cukup tinggi.

    **Hasil:**

    ![Hasil Model](/images/7_xgb.png)


###  **Model Terbaik**

Logistic Regression dipilih sebagai model terbaik karena memiliki akurasi tertinggi, yaitu sebesar 92.78%.


## Evaluation

Pada tahap evaluasi, metrik yang digunakan adalah akurasi. Akurasi mengukur persentase jumlah prediksi yang benar dari total prediksi. Formula akurasi ditunjukkan pada gambar dibawah.

![Akurasi Formula](/images/akurasi.png)

Di bawah ini merupakan tabel akurasi hasil testing dari model-model yang digunakan.

| Model      | Akurasi      |
| ------------- | ------------- |
| SVM | 92.22% |
| KNN | 91.11% |
| Random Forest | 90.00% |
| Logistic Regression | 92.78% |
| Naive Bayes | 88.88% |
| MLP | 90.55% |
| XGBoost | 90.55 |

Berikut merupakan visualisasi perbandingan akurasi dari semua model.

![Akurasi Formula](/images/barplot-accuracy.png)

Berdasarkan grafik di atas, semua model mendapatkan hasil yang baik dengan akurasi terendah yaitu Naive Bayes dengan akurasi 88.89% dan akurasi terbaik yaitu Logistic Regression dengan akurasi 92.78%.

## Summary

Berdasarkan percobaan-percobaan yang telah dilakukan, model machine learning berhasil mendapatkan hasil yang baik dalam mengklasifikasikan varietas kurma. Model machine learning yang mendapatkan akurasi  terbaik adalah Logistic Regression dengan akurasi 92.78%.