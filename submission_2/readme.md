# Laporan Proyek Machine Learning - Umar sani

![alt text](/images/image-32.png)

## Project Overview

Dalam era digital saat ini, pengguna memiliki akses ke ribuan film melalui berbagai platform streaming. Namun, banyaknya pilihan sering kali membuat pengguna kesulitan menemukan film yang sesuai dengan preferensi mereka. Oleh karena itu, diperlukan sistem rekomendasi yang dapat membantu mengatasi masalah ini dengan memberikan rekomendasi film berdasarkan preferensi pengguna.

Sistem rekomendasi dapat memberikan rekomendasi berdasarkan preferensi pengguna, misalnya riwayat film yang ditonton, metadata film yang mirip, dan lain-lain. Percobaan ini akan menggunakan sistem rekomendasi berbasis konten (Content-based filtering). Sistem rekomendasi ini akan menggunakan metadata film seperti genre, keywords, cast, dan director untuk memberikan rekomendasi. Penelitian Nassa *et al.* (2021) menggunakan teknik Count Vectorizer dan Cosine Similarity untuk membuat sistem rekomendasi film berbasis konten. Count Vectorizer digunakan untuk menghitung frekuensi kemunculan kata dalam metadata film. Setelah itu, Cosine Similarity digunakan untuk mengukur jarak similaritas antara film berdasarkan vektor yang dihasilkan oleh Count Vectorizer. Dengan pendekatan ini, sistem dapat memberikan rekomendasi film yang relevan dan sesuai dengan preferensi pengguna.


## Business Understanding

### Problem Statements

- Bagaimana cara membuat sistem rekomendasi film berbasis konten berdasarkan preferensi film yang ditonton pengguna?
- Bagaimana performa sistem rekomendasi film berbasis konten dalam merekomendasikan film yang sesuai?

### Goals

- Membuat sistem rekomendasi film berbasis konten berdasarkan preferensi film yang ditonton pengguna
- Mengukur performa sistem rekomendasi film berbasis konten dalam merekomendasikan film yang sesuai

### Solutions Approach

- Menggunakan teknik Count Vectorizer untuk menghitung frekuensi kemunculan kata dalam metadata film dan menghitung jarak similaritas antar film dengan Cosine Similarity.
- Menggunakan metrik precision untuk menghitung seberapa akurat sistem menghasilkan rekomendasi berdasarkan film-film yang terkait.

## Data Understanding

Dataset yang digunakan adalah The Movie Dataset. Dataset ini terdiri dari film-film yang dirilis pada atau sebelum Juli 2017. Dataset ini memiliki metadata 45.466 film yang terdaftar di Full MovieLens. Dataset ini terdiri dari beberapa file, seperti credits, keywords, links, links_small, movies_metadata, ratings, ratings_small. 

Percobaan ini hanya akan menggunakan data yang relevan untuk sistem rekomendasi, seperti movies_metadata, credits, dan keywords. Ketiga file tersebut akan dibaca menjadi dataframe untuk kemudian digabungkan menjadi satu. Dataset memiliki beberapa kolom yang bertipe data object dan memiliki missing value. Kondisi tersebut akan diproses pada tahap Data Preparation.

Tautan Dataset: https://www.kaggle.com/datasets/rounakbanik/the-movies-dataset

### Movies Metadata

Movie Metadata berisi informasi terkait mengenai sebuah film. Dataframe ini terdiri dari 45.466 baris dengan 24 fitur.

![alt text](/images/image.png)

**Fitur-fitur:**

- **adult**: Menunjukkan apakah film tersebut untuk dewasa atau tidak.
- **belongs_to_collection**: Menunjukkan apakah film tersebut bagian dari koleksi - film tertentu.
- **budget**: Anggaran yang digunakan untuk produksi film.
- **genres**: Genre atau jenis film.
- **homepage**: URL halaman utama film.
- **id**: ID unik untuk setiap film.
- **imdb_id**: ID unik film di IMDb.
- **original_language**: Bahasa asli film.
- **original_title**: Judul asli film.
- **overview**: Ringkasan atau deskripsi singkat tentang film.
- **popularity**: Skor popularitas film.
- **poster_path**: URL poster film.
- **production_companies**: Perusahaan produksi yang terlibat dalam pembuatan film.
- **production_countries**: Negara-negara tempat produksi film dilakukan.
- **release_date**: Tanggal rilis film.
- **revenue**: Pendapatan yang dihasilkan oleh film.
- **runtime**: Durasi film dalam menit.
- **spoken_languages**: Bahasa yang digunakan dalam film.
- **status**: Status rilis film (misalnya, dirilis, belum dirilis).
- **tagline**: Slogan atau tagline film.
- **title**: Judul film.
- **video**: Menunjukkan apakah ada video terkait film.
- **vote_average**: Rata-rata penilaian film.
- **vote_count**: Jumlah pengguna yang memberikan penilaian

### Keywords

Keywords berisi kata kunci yang terkait dengan sinopsis film. Dataframe ini terdiri dari 46.419 baris dengan 2 fitur.

![alt text](/images/image-1.png)

**Fitur-fitur:**
- **id**: ID unik untuk setiap film.
- **keywords**: Kata kunci dari sinopsis film.

### Crew

Crew berisi daftar pemeran dan kru yang terlibat dalam produksi film. Dataframe ini terdiri dari 45.476 baris dengan 3 fitur.

![alt text](/images/image-2.png)

**Fitur-fitur:**
- **id**: ID unik untuk setiap film.
- **cast**: Pemeran yang tampil dalam film.
- **crew**: Kru yang terlibat dalam pembuatan film.

## Data Preparation

Dataframe Movie, Credits, dan Crew memiliki struktur yang belum rapi. Terdapat beberapa kolom yang bertipe data object JSON. Tipe data ini perlu diubah ke format yang lebih mudah untuk diolah, seperti tipe data list. Selain itu, pengecekan nilai null akan dilakukan dan diisi dengan nilai lain yang sesuai.

### Movies

Berikut merupakan informasi dataset Movies.

![alt text](/images/image-4.png)

Movies memiliki beberapa kolom yang tidak memberikan informasi apapun mengenai film terkait seperti 'belongs_to_collection', 'homepage', 'imdb_id', 'poster_path', 'status', 'title', 'video'. Oleh karena itu, kolom-kolom tersebut akan dihapus. Selain itu, hapus beberapa baris yang bertipe data salah, karena akan membuat program error pada saat pengolahan data.

Kemudian, dapat dilihat bahwa dataset memiliki banyak kolom yang bertipe data object, seperti 'production_companies', 'production_countries', 'spoken_languages', dan  'genres'.

![alt text](/images/image-7.png)

Ubah kolom-kolom tersebut menjadi list agar lebih mudah untuk diolah.

![alt text](/images/image-8.png)

###  Keywords

Berikut merupakan informasi dataset Keywords.

![alt text](/images/image-5.png)

Keywords memiliki kolom 'keywords' yang bertipe data object.

![alt text](/images/image-9.png)

Sama seperti sebelumnya, lakukan konversi tipe data object menjadi list.

![alt text](/images/image-10.png)


###  Credits

Berikut merupakan informasi dataset Credits.

![alt text](/images/image-6.png)

Credits memiliki kolom 'cast' dan 'crew' yang bertipe data object.

![alt text](/images/image-11.png)

Sama seperti sebelumnya, lakukan konversi tipe data object menjadi list. Selain itu, kolom crew hanya akan diambil nama director-nya saja dan men-drop kolom crew itu sendiri.

![alt text](/images/image-12.png)

### Dataset Gabungan

Setelah setiap dataframe telah rapi, gabungkan semua dataframe menjadi satu.

![alt text](/images/image-13.png)

Cek informasi setiap kolom dataset dan apakah ada nilai null.

![alt text](/images/image-14.png)

Terdapat cukup banyak kolom yang memiliki nilai null. Nilai null ini akan diisi dengan nilai lain agar tidak kehilangan data. Kolom dengan tipe string akan diisi dengan string kosong. Sedangkan kolom dengan tipe numerik akan diisi dengan nilai mean.

Berikut hasil dataframe setelah proses cleaning.

![alt text](/images/image-15.png)

Sekarang, dataset telah bersih dari nilai null.

Pada percobaan ini, sistem rekomendasi akan menggunakan pendekatan Content-based Filtering. Metadata items yang digunakan adalah genres, keywords, cast, dan director yang terkait dengan masing-masing film.

![alt text](/images/image-19.png)

Pertama, bersihkan data dengan mengubah semua string menjadi lower case dan menghapus spasi di setiap frasanya.

![alt text](/images/image-20.png)

Setelah itu, buat kolom baru dengan nama content_filter, yang merupakan string gabungan genres, keywords, cast, dan director.

![alt text](/images/image-21.png)

Sekarang, dataset telah bersih dan siap untuk dilakukan visualisasi dan modeling.

## Data Visualization

### Genre Movie Terbanyak

Dari visualisasi dibawah, dapat dilihat bahwa genre film dengan jumlah terbanyak adalah drama, diikuti oleh komedi dan aksi.

![alt text](/images/image-16.png)

### Distribusi Film Berdasarkan Tanggal Rilis

Berdasarkan visualisasi di bawah, dapat dilihat bahwa produksi film meningkat pada dekade 2000 hingga sekarang.

![alt text](/images/image-17.png)

### Word Cloud

Berdasarkan word cloud dari plot film, dapat dilihat bahwa kata yang sering muncul adalah "find", "life", "love", "one" "family", dan "live".

![alt text](/images/image-18.png)

## Recommender System with Content Based Filtering

Sistem rekomendasi pada percobaan ini akan menggunakan Count Vectorizer. Count Vectorizer akan menghitung frekuensi kemunculan kata dalam dokumen dan menghasilkan vektor yang mewakili jumlah kemunculan kata-kata tersebut.

Di bawah ini merupakan visualisasi dari Count Vectorizer (Shah *et al.*, 2020).

![alt text](/images/image-22.png)

Berdasarkan matriks vektor hasil Count Vectorizer, jarak similaritas diantara setiap vektor akan dihitung dengan Cosine Similarity. 

Cosine similarity mengukur sudut kosinus antara dua vektor (Neuralis, 2023). Nilai cosine similarity berkisar antara -1 hingga 1, di mana:

- 1 menunjukkan bahwa dua vektor memiliki arah yang sama (sangat mirip).

- 0 menunjukkan bahwa dua vektor ortogonal (tidak ada kesamaan).

- -1 menunjukkan bahwa dua vektor memiliki arah yang berlawanan (sangat tidak mirip).

Di bawah ini merupakan visualisasi dari Cosine Similarity (Neuralis, 2023).

![alt text](/images/image-23.png)

Setelah matriks similaritas didapatkan, buat fungsi find_recommendations untuk mendapatkan rekomendasi film. Fungsi tersebut akan menghitung jarak similaritas antara film yang diinputkan dengan film-film lainnya. Setelah itu, film-film dengan skor similaritas yang sama akan didapatkan dan diurutkan berdasarkan skor tertinggi. Kemudian, 10 besar film dengan skor similaritas tertinggi akan dikembalikan.

Sebagai contoh, cari rekomendasi film yang mirip dengan film "Toy Story".

![alt text](/images/image-24.png)

Dapat dilihat bahwa sistem rekomendasi berhasil membuat rekomendasi film yang mirip dengan "Toy Story".

## Evaluation

Untuk mengukur seberapa baik sistem rekomendasi bekerja. Metrik yang dapat digunakan adalah precision. Metrik precision akan menghitung proporsi item yang direkomendasikan yang relevan dengan pengguna. 

Metrik precision pada sistem rekomendasi ditunjukkan oleh formula berikut.

![alt text](/images/image-26.png)

Jumlah item relevan adalah jumlah film dengan genre yang mirip. 

Jumlah total item adalah jumlah film yang direkomendasikan oleh sistem.

Sebagai contoh, buat rekomendasi film berdasarkan film "Star Wars"

![alt text](/images/image-25.png)

10 rekomendasi film telah didapatkan. 

Untuk mendapatkan jumlah film yang relevan, hitung similaritas berdasarkan kolom genre antara movie_to_find dengan film-film hasil rekomendasi.

![alt text](/images/image-27.png)

![alt text](/images/image-28.png)

Similaritas genre antara satu film dengan film yang lain akan dihitung dengan menggunakan jaccard similarity. Jackard similarity adalah metrik yang digunakan untuk mengukur tingkat kesamaan antara dua himpunan.

Jackard similarity ditunjukkan oleh formula berikut.

![alt text](/images/image-29.png)

Berikut merupakan hasil perhitungan jaccard similarity dari film yang ingin dicari dengan film-film hasil rekomendasi.

![alt text](/images/image-30.png)

Misalkan ditentukan film yang relevan adalah film dengan similaritas diatas 0.8, sehingga dari tabel similaritas diatas didapatan ada 9 film yang relevan.

Untuk menghitung precision, jumlah dari film yang relevan akan dibagi dengan jumlah film rekomendasi.

![alt text](/images/image-31.png)

Berdasarkan perhitungan diatas, precision sebesar 90%. Hasil tersebut sudah cukup bagus untuk sebuah sistem rekomendasi.

## Summary

Berdasarkan percobaan-percobaan yang telah dilakukan, model sistem rekomendasi yang dibuat berhasil mendapatkan rekomendasi film-film yang terkait dengan film yang diinputkan. Rekomendasi didapatkan berdasarkan kemiripan genre, kata kunci, pemeran, dan sutradara. Model sistem rekomendasi berhasil mendapatkan presisi sebesar 90%.

## Referensi

Nassa, A., Gupta, S., Jindal, P., Jain, A., & Lamba, P. S. (2021). A personalized recommender system. Fusion: Practice and Applications, 6(1), 32-42.

Neuralis. (2023, February 25). How Cosine similarity can improve your machine learning Models - AITechTrend. AITechTrend - Further into the Future. https://aitechtrend.com/how-cosine-similarity-can-improve-your-machine-learning-models/

Shah, S. R., Kaushik, A., Sharma, S., & Shah, J. (2020). Opinion-mining on marglish and devanagari comments of youtube cookery channels using parametric and non-parametric learning models. Big Data and Cognitive Computing, 4(1), 3.
