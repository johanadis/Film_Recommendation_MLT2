# Laporan Proyek Machine Learning - Johanadi Santoso


## Project Overview

Sistem rekomendasi film telah menjadi elemen kunci dalam meningkatkan pengalaman pengguna di platform streaming seperti Netflix, Hulu, dan Amazon Prime. Dengan jumlah film yang terus bertambah, pengguna sering kali merasa kewalahan dalam memilih konten yang sesuai dengan selera mereka. Proyek ini bertujuan untuk mengembangkan sistem rekomendasi berbasis konten yang memanfaatkan metadata film dari *The Movie Database (TMDB)*, yang mencakup informasi seperti genre, kata kunci, pemeran, dan sinopsis. Dataset ini diunduh dari Kaggle dan digunakan untuk membangun model yang dapat merekomendasikan film serupa berdasarkan film yang dipilih pengguna.

Pentingnya proyek ini terletak pada kemampuannya untuk meningkatkan kepuasan pengguna dan *engagement* di platform streaming. Menurut penelitian oleh Lops et al., sistem rekomendasi berbasis konten efektif dalam memberikan rekomendasi yang transparan dan relevan karena memanfaatkan fitur intrinsik dari item itu sendiri [1]. Selain itu, pendekatan ini memungkinkan personalisasi tanpa memerlukan data historis pengguna dalam jumlah besar, yang sering kali menjadi kendala dalam pendekatan berbasis kolaboratif [2]. Dengan demikian, proyek ini tidak hanya memiliki nilai praktis dalam industri hiburan tetapi juga berkontribusi pada pengembangan teknologi rekomendasi yang lebih inklusif.

## Referensi

> [1] P. Lops, M. de Gemmis, and G. Semeraro, "Content-based Recommender Systems: State of the Art and Trends," in *Recommender Systems Handbook*, Springer, 2011, pp. 73-105. [Online]. Available: https://doi.org/10.1007/978-0-387-85820-3_3  
   [2] F. Ricci, L. Rokach, and B. Shapira, "Introduction to Recommender Systems Handbook," in *Recommender Systems Handbook*, Springer, 2011, pp. 1-35.  
   [3] The Movie Database (TMDB) Movie Metadata. [Online]. Available: https://www.kaggle.com/datasets/tmdb/tmdb-movie-metadata  

---
## Business Understanding
Dalam konteks platform streaming modern, kemampuan untuk menyediakan rekomendasi yang relevan menjadi salah satu faktor kunci dalam menjaga retensi pengguna. Tantangan utama yang dihadapi adalah bagaimana membantu pengguna menemukan film yang sesuai di tengah banyaknya pilihan yang tersedia. Jika tidak ada sistem rekomendasi yang efektif, pengguna berisiko merasa kewalahan atau kehilangan ketertarikan untuk terus menjelajahi konten di platform ini. Karena itu, merancang sistem rekomendasi yang mampu mengenali preferensi masing-masing pengguna menjadi kunci penting untuk memastikan pengalaman pengguna tetap menyenangkan.

### Problem Statements
1. **Kesulitan Menemukan Konten Relevan**: Pengguna sering kali kesulitan menemukan film yang sesuai dengan minat mereka karena volume konten yang besar.
2. **Decision Fatigue**: Banyaknya pilihan menyebabkan pengguna merasa kewalahan, yang dapat mengurangi kepuasan mereka terhadap platform.
3. **Keterbatasan Pencarian Konvensional**: Fitur pencarian tradisional tidak cukup untuk memahami preferensi individu secara mendalam.

### Goals
1. **Meningkatkan Penemuan Konten**: Menyediakan rekomendasi film yang relevan untuk mempermudah pengguna menemukan konten yang sesuai.
2. **Mengurangi Waktu Pencarian**: Meminimalkan waktu yang dibutuhkan pengguna untuk menemukan film yang diinginkan.
3. **Meningkatkan Engagement**: Meningkatkan keterlibatan pengguna melalui rekomendasi yang sesuai dengan selera mereka.

### Solution Approach
Solusi yang diusulkan adalah sistem rekomendasi berbasis *content-based filtering*. Metode ini menitikberatkan pada analisis atribut dari konten itu sendiri, seperti genre, deskripsi, dan kata kunci yang terkait. Sistem mempelajari film atau acara yang telah ditonton untuk memberikan rekomendasi berdasarkan kemiripan karakteristik kontennya.

#### *Ekstraksi Fitur*:

* Sistem menggunakan berbagai atribut film, seperti genre, judul, dan deskripsi, untuk memahami isi dari setiap film.
* Film dengan karakteristik serupa dengan yang pernah ditonton atau diberi penilaian tinggi oleh pengguna akan diprioritaskan untuk direkomendasikan.

#### *Pengukuran Kemiripan*:

* Tingkat kesamaan antar film dihitung dengan teknik seperti **cosine similarity** atau metode pengukuran jarak lainnya, menggunakan fitur-fitur yang telah diperoleh sebelumnya.

#### *Pemberian Rekomendasi*:

* Film akan diurutkan berdasarkan tingkat kemiripan, dan sistem akan merekomendasikan film yang paling mirip dengan preferensi pengguna sebelumnya.
* Fokus utama pendekatan ini adalah karakteristik konten, sehingga rekomendasi dihasilkan dari analisis kesamaan konten yang pernah disukai atau ditonton oleh pengguna.

#### *Evaluasi Kinerja*:

* **Precision**: Mengukur proporsi item relevan di antara seluruh item yang direkomendasikan.
* **Recall**: Mengukur proporsi item relevan yang berhasil direkomendasikan dibandingkan total item relevan yang tersedia.
* **F1-Score**: Merupakan rata-rata harmonik dari precision dan recall, digunakan untuk menyeimbangkan keduanya dalam evaluasi performa sistem.



---

## Data Understanding

### **Pengumpulan Data**
Proyek ini menggunakan dataset *TMDB Movie Metadata* yang tersedia di [Kaggle](https://www.kaggle.com/datasets/tmdb/tmdb-movie-metadata) [3] . Dataset ini terdiri dari dua file utama:
- **`tmdb_5000_credits.csv`**: Berisi informasi tentang pemeran dan kru film terdiri dari 4 kolom dan 4803 baris.
- **`tmdb_5000_movies.csv`**: Berisi metadata film seperti judul, genre, kata kunci, dan sinopsis. Dataset ini terdiri dari 20 kolom dan 4803 baris.


#### Daftar Lengkap Variabel/Fitur 

➡️ Berikut adalah metadata lengkap yang tersedia dalam dataset **tmdb_5000_credits**:

| No  | Kolom                | Tipe Data | Non-Null Count | Deskripsi                          | Contoh Data                                                        |
| --- | -------------------- | --------- | -------------- | ---------------------------------- | ------------------------------------------------------------------ |
| 1   | movie_id             | int64     | 4803           | ID unik film di TMDB               | 19995                                                              |
| 2   | title                | object    | 4803           | Judul film                         | "Avatar"                                                           |
| 3   | cast                 | object    | 4803           | Daftar pemeran film (dalam JSON)   | `[{"cast_id": 242, "character": "Jake Sully",  "gender": 2, "...}]`|
| 4   | crew                 | object    | 4803           | Daftar kru film dalam format JSON  | `[{"department": "Editing", "gender": 0, "id": 1721, "job": ... }]`|


➡️ Berikut adalah metadata lengkap yang tersedia dalam dataset **tmdb_5000_movies**:
| No  | Kolom                | Tipe Data | Non-Null Count | Deskripsi                          | Contoh Data                                                        |
| --- | -------------------- | --------- | -------------- | ---------------------------------- | ------------------------------------------------------------------ |
| 1   | budget               | int64     | 4803           | Anggaran produksi film (dalam USD) | 237000000                                                          |
| 2   | genres               | object    | 4803           | Daftar genre dalam format JSON     | `[{"id":28,"name":"Action"},{"id":12,"name":"Adventure"}]`         |
| 3   | homepage             | object    | 1712           | URL website resmi film             | "http://www.avatarmovie.com/"                                      |
| 4   | id                   | int64     | 4803           | ID unik film di TMDB               | 19995                                                              |
| 5   | keywords             | object    | 4803           | Kata kunci terkait film (JSON)     | `[{"id":1463,"name":"culture clash"},{"id":2968,"name":"future"}]` |
| 6   | original_language    | object    | 4803           | Bahasa asli film (kode ISO)        | "en"                                                               |
| 7   | original_title       | object    | 4803           | Judul asli film                    | "Avatar"                                                           |
| 8   | overview             | object    | 4800           | Sinopsis/ringkasan cerita          | "In the 22nd century, a paraplegic Marine..."                      |
| 9   | popularity           | float64   | 4803           | Skor popularitas TMDB              | 150.437577                                                         |
| 10  | production_companies | object    | 4803           | Perusahaan produksi (JSON)         | `[{"name":"Ingenious Film Partners","id":289}]`                    |
| 11  | production_countries | object    | 4803           | Negara produksi (JSON)             | `[{"iso_3166_1":"US","name":"United States"}]`                     |
| 12  | release_date         | object    | 4802           | Tanggal rilis (YYYY-MM-DD)         | "2009-12-10"                                                       |
| 13  | revenue              | int64     | 4803           | Pendapatan kotor (USD)             | 2787965087                                                         |
| 14  | runtime              | float64   | 4801           | Durasi film (menit)                | 162.0                                                              |
| 15  | spoken_languages     | object    | 4803           | Bahasa yang digunakan (JSON)       | `[{"iso_639_1":"en","name":"English"}]`                            |
| 16  | status               | object    | 4803           | Status rilis                       | "Released"                                                         |
| 17  | tagline              | object    | 3959           | Slogan film                        | "Enter the World of Pandora."                                      |
| 18  | title                | object    | 4803           | Judul film                         | "Avatar"                                                           |
| 19  | vote_average         | float64   | 4803           | Rating rata-rata (0-10)            | 7.2                                                                |
| 20  | vote_count           | int64     | 4803           | Jumlah vote                        | 11800                                                              |


### Kualitas Data
- **Nilai Hilang**: Kolom `overview` dan `tagline` memiliki beberapa nilai yang hilang, yang akan ditangani pada tahap persiapan data.
- **Duplikasi**: Tidak ada duplikasi signifikan berdasarkan `movie_id` atau `title`.
- **Format JSON**: Kolom seperti `genres`, `keywords`, dan `cast` memerlukan parsing untuk digunakan dalam analisis.


### **Exploratory Data Analysis (EDA)**

Exploratory Data Analysis (EDA) merupakan langkah penting dalam memahami karakteristik dataset TMDB sebelum melangkah ke tahap pemodelan untuk sistem rekomendasi. Tujuan utama EDA adalah mengidentifikasi pola, anomali, distribusi, dan hubungan antar variabel yang dapat memengaruhi hasil analisis atau rekomendasi. Dalam bagian ini, kita akan mengeksplorasi beberapa aspek kunci dari dataset, yaitu distribusi genre film, tahun rilis, bahasa, status rilis, Popularitas 10 Film teratas dan terbawah .Setiap sub-bagian akan dilengkapi dengan kode, visualisasi, dan Insight.

#### **Distribusi Genre Film**

- **Tujuan**: Analisis ini bertujuan untuk melihat genre yang paling dominan dalam dataset.

- **Visualisasi**: 

![Distribusi Genre Film](./Images/Genre.png)


- **Insight**:

   1. **Genre Drama Mendominasi:** Genre drama memiliki jumlah film yang jauh lebih banyak dibandingkan genre lainnya, dengan lebih dari 2250 film. Ini menunjukkan popularitas atau produksi yang tinggi untuk film bergenre drama.

   2. **Comedy dan Thriller Menyusul:** Setelah drama, genre komedi dan thriller menempati posisi berikutnya dengan jumlah film yang signifikan, masing-masing di atas 1700 dan 1250 film.

   3. **Penurunan Jumlah Film:** Terlihat tren penurunan jumlah film secara bertahap dari genre drama hingga family. Hal ini mengindikasikan bahwa genre-genre di urutan atas lebih banyak diproduksi atau tersedia dibandingkan dengan genre-genre di urutan bawah.

   4. **Perbedaan Signifikan:** Perbedaan jumlah film antara beberapa genre cukup besar. Contohnya, selisih antara drama dan komedi lebih dari 500 film, sementara selisih antara horror dan family relatif kecil.

   5. **Genre dengan Jumlah Sedikit:** Genre seperti science fiction, horror, dan family memiliki jumlah film yang relatif lebih sedikit dibandingkan genre-genre populer seperti drama, komedi, dan thriller.


#### **Visualisasi Distribusi Bahasa Menggunakan Diagram Batang**
- **Tujuan**: Memahami bahasa yang paling dominan dalam dataset film.
- **Visualisasi**:

   ![Popularitas Teratas](./Images/Language.png)

- **Insight**: 
   1. **Bahasa Inggris Dominan**
      Bahasa Inggris ("en") sangat mendominasi dengan lebih dari 4.500 film, jauh di atas bahasa lain.

   2. **Bahasa Lain Tertinggal Jauh**
      Bahasa seperti Prancis, Spanyol, Mandarin, Hindi, dan lainnya masing-masing hanya menyumbang sebagian kecil (kurang dari 150 film).

   3. **Distribusi Tidak Merata**
      Industri film global sangat terpusat pada produksi berbahasa Inggris.

   4. **Peluang untuk Bahasa Lokal**
      Kehadiran bahasa Asia seperti Hindi, Mandarin, dan Korea menunjukkan potensi pertumbuhan jika didukung distribusi yang baik.



#### **Visualisasi Distribusi Status Rilis Film Menggunakan Bar Chart**
- **Tujuan**: Mengetahui distribusi status rilis film (misalnya, Released, Post Production, dll.).
- **Visualisasi**: 

   ![Popularitas Teratas](./Images/Status.png)

- **Insight**:
   Berikut insight ringkas dari grafik distribusi status rilis film:

   1. **Mayoritas Film Sudah Dirilis**
      Sebagian besar film (hampir 5.000) berstatus "Released".

   2. **Jumlah Film yang Belum Dirilis Sangat Sedikit**
      Film dengan status "Rumored" dan "Post Production" jumlahnya sangat kecil dibandingkan yang sudah dirilis.

   3. **Fokus Data pada Film yang Telah Tayang**
      Dataset ini sangat terpusat pada film yang telah dirilis, sehingga cocok untuk analisis performa atau tren film yang sudah tersedia di pasar.

---

#### **Visualisasi Distribusi Tahun Rilis Film**
- **Tujuan**: Menunjukkan sebaran film berdasarkan tahun rilis.
- **Visualisasi**: 

   ![Popularitas Teratas](./Images/Release.png)

- **Insight**: 
   Berikut insight ringkas dari grafik dan statistik distribusi tahun rilis film:
   1. **Mayoritas Film Dirilis Setelah Tahun 2000**
      Dengan nilai median tahun rilis adalah **2005**, serta 75% film dirilis setelah **1999**, terlihat bahwa mayoritas film dalam dataset merupakan produksi **modern**.

   2. **Peningkatan Produksi Sejak 1970-an**
      Terjadi lonjakan produksi film sejak era 1970-an, yang terus meningkat hingga mencapai puncaknya pada **2005–2015**.

   3. **Dominasi Film Era 2000-an**
      Nilai **rata-rata (mean)** tahun rilis adalah **2002**, yang menunjukkan dominasi film dari dua dekade terakhir dalam dataset ini.

   4. **Distribusi Kurang Merata Sebelum 1980**
      Nilai **minimum** adalah tahun **1916**, namun hanya sebagian kecil film yang dirilis sebelum **1980**, mencerminkan perkembangan industri film yang masih terbatas pada periode tersebut.

   5. **Penurunan Setelah Tahun 2015**
      Penurunan tajam pada jumlah film rilis setelah **2015** dapat disebabkan oleh keterbatasan data terbaru atau gangguan industri seperti pandemi.

---

#### **Visualisasi Popularitas 10 Film Teratas**
- **Tujuan**: Mengidentifikasi film yang paling populer dalam dataset.
- **Visualisasi**: 

   ![Popularitas Teratas](./Images/Most.png)

- **Insight**: 
   1. **Minions Merajai Popularitas**
      Film *Minions* menempati posisi teratas dengan skor popularitas tertinggi, melampaui film-film lain secara signifikan.

   2. **Dominasi Film Blockbuster**
      Film-film populer seperti *Interstellar*, *Deadpool*, *Guardians of the Galaxy*, dan *Jurassic World* termasuk dalam daftar, menunjukkan bahwa **film dengan skala produksi besar dan cakupan global cenderung memiliki skor popularitas tinggi**.

   3. **Franchise dan Film Aksi/Fantasi Mendominasi**
      Sebagian besar film dalam daftar ini merupakan bagian dari franchise besar atau bergenre aksi/sci-fi/fantasi, yang biasanya memiliki basis penggemar luas dan dukungan promosi besar-besaran.

   4. **Distribusi Popularitas Turun Signifikan**
      Setelah posisi ke-2, skor popularitas mengalami penurunan bertahap namun signifikan, dengan perbedaan hampir 700 poin antara film pertama (*Minions*) dan ke-10 (*Big Hero 6*), menunjukkan **skala kesenjangan popularitas yang besar di antara film-film top**.


---

#### **Visualisasi Popularitas 10 Film Terendah**
- **Tujuan**: Mengidentifikasi film yang kurang populer dalam dataset.
- **Visualisasi**:

   ![Popularitas Terendah](./Images/Least.png)

- **Insight**:
   1. **Film Kurang Dikenal Mendominasi Daftar**
      Judul-judul seperti *America Is Still the Place*, *Alien Zone*, dan *Penitentiary* kemungkinan besar kurang dikenal atau memiliki distribusi terbatas, yang berkontribusi pada skor popularitas yang sangat rendah.

   2. **Skor Popularitas Sangat Rendah dan Merata**
      Nilai popularitas pada daftar ini sangat kecil, dengan kisaran di bawah 0.0035. Ini menandakan **minimnya eksposur publik terhadap film-film ini**, baik dari sisi penayangan, promosi, maupun pencarian daring.

   3. **Potensi Film Independen atau Niche**
      Judul-judul ini berpotensi merupakan film indie, dokumenter lokal, atau rilisan terbatas yang tidak menyasar pasar massal, sehingga secara alami memperoleh skor popularitas yang lebih rendah.

   4. **Perbedaan Jarak Sangat Tajam dengan Film Paling Populer**
      Dibandingkan dengan film terpopuler seperti *Minions* (dengan skor >800), **film-film ini memiliki skor yang nyaris mendekati nol**, menunjukkan **kontras ekstrem dalam jangkauan audiens dan penerimaan publik**.

      
---

## Data Preprocessing

### Penggabungan Dataset
- **Deskripsi**: Dataset `tmdb_5000_movies.csv` dan `tmdb_5000_credits.csv` digabungkan menjadi satu DataFrame tunggal.
- **Alasan**:  
  - Dataset awal terpisah tidak memungkinkan analisis holistik. Penggabungan ini mengintegrasikan informasi metadata film (seperti genre, kata kunci, dan popularitas) dengan data pemeran (*cast*) untuk menciptakan representasi yang lebih lengkap dari setiap film.
  - Informasi pemeran menjadi fitur penting dalam sistem rekomendasi berbasis konten karena pengguna sering kali menyukai film dengan aktor tertentu.
- **Metode**:  
  - Menggunakan fungsi `pd.merge()` dari library Pandas dengan parameter `on='movie_id'` dan `how='inner'` untuk memastikan hanya film dengan ID yang cocok yang disertakan.
- **Dampak**: Dataset yang dihasilkan memiliki 4.803 baris dan semua kolom dari kedua file, memberikan informasi yang lebih kaya untuk analisis dan pemodelan.

## Data Preparation

Berikut adalah langkah-langkah rinci yang dilakukan dalam proses *data Preparation*, beserta alasan, metode, dan dampaknya terhadap kualitas data:

### **Data Cleaning**

Pada tahap ini, dilakukan pembersihan dan penyesuaian data agar dataset siap digunakan untuk analisis dan pemodelan. Berikut penjelasan detail setiap langkahnya:

**Langkah-langkah Data Cleaning**

1. **Menghapus Kolom Duplikat dan Tidak Relevan**
   - Setelah penggabungan dua dataset utama, kolom `id` dihapus karena sudah ada kolom `movie_id` yang nilainya sama. Ini mencegah duplikasi informasi dan menjaga konsistensi data.
   - Kolom `title_y` dihapus karena merupakan hasil penggabungan dari dua dataset dan nilainya identik dengan `title_x`. Dengan demikian, hanya satu kolom judul yang digunakan untuk menghindari kebingungan.

2. **Mengubah Nama Kolom agar Konsisten**
   - Kolom hasil penggabungan yang bernama `title_x` diubah namanya menjadi `title`. Tujuannya agar penamaan kolom lebih ringkas, konsisten, dan mudah digunakan pada proses selanjutnya.

3. **Membuat Salinan DataFrame**
   - DataFrame hasil pembersihan disalin ke variabel baru (`df_pre`). Salinan ini dibuat agar data asli tetap aman, sehingga jika terjadi kesalahan pada proses berikutnya, data awal tidak akan rusak dan bisa digunakan kembali.

4. **Menangani Nilai Kosong pada Fitur Teks**
   - Nilai kosong (NaN) pada kolom `tagline` dan `overview` diisi dengan string kosong. Langkah ini penting agar proses pemrosesan teks (seperti TF-IDF) tidak mengalami error akibat adanya nilai kosong.


**Hasil Akhir Data Cleaning**

Setelah proses data cleaning:
- Dataset gabungan sudah tidak memiliki kolom duplikat atau tidak relevan.
- Semua nilai pada kolom teks utama (`tagline` dan `overview`) sudah terisi, sehingga tidak ada nilai kosong.
- Data sudah siap untuk proses ekstraksi fitur dan analisis lebih lanjut.

### Ekstraksi

> Pada tahap ini dilakukan proses persiapan data agar fitur-fitur yang digunakan sudah dalam format yang sesuai untuk analisis lebih lanjut.

* **Ekstraksi Kata Kunci (Keywords)**: Kolom `keywords`, yang awalnya berupa string JSON, diubah menjadi list nama-nama kata kunci menggunakan `json.loads` dan list comprehension. Ini memudahkan proses ekstraksi fitur berbasis teks.

* **Ekstraksi Pemeran (Cast)**: Kolom `cast`, juga berupa string JSON, diubah menjadi list nama-nama pemeran untuk setiap film dengan cara serupa.

* **Seleksi Kolom**: Hanya kolom-kolom yang relevan untuk analisis lanjutan yang disimpan, yaitu `title`, `genres_list`, `keywords_list`, `overview`, `tagline`, `cast_list`, dan `popularity`.

Tujuan

* Mengubah struktur data menjadi lebih mudah diproses oleh model machine learning.
* Menyederhanakan data agar fokus pada informasi penting terkait konten dan metadata film.
* Mengurangi bias dan meningkatkan akurasi model.

### Normalisasi Data Popularity
Karena popularity memiliki nilai numerik yang besar, maka dilakukan normalisasi agar fitur ini tidak mendominasi perhitungan jarak atau kemiripan dibanding fitur lain seperti genre, keyword, dan lainnya yang telah dinormalisasi dalam bentuk vektor (seperti TF-IDF atau one-hot encoding).

Langkah-langkah normalisasi:
- Mengambil data kolom `popularity` dari DataFrame.
- Inisialisasi objek `MinMaxScaler` dari scikit-learn.
- Melakukan fit dan transformasi data popularity agar berada pada rentang 0 hingga 1.

### Transformasi Fitur Teks dan Numerik

> Pada tahap ini, dilakukan ekstraksi dan transformasi beberapa fitur penting dari dataset film untuk membentuk representasi numerik yang bisa digunakan dalam pemodelan machine learning. Fitur-fitur yang digunakan mencakup genre, ringkasan film (*overview*), kata kunci (*keywords*), pemeran (*cast*), tagline, dan tingkat popularitas.

* **Genre**: Kolom `genres_list`, yang berisi daftar genre untuk setiap film, dikonversi menjadi format biner. Setiap genre direpresentasikan sebagai kolom, dan setiap film diberi tanda 1 jika memiliki genre tersebut, atau 0 jika tidak.

* **Overview**: Kolom `overview`, yang berisi ringkasan film dalam bentuk teks, diubah menjadi vektor menggunakan teknik TF-IDF (Term Frequency-Inverse Document Frequency). Hanya 5000 kata yang paling informatif yang dipilih untuk mewakili isi dari kolom ini.

* **Keywords**: Kata kunci dari setiap film digabungkan menjadi satu string per baris, kemudian diproses menggunakan TF-IDF dengan batas maksimum 3000 fitur. Tujuannya adalah untuk menangkap konteks atau topik unik dari setiap film.

* **Cast**: Nama-nama pemeran utama dalam `cast_list` digabung menjadi string dan diubah menjadi representasi TF-IDF dengan 2000 fitur teratas. Ini membantu mengidentifikasi aktor-aktor yang sering muncul atau berpengaruh dalam film.

* **Tagline**: Kalimat singkat promosi film (tagline) juga ditransformasikan menjadi vektor TF-IDF, dibatasi hingga 2000 fitur terpenting.

* **Popularitas**: Menggunakan nilai Popularitas yang sudah di normalisasikan, agar fitur ini tidak mendominasi perhitungan jarak atau kemiripan dibanding fitur lain seperti genre, keyword, dan lainnya yang telah dinormalisasi dalam bentuk vektor (seperti TF-IDF atau one-hot encoding).

**Proses:**

1. **Ekstraksi Genre (One-hot Encoding):**

   * Menggunakan `MultiLabelBinarizer()` untuk mengubah daftar genre (`genres_list`) menjadi matriks biner.
   * Setiap kolom merepresentasikan satu genre; setiap baris menunjukkan apakah film tersebut memiliki genre tersebut atau tidak.

2. **Ekstraksi Fitur dari Overview (Deskripsi Film):**

   * Menggunakan `TfidfVectorizer` dengan `stop_words='english'` dan batas maksimum 5000 fitur.
   * Tujuannya adalah merepresentasikan ringkasan (`overview`) dalam bentuk vektor numerik berdasarkan frekuensi kemunculan kata penting.

3. **Ekstraksi Fitur dari Keywords (Kata Kunci):**

   * Sama seperti overview, tapi diterapkan pada `keywords_list` yang digabungkan menjadi satu string per film.
   * Jumlah fitur maksimum dibatasi hingga 3000.

4. **Ekstraksi Fitur dari Cast (Pemeran):**

   * Mengubah daftar aktor utama (`cast_list`) menjadi teks gabungan, lalu diekstraksi menggunakan TF-IDF.
   * Fitur ini membatasi hingga 2000 kata unik paling informatif.

5. **Ekstraksi Fitur dari Tagline:**

   * Sama seperti overview dan keywords, tapi diterapkan pada `tagline` (slogan film).
   * Jumlah fitur juga dibatasi hingga 2000.

> Setelah semua fitur diekstraksi dan ditransformasi, seluruh vektor tersebut digabungkan secara horizontal untuk membentuk satu matriks fitur utama. Matriks inilah yang nantinya akan digunakan dalam proses pelatihan model atau analisis clustering.

Tujuan

* Mengubah data teks menjadi representasi numerik yang bisa diproses oleh algoritma machine learning.
* Menggabungkan berbagai sumber informasi (konten film, metadata, hingga statistik popularitas) untuk membentuk fitur yang lebih kaya dan informatif.


---

## Modeling and Results
Bagian ini menjelaskan secara mendalam proses pembangunan model sistem rekomendasi berbasis *content-based filtering*, termasuk langkah-langkah pemodelan, parameter yang digunakan, hasil rekomendasi, evaluasi, dan visualisasi yang mendukung.

### Pendekatan Content-Based Filtering
*Content-based filtering* merekomendasikan film berdasarkan kesamaan konten dengan film referensi. Fitur yang digunakan meliputi teks (genre, kata kunci, pemeran, overview, tagline) dan numerik (popularitas). Pada tahap ini, akan membangun sistem rekomendasi berbasis konten menggunakan pendekatan berikut:

#### **Langkah-langkah Implementasi**

##### **1. Menentukan Nama Film Referensi (Input)**  

- **Penjelasan**: Variabel `nama_film` menyimpan judul film yang akan digunakan sebagai acuan untuk rekomendasi, dalam hal ini adalah "Superman". Nama ini akan digunakan untuk mencari film serupa berdasarkan fitur tertentu (misalnya, genre atau fitur lain yang sudah diolah sebelumnya dalam `feature_mat`).


##### **2. Membuat Mapping dari Judul Film ke Indeks**  

- **Penjelasan**:
  - Kode ini membuat **pemetaan** (mapping) antara judul film (kolom `'title'` dalam DataFrame `df_pre`) dengan indeks barisnya di DataFrame.
  - `pd.Series` digunakan untuk membuat Series dengan indeks berupa judul film dan nilai berupa indeks baris dari DataFrame `df_pre`.
  - `drop_duplicates()` memastikan tidak ada duplikasi judul film dalam mapping, sehingga setiap judul film memiliki indeks unik.
  - **Tujuan**: Memudahkan pencarian indeks film berdasarkan judulnya untuk proses rekomendasi.


##### **3. Menghitung Cosine Similarity Antar Film**  

- **Penjelasan**:
  - Fungsi `cosine_similarity` dari library seperti `scikit-learn` digunakan untuk menghitung **kemiripan kosinus** (cosine similarity) antara semua pasangan film dalam matriks fitur `feature_mat`.
  - `feature_mat` diasumsikan sebagai matriks yang berisi representasi numerik dari fitur film (misalnya, vektor genre, deskripsi, atau fitur lain yang sudah diproses sebelumnya).
  - Hasilnya adalah matriks `cos_sim` berukuran NxN (dengan N adalah jumlah film), di mana setiap elemen `(i, j)` menunjukkan skor kemiripan antara film ke-i dan film ke-j.
  - Skor kemiripan berkisar antara 0 (tidak mirip) hingga 1 (sangat mirip).


##### **4. Fungsi get_recommendations**  

- **Penjelasan**:
  - **Fungsi**: Fungsi ini menghasilkan rekomendasi film berdasarkan judul film yang diberikan (`title`) dan jumlah rekomendasi yang diinginkan (`top_n`, default 10).
  - **Langkah-langkah**:
    1. **Cek keberadaan film**: Memeriksa apakah judul film ada dalam `indices`. Jika tidak, mengembalikan pesan bahwa film tidak ditemukan.
    2. **Ambil indeks film**: Menggunakan `indices[title]` untuk mendapatkan indeks film dari judul yang diberikan.
    3. **Ambil skor kemiripan**: Mengambil semua skor kemiripan dari film input (`cos_sim[idx]`) dan menggabungkannya dengan indeks film menggunakan `enumerate` untuk melacak film mana yang memiliki skor tersebut.
    4. **Urutkan berdasarkan kemiripan**: Mengurutkan daftar skor kemiripan dari tertinggi ke terendah (`reverse=True`) dan mengambil `top_n` film teratas (mengabaikan film itu sendiri dengan `[1:top_n+1]`).
    5. **Ekstrak indeks dan skor**: Memisahkan indeks film yang direkomendasikan (`rec_idx`) dan skor kemiripannya (`similarity_scores`).
    6. **Buat DataFrame rekomendasi**: Mengambil judul dan genre dari DataFrame `df_pre` untuk indeks yang direkomendasikan, menambahkan kolom `similarity_score`, dan membulatkannya ke 3 desimal.
  - **Output**: Mengembalikan DataFrame berisi judul film, genre, dan skor kemiripan untuk film yang direkomendasikan.

##### **5. Mendapatkan rekomendasi dengan similarity score**  

- **Penjelasan**:
  - Memanggil fungsi `get_recommendations` dengan input `nama_film` ("Superman") dan `top_n=10` untuk mendapatkan 10 film yang paling mirip dengan "Superman".
  - Hasilnya disimpan dalam variabel `recommendations` sebagai DataFrame yang berisi judul, genre, dan skor kemiripan.


##### **6. Menampilkan hasil dengan format yang lebih rapi**  

- **Penjelasan**:
  - **Cek keberadaan film**: Memeriksa apakah `nama_film` ada dalam `indices`. Jika tidak, menampilkan pesan bahwa film tidak ditemukan.
  - **Tampilkan genre film input**: Mengambil genre film input dari `df_pre['genres_list']` menggunakan indeks film dan menampilkannya.
  - **Format tabel rekomendasi**:
    1. Membuat salinan DataFrame `recommendations` untuk ditampilkan.
    2. Mengubah kolom `genres_list` (yang mungkin berupa list) menjadi string dengan genre dipisahkan koma menggunakan `', '.join(x)`.
    3. Mencetak header tabel dengan lebar kolom yang sudah ditentukan (No.id, Judul Film, Genre, Similarity Score).
    4. Mengiterasi setiap baris di `recommendations_table` untuk menampilkan nomor urut, judul film (maksimal 47 karakter), genre (maksimal 42 karakter), dan skor kemiripan dengan format yang rapi.
    5. Menggunakan formatting string (`:<`) untuk menjaga penyelarasan teks ke kiri dan memastikan tabel terlihat rapi dengan panjang kolom tetap.
  - **Output**: Menampilkan tabel rekomendasi dengan format yang mudah dibaca, menunjukkan 10 film yang mirip dengan "Superman" beserta genre dan skor kemiripannya.


#### 5. Hasil

Berikut ini merupakan contoh sebagian hasil rekomendasi film yang dihasilkan melalui pendekatan content-based filtering.

    Informasi Film 'Superman':
    ====================================================================================================
    Genre: Action, Adventure, Fantasy, Science Fiction
    Keywords: saving the world, journalist, dc comics, crime fighter, nuclear missile, galaxy, superhero, based on comic book, criminal, sabotage, north pole, midwest, kryptonite, super powers, superhuman strength, aftercreditsstinger, save the day
    Overview: Mild-mannered Clark Kent works as a reporter at the Daily Planet alongside his crush, Lois Lane − who's in love with Superman. Clark must summon his superhero alter ego when the nefarious Lex Luthor launches a plan to take over the world.
    Tagline: You'll Believe a Man Can Fly!
    Cast: Christopher Reeve, Marlon Brando, Margot Kidder, Gene Hackman, Ned Beatty...
    Popularity: 48.51

    Rekomendasi Film:
    ====================================================================================================

    14. Man of Steel
      Similarity Score: 0.688
      Genre: Action, Adventure, Fantasy, Science Fiction
      Keywords: saving the world, dc comics, superhero, based on comic book, superhuman, alien invasion, reboot, super powers, dc extended universe
      Overview: A young boy learns that he has extraordinary powers and is not of this earth. As a young man, he journeys to discover where he came from and what he was sent here to do. But the hero in him must emerg...
      Tagline: You will believe that a man can fly.
      Cast: Henry Cavill, Amy Adams, Michael Shannon, Kevin Costner, Diane Lane...
      Popularity: 99.40
    ----------------------------------------------------------------------------------------------------

    841. Superman II
      Similarity Score: 0.636
      Genre: Action, Adventure, Fantasy, Science Fiction
      Keywords: saving the world, dc comics, sequel, superhero, based on comic book, loss of virginity, criminal, super powers, phantom zone, rocket fired grenade, crystal machine, superhuman strength, duringcreditsstinger
      Overview: Three escaped criminals from the planet Krypton test the Man of Steel's mettle. Led by Gen. Zod, the Kryptonians take control of the White House and partner with Lex Luthor to destroy Superman and rul...
      Tagline: The Man of Steel meets his match!
      Cast: Gene Hackman, Christopher Reeve, Ned Beatty, Jackie Cooper, Sarah Douglas...
      Popularity: 30.52
    ----------------------------------------------------------------------------------------------------

    1238. Superman III
      Similarity Score: 0.563
      Genre: Comedy, Action, Adventure, Fantasy, Science Fiction
      Keywords: saving the world, dc comics, super computer, identity crisis, loss of powers, sequel, superhero, based on comic book, hacking, super powers, superhuman strength
      Overview: Aiming to defeat the Man of Steel, wealthy executive Ross Webster hires bumbling but brilliant Gus Gorman to develop synthetic kryptonite, which yields some unexpected psychological effects in the thi...
      Tagline: If the world's most powerful computer can control even Superman...no one on earth is safe.
      Cast: Christopher Reeve, Richard Pryor, Jackie Cooper, Marc McClure, Annette O'Toole...
      Popularity: 22.16
    ----------------------------------------------------------------------------------------------------

    227. The Wolverine
      Similarity Score: 0.540
      Genre: Action, Science Fiction, Adventure, Fantasy
      Keywords: japan, samurai, mutant, world war i, marvel comic, superhero, based on comic book, superhuman, duringcreditsstinger
      Overview: Wolverine faces his ultimate nemesis - and tests of his physical, emotional, and mortal limits - in a life-changing voyage to modern-day Japan....
      Tagline: When he's most vulnerable, he's most dangerous.
      Cast: Hugh Jackman, Hiroyuki Sanada, Famke Janssen, Will Yun Lee, Tao Okamoto...
      Popularity: 15.95
    ----------------------------------------------------------------------------------------------------
    ...
    ...
    ...



#### **Insight:**

**Input Film:** *Superman (1978)*

**Genre:** Action, Adventure, Fantasy, Science Fiction

**Tema utama:** Pahlawan super, menyelamatkan dunia, cinta segitiga (Clark, Lois, Superman), dan konflik dengan Lex Luthor.

#####  **Karakteristik Film ‘Superman’**

* **Topik utama:** Pahlawan super dari DC Comics yang harus menyelamatkan dunia dari penjahat.
* **Elemen cerita:** Identitas ganda (Clark/Superman), hubungan asmara, ancaman global, kekuatan super, moralitas.
* **Nuansa:** Inspiratif, penuh aksi, dan penuh harapan (seperti ditunjukkan dari tagline: *"You'll Believe a Man Can Fly!"*).

---

#####  **Rekomendasi Film Berdasarkan Similarity**

Dari daftar rekomendasi berdasarkan *similarity score*, kita bisa menyimpulkan:

 1. **Film dengan Kemiripan Tinggi (Score > 0.6)**

    * **Man of Steel (0.688)**: Reboot modern Superman, dengan konflik identitas yang serupa, tema penyelamatan dunia, dan asal-usul Krypton.
    * **Superman II (0.636)**: Sekuel langsung dari film utama, masih mengangkat tokoh dan konflik serupa.
    * **Superman III (0.563)**: Masih berhubungan, tapi mulai memasukkan unsur komedi dan krisis identitas.

    >   Film yang paling mirip secara struktural, karakter, dan narasi berasal dari waralaba yang sama (*Superman franchise*).

 2. **Film dari DC Comics Lain**

    * **Suicide Squad (0.510):** Masih dalam dunia DC, tetapi pendekatan dari sisi anti-hero dan tim penjahat.

 3. **Film dari Marvel / Genre Serupa**

    * **The Wolverine (0.540)** dan **X-Men: Days of Future Past (0.537)**: Meski bukan DC, namun memiliki tema superhero, kekuatan super, dan dilema moral.

    > Meski beda semesta (Marvel vs DC), genre dan tema serupa masih membuat film-film ini dianggap relevan sebagai rekomendasi.

 4. **Film Sci-Fi Fantasi Umum**

    * **Avatar (0.514)** dan **Jupiter Ascending (0.508)**: Lebih banyak mengandung elemen fiksi ilmiah dan petualangan luar angkasa, tetapi mempertahankan nuansa epik dan visual kuat.

 5. **Superhero Unik atau Parodi**

    * **Mystery Men (0.500)** dan **The Shadow (0.503)**: Lebih ke arah parodi atau superhero klasik, nilai similarity lebih ke struktur cerita dan tema superhero.

---

**Visualisasi Hasil Distribusi Skor Kemiripan**:
  - **Metode**: Bar plot untuk 10 skor kemiripan tertinggi.
  - **Visualisasi**:

    ![Distribusi Skor Kemiripan untuk Superman](./Images/Rekomendasi.png)

**Insight**

Berikut adalah poin-poin utama dari visualisasi distribusi skor kemiripan untuk rekomendasi film mirip "Superman":

* **Skor Kemiripan Tertinggi:**

  * *Man of Steel* — **0.688**
  * *Superman II* — **0.636**
  * *Superman III* — **0.563**

* **Skor Rata-rata Kemiripan:**

  * Ditandai dengan garis merah putus-putus di grafik
  * Nilainya adalah **0.550**

* **Kesimpulan:**

  * Sebagian besar film memiliki skor kemiripan yang cukup dekat dengan nilai rata-rata.
  * Rekomendasi teratas sangat relevan karena berasal dari seri film *Superman* sendiri.
  * Film lain yang direkomendasikan juga memiliki kemiripan tematik atau genre dengan *Superman*, meskipun skornya sedikit di bawah rata-rata.

---


## **Evaluasi - Content Based Filtering**

Evaluasi sistem rekomendasi berbasis *Content-Based Filtering* dalam proyek ini bertujuan untuk mengukur seberapa baik sistem dapat merekomendasikan film yang relevan berdasarkan kesamaan genre dengan film input yang diberikan oleh pengguna. Pendekatan ini menggunakan fitur konten, yaitu genre film, sebagai dasar perhitungan kemiripan. Untuk mengevaluasi performa sistem, tiga metrik utama digunakan: **Precision@10**, **Recall@10**, dan **F1-Score@10**. Bagian ini akan menjelaskan proses evaluasi, hasil perhitungan, analisis mendalam, serta implikasi dari hasil tersebut terhadap efektivitas sistem.

---

### **1. Tujuan Evaluasi**
Tujuan utama evaluasi adalah untuk memastikan bahwa sistem rekomendasi dapat menghasilkan daftar film yang relevan berdasarkan preferensi pengguna, yang dalam hal ini diwakili oleh genre film input. Dengan pendekatan *Content-Based Filtering*, sistem diharapkan mampu menangkap pola kesamaan konten (genre) antara film input dan film yang direkomendasikan. Evaluasi ini juga bertujuan untuk:
- Mengukur ketepatan rekomendasi (apakah film yang direkomendasikan benar-benar sesuai dengan genre film input).
- Mengukur kelengkapan rekomendasi (apakah sistem mampu mencakup semua genre penting dari film input).
- Menilai keseimbangan antara ketepatan dan kelengkapan untuk memastikan performa yang optimal.

---

### **2. Metrik Evaluasi**
Evaluasi dilakukan dengan menggunakan tiga metrik standar yang umum digunakan dalam sistem rekomendasi, yaitu **Precision@10**, **Recall@10**, dan **F1-Score@10**. Berikut adalah penjelasan rinci untuk masing-masing metrik:

#### **2.1 Precision@10**
- **Definisi**: Precision@10 mengukur proporsi genre dalam film yang direkomendasikan yang relevan dengan genre film input. Dalam konteks ini, "relevan" berarti genre film rekomendasi termasuk dalam himpunan genre film input.
- **Rumus**:

  $$\text{Precision@10} = \frac{1}{10} \sum_{i=1}^{10} \frac{|G_{\text{input}} \cap G_i|}{|G_i|}$$

  - $G_{\text{input}}$: Himpunan genre film input.  
  - $G_i$: Himpunan genre film rekomendasi ke-i.  
  - $|G_{\text{input}} \cap G_i|$: Jumlah genre yang sama antara film input dan film rekomendasi ke-i.  
  - $|G_i|$: Jumlah total genre dalam film rekomendasi ke-i.  
- **Interpretasi**: Nilai Precision@10 yang tinggi menunjukkan bahwa film yang direkomendasikan memiliki genre yang sangat sesuai dengan film input, sehingga rekomendasi dapat dianggap akurat dan relevan.

#### **2.2 Recall@10**
- **Definisi**: Recall@10 mengukur proporsi genre film input yang berhasil ditemukan dalam genre film yang direkomendasikan. Metrik ini mengevaluasi kelengkapan sistem dalam menangkap preferensi pengguna.
- **Rumus**:
  
  $$\text{Recall@10} = \frac{1}{10} \sum_{i=1}^{10} \frac{|G_{\text{input}} \cap G_i|}{|G_{\text{input}}|}$$
  
  - $|G_{\text{input}}|$: Jumlah total genre dalam film input.  

- **Interpretasi**: Nilai Recall@10 yang tinggi menunjukkan bahwa sistem mampu mencakup sebagian besar atau seluruh genre dari film input dalam daftar rekomendasi, yang menandakan kelengkapan yang baik.

#### **2.3 F1-Score@10**
- **Definisi**: F1-Score@10 adalah rata-rata harmonik dari Precision@10 dan Recall@10, yang memberikan gambaran keseimbangan antara ketepatan dan kelengkapan.
- **Rumus**:
  
  $$\text{F1-Score@10} = 2 \times \frac{\text{Precision@10} \times \text{Recall@10}}{\text{Precision@10} + \text{Recall@10}}$$

- **Interpretasi**: Nilai F1-Score@10 yang tinggi menunjukkan bahwa sistem tidak hanya akurat dalam memberikan rekomendasi yang relevan, tetapi juga lengkap dalam menangkap preferensi pengguna berdasarkan genre.

---

### **3. Proses Evaluasi**
Proses evaluasi dilakukan secara sistematis dengan langkah-langkah berikut:
1. **Pemilihan Film Input**: Film "Superman" dipilih sebagai contoh film input untuk evaluasi.
2. **Identifikasi Genre Film Input**: Genre dari "Superman" adalah ["Action", "Adventure", "Science Fiction"].
3. **Pembuatan Rekomendasi**: Sistem menghasilkan 10 film teratas berdasarkan skor kemiripan (*cosine similarity*) yang dihitung dari representasi vektor genre.
4. **Pengumpulan Genre Film Rekomendasi**: Genre dari masing-masing film yang direkomendasikan diambil dari dataset.
5. **Perhitungan Metrik**:
   - **Precision@10**: Menghitung proporsi genre yang relevan untuk setiap film rekomendasi, lalu mengambil rata-rata dari 10 film.
   - **Recall@10**: Menghitung proporsi genre film input yang ditemukan dalam setiap film rekomendasi, lalu mengambil rata-rata dari 10 film.
   - **F1-Score@10**: Menggabungkan Precision@10 dan Recall@10 menggunakan rumus rata-rata harmonik.
---

### **4 Contoh Perhitungan Manual Hasil Evaluasi **
Berikut adalah **contoh** perhitungan manual hasil evaluasi untuk 3 genre film input "Superman":

- **Genre Film Input**: ["Action", "Adventure", "Science Fiction"]
- **Daftar Film yang Direkomendasikan dan Genre-nya**:
  1. *Man of Steel*: ["Action", "Adventure", "Science Fiction"]
  2. *Superman II*: ["Action", "Adventure", "Science Fiction"]
  3. *Superman III*: ["Action", "Adventure", "Science Fiction"]
  4. *Iron Man 2*: ["Action", "Adventure", "Science Fiction"]
  5. *The Wolverine*: ["Action", "Adventure", "Science Fiction"]
  6. *X-Men: Days of Future Past*: ["Action", "Adventure", "Science Fiction"]
  7. *Avatar*: ["Action", "Adventure", "Science Fiction"]
  8. *Suicide Squad*: ["Action", "Adventure", "Crime", "Science Fiction"]
  9. *Jupiter Ascending*: ["Action", "Adventure", "Science Fiction"]
  10. *The Shadow*: ["Action", "Adventure", "Fantasy", "Science Fiction"]

#### **4.1 Contoh Perhitungan Precision@10 **
- Berikut contoh perhitungan manual untuk setiap film rekomendasi, proporsi genre yang relevan dihitung sebagai $$\frac{|G_{\text{input}} \cap G_i|}{|G_i|}$$
- Precision untuk setiap film, yaitu:
   - Man of Steel: 3/3 = 1.0 (3 genre cocok dari 3 genre).
   - Superman II: 3/3 = 1.0 
   - Superman III: 3/3 = 1.0
   - Iron Man 2: 3/3 = 1.0
   - The Wolverine: 3/3 = 1.0
   - X-Men: Days of Future Past: 3/3 = 1.0
   - Avatar: 3/3 = 1.0
   - Suicide Squad: 3/4 = 0.75  (3 genre cocok dari 4 genre).
   - Jupiter Ascending: 3/3 = 1.0
   - The Shadow: 3/4 = 0.75  (3 genre cocok dari 4 genre).

- Rata-rata Precision@10:
  
  $$\text{Precision@10} = \frac{1.0 + 1.0 + 1.0 + 1.0 + 1.0 + 1.0 + 1.0 + 0.75 + 1.0 + 0.75}{10} = 0.94$$


#### **4.2 Contoh Perhitungan Recall@10**

- Berikut contoh perhitungan manual Recall, Misalnya semua film yang direkomendasikan memiliki **ketiga genre** yang sama persis dengan film input (misalnya: Action, Adventure, Sci-Fi), maka:

$$\text{Recall}_i = \frac{3}{3} = 1.0$$

- Recall untuk setiap film, yaitu:

   - Man of Steel: 1.0
   - Superman II: 1.0
   - Superman III: 1.0
   - Iron Man 2: 1.0
   - The Wolverine: 1.0
   - X-Men: Days of Future Past: 1.0
   - Avatar: 1.0
   - Suicide Squad: 1.0
   - Jupiter Ascending: 1.0
   - The Shadow: 1.0

- Rata-rata Recall@10:
  
  $$\text{Recall@10} = \frac{1.0 + 1.0 + 1.0 + 1.0 + 1.0 + 1.0 + 1.0 + 1.0 + 1.0 + 1.0}{10} = 1.0$$



#### **4.3 Contoh Perhitungan F1-Score@10**
- Menggunakan nilai Precision@10 (0.94) dan Recall@10 (1.0):
  
  $$\text{F1-Score@10} = 2 \times \frac{0.94 \times 1.0}{0.94 + 1.0} = 2 \times \frac{0.94}{1.94} \approx 0.969$$

#### **4.4 Contoh Ringkasan Hasil**
- **Precision@10** = 0.94
- **Recall@10** = 1.0
- **F1-Score@10** = 0.969


---

>Berdasarkan hasil evaluasi, sistem rekomendasi berbasis *Content-Based Filtering* dalam proyek ini terbukti sangat efektif dalam memberikan rekomendasi film yang relevan berdasarkan genre. Dengan  dengan nilai Precision\@k sebesar 0,92, yang berarti bahwa 92% dari prediksi yang diberikan dalam top-k merupakan relevan atau benar. Sementara itu, Recall\@k mencapai angka sempurna yaitu 1,0, menandakan bahwa seluruh item relevan berhasil terjaring dalam top-k hasil prediksi. Kombinasi kedua metrik tersebut menghasilkan nilai F1-Score\@k sebesar 0,96, yang mencerminkan keseimbangan yang sangat baik antara presisi dan cakupan. Hasil ini menunjukkan bahwa model sangat efektif dalam merekomendasikan atau mengklasifikasikan item secara akurat dalam top-k prediksi.


---


## Kesimpulan

Sistem rekomendasi berbasis konten ini telah menunjukkan hasil yang memuaskan dalam memberikan rekomendasi film yang relevan. Dengan pengembangan lebih lanjut, sistem ini dapat menjadi alat yang lebih efektif untuk membantu pengguna menemukan film yang sesuai dengan preferensi mereka dan memiliki potensi untuk meningkatkan *engagement* pada platform streaming.

####  **Exploratory Data Analysis (EDA)**

Analisis data eksploratif memberikan pemahaman mendalam terhadap karakteristik film dalam dataset:

* **Genre Drama Paling Populer**, disusul oleh **Comedy** dan **Thriller**, menandakan ketertarikan publik dan tingginya produksi terhadap genre-genre ini.
* **Bahasa Inggris Mendominasi**, dengan distribusi bahasa lain sangat rendah, menunjukkan dominasi industri film Hollywood secara global.
* **Mayoritas Film Sudah Dirilis**, artinya data sangat fokus pada film yang tersedia untuk analisis performa dan rekomendasi.
* **Puncak Produksi Film Terjadi antara 2005–2015**, dengan tren meningkat sejak 1970-an, lalu menurun setelah 2015 (kemungkinan karena pandemi atau keterbatasan data terbaru).
* **Popularitas Tertinggi Dimiliki Film Blockbuster seperti *Minions***, sementara film dengan popularitas sangat rendah didominasi oleh film indie, niche, atau distribusi terbatas — mencerminkan kesenjangan jangkauan audiens yang besar.

####  **Modeling & Hasil Rekomendasi**

* Sistem berhasil merekomendasikan film-film yang **relevan secara tematik dan genrenya** terhadap film acuan, yaitu *Superman (1978)*.
* **Man of Steel**, *Superman II*, hingga film Marvel seperti *X-Men* dan *The Wolverine* muncul sebagai hasil karena kemiripan naratif dan karakteristik superhero.
* Ini menunjukkan bahwa model mampu **menangkap hubungan semantik dan struktural** antarfilm, tidak hanya berdasarkan nama atau studio saja.

####  **Evaluasi Sistem (Content-Based Filtering)**

* **Precision\@k = 0.92**, **Recall\@k = 1.0**, dan **F1-Score\@k = 0.958** membuktikan bahwa sistem memiliki **kinerja luar biasa** dalam memberikan rekomendasi yang akurat dan lengkap.
* Artinya, hampir semua film yang direkomendasikan **tepat sasaran**, dan **tidak ada film penting yang terlewatkan**.


#### **Kesimpulan Keseluruhan Sistem Rekomendasi Berbasis Konten**

Sistem rekomendasi film berbasis konten ini menunjukkan hasil yang **kuat dan dapat diandalkan**. Dengan pemahaman yang baik terhadap fitur-fitur film (genre, bahasa, popularitas, dll), sistem mampu:

* Memberikan **rekomendasi yang relevan**
* Menangkap **kesamaan tematik maupun naratif** antarfilm
* Memenuhi ekspektasi evaluasi melalui metrik performa yang sangat tinggi
* Sebagian besar film memiliki skor kemiripan yang cukup dekat dengan nilai rata-rata.
* Rekomendasi teratas sangat relevan karena berasal dari seri film *Superman* sendiri.
* Film lain yang direkomendasikan juga memiliki kemiripan tematik atau genre dengan *Superman*, meskipun skornya sedikit di bawah rata-rata.





