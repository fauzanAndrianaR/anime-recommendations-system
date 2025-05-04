# Laporan Proyek Machine Learning Anime Recommendations - Fauzan Andriana Rachman

## Project Overview

![coverpage](https://storage.googleapis.com/kaggle-datasets-images/571/1094/c633ae058ddaa59f43649caac1748cf4/dataset-cover.png)

Rekomendasi anime merupakan salah satu fitur yang penting dalam dunia hiburan digital, terutama bagi pengguna platform seperti MyAnimeList. Seiring dengan meningkatnya jumlah anime setiap tahun, pengguna kesulitan menemukan anime baru yang sesuai dengan preferensinya. Oleh karena itu, dibutuhkan sistem rekomendasi yang mampu memberikan saran anime berdasarkan data dan algoritma yang tepat.

Pada proyek ini, sistem rekomendasi dibuat menggunakan pendekatan **content-based filtering**. Model ini memanfaatkan fitur dari anime seperti `genre` dan `type` untuk menghitung kesamaan antar anime menggunakan **cosine similarity**. Dataset yang digunakan berasal dari file `anime.csv`, yang kemudian diolah dan digunakan untuk menyarankan anime yang mirip dengan judul yang dimasukkan pengguna.


**Rubrik/Kriteria Tambahan (Opsional)**:
- Masalah ini penting untuk diselesaikan karena semakin besar database anime, semakin sulit bagi pengguna untuk menemukan anime yang sesuai secara manual.
- Sistem rekomendasi dapat meningkatkan pengalaman pengguna dan efisiensi dalam memilih anime yang akan ditonton.
- Referensi relevan:
  - Iklil jayaperwira,dkk.(2023). Anime Rekomendasi Menggunakan Collaborative Filtering. Jurnal e-Proceeding of Engineering.Vol.10, No.3 Juni 2023. Tersedia: [tautan.](https://openlibrary.telkomuniversity.ac.id/home/catalog/id/185894/slug/anime-rekomendasi-menggunakan-collaborative-filtering.html)
  -  Sindhu Seelam. (2021). Machine Learning Fundamentals: Cosine Similarity and Cosine Distance. Published in Geek Culture. Tersedia: [tautan.](https://medium.com/geekculture/)

## Business Understanding

Pada bagian ini, Anda perlu menjelaskan proses klarifikasi masalah.

Bagian laporan ini mencakup:

### Problem Statements

- Bagaimana cara membuat sistem rekomendasi anime yang merekomendasikan anime yang cocok untuk pengguna?

- Bagaimana merekomendasikan anime yang mirip dengan anime yang disukai pengguna berdasarkan fitur-fitur kontennya seperti `genre` dan `type`?



### Goals

- Membangun sistem rekomendasi anime dengan pendekatan content-based filtering menggunakan Cosine Similarity.
- Memberikan rekomendasi Top 10 anime yang paling mirip dengan anime yang dimasukkan/disukai pengguna.



    ### Solution statements
- Menggunakan metode **TF-IDF Vectorizer** untuk mengubah teks pada kolom `genre` menjadi vektor numerik.
- Menghitung **cosine similarity** antar vektor genre untuk mengukur kemiripan antar anime.
- Menyusun fungsi interaktif agar pengguna dapat memasukkan nama anime dan mendapatkan rekomendasi anime yang mirip.

## Data Understanding
### EDA - Deskripsi Variabel

| Jenis    | Keterangan                                                |
|----------|-----------------------------------------------------------|
| Title    | Anime Recommendations Database                                       |
| Source   |[Kaggle](https://www.kaggle.com/datasets/CooperUnion/anime-recommendations-database)                                                  |



Berikut adalah informasi pada dataset: 

Kumpulan dataset ini dikumpulkan dari platform [MyAnimeList](https://myanimelist.net/)  , komunitas online populer dan database untuk penggemar anime dan manga. Platform ini menyediakan informasi berharga tentang acara anime, profil pengguna, dan skor pengguna untuk berbagai anime. Dataset yang digunakan pada proyek kali ini disediakan secara publik di kaggle dengan nama datasets yaitu: Anime Recommendations Database  . Dataset ini dapat diunduh di Kaggle : [Anime Recommendations Database ](https://www.kaggle.com/datasets/CooperUnion/anime-recommendations-database) .



**Berikut informasi pada dataset** :
 - Datasets berupa file csv (Comma-Seperated Values).
 - Dataset berupa 2 buah file CSV yaitu: 
    * anime.csv  
    * rating.csv      
   

Pada model kali ini dataset yang digunakan adalah file `anime.csv`
 - Dataset memiliki 12294 rows dan 7 columns
 - Dataset memilik 4 fitur `object`, 2 fitur `int64`, dan 1 fitur `float64`.
 - Terdapat *Missing value* pada fitur  `genre sebanyak 62, type sebanyak 25 dan rating sebanyak 230`.
 - Tidak ada data yang duplikat.


 
### Variable - variable pada dataset
Kolom datasets anime memiliki informasi berikut:
- **anime_id** - ID unik dari MyAnimeList.net yang mengidentifikasi sebuah anime.
- **name** - Nama lengkap dari anime.
- **genre** - Daftar genre yang dipisahkan dengan koma untuk anime ini.
- **type** - Jenis anime, seperti Movie, TV, OVA, dll.
- **episodes** - Jumlah episode dalam anime ini. (1 jika berupa film).
- **rating** - Rata-rata rating dari 10 untuk anime ini.
- **members** - Jumlah anggota komunitas yang tergabung dalam grup anime ini.

![genre](https://raw.githubusercontent.com/fauzanAndrianaR/anime-recommendations-system/refs/heads/main/genre.png)
![type](https://raw.githubusercontent.com/fauzanAndrianaR/anime-recommendations-system/refs/heads/main/type.png)


### 🎭 Top 10 Genre Anime Berdasarkan Jumlah Kemunculan

1. **Comedy** adalah genre paling populer, muncul paling sering dibanding genre lainnya. Ini menunjukkan bahwa elemen humor sangat disukai oleh penonton anime.
2. **Action** dan **Adventure** menempati posisi kedua dan ketiga, mengindikasikan ketertarikan tinggi terhadap cerita penuh aksi dan petualangan.
3. Genre **Fantasy** dan **Sci-Fi** juga cukup mendominasi, menunjukkan minat terhadap dunia imajinatif dan teknologi fiksi.
4. **Drama** dan **Shounen** masih populer, sering menjadi fondasi utama dalam banyak cerita anime.
5. **Romance** dan **School** menempati posisi 9 dan 10, menandakan bahwa tema percintaan dan kehidupan sekolah cukup populer namun lebih niche.
6. Dari total 43 genre unik, hanya 10 yang mendominasi pasar anime — distribusi genre tidak merata.

---

### 📺 Distribusi Tipe Anime

1. **TV Series** adalah tipe anime paling umum, dengan jumlah hampir mencapai 4.000. Ini sesuai karena format serial cocok untuk pengembangan cerita jangka panjang.
2. **OVA** (Original Video Animation) dan **Movie** berada di posisi kedua dan ketiga. OVA sering digunakan untuk spin-off, sedangkan Movie memiliki kualitas produksi yang lebih tinggi.
3. **Special** dan **ONA** (Original Net Animation) memiliki jumlah yang cukup signifikan, namun digunakan sebagai pelengkap.
4. **Music** merupakan tipe anime paling sedikit jumlahnya, menunjukkan bahwa anime bertema musikal bukanlah format utama dalam industri.

---

### 📌 Insight Umum

- Industri anime sangat dipengaruhi oleh genre populer seperti *Comedy*, *Action*, dan *Adventure*.
- Format anime didominasi oleh *TV Series*, sementara format seperti *OVA*, *Movie*, dan *ONA* digunakan untuk tujuan khusus.
- Keberagaman genre menunjukkan fleksibilitas industri anime, tetapi hanya sebagian kecil genre yang mendominasi secara kuantitas.

## Data Preparation

Proses data preparation dilakukan melalui beberapa tahapan agar data siap digunakan dalam membangun sistem rekomendasi. Pertama, penanganan nilai kosong dilakukan karena kolom `genre` dan `type` memiliki data yang hilang (`NaN`) yang dapat memengaruhi proses ekstraksi fitur. Oleh karena itu, nilai-nilai kosong tersebut dihapus dari dataset. 
Selanjutnya, dibuat fitur gabungan baru bernama `combined_features` dengan menggabungkan nilai dari kolom `genre` dan `type`. Sebelum digabung, setiap nilai diubah terlebih dahulu menjadi bentuk string agar dapat disatukan, misalnya menjadi `'Action, Adventure TV'`. Setelah itu, dilakukan ekstraksi fitur teks dari kolom `combined_features` menggunakan metode **TF-IDF Vectorizer** untuk mengubah data teks menjadi representasi numerik. Pendekatan ini memungkinkan model memahami tingkat kepentingan suatu kata (seperti genre atau tipe) dalam keseluruhan data. Langkah terakhir adalah normalisasi, di mana representasi vektor dari hasil TF-IDF digunakan untuk menghitung kemiripan antar anime.


**Alasan Tahapan Preparation Dilakukan**:
- Nilai kosong perlu diisi agar tidak mengganggu proses vektorisasi.
- Menggabungkan fitur menjadi satu kolom menyederhanakan proses ekstraksi fitur.
- TF-IDF efektif untuk merepresentasikan data berbasis teks, terutama genre, karena mampu memberikan bobot yang mencerminkan pentingnya suatu kata dalam keseluruhan dokumen.


## Modeling
Pada proyek ini hanya gunakan Model Cosine Similarity . Algoritma ini akan mempelajari kesamaan antar data dalam fitur yang ada.

### Cosine similarity

_Cosine similarity_ merupakan  metode untuk mengukur seberapa mirip dua vektor dalam ruang multidimensi. Ini adalah pengukuran kosinus sudut antara dua vektor yang dimensi dan magnitudonya direpresentasikan sebagai titik dalam ruang. Nilai similaritas kosinus berkisar antara -1 hingga 1, di mana nilai 1 menunjukkan kedua vektor sepenuhnya sejajar (100% mirip), 0 menunjukkan vektor tegak lurus (tidak ada keterkaitan), dan -1 menunjukkan kedua vektor sepenuhnya berlawanan arah (100% tidak mirip). Metode ini sering digunakan dalam pemrosesan teks dan pengelompokan data untuk menentukan tingkat kesamaan antara dokumen atau fitur dalam dataset. [[5](https://medium.com/geekculture/cosine-similarity-and-cosine-distance-48eed889a5c4)]

Cosine Similarity dituliskan dalam rumus: 

$$Cosine Similarity (A, B) = (A · B) / (||A|| * ||B||)$$ 

dimana: 
- (A·B)menyatakan produk titik dari vektor A dan B.
- ||A|| mewakili norma Euclidean (magnitudo) dari vektor A.
- ||B|| mewakili norma Euclidean (magnitudo) dari vektor B.

Untuk melakukan pengujian model, digunakan potongan kode berikut.
```python
 get_recommendations("kimi no na wa.")
 
 ```
| Name   | Title                                                           | Genre                                                     | Type  |
|--------|-----------------------------------------------------------------|-----------------------------------------------------------|-------|
| 1111   | Aura: Maryuuin Kouga Saigo no Tatakai                          | Comedy, Drama, Romance, School, Supernatural              | Movie |
| 1494   | Harmonie                                                       | Drama, School, Supernatural                               | Movie |
| 1959   | Air Movie                                                      | Drama, Romance, Supernatural                              | Movie |
| 6391   | Wind: A Breath of Heart (TV)                                   | Drama, Romance, School, Supernatural                      | TV    |
| 5803   | Wind: A Breath of Heart OVA                                    | Drama, Romance, School, Supernatural                      | OVA   |
| 208    | Kokoro ga Sakebitagatterunda.                                  | Drama, Romance, School                                    | Movie |
| 11005  | Suki ni Naru Sono Shunkan wo.: Kokuhaku Jikkou...             | Comedy, Drama, Romance, School                            | Movie |
| 2103   | Clannad Movie                                                  | Drama, Fantasy, Romance, School                           | Movie |
| 878    | Shakugan no Shana II (Second)                                  | Action, Drama, Fantasy, Romance, School, Supernatural     | TV    |
| 986    | Shakugan no Shana                                              | Action, Drama, Fantasy, Romance, School, Supernatural     | TV    |



Table 1. Hasil Pengujian Model Content Based Filtering .

Berdasarkan tabel. Hasil Pengujian Model Content Based Filtering.  Sistem telah berhasil merekomendasikan top 10 persen anime yang mirip dengan **Kimi no na Wa**, yang kebanyakan beberapa film memiliki kesamaan genre dan yaitu `drama`dan `romance` juga kesamaan type yaitu `Movie`.  Dengan pendekatan ini, sistem mengidentifikasi anime-anime yang memiliki kemiripan dalam genre dan type dengan **Kimi no na Wa**, sehingga memungkinkan pengguna untuk menemukan konten yang sesuai dengan preferensi mereka berdasarkan kesukaan mereka terhadap anime.

#### ✅ Kelebihan Cosine Similarity

1. **Mengabaikan Magnitude (Skala)**  
   Cosine similarity hanya memperhatikan arah vektor, bukan panjangnya. Jadi, dua dokumen dengan kata-kata serupa tetapi panjang berbeda tetap bisa dianggap mirip.

2. **Cocok untuk Teks/Dokumen**  
   Sangat populer di bidang NLP, terutama dalam pengukuran kemiripan antar dokumen atau kalimat.

3. **Sederhana dan Cepat Dihitung**  
   Rumusnya hanya melibatkan dot product dan norm, sehingga perhitungannya efisien, terutama pada data sparse (seperti representasi teks dalam vektor TF-IDF).

4. **Skalabilitas Baik**  
   Bisa digunakan pada dataset besar, apalagi jika dikombinasikan dengan indexing (seperti dalam information retrieval).

---

## ❌ Kekurangan Cosine Similarity

1. **Tidak Memperhatikan Frekuensi Absolut**  
   Dua dokumen bisa dianggap mirip meskipun satu dokumen jauh lebih sering menyebutkan kata-kata tertentu (karena hanya sudut yang dihitung, bukan panjang vektor).

2. **Kurang Akurat untuk Konteks Semantik**  
   Tidak memahami makna kata. Dua kalimat dengan arti sama tapi kata berbeda bisa dianggap tidak mirip (contoh: "mobil" vs "kendaraan").

3. **Tidak Cocok untuk Data dengan Skala Penting**  
   Dalam data numerik (seperti pengukuran fisik atau rating), kadang magnitude penting. Cosine similarity bisa mengabaikan informasi ini.

4. **Performa Buruk pada Data Sangat Pendek**  
   Pada teks pendek (seperti tweet atau kalimat), cosine similarity bisa tidak stabil karena vektornya terlalu pendek dan sparse.



**Rubrik/Kriteria Tambahan (Opsional)**: 
- Menyajikan dua solusi rekomendasi dengan algoritma yang berbeda.
- Menjelaskan kelebihan dan kekurangan dari solusi/pendekatan yang dipilih.

## Evaluation


### Precission
Precission adalah metrik yang penting untuk mengevaluasi kinerja model pengelompokan. Metrik ini membantu dalam memahami seberapa akurat model dalam mengidentifikasi data positif. Nilai presisi yang tinggi menunjukkan bahwa model jarang membuat prediksi positif yang salah, sehingga prediksi positifnya dapat lebih dipercaya.[[7](https://esairina.medium.com/memahami-confusion-matrix-accuracy-precision-recall-specificity-dan-f1-score-610d4f0db7cf)] 

_Precission_ dituliskan dalam rumus:

$$Presisi = \frac{TP}{TP + FP}$$

dimana: 
- TP (True Positive): Jumlah data yang diprediksi positif dan memang benar-benar positif.
- FP (False Positive): Jumlah data yang diprediksi positif, namun kenyataannya adalah negatif.


*Interpretasi*
 Jika kita hanya melihat dari hasil presisi berdasarkan  tabel secara manual . Hasil Pengujian Model Content Based Filtering . Menunjukkan bahwa presisi model rekomendasi Top-10 adalah bisa dibilang sempurna, yaitu 10/10. Ini menandakan bahwa model tersebut memberikan rekomendasi dengan tingkat presisi yang sangat tinggi. Ini sesuai dengan hasil pengujian yang menunjukkan bahwa model mampu memberikan rekomendasi dengan nama dan genre yang mirip dengan anime `Kimi no na WA`, seperti Drama, Romance, school dan Supernatural, juga hasil menampilkan type yang kebanyakan sama yaitu Movies.


### Contoh Evaluasi Berdasarkan Overlap Precision@K dengan Code

Kita akan mengukur berapa banyak genre yang sama antara anime input dan hasil rekomendasinya. Ini cocok untuk sistem berbasis konten.

 Langkah Evaluasi:
1. Ambil anime target (misal: "One Piece")

2. Ambil rekomendasi dari fungsi get_recommendations

3. Bandingkan genre-nya → hitung berapa genre yang cocok

Hitung precision:
```python
evaluate_recommendation("One Piece") 
 ```

 hasil :
```python

 Evaluasi untuk rekomendasi berdasarkan 'One Piece':
Genre anime input: {'adventure', 'action', 'comedy', 'super power', 'fantasy', 'shounen', 'drama'}
Total genre overlap: 57
Precision@10: 1.00
```

ini adalah hasil dari get rekomendasi One Piece nya:
```python
 get_recommendations("One piece")
 
 ```

| ID     | Name                                                                 | Genre                                                       | Type   |
|--------|----------------------------------------------------------------------|--------------------------------------------------------------|--------|
| 86     | Shingeki no Kyojin                                                   | Action, Drama, Fantasy, Shounen, Super Power                | TV     |
| 10851  | Shingeki no Kyojin Season 2                                          | Action, Drama, Fantasy, Shounen, Super Power                | TV     |
| 231    | One Piece: Episode of Merry - Mou Hitori no Nakama no Monogatari     | Action, Adventure, Comedy, Drama, Fantasy, Shounen          | Special|
| 241    | One Piece: Episode of Nami - Koukaishi no Namida to Nakama no Kizuna | Action, Adventure, Comedy, Drama, Fantasy, Shounen          | Special|
| 896    | One Piece: Episode of Sabo - 3 Kyoudai no Kizuna to Mirai no Bouken  | Action, Adventure, Comedy, Drama, Fantasy, Shounen          | Special|
| 352    | One Piece Film: Strong World Episode 0                               | Action, Adventure, Comedy, Fantasy, Shounen, Super Power    | OVA    |
| 2161   | One Piece Recap                                                       | Action, Adventure, Comedy, Fantasy, Shounen, Super Power    | OVA    |
| 3837   | One Piece: Taose! Kaizoku Ganzack                                     | Action, Adventure, Comedy, Fantasy, Shounen, Super Power    | OVA    |
| 6      | Hunter x Hunter (2011)                                                | Action, Adventure, Shounen, Super Power                     | TV     |
| 112    | Hunter x Hunter                                                       | Action, Adventure, Shounen, Super Power                     | TV     |
