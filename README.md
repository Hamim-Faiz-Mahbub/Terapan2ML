# Laporan Proyek Machine Learning Terapan Akhir - Hamim Faiz Mahbub

## Latar Belakang

<p align="center">
  <img src="https://github.com/user-attachments/assets/023fe79b-84e0-4477-b92e-24c669e26dc0"/>
</p>


- Di era informasi digital yang melimpah, pengguna seringkali dihadapkan pada information overload, terutama dalam memilih konten hiburan seperti film. Platform streaming dan database film menawarkan ribuan hingga jutaan judul, membuat pengguna kesulitan menemukan film yang benar-benar sesuai dengan preferensi unik mereka. Sistem rekomendasi hadir sebagai solusi untuk mengatasi tantangan ini dengan menyaring dan memprioritaskan item yang paling relevan bagi setiap individu. [1]

- Kemampuan untuk memberikan rekomendasi film yang akurat dan personal tidak hanya meningkatkan pengalaman dan kepuasan pengguna, tetapi juga menjadi strategi bisnis yang vital bagi penyedia layanan. Rekomendasi yang baik dapat meningkatkan engagement pengguna (waktu yang dihabiskan di platform, jumlah film yang ditonton), mendorong loyalitas, dan pada akhirnya meningkatkan pendapatan. [2] Oleh karena itu, pengembangan sistem rekomendasi yang efektif merupakan investasi penting dalam industri hiburan digital.

- Proyek ini bertujuan untuk membangun dan mengevaluasi dua pendekatan fundamental dalam sistem rekomendasi—Content-Based Filtering (CBF) dan Collaborative Filtering (CF)—menggunakan dataset publik MovieLens. CBF merekomendasikan film berdasarkan kemiripan atribut konten (seperti genre dan tag) dengan film yang disukai pengguna sebelumnya. Sementara itu, CF memanfaatkan pola perilaku kolektif pengguna (rating) untuk menemukan pengguna dengan selera serupa atau item yang sering dinilai bersama, lalu memberikan rekomendasi berdasarkan pola tersebut. Dengan mengimplementasikan dan membandingkan kedua metode ini, proyek ini diharapkan dapat memberikan pemahaman praktis tentang cara kerja, kekuatan, dan keterbatasan masing-masing pendekatan dalam konteks rekomendasi film.

## Business Understanding

Domain proyek ini adalah sistem rekomendasi untuk platform film. Dalam industri hiburan digital yang sangat kompetitif, kemampuan untuk menyajikan konten yang relevan dan menarik bagi setiap pengguna adalah kunci untuk mempertahankan dan meningkatkan basis pengguna. Sistem rekomendasi film bertujuan untuk mempersonalisasi pengalaman menonton dengan menyarankan film yang kemungkinan besar akan disukai pengguna, berdasarkan preferensi historis mereka dan/atau perilaku pengguna lain.

### Pernyataan Masalah (Problem Statements)
- Berdasarkan domain proyek, beberapa permasalahan utama yang ingin dipecahkan adalah:

- Bagaimana cara efektif menyarankan film kepada pengguna berdasarkan atribut konten film (seperti genre dan tag) yang sesuai dengan film-film yang pernah mereka nikmati sebelumnya?

- Bagaimana cara memanfaatkan data rating historis dari banyak pengguna untuk mengidentifikasi film-film yang mungkin disukai oleh pengguna tertentu, bahkan jika pengguna tersebut belum pernah menonton atau memberi rating pada film-film tersebut?

- Di antara dua pendekatan utama (Content-Based Filtering dan Collaborative Filtering dengan SVD), pendekatan mana yang menghasilkan rekomendasi yang lebih akurat dan relevan untuk dataset MovieLens yang digunakan, berdasarkan metrik evaluasi yang ditetapkan?

- Faktor-faktor apa saja (misalnya, fitur konten untuk CBF, parameter model untuk CF) yang paling berpengaruh terhadap kualitas rekomendasi yang dihasilkan?

### Goals
Proyek ini memiliki tujuan-tujuan berikut:

- Melakukan analisis data eksploratif (EDA) yang komprehensif pada dataset MovieLens untuk mengidentifikasi pola, distribusi, dan kualitas data yang relevan untuk sistem rekomendasi.

- Melaksanakan pra-pemrosesan data yang diperlukan untuk membersihkan dan mentransformasi data agar siap digunakan oleh model CBF dan CF.

- Mengimplementasikan model Content-Based Filtering (CBF) yang menggunakan representasi TF-IDF dari fitur konten film (genre dan tag) dan Cosine Similarity untuk mengukur kemiripan.

- Mengimplementasikan model Collaborative Filtering (CF) menggunakan algoritma Singular Value Decomposition (SVD) dari library Surprise, termasuk melakukan hyperparameter tuning menggunakan GridSearchCV untuk optimasi.

- Mengevaluasi kinerja kedua model menggunakan metrik kuantitatif (RMSE, MAE, Precision@k, Recall@k untuk CF; Cosine Similarity untuk CBF) dan analisis kualitatif terhadap contoh rekomendasi yang dihasilkan.

- Menganalisis dan membandingkan hasil dari kedua pendekatan, serta mendiskusikan kelebihan dan kekurangan masing-masing dalam konteks proyek ini.

### Pendekatan Solusi (Solution Statements/Approach)
Untuk mencapai tujuan-tujuan di atas, proyek ini akan mengadopsi pendekatan solusi sebagai berikut:

- Eksplorasi dan Pemahaman Data (Data Understanding):

Sumber dan Deskripsi Data: Mengidentifikasi sumber dataset (MovieLens Small Latest) dan mendeskripsikan secara detail semua file yang digunakan (movies.csv, ratings.csv, links.csv, tags.csv) beserta semua variabel/fitur di dalamnya.

Analisis Data Eksploratif (EDA): Melakukan EDA yang mendalam yang mencakup pemeriksaan bentuk data, tipe data, nilai unik, statistik deskriptif, identifikasi dan visualisasi nilai hilang, pemeriksaan data duplikat, serta analisis spesifik seperti distribusi rating, popularitas genre dan tag (termasuk WordCloud), konsistensi data antar file, film dengan rating tertinggi per genre, genre dengan rata-rata rating tertinggi, dan distribusi jumlah rating per film. Semua analisis akan disertai dengan interpretasi output dan visualisasi yang relevan.

- Persiapan Data (Data Preparation):

Melakukan pra-pemrosesan yang diperlukan berdasarkan temuan EDA.

Untuk CBF: Membersihkan kolom genres (menangani (no genres listed) dan NaN, memformat ulang pemisah, mengubah ke huruf kecil). Memproses dan mengagregasi tags per movieId. Membuat feature_soup dengan menggabungkan genre dan tag yang telah diproses.

Untuk CF: Menyiapkan data rating (userId, movieId, rating) dalam format yang sesuai untuk library Surprise.

Melakukan pengecekan akhir untuk nilai hilang pada data yang telah diproses.

- Pengembangan Model (Modeling):

Content-Based Filtering (CBF): Menggunakan TfidfVectorizer untuk mengubah feature_soup menjadi matriks TF-IDF. Menghitung matriks cosine_similarity global antar film. Mengembangkan fungsi get_content_based_recommendations yang menghasilkan top-N rekomendasi.

Collaborative Filtering (CF): Menggunakan library Surprise dan algoritma SVD. Membagi data menjadi set pelatihan dan pengujian. Melatih model SVD baseline. Melakukan hyperparameter tuning menggunakan GridSearchCV untuk SVD. Melatih model SVD final dengan parameter terbaik. Mengembangkan fungsi get_top_n_collaborative_recommendations_tuned.

- Evaluasi Model (Evaluation):

CBF: Evaluasi kualitatif dengan menganalisis contoh rekomendasi dan skor cosine_similarity. Menjelaskan cara kerja Cosine Similarity.

CF: Mengevaluasi model SVD (baseline dan tuned) menggunakan metrik RMSE dan MAE. Mengevaluasi kualitas rekomendasi top-N menggunakan Precision@k dan Recall@k. Menjelaskan cara kerja metrik RMSE, MAE, Precision@k, dan Recall@k, termasuk formulanya.

Melakukan perbandingan antara model CBF dan CF.

## Data Understanding
Pemahaman data adalah fondasi penting dalam setiap proyek machine learning. Tahap ini melibatkan eksplorasi mendalam terhadap dataset yang digunakan untuk mengidentifikasi karakteristik, kualitas, dan pola yang relevan.

### Sumber Data
- Dataset yang digunakan dalam proyek ini adalah MovieLens Latest Datasets (small), yang disediakan oleh GroupLens Research dari University of Minnesota. Dataset ini merupakan versi terbaru yang lebih kecil dan dirancang untuk tujuan edukasi dan eksperimen.

Judul Dataset: MovieLens Latest Datasets (small)

Sumber Unduhan Utama: [GroupLens - MovieLens Latest Datasets](https://grouplens.org/datasets/movielens/latest/)

Sumber Yang Digunakan: https://www.kaggle.com/datasets/aparajitasingh15/imdb-data?select=links.csv

Penyedia: GroupLens Research, University of Minnesota.

Tanggal Pembaruan Terakhir Dataset (Versi "small" yang digunakan): September 2018.

Lisensi: Dataset MovieLens dilisensikan untuk penggunaan riset dan non-komersial. 

### Deskripsi Umum Dataset
1. Dataset MovieLens (small) terdiri dari empat file utama dalam format CSV:

2. movies.csv: Berisi informasi detail untuk setiap film.

3. ratings.csv: Berisi data rating yang diberikan oleh pengguna untuk film.

4. tags.csv: Berisi tag atau label teks yang diberikan oleh pengguna untuk film.

5. links.csv: Berisi ID eksternal yang menghubungkan film di dataset ini dengan database film lain seperti IMDb dan TMDB.

- Kombinasi file-file ini menyediakan data yang kaya untuk membangun sistem rekomendasi berbasis konten (menggunakan informasi dari movies.csv dan tags.csv) dan berbasis kolaboratif (menggunakan informasi dari ratings.csv).

### Deskripsi Variabel (Fitur)
Berikut adalah deskripsi detail untuk setiap variabel (kolom) dalam masing-masing file CSV yang digunakan, sesuai dengan output dari df.info() dan pemeriksaan manual :

- movies.csv

movieId (Tipe: int64): Pengenal unik numerik untuk setiap film. Ini adalah kunci utama untuk tabel ini dan akan digunakan untuk menghubungkan dengan data di tabel lain.

title (Tipe: object / string): Judul resmi film. Seringkali menyertakan tahun rilis film dalam tanda kurung di akhir judul (misalnya, "Toy Story (1995)").

genres (Tipe: object / string): Satu atau lebih genre yang dikaitkan dengan film. Jika sebuah film memiliki multiple genre, genre-genre tersebut dipisahkan oleh karakter pipa | (misalnya, "Adventure|Animation|Children|Comedy|Fantasy"). Terdapat beberapa entri dengan nilai (no genres listed) yang menandakan tidak ada informasi genre spesifik.

- ratings.csv

userId (Tipe: int64): Pengenal unik numerik untuk setiap pengguna yang memberikan rating.

movieId (Tipe: int64): Pengenal unik film yang diberi rating. Ini adalah foreign key yang merujuk ke movieId di movies.csv.

rating (Tipe: float64): Rating numerik yang diberikan oleh pengguna untuk film. Skala rating dalam dataset ini adalah dari 0.5 hingga 5.0, dengan interval 0.5.

timestamp (Tipe: int64): Waktu (dalam format Unix epoch, yaitu detik sejak 1 Januari 1970 UTC) ketika rating diberikan oleh pengguna. Meskipun tidak digunakan secara langsung dalam model dasar kita, fitur ini bisa berguna untuk analisis temporal atau model yang lebih canggih.

- tags.csv

userId (Tipe: int64): Pengenal unik pengguna yang memberikan tag.

movieId (Tipe: int64): Pengenal unik film yang diberi tag. Ini adalah foreign key yang merujuk ke movieId di movies.csv.

tag (Tipe: object / string): Label teks atau kata kunci yang diterapkan pengguna pada film. Tag ini bersifat subjektif dan bisa berupa deskripsi konten, opini, atau kategori buatan pengguna.

timestamp (Tipe: int64): Waktu (dalam format Unix epoch) ketika tag diberikan.

- links.csv

movieId (Tipe: int64): Pengenal unik film, sesuai dengan movieId di movies.csv.

imdbId (Tipe: int64): Pengenal unik film di database IMDb (Internet Movie Database).

tmdbId (Tipe: float64): Pengenal unik film di database TMDB (The Movie Database). Tipe datanya float64 karena adanya beberapa nilai yang hilang (NaN) dalam kolom ini.

- Pemahaman detail terhadap setiap variabel ini sangat penting untuk menentukan bagaimana data akan diproses dan fitur mana yang akan digunakan dalam pembangunan model rekomendasi.

### Jumlah Nilai Unik untuk Kolom Kunci 

- Nilai Unik di movies_df:
  Jumlah movieId unik: 9742

- Nilai Unik di ratings_df:
  Jumlah movieId unik: 9724
  Jumlah userId unik: 610

- Nilai Unik di links_df:
  Jumlah movieId unik: 9742

- Nilai Unik di tags_df:
  Jumlah movieId unik: 1572
  Jumlah userId unik: 58
  Jumlah tag unik: 1589
  
movies_df: Jumlah movieId unik: 9742 (sesuai jumlah film).

ratings_df: Jumlah movieId unik: 9724 (beberapa film tidak ada ratingnya), Jumlah userId unik: 610 (jumlah pengguna yang memberi rating).

links_df: Jumlah movieId unik: 9742 (konsisten dengan movies_df).

tags_df: Jumlah movieId unik: 1572 (hanya sebagian film yang diberi tag), Jumlah userId unik: 58 (hanya sebagian kecil pengguna yang memberi tag), Jumlah tag unik: 1589 (variasi tag yang diberikan).

### Statistik Deskriptif `ratings_df` 
    
        |       | userId         | movieId        | rating         | timestamp      |
        | :---- | :------------- | :------------- | :------------- | :------------- |
        | count | 100836.000000  | 100836.000000  | 100836.000000  | 1.008360e+05   |
        | mean  | 326.127564     | 19435.295718   | 3.501557       | 1.205946e+09   |
        | std   | 182.618491     | 35530.987199   | 1.042529       | 2.162610e+08   |
        | min   | 1.000000       | 1.000000       | 0.500000       | 8.281246e+08   |
        | 25%   | 177.000000     | 1199.000000    | 3.000000       | 1.019124e+09   |
        | 50%   | 325.000000     | 2991.000000    | 3.500000       | 1.186087e+09   |
        | 75%   | 477.000000     | 8122.000000    | 4.000000       | 1.435994e+09   |
        | max   | 610.000000     | 193609.000000  | 5.000000       | 1.537799e+09   |

- Rata-rata rating adalah 3.50 pada skala 0.5-5.0, dengan median juga 3.5. Ini menunjukkan distribusi rating yang cukup terpusat dengan sedikit kecenderungan ke nilai yang lebih tinggi. Standar deviasi 1.04 menunjukkan variasi yang wajar dalam rating.

### Baris dan Kolom Dataframes 

- Bentuk Data movies_df: (9742, 3)
Movies_df memiliki 9742 baris dengan 3 attribute/kolom
- Bentuk Data ratings_df: (100836, 4)
Ratings_df memiliki 100836 baris dengan 4 attribute/kolom
- Bentuk Data links_df: (9742, 3)
Dataframe links_df memiliki 9742 dan 3 attribute seperti Movies_df
- Bentuk Data tags_df: (3683, 4)
tags_df memiliki 3683 baris dengan 4 attribute

 Informasi dasar bentuk dataframe ini digunakan untuk memberikan gambaran awal ukuran data

 ### Pemeriksaan Kualitas Data


* **Pemeriksaan Nilai Hilang :**
   
        * `movies_df`, `ratings_df`, `tags_df`: Tidak ada nilai hilang.
        * `links_df`: Kolom `tmdbId` memiliki 8 nilai hilang.
        * Visualisasi `missingno.matrix()` untuk `links_df`  menampilkan matriks dengan garis putih tipis pada kolom `tmdbId` yang menandakan 8 nilai hilang tersebut.
     Sebagian besar dataset bersih dari nilai hilang. Kekosongan pada `tmdbId` (8 dari 9742 film) dianggap minor dan tidak akan berdampak signifikan pada model dasar yang direncanakan, karena `tmdbId` tidak digunakan secara langsung sebagai fitur utama.
    * `![Image](https://github.com/user-attachments/assets/757c2d36-a1f3-4d0b-a2d5-016d5b5c84e2)

* **Pemeriksaan Data Duplikat :**
    
        * `movies_df` (berdasarkan `movieId`): 0 duplikat.
        * `ratings_df` (baris penuh): 0 duplikat.
        * `tags_df` (berdasarkan `userId`, `movieId`, `tag`): 0 duplikat.
        * `links_df` (berdasarkan `movieId`): 0 duplikat.
    Tidak ditemukan data duplikat berdasarkan kriteria yang diperiksa. Ini menunjukkan kualitas data yang baik dari segi keunikan entri dan tidak memerlukan langkah penghapusan duplikat yang signifikan.

* **Pemeriksaan Genre `(no genres listed)` :**
  
    Dilakukan pengecekan untuk memastikan dataset film memiliki genre 
        * Jumlah film dengan genre `(no genres listed)`: 34
        * Persentase film dengan genre `(no genres listed)`: 0.35%
        * Contoh film: "La cravate (1957)", "Ben-hur (2016)", dll.
     Sejumlah kecil film (34) tidak memiliki informasi genre spesifik. Ini perlu ditangani di Data Preparation agar tidak dianggap sebagai genre valid "no genres listed".

* **Konsistensi Data Antar File :**
   
        * `movies_df` vs `links_df`: 0 `movieId` di `movies_df` yang tidak ada di `links_df`; 0 `movieId` di `links_df` yang tidak ada di `movies_df`.
        * `ratings_df` vs `movies_df`: Semua `movieId` di `ratings_df` ada di `movies_df`.
        * `tags_df` vs `movies_df`: Semua `movieId` di `tags_df` ada di `movies_df`.
     Integritas referensial untuk `movieId` terjaga dengan baik. Semua rating dan tag merujuk pada film yang valid di `movies_df`. Setiap film juga memiliki entri link.


### Analisis Univariat


* **Distribusi Rating Film :**
    * **Visualisasi:** `countplot` rating.

 Plot menunjukkan bahwa rating 4.0 adalah yang paling sering diberikan oleh pengguna (lebih dari 25.000 kali), diikuti oleh rating 3.0 (sekitar 20.000 kali) dan 5.0 (lebih dari 10.000 kali). Rating 3.5 juga cukup signifikan. Sebaliknya, rating yang lebih rendah seperti 0.5, 1.0, dan 1.5 jauh lebih jarang diberikan, dengan frekuensi di bawah 5.000. Ini mengindikasikan adanya bias positif dalam pemberian rating oleh pengguna; pengguna lebih cenderung memberikan rating di atas rata-rata atau rating sempurna, dan lebih jarang memberikan rating yang sangat rendah. Distribusi ini tidak simetris dan condong ke kiri (left-skewed) jika melihat puncak di rating 4.0.
    * ![Image](https://github.com/user-attachments/assets/3c51021d-d58d-492e-bdbe-24648196b4e8)

* **Analisis Genre Film :**
* ![Image](https://github.com/user-attachments/assets/10b24dba-cbf7-4d6c-8b32-1ff6502c52ec)
        * **Top 10 Genre (misalnya, Drama: 4361 film, Comedy: 3756 film) menunjukkan dominasi genre tertentu.**
Genre Drama (4.361 film) dan Comedy (3.756 film) adalah dua genre yang paling banyak muncul dalam dataset. Frekuensi kedua genre ini masing-masing hampir dua kali lipat lebih banyak dibandingkan genre "Thriller" (1.894 film) yang berada di peringkat ketiga. Hal ini mengindikasikan bahwa mayoritas film dalam koleksi ini cenderung bergenre drama atau komedi, yang merupakan genre-genre mainstream dengan cakupan tema yang luas.
        * **Bar plot Top 20 Genre**
  Secara visual dan memungkinkan perbandingan frekuensi relatif antar genre. Setelah Drama dan Comedy, genre seperti Thriller (1.894 film) dan Action (1.828 film) menempati posisi tinggi, menunjukkan popularitas film-film dengan elemen ketegangan dan aksi. Genre Romance (1.596 film) dan Adventure (1.263 film) juga cukup umum, menandakan bahwa film dengan tema aksi, petualangan, dan kisah percintaan juga memiliki representasi yang signifikan dalam dataset, meskipun frekuensinya tidak setinggi drama atau komedi.
        * Genre seperti Crime (1.199 film), Sci-Fi (980 film), dan Horror (978 film) masing-masing memiliki frekuensi yang mendekati angka seribu, sementara Fantasy (779 film) sedikit di bawahnya. Hal ini menunjukkan bahwa sub-genre seperti thriller-kejahatan, fiksi ilmiah, dan horor cukup diminati dan memiliki banyak perwakilan film, tetapi tidak seumum genre-genre utama seperti drama, komedi, atau aksi.
        * Di luar sepuluh besar, kita menemukan genre-genre yang jauh lebih jarang ditemui, seperti Musical (334 film berdasarkan outpu), War (382 film), dan Documentary (440 film), hingga genre-genre yang sangat langka seperti IMAX (158 film), dan Film-Noir (87 film). Kategori `(no genres listed)` sendiri hanya ada sekitar 34 film. Frekuensi yang menurun drastis ini membentuk "ekor panjang" (*long-tail distribution*) di mana sejumlah kecil film masuk ke dalam kategori genre yang sangat spesifik atau kurang umum. Pemahaman distribusi genre ini penting untuk Content-Based Filtering karena akan mempengaruhi variasi fitur konten.
        * **Word Cloud genre representasi visual frekuensi genre**,
  di mana ukuran teks genre (seperti "Drama", "Comedy") sebanding dengan frekuensinya dalam dataset, secara intuitif menyoroti genre-genre yang paling dominan dan sering muncul.
    * `![Image](https://github.com/user-attachments/assets/32d6a68b-0aeb-4b6c-8aff-b44d460dd582)

* **Eksplorasi Tags :**
    
- Jumlah tag unik (1589) menunjukkan variasi deskriptor yang cukup kaya yang digunakan pengguna untuk melabeli atau mendeskripsikan film. Ini menandakan potensi fitur konten yang detail dari perspektif pengguna.
- Top 10 Tag (misalnya, "In Netflix queue" muncul 131 kali, "atmospheric" 36 kali, "thought-provoking" dan "superhero" masing-masing 24 kali) menunjukkan campuran antara tag yang berkaitan dengan perilaku pengguna (seperti menandai untuk ditonton nanti) dan tag yang lebih deskriptif mengenai konten, suasana, atau tema film.
- Word Cloud tag secara visual akan menyoroti tag-tag yang paling sering digunakan oleh komunitas pengguna, memberikan gambaran cepat tentang kata kunci populer yang terkait dengan film.
- Contoh tag untuk film "Pulp Fiction (1994)" (seperti "good dialogue", "cult film", "nonlinear", "violent", "Quentin Tarantino") menunjukkan bagaimana tag dapat memberikan deskripsi yang kaya, spesifik, dan terkadang subjektif mengenai berbagai aspek film, mulai dari kualitas dialog, status kultus, gaya naratif, konten kekerasan, hingga nama sutradara. Ini memperkaya pemahaman kita tentang bagaimana pengguna menginterpretasikan dan mengkategorikan film.
    * `![Image](https://github.com/user-attachments/assets/f4531b5d-5ce0-4cac-89b0-4708800bf66e)

## EDA MultiVariate


* **Distribusi Film Berdasarkan Rating Tertinggi per Genre :**

Untuk setiap genre (dengan minimal 10 penilaian), kita menemukan judul‐judul klasik dan favorit penonton yang meraih rata‐rata rating sangat tinggi (di atas 4,2).
    * `![Image](https://github.com/user-attachments/assets/015f07d2-8a01-4b6a-b4d4-f2ace55d1f82)
- Drama: Film seperti Secrets & Lies (1996) (4,59 dari 11 rating), Guess Who’s Coming to Dinner (1967) (4,55 dari 11), dan Paths of Glory (1957) (4,54 dari 12) menempati peringkat teratas menunjukkan kecenderungan pengguna memberi nilai tinggi pada drama‐drama klasik dengan tema mendalam.
    * `![Image](https://github.com/user-attachments/assets/293d6f0f-67cc-4857-a23e-221cfe5e1ad9)
- Comedy: Di urutan teratas muncul His Girl Friday (1940) (4,39 dari 14), It Happened One Night (1934) (4,32 dari 14), dan Philadelphia Story (1940) (4,31 dari 29), memotret selera penonton yang menghargai komedi lawas berkarakter kuat.
 * `![Image](https://github.com/user-attachments/assets/d0e7d7e0-1916-4ef2-887e-b1a8ec347bc8)
- Action: All Quiet on the Western Front (1930) (4,35 dari 10) dan Once Upon a Time in the West (1968) (4,31 dari 18) menunjukkan bahwa meski genre “aksi” sering dikaitkan dengan tontonan modern, penonton memberi rating tinggi pada film perang‐barat klasik; di sisi modern, Logan (2017) (4,28 dari 25) juga masuk lima besar, sedangkan Fight Club (1999) (4,27 dari 218) mendemonstrasikan popularitas dan kepercayaan pengguna pada film aksi‐thriller abad ke‐90.
  * `![Image](https://github.com/user-attachments/assets/c3afc144-d59b-4456-9097-deeca289caf3)
- Thriller: Selain Elite Squad (2007) (4,30 dari 10) dan Fight Club (1999) (4,27), ada Touch of Evil (1958) (4,26 dari 17) dan Rear Window (1954) (4,26 dari 84), memperlihatkan minat kuat pada thriller noir klasik, serta The Departed (2006) (4,25 dari 107) mewakili kesukaan penonton pada thriller modern yang berkarakter.
    * `![Image](https://github.com/user-attachments/assets/44f9ae3d-b59f-4f4d-8067-56a4d0da9230)
- Romance: Sekali lagi His Girl Friday dan It Happened One Night muncul—menunjukkan tumpang‐tindih antara Comedy dan Romance—diikuti oleh Sunset Blvd. (1950) (4,33 dari 27) dan Philadelphia Story (1940) (4,31), serta Harold and Maude (1971) (4,29 dari 26), yang bersama‐sama menegaskan bahwa film romantis klasik dengan naskah kuat cenderung mendapatkan rating tertinggi.

Secara keseluruhan, data ini menegaskan bahwa—terlepas dari hadirnya film‐film modern—film‐film klasik era 1930–1970 mendominasi puncak rata‐rata rating di berbagai genre, dan beberapa judul (“Fight Club,” “His Girl Friday,” “Philadelphia Story”) bahkan konsisten masuk ke dalam daftar top untuk lebih dari satu genre.


   
  


* **Genre dengan Rata-Rata Rating Tertinggi :**

Daftar Top 10 Genre berdasarkan rata-rata rating (misalnya, Film-Noir: 3.92, War: 3.81, Documentary: 3.80) dan bar plot yang menyertainya menunjukkan bahwa genre yang mungkin lebih "niche", dianggap lebih "serius", atau memiliki standar kualitas artistik yang tinggi cenderung mendapatkan rata-rata rating yang lebih tinggi dari pengguna yang menonton dan menilainya. Ini bisa jadi karena audiens genre tersebut lebih apresiatif, atau karena kualitas rata-rata film dalam genre tersebut memang lebih tinggi secara konsisten dibandingkan genre yang lebih mainstream dengan variasi kualitas yang lebih besar.
    * `![Image](https://github.com/user-attachments/assets/60d4d191-a03f-459e-9639-656d1e13cd97)

* **Distribusi Jumlah Rating per Film :**

  ![Image](https://github.com/user-attachments/assets/a2b4f16a-976a-4695-822a-8bdb0e7b6bfb)
Statistik deskriptif (rata-rata ~10.37 rating/film, median 3 rating/film, standar deviasi ~22.40, max 329 rating/film) menunjukkan bahwa distribusi jumlah rating per film sangat tidak merata. Rata-rata yang lebih tinggi dari median, serta standar deviasi yang besar, mengindikasikan adanya beberapa film yang menerima jumlah rating yang sangat tinggi (outlier positif).
   ![Image](https://github.com/user-attachments/assets/46c4c0d3-e6d4-4eb3-b3ea-b648c0c0c8d3)
* Histogram (dengan skala log pada sumbu Y) dan Boxplot (dengan skala log pada sumbu X) akan secara visual mengkonfirmasi distribusi *long-tail* yang sangat condong ke kanan. Ini berarti ada banyak sekali film yang hanya menerima sejumlah kecil rating (misalnya, 1, 2, atau 3 rating, seperti yang ditunjukkan oleh kuartil pertama dan median), sementara hanya segelintir film yang sangat populer dan sering dirating oleh banyak pengguna. Fenomena ini adalah karakteristik umum dari dataset rekomendasi dan mengindikasikan *data sparsity* yang signifikan, yang bisa menjadi tantangan untuk model Collaborative Filtering.


## 4. Data Preparation
Tahap ini bertujuan untuk membersihkan, mentransformasi, dan melakukan rekayasa fitur pada data agar siap digunakan sebagai input untuk model Content-Based Filtering (CBF) dan Collaborative Filtering (CF). 


### Persiapan Data untuk Content-Based Filtering

* **Tujuan:** Menggabungkan informasi konten dari `movies_df` (genre) dan `tags_df` (tag pengguna) menjadi satu representasi tekstual per film yang disebut `feature_soup`.
* **Langkah-langkah:**
    1.  **Inisialisasi dan Penyalinan :** DataFrame `movies_cbf_df` dibuat sebagai salinan dari `movies_df` untuk menghindari modifikasi data asli.
    2.  **Pembersihan dan Pemrosesan Kolom `genres`:**
        * Nilai `(no genres listed)` dan `NaN` pada kolom `genres` diganti dengan string kosong (`''`).
        * Karakter pipa `|` (pemisah antar genre) diganti dengan spasi.
        * Semua teks genre diubah menjadi huruf kecil untuk konsistensi.
        * Hasilnya disimpan dalam kolom baru `genres_processed`.
        * **Output Tahap Ini:** Sampel `movies_cbf_df` yang menunjukkan kolom `genres` asli dan `genres_processed` yang sudah bersih.
    3.  **Pemrosesan dan Penggabungan Data `tags` :**
        * DataFrame `tags_cbf_df` dibuat sebagai salinan dari `tags_df`.
        * Kolom `tag` diubah menjadi tipe data string, huruf kecil, dan spasi berlebih di awal/akhir dihilangkan. Hasilnya disimpan di `tag_processed`.
        * Tag diagregasi per `movieId`: semua tag unik untuk setiap film digabungkan menjadi satu string tunggal, dipisahkan spasi, dan diurutkan secara alfabetis. Hasilnya disimpan dalam kolom `tags_aggregated`.
        * Kolom `tags_aggregated` dari hasil agregasi tag kemudian digabungkan (`merge`) ke `movies_cbf_df` berdasarkan `movieId`. Jika sebuah film tidak memiliki tag, nilai `NaN` yang muncul setelah merge akan diisi dengan string kosong (`''`).
        * **Output Tahap Ini:** Sampel `movies_cbf_df` yang menunjukkan `movieId` dan `tags_aggregated` (untuk film yang memiliki tag).
    4.  **Pembuatan "Feature Soup" :**
        * Kolom `feature_soup` dibuat dengan menggabungkan string dari `genres_processed` dan `tags_aggregated`, dengan spasi sebagai pemisah.
        * Spasi ganda yang mungkin muncul (jika salah satu komponen kosong atau sudah ada spasi di akhir) dibersihkan menjadi spasi tunggal, dan spasi di awal/akhir `feature_soup` dihilangkan.
        * **Output Tahap Ini:** Sampel `movies_cbf_df` yang menunjukkan `movieId` dan `feature_soup` yang dihasilkan.
    5.  **Finalisasi `movies_for_cbf` :**
        * DataFrame `movies_for_cbf` dibuat dengan memilih hanya kolom `movieId`, `title`, dan `feature_soup` dari `movies_cbf_df`.
        * Indeks DataFrame di-reset. (Sesuai diskusi, `.drop_duplicates(subset=['movieId'])` tidak diterapkan di sini karena diasumsikan `movies_df` sudah unik berdasarkan `movieId` dari tahap EDA).
        * **Output Tahap Ini:** Sampel `movies_for_cbf` dan jumlah film unik yang siap untuk CBF (9742 film).
* **Hasil Akhir untuk CBF:** DataFrame `movies_for_cbf` berisi fitur konten gabungan yang siap untuk diubah menjadi representasi numerik oleh `TfidfVectorizer`.
####  Implementasi: TF-IDF dan Pembentukan Matriks Fitur


Langkah pertama dalam implementasi CBF adalah mengubah fitur konten tekstual (`feature_soup` yang telah dibuat pada tahap Data Preparation) menjadi representasi numerik yang dapat diproses oleh algoritma. Teknik yang digunakan adalah TF-IDF (Term Frequency-Inverse Document Frequency).

* **Proses:**

  1. **Inisialisasi `TfidfVectorizer`:** Objek `tfidf_vectorizer` dari `sklearn.feature_extraction.text` diinisialisasi. `TfidfVectorizer` akan mengonversi koleksi dokumen teks mentah menjadi matriks fitur TF-IDF.

     * **TF (Term Frequency):** Mengukur seberapa sering sebuah kata (term) muncul dalam sebuah dokumen (dalam kasus ini, `feature_soup` sebuah film).

     * **IDF (Inverse Document Frequency):** Mengukur seberapa penting sebuah kata dalam keseluruhan koleksi dokumen. Kata yang sering muncul di banyak dokumen akan memiliki bobot IDF yang lebih rendah, sedangkan kata yang jarang muncul tetapi spesifik pada beberapa dokumen akan memiliki bobot IDF yang lebih tinggi.

  2. **Penanganan `NaN` (Pencegahan):** Kolom `feature_soup` pada `movies_for_cbf` dipastikan tidak mengandung nilai `NaN` dengan menggantinya menggunakan string kosong (`''`). Ini penting karena `TfidfVectorizer` mengharapkan input berupa string.

  3. **Pembuatan Matriks TF-IDF:** Metode `fit_transform()` dari `tfidf_vectorizer` diterapkan pada kolom `feature_soup`.

     * `fit()`: Mempelajari kosakata (semua token unik dari genre dan tag) dari seluruh `feature_soup` dan menghitung bobot IDF untuk setiap token.

     * `transform()`: Mengubah setiap `feature_soup` film menjadi vektor numerik berdasarkan bobot TF-IDF dari token-token yang ada di dalamnya.

  * Hasil dari proses ini adalah `tfidf_matrix`, sebuah matriks sparse (jarang) di mana setiap baris merepresentasikan satu film dan setiap kolom merepresentasikan satu token (kata/fitur) unik dari kosakata yang dipelajari. Nilai dalam sel matriks adalah bobot TF-IDF dari token tersebut untuk film tersebut.

* **Output Sel :**

  * Pesan: "Menerapkan TfidfVectorizer pada 'feature_soup'"

  * `Bentuk matriks TF-IDF: (9742, 1746)`

  * Pesan: "Matriks TF-IDF (tfidf_matrix) dan objek TfidfVectorizer (tfidf_vectorizer) berhasil dibuat."

* **Penjelasan:**

  * Bentuk matriks `(9742, 1746)` menunjukkan bahwa dari 9742 film yang diproses, telah diekstraksi sebanyak 1.746 fitur kata unik (kombinasi dari semua genre dan tag yang ada setelah diproses). Setiap film kini direpresentasikan sebagai vektor dalam ruang berdimensi tinggi ini. Matriks `tfidf_matrix` ini menjadi dasar untuk menghitung kemiripan antar film. Objek `tfidf_vectorizer` yang telah di-"fit" juga disimpan karena berisi informasi kosakata dan bobot IDF yang dapat digunakan kembali jika ada data baru.


### Persiapan Data untuk Collaborative Filtering

* **Tujuan:** Menyiapkan data rating dalam format yang sesuai untuk digunakan oleh library `Surprise`.
* **Langkah-langkah:**
    1.  DataFrame `ratings_for_cf_df` dibuat dengan memilih hanya kolom `userId`, `movieId`, dan `rating` dari `ratings_df` asli. Kolom `timestamp` tidak digunakan untuk model SVD dasar ini.
* **Output Tahap Ini:** Pesan konfirmasi, jumlah total data rating yang akan digunakan (100.836), dan sampel `head(3)` dari `ratings_for_cf_df`.
* **Hasil Akhir untuk CF:** DataFrame `ratings_for_cf_df` yang berisi interaksi pengguna-item-rating, siap untuk dimuat ke dalam format dataset `Surprise`.

### Pengecekan Nilai Hilang (Final)

* **Tujuan:** Melakukan verifikasi akhir untuk memastikan tidak ada nilai `NaN` pada fitur-fitur kunci yang akan digunakan dalam pemodelan.
* **Langkah-langkah:**
    1.  Memeriksa jumlah nilai `NaN` pada kolom `feature_soup` di `movies_for_cbf`.
    2.  Memeriksa jumlah nilai `NaN` pada semua kolom di `ratings_for_cf_df`.
* **Output Tahap Ini:**
    * `Nilai hilang di kolom 'feature_soup' (CBF): 0`
    * Output untuk `ratings_for_cf_df` juga menunjukkan 0 nilai hilang untuk `userId`, `movieId`, dan `rating`.
* **Interpretasi:** Hasil ini mengonfirmasi bahwa fitur `feature_soup` untuk CBF dan kolom-kolom data untuk CF tidak mengandung nilai `NaN`, sehingga data telah siap sepenuhnya untuk tahap pemodelan. String kosong pada `feature_soup` (untuk film tanpa genre dan tag) akan ditangani dengan baik oleh `TfidfVectorizer`.

## 5. Modeling

Tahap ini merupakan inti dari proyek, di mana model-model sistem rekomendasi dikembangkan, dilatih, dan diuji untuk menghasilkan rekomendasi film. Dua pendekatan utama yang diimplementasikan adalah Content-Based Filtering (CBF) dan Collaborative Filtering (CF).

### Content-Based Filtering (CBF)

#### Konsep Dasar CBF

Content-Based Filtering (CBF) adalah teknik sistem rekomendasi yang menyarankan item kepada pengguna berdasarkan kemiripan antara atribut item tersebut dengan atribut item yang telah disukai atau berinteraksi dengan pengguna di masa lalu. Dalam konteks rekomendasi film, atribut ini bisa berupa genre, aktor, sutradara, kata kunci dari sinopsis, atau tag yang diberikan pengguna. Ide dasarnya adalah "jika Anda menyukai item X, Anda mungkin juga akan menyukai item Y yang mirip dengan X."

Proses CBF umumnya melibatkan langkah-langkah berikut:

1. **Representasi Item (Item Profiling):** Setiap item (film) direpresentasikan sebagai vektor fitur berdasarkan atribut kontennya.

2. **Representasi Pengguna (User Profiling):** Profil preferensi pengguna dibangun berdasarkan fitur konten dari item yang telah mereka sukai atau beri rating tinggi.

3. **Perhitungan Kemiripan:** Kemiripan antara profil pengguna dan profil item (atau antara item dengan item lain) dihitung menggunakan metrik seperti Cosine Similarity.

4. **Generasi Rekomendasi:** Item-item yang paling mirip dengan profil pengguna atau item yang disukai pengguna akan direkomendasikan.

Dalam proyek ini, kita akan fokus pada kemiripan item-item, di mana fitur konten utama adalah gabungan dari genre dan tag film.



#### Implementasi: Perhitungan Kemiripan dan Fungsi Rekomendasi


Setelah mendapatkan representasi vektor TF-IDF untuk setiap film, langkah selanjutnya adalah menghitung kemiripan antar film dan membuat fungsi untuk menghasilkan rekomendasi.

* **Perhitungan Matriks Kemiripan Kosinus :**

  * **Metode:** `cosine_similarity` dari `sklearn.metrics.pairwise` digunakan untuk menghitung kemiripan kosinus antara semua pasangan vektor film dalam `tfidf_matrix`.

  * **Proses:** `cosine_similarity(tfidf_matrix, tfidf_matrix)` akan menghasilkan matriks persegi (`cosine_sim_values`) berukuran (jumlah film x jumlah film). Setiap elemen `(i, j)` dalam matriks ini merepresentasikan skor kemiripan kosinus antara film ke-i dan film ke-j.

  * **DataFrame Kemiripan:** Matriks `cosine_sim_values` kemudian diubah menjadi DataFrame Pandas (`cosine_sim_df` atau `cosine_sim_df_eval` ) dengan judul film sebagai indeks dan kolom untuk memudahkan interpretasi dan pencarian.

  * **Output :**

    * `Bentuk matriks kemiripan kosinus: (9742, 9742)`

    * Sampel 5x5 dari `cosine_sim_df_eval` menunjukkan skor kemiripan. Nilai diagonal adalah 1.0 (kemiripan film dengan dirinya sendiri).
    * ![Image](https://github.com/user-attachments/assets/f007e6e6-3e48-44d1-b531-6d48afc823f0)

* **Pembuatan Fungsi Rekomendasi :**

  * **Tujuan Fungsi:** Untuk menerima judul film sebagai input dan mengembalikan N film teratas yang paling mirip berdasarkan skor kemiripan kosinus.

  * **Langkah-langkah dalam Fungsi `get_content_based_recommendations`:**

    1. **Pemetaan Indeks:** Sebuah `pandas.Series` (`indices_cbf`) dibuat untuk memetakan judul film ke indeks numerik barisnya dalam `movies_for_cbf` (dan juga dalam `cosine_sim_df`). Ini memungkinkan pencarian film yang efisien berdasarkan judul. `drop_duplicates()` digunakan pada `Series` ini untuk menangani jika ada judul film yang sama (meskipun `movieId` unik, judul bisa saja tidak unik secara teoretis, meskipun jarang dalam dataset ini).

    2. **Validasi Input:** Fungsi memeriksa apakah judul film input ada dalam `indices_cbf`. Jika tidak, pesan error dikembalikan.

    3. **Mendapatkan Skor Kemiripan:** Indeks film input (`idx`) digunakan untuk mengambil baris skor kemiripan film tersebut dengan semua film lain dari `similarity_data` (yaitu, `cosine_sim_df`).

    4. **Pengurutan:** Daftar skor kemiripan (pasangan indeks film dan skor) diurutkan secara menurun berdasarkan skor kemiripan.

    5. **Pemilihan Top-N:** `top_n` film teratas dipilih, dengan mengabaikan film input itu sendiri (yang akan memiliki skor kemiripan 1.0 dengan dirinya sendiri dan berada di posisi pertama setelah diurutkan).

    6. **Pengambilan Detail Film:** Indeks dari film-film yang direkomendasikan digunakan untuk mengambil detailnya (judul dan `feature_soup`) dari DataFrame `items_df` (yaitu, `movies_for_cbf`).

    7. **Penyertaan Skor (Opsional):** Fungsi memiliki parameter `include_score` (default `False` di Cell 45, diatur `True` di Cell 53) untuk menentukan apakah kolom `similarity_score` akan ditambahkan ke output DataFrame rekomendasi.

  * **Output:** Fungsi mengembalikan DataFrame yang berisi film-film yang direkomendasikan.

#### 5.1.4. Pengujian CBF



Untuk mendemonstrasikan cara kerja model CBF, kita mengambil film **"Kung Fu Panda (2008)"** sebagai contoh input, sesuai dengan output .
* **Feature Soup Asli untuk "Kung Fu Panda (2008)":** (Ini adalah gabungan dari genre dan tag yang relevan setelah diproses dari `movies_for_cbf`. Contohnya akan seperti: `action adventure animation children comedy imax kungfu panda`)
* **Top 5 Rekomendasi CBF :**
![Image](https://github.com/user-attachments/assets/26c050ab-64be-47df-bbe7-581bd114e3f9)

* **Penjelasan:**
    * Model CBF merekomendasikan film-film yang memiliki kesamaan genre yang signifikan dengan "Kung Fu Panda (2008)". Sebagian besar film yang direkomendasikan adalah film animasi dengan elemen komedi, petualangan, dan untuk anak-anak.
    * "Kung Fu Panda 2 (2011)" muncul sebagai rekomendasi, yang sangat relevan karena merupakan sekuel langsung dan berbagi banyak atribut konten.
    * Film lain seperti "Happy Feet Two", "Despicable Me 2", "Madagascar: Escape 2 Africa", dan "Open Season" juga merupakan film animasi keluarga dengan elemen komedi dan petualangan, menunjukkan kemampuan model untuk menemukan item dengan karakteristik konten serupa.
    

#### Kelebihan dan Kekurangan CBF dalam Proyek Ini

Berdasarkan implementasi dan hasil yang diperoleh:

* **Kelebihan:**

  1. **Interpretasi yang Jelas:** Rekomendasi mudah dijelaskan karena didasarkan pada kesamaan fitur konten yang eksplisit (genre dan tag). Kita bisa melihat mengapa sebuah film direkomendasikan.
  2. **Penanganan *Item Cold-Start*:** Model dapat merekomendasikan film baru selama film tersebut memiliki deskripsi genre dan/atau tag. Tidak memerlukan data rating historis untuk film baru tersebut.
  3. **Tidak Memerlukan Data Pengguna Lain:** Rekomendasi untuk satu film bersifat independen dari preferensi pengguna lain, hanya berdasarkan konten film itu sendiri.
  4. **Sederhana untuk Diimplementasikan:** Dengan library seperti `sklearn`, implementasi TF-IDF dan Cosine Similarity relatif mudah.

* **Kekurangan:**

  1. **Ketergantungan pada Kualitas Fitur Konten:** Akurasi sangat bergantung pada seberapa baik `feature_soup` (genre dan tag) dapat merepresentasikan esensi film. Jika genre atau tag kurang deskriptif atau tidak konsisten, kualitas rekomendasi akan menurun. Dalam proyek ini, `feature_soup` hanya terdiri dari genre dan tag; fitur lain seperti aktor, sutradara, atau plot tidak digunakan, yang mungkin membatasi kedalaman pemahaman konten.
  2. **Kurangnya Serendipity (Over-Specialization):** Model cenderung merekomendasikan film yang sangat mirip dengan apa yang sudah diketahui. Sulit untuk menemukan rekomendasi yang mengejutkan atau di luar genre/tag yang sudah ada dalam `feature_soup` film input.
  3. **Masalah *User Cold-Start* Tetap Ada:** Model ini tidak secara langsung memodelkan preferensi pengguna. Untuk memberikan rekomendasi kepada pengguna baru, kita masih memerlukan informasi tentang film apa yang mereka sukai terlebih dahulu untuk memulai proses pencarian kemiripan.
  4. **Representasi Fitur:** TF-IDF adalah teknik yang baik, tetapi mungkin tidak menangkap nuansa semantik atau hubungan antar kata (genre/tag) sebaik teknik *embedding* yang lebih canggih (seperti Word2Vec atau BERT). Misalnya, TF-IDF mungkin tidak memahami bahwa "sci-fi" dan "science fiction" adalah hal yang sama jika tidak dipra-proses dengan baik, atau bahwa "action" dan "adventure" seringkali terkait.

### Collaborative Filtering (CF)
#### Konsep Dasar CF dan SVD

Collaborative Filtering (CF) adalah pendekatan yang merekomendasikan item kepada pengguna berdasarkan preferensi dan perilaku kolektif dari banyak pengguna. Ide utamanya adalah "pengguna yang setuju di masa lalu cenderung setuju di masa depan." CF tidak memerlukan pemahaman tentang konten item itu sendiri, melainkan mengandalkan matriks interaksi pengguna-item (misalnya, matriks rating).

Ada dua jenis utama CF:

* **Memory-Based CF:** Mencari pengguna (user-based) atau item (item-based) yang mirip berdasarkan pola rating dan kemudian membuat prediksi.
* **Model-Based CF:** Membangun model prediktif dari data rating untuk memperkirakan rating pengguna terhadap item yang belum dinilai. **Singular Value Decomposition (SVD)** adalah salah satu teknik faktorisasi matriks yang populer untuk model-based CF. SVD bekerja dengan menguraikan matriks interaksi pengguna-item (matriks rating R yang berukuran m pengguna x n item) menjadi produk dari tiga matriks: $R \approx U \Sigma V^T$. Dalam konteks rekomendasi, varian SVD (seperti yang diimplementasikan di library `Surprise`) sering digunakan untuk mempelajari vektor faktor laten (fitur tersembunyi) untuk setiap pengguna (P) dan setiap item (Q). Rating prediksi $\hat{r}_{ui}$ untuk pengguna u$ dan item $i$ kemudian dihitung sebagai produk titik dari vektor faktor laten mereka: $\hat{r}_{ui} = q_i^T p_u$. Model ini dilatih dengan meminimalkan error prediksi pada rating yang diketahui.

#### Implementasi: Persiapan Data dan Model SVD Baseline



* **Proses:**

  1. **Persiapan Data untuk Library `Surprise`:**
     * Objek `Reader` dari `surprise` diinisialisasi dengan parameter `rating_scale=(0.5, 5.0)`. Ini memberitahu library tentang rentang nilai rating yang ada dalam dataset kita.
     * Data dari DataFrame `ratings_for_cf_df` (yang berisi kolom `userId`, `movieId`, `rating`) dimuat ke dalam format dataset internal `Surprise` menggunakan fungsi `Dataset.load_from_df()`. Format ini diperlukan agar algoritma dari library `Surprise` dapat memproses data dengan benar.
  2. **Pembagian Data:** Dataset `Surprise` yang telah dimuat kemudian dibagi menjadi data latih (`trainset`) dan data uji (`testset`). Dalam proyek ini, digunakan perbandingan 80% untuk data latih dan 20% untuk data uji, yang dilakukan menggunakan fungsi `surprise_train_test_split`. Parameter `random_state=42` digunakan untuk memastikan bahwa pembagian data bersifat deterministik dan dapat direproduksi setiap kali kode dijalankan.
     * **Output Tahap Ini:** Pesan konfirmasi jumlah data dalam set pelatihan (80668) dan set pengujian (20168).
  3. **Inisialisasi dan Pelatihan Model SVD Baseline:**
     * Algoritma `SVD` dari `surprise` diinisialisasi sebagai `algo_svd_baseline`. Parameter yang digunakan pada tahap ini adalah parameter yang telah ditentukan sebagai parameter yang cukup baik sebagai titik awal: `n_factors=100` (jumlah faktor laten yang akan dipelajari), `n_epochs=30` (jumlah iterasi pelatihan), `lr_all=0.005` (laju pembelajaran untuk semua parameter), `reg_all=0.04` (koefisien regularisasi untuk semua parameter), dan `random_state=42` untuk reproduktifitas.
     * Model SVD baseline kemudian dilatih pada `trainset` yang telah dibuat menggunakan metode `algo_svd_baseline.fit(trainset)`. Selama proses `fit`, algoritma SVD akan mencoba menemukan vektor faktor laten untuk pengguna dan item yang paling baik merekonstruksi rating yang diketahui di `trainset`.
  4. **Evaluasi Model Baseline:**
     * Setelah model dilatih, kemampuannya untuk memprediksi rating diuji pada `testset` (data yang belum pernah dilihat model selama pelatihan) menggunakan metode `algo_svd_baseline.test(testset)`. Ini menghasilkan daftar prediksi.
     * Kinerja model dievaluasi menggunakan metrik Root Mean Squared Error (RMSE) dan Mean Absolute Error (MAE). Fungsi `accuracy.rmse()` dan `accuracy.mae()` dari `surprise` digunakan untuk menghitung nilai-nilai ini berdasarkan prediksi yang dibuat pada `testset`.
* **Output Sel :**
  * Pesan konfirmasi pemuatan data ke format `Surprise`.
  * Informasi jumlah data training (misalnya, 80668) dan data testing (misalnya, 20168).
  * Pesan progres pelatihan model SVD baseline.
  * Hasil evaluasi:
    * `RMSE SVD Baseline: 0.8710`
    * `MAE SVD Baseline: 0.6681`
* **Interpretasi Output:**
  * Model SVD baseline berhasil dilatih dan dievaluasi. Nilai RMSE sekitar 0.8710 menunjukkan bahwa, secara rata-rata, kesalahan prediksi model (dengan memberikan penalti lebih untuk kesalahan yang lebih besar) adalah sekitar 0.87 poin rating pada skala 0.5 hingga 5.0. Nilai MAE sekitar 0.6681 menunjukkan bahwa, secara rata-rata, kesalahan absolut dari prediksi model adalah sekitar 0.67 poin rating. Kedua metrik ini adalah metrik error, sehingga nilai yang lebih rendah mengindikasikan performa prediksi yang lebih baik. Hasil ini menjadi *baseline* atau titik acuan performa sebelum dilakukan *hyperparameter tuning*.

#### Hyperparameter Tuning dengan GridSearchCV

* **Tujuan:** Untuk menemukan kombinasi hiperparameter terbaik untuk algoritma SVD yang dapat menghasilkan model dengan error prediksi (RMSE dan MAE) yang paling minimal. Hiperparameter adalah parameter yang nilainya diatur sebelum proses pembelajaran dimulai.
* **Proses:**
  1.  **Definisi `param_grid`:** Sebuah dictionary Python bernama `param_grid` didefinisikan. Dictionary ini berisi daftar hiperparameter yang akan diuji dan rentang nilai yang mungkin untuk setiap hiperparameter tersebut:
        * `'n_factors'`: `[50, 100, 150]` – Jumlah faktor laten. Faktor laten adalah dimensi tersembunyi yang digunakan untuk merepresentasikan pengguna dan item.
        * `'n_epochs'`: `[20, 30]` – Jumlah iterasi (epoch) yang akan dijalankan oleh algoritma optimasi (seperti Stochastic Gradient Descent) selama proses pelatihan.
        * `'lr_all'`: `[0.002, 0.005]` – Laju pembelajaran (learning rate) untuk semua parameter model.
        * `'reg_all'`: `[0.02, 0.04, 0.06]` – Koefisien regularisasi untuk semua parameter. Regularisasi membantu mencegah *overfitting*.
  2.  **Inisialisasi `GridSearchCV`:** Objek `GridSearchCV` dari modul `surprise.model_selection` diinisialisasi. Parameter yang diberikan adalah:
        * Algoritma yang akan di-tuning: `SVD`.
        * `param_grid`: Dictionary hiperparameter yang telah didefinisikan.
        * `measures=['rmse', 'mae']`: Daftar metrik yang akan digunakan untuk mengevaluasi setiap kombinasi hiperparameter.
        * `cv=3`: Menentukan penggunaan 3-fold cross-validation. Artinya, data akan dibagi menjadi 3 bagian (fold); model akan dilatih pada 2 fold dan diuji pada 1 fold yang tersisa, dan proses ini diulang 3 kali sehingga setiap fold pernah menjadi set pengujian.
  3.  **Pelaksanaan Grid Search:** Metode `gs.fit(data_surprise)` menjalankan proses pencarian grid. `GridSearchCV` akan secara sistematis melatih dan mengevaluasi model SVD untuk setiap kemungkinan kombinasi parameter yang ada dalam `param_grid`. Evaluasi dilakukan menggunakan skema 3-fold cross-validation pada keseluruhan dataset `data_surprise` (bukan hanya `trainset` awal) untuk mendapatkan estimasi performa yang lebih robust.
* **Output Sel:**
    * Pesan progres seperti "Memulai GridSearchCV..." dan "GridSearchCV selesai."
    * Hasil terbaik yang ditemukan oleh `GridSearchCV` untuk setiap metrik:
        * `RMSE terbaik dari GridSearchCV: 0.8702`
        * `Parameter terbaik untuk RMSE: {'n_factors': 100, 'n_epochs': 30, 'lr_all': 0.005, 'reg_all': 0.06}`
        * `MAE terbaik dari GridSearchCV: 0.6690`
        * `Parameter terbaik untuk MAE: {'n_factors': 100, 'n_epochs': 30, 'lr_all': 0.005, 'reg_all': 0.06}`
* **Interpretasi Output:**
    * `GridSearchCV` telah berhasil mengeksplorasi berbagai kombinasi hiperparameter dan menemukan set parameter yang menghasilkan skor RMSE dan MAE terbaik selama proses cross-validation.
    * Terlihat bahwa parameter terbaik untuk meminimalkan RMSE (0.8702) adalah dengan `n_factors=100`, `n_epochs=30`, `lr_all=0.005`, dan `reg_all=0.06`.
    * Sedangkan parameter terbaik untuk meminimalkan MAE (0.6690) dengan parameter sama.
    * Hasil RMSE terbaik dari GridSearchCV (0.8702) sedikit lebih rendah dibandingkan RMSE model baseline (0.8710), yang mengindikasikan bahwa *hyperparameter tuning* berpotensi memberikan perbaikan pada performa model.

#### Implementasi: Model SVD Tuned dan Fungsi Rekomendasi

* **Proses :**
    1.  Model SVD baru, `algo_svd_tuned`, diinisialisasi menggunakan parameter terbaik yang ditemukan oleh `GridSearchCV` untuk metrik RMSE (yaitu, `{'n_factors': 100, 'n_epochs': 30, 'lr_all': 0.005, 'reg_all': 0.06}`). Penggunaan `random_state=42` dipertahankan untuk konsistensi.
    2.  Model `algo_svd_tuned` kemudian dilatih pada `trainset` awal (yang sama dengan yang digunakan untuk melatih model baseline). Ini dilakukan agar perbandingan performa antara model baseline dan model tuned dapat dilakukan secara adil pada `testset` yang sama.
    3.  Setelah dilatih, model yang sudah di-tuning dievaluasi pada `testset` awal menggunakan metrik RMSE dan MAE.
* **Output :**
    * Pesan progres pelatihan model SVD yang sudah di-tuning.
    * Hasil evaluasi model tuned pada `testset` awal:
        * `RMSE SVD (Tuned): 0.8690`
        * `MAE SVD (Tuned): 0.6672`
* **Interpretasi Output :**
    * Setelah dilatih dengan parameter optimal (berdasarkan RMSE dari `GridSearchCV`) pada `trainset` awal, model yang di-tuning menghasilkan RMSE 0.8690 dan MAE 0.6672 pada `testset`.
    * Nilai RMSE ini (0.8690) sedikit lebih baik (lebih rendah) dibandingkan RMSE model baseline (0.8710). Demikian pula, MAE tuned (0.6672) juga sedikit lebih baik dari MAE baseline (0.6681).
    * Ini menunjukkan bahwa *hyperparameter tuning*, meskipun peningkatannya tidak drastis, berhasil memberikan sedikit perbaikan pada akurasi prediksi rating model SVD pada pembagian data latih/uji yang spesifik ini.

* **Proses Fungsi Rekomendasi):**
    1.  Fungsi `get_top_n_collaborative_recommendations_tuned` didefinisikan untuk menghasilkan daftar rekomendasi film yang dipersonalisasi.
    2.  **Input Fungsi:** `model`, `user_id`, `all_movie_ids_list`, `rated_movie_ids_list`, `n`, dan `movies_data`.
    3.  **Logika Fungsi:** Mengidentifikasi film yang belum dirating, membuat prediksi rating, mengurutkan, mengambil top-N, dan menggabungkan dengan info film.
* **Output Sel :**
    * Pesan konfirmasi: "Fungsi 'get_top_n_collaborative_recommendations_tuned' telah dibuat."
* **Interpretasi Output :** Fungsi siap digunakan untuk menghasilkan rekomendasi.

#### Contoh Rekomendasi CF dan Interpretasi

* **Proses:**
    1.  Pengguna sampel (`sample_user_id_cf_tuned` = 46) dipilih.
    2.  Daftar film yang sudah dirating oleh User 46 diambil.
    3.  Fungsi `get_top_n_collaborative_recommendations_tuned` dipanggil untuk User 46.
    4.  Hasil rekomendasi dan daftar film yang sudah dirating ditampilkan.
* **Output Sel :**
    * Pesan: "Mencari rekomendasi (tuned) untuk user: 46..."
    * **Top 5 Rekomendasi Collaborative Filtering (Tuned) untuk User 46:**
        1.  Paths of Glory (1957) (Drama|War) - Estimated Rating: 4.952371
        2.  Rear Window (1954) (Mystery|Thriller) - Estimated Rating: 4.913711
        3.  Cinema Paradiso (Nuovo cinema Paradiso) (1989) (Drama) - Estimated Rating: 4.883816
        4.  Dr. Strangelove or: How I Learned to Stop Worr... (Comedy|War) - Estimated Rating: 4.823977
        5.  Philadelphia Story, The (1940) (Comedy|Drama|Romance) - Estimated Rating: 4.812567
    * Informasi: "User 46 hanya merating 42 film."
    * Daftar 5 film teratas yang sudah dirating oleh User 46
      ![Image](https://github.com/user-attachments/assets/a214e50e-2254-400f-b8c0-04a32d6f8ad5)
* **Interpretasi Output:**
    * Model CF (SVD tuned) berhasil menghasilkan daftar 5 rekomendasi film yang belum pernah ditonton oleh User 46.
    * Estimasi rating yang sangat tinggi (mendekati 5.0) menunjukkan keyakinan model bahwa pengguna akan menyukai film-film ini.
    * Dari 42 film yang sudah dirating user 46, terlihat bahwa mereka cenderung memberi nilai tinggi pada film bergenre Drama, War, Thriller, Crime, dan juga beberapa Comedy/Drama, misalnya “Braveheart (1995)” (Action/Drama/War) dan “Legends of the Fall (1994)” (Drama/Romance/War/Western) mendapat rating 5, serta “Fugitive, The (1993)” (Thriller) dan “Silence of the Lambs (1991)” (Crime/Horror/Thriller) juga dirating 5. Oleh karena itu, rekomendasi collaborative filtering “Paths of Glory (1957)” (Drama/War) dan “Rear Window (1954)” (Mystery/Thriller) sangat sesuai—mereka merefleksikan kecenderungan user menyukai drama‐war dan thriller—sementara “Cinema Paradiso (1989)” (Drama), “Dr. Strangelove (1964)” (Comedy/War), dan “Philadelphia Story, The (1940)” (Comedy/Drama/Romance) juga selaras dengan kombinasi genre (Drama/Comedy/War) yang user tunjukkan pada daftar rated movies.
    * Daftar film yang sudah dirating membantu memverifikasi bahwa rekomendasi adalah item baru bagi pengguna.

#### 5.2.6. Kelebihan dan Kekurangan CF (SVD) dalam Proyek Ini
Berdasarkan implementasi dan hasil yang diperoleh:
* **Kelebihan:**
    1.  **Kemampuan Menemukan Rekomendasi Baru (Serendipity):** Model SVD dapat menemukan film yang mungkin tidak akan ditemukan pengguna jika hanya berdasarkan pencarian genre atau kemiripan konten eksplisit. Rekomendasi didasarkan pada preferensi laten yang dipelajari dari seluruh komunitas pengguna. Contohnya, User 46 mungkin belum pernah menonton film perang klasik seperti "Paths of Glory" tetapi model merekomendasikannya dengan skor tinggi.
    2.  **Tidak Memerlukan Fitur Konten Eksplisit:** Model bekerja hanya dengan data rating (`userId`, `movieId`, `rating`), sehingga tidak bergantung pada ketersediaan atau kualitas metadata konten film. Ini berguna jika metadata item sulit diperoleh atau tidak informatif.
    3.  **Performa Prediksi Rating yang Baik:** Dengan RMSE/MAE di bawah 0.9 (sekitar 0.87/0.67 setelah tuning), model SVD menunjukkan kemampuan yang baik dalam memprediksi bagaimana seorang pengguna akan memberi rating pada sebuah film.
    4.  **Personalisasi yang Kuat:** Rekomendasi sangat dipersonalisasi berdasarkan perilaku rating pengguna dan perbandingannya dengan pengguna lain.
* **Kekurangan:**
    1.   Model SVD kesulitan memberikan rekomendasi yang baik untuk pengguna baru (yang belum memiliki riwayat rating yang cukup untuk membangun profil faktor laten) atau untuk film baru (yang belum menerima rating dari pengguna manapun sehingga tidak memiliki representasi faktor laten item).
    2.   sebagai teknik faktorisasi matriks, dirancang untuk menangani *sparsity* dengan baik dengan memproyeksikan data ke ruang laten berdimensi lebih rendah, performanya masih bisa dipengaruhi jika data rating sangat jarang. EDA kita menunjukkan adanya *long-tail* pada distribusi jumlah rating per film, yang berarti banyak item memiliki sedikit interaksi.
    3.   Meskipun faktor laten yang dipelajari oleh SVD menangkap pola preferensi, faktor-faktor ini tidak selalu mudah diinterpretasikan dalam istilah fitur konten yang konkret. Sulit untuk menjelaskan kepada pengguna *mengapa* sebuah film direkomendasikan selain karena "pengguna lain yang mirip dengan Anda menyukainya" atau "Anda menyukai film lain dengan pola faktor laten serupa."
    4.   Model CF, termasuk SVD, cenderung lebih sering merekomendasikan film-film populer karena film tersebut memiliki lebih banyak data rating, yang mungkin mengabaikan film-film *niche* yang bisa jadi relevan bagi pengguna tertentu.
    5.   Meskipun SVD efisien untuk prediksi setelah dilatih, pelatihan ulang model dengan data baru bisa memakan waktu dan sumber daya komputasi, terutama untuk dataset besar.

## Evaluation

Tahap evaluasi bertujuan untuk mengukur dan menganalisis kinerja dari model-model sistem rekomendasi yang telah dikembangkan, yaitu Content-Based Filtering (CBF) dan Collaborative Filtering (CF) dengan algoritma SVD. Evaluasi dilakukan menggunakan metrik kuantitatif dan analisis kualitatif.

### Metrik Evaluasi yang Digunakan
Pemilihan metrik evaluasi yang tepat sangat penting untuk menilai aspek-aspek berbeda dari performa sistem rekomendasi. Penjelasan detail mengenai definisi, cara kerja, dan formula untuk setiap metrik disajikan dalam dokumen terpisah (`recsys_metrics_explanation` atau dapat diintegrasikan di sini). Metrik utama yang digunakan adalah:

#### Cosine Similarity (untuk CBF)
* **Penggunaan:** Mengukur kemiripan konten antar film berdasarkan vektor TF-IDF dari `feature_soup`. Skor yang lebih tinggi (mendekati 1) menunjukkan kemiripan yang lebih besar.
* **Formula:** $$\cos(\theta) = \frac{A \cdot B}{\|A\| \|B\|}$$

#### RMSE (Root Mean Squared Error) (untuk CF)
* **Penggunaan:** Mengukur rata-rata besarnya error prediksi rating, dengan memberikan bobot lebih pada error yang besar. Nilai lebih rendah lebih baik.
* **Formula:** $$\text{RMSE} = \sqrt{\frac{\sum_{i=1}^{N} (pred_i - actual_i)^2}{N}}$$

#### MAE (Mean Absolute Error) (untuk CF)
* **Penggunaan:** Mengukur rata-rata besarnya error prediksi rating secara absolut. Nilai lebih rendah lebih baik.
* **Formula:** $$\text{MAE} = \frac{\sum_{i=1}^{N} |pred_i - actual_i|}{N}$$

#### Precision@k dan Recall@k (untuk CF Top-N)
* **Penggunaan:** Mengevaluasi kualitas daftar N item teratas yang direkomendasikan. Item dianggap relevan jika rating aktualnya pada `testset` $\ge 4.0$.
* **Precision@k:** Proporsi item relevan dari `k` item teratas yang direkomendasikan.
    * **Formula:** $$\text{Precision@k} = \frac{\text{Jumlah item relevan & direkomendasikan di top-k}}{k}$$
* **Recall@k:** Proporsi item relevan yang berhasil direkomendasikan dari semua item yang relevan bagi pengguna (di `testset`).
    * **Formula:** $$\text{Recall@k} = \frac{\text{Jumlah item relevan & direkomendasikan di top-k}}{\text{Total item relevan untuk pengguna di testset}}$$

### Hasil Evaluasi Model CBF

Evaluasi model CBF dilakukan secara kualitatif dengan menganalisis contoh rekomendasi dan skor kemiripan kosinus yang dihasilkan.

* **Perhitungan Matriks Kemiripan Kosinus Global:**
    * Matriks `cosine_sim_df_eval` berukuran (9742, 9742) berhasil dibuat, menunjukkan skor kemiripan antara setiap pasang film dalam dataset.
    * Sampel 5x5 dari matriks ini menunjukkan nilai diagonal 1.0 (kemiripan sempurna film dengan dirinya sendiri) dan nilai non-diagonal yang merepresentasikan kemiripan konten. Skor yang lebih tinggi menandakan kemiripan yang lebih besar.
* **Pengujian Kualitatif Rekomendasi untuk "Kung Fu Panda (2008)" :**
    * **Feature Soup Asli untuk "Kung Fu Panda (2008)":** `action animation children comedy imax` (berdasarkan output pengujian Anda)
    * **Top 5 Rekomendasi CBF (dengan Skor Kemiripan, dari output pengujian Anda di chat):**
        1. Happy Feet Two (2011) - Feature Soup: `animation children comedy imax` - Skor: 0.943
        2. Despicable Me 2 (2013) - Feature Soup: `animation children comedy imax` - Skor: 0.943
        3. Madagascar: Escape 2 Africa (2008) - Feature Soup: `action adventure animation children comedy imax` - Skor: 0.935
        4. Kung Fu Panda 2 (2011) - Feature Soup: `action adventure animation children comedy imax` - Skor: 0.935
        5. Open Season (2006) - Feature Soup: `adventure animation children comedy imax` - Skor: 0.876
    * **Analisis :** Rekomendasi untuk "Kung Fu Panda (2008)" juga menunjukkan relevansi konten yang sangat tinggi. Skor kemiripan yang dihasilkan sangat tinggi (semua di atas 0.87), menandakan kesamaan konten yang kuat. "Kung Fu Panda 2" sebagai sekuel langsung memiliki skor kemiripan yang tinggi (0.935). Film animasi lain dengan elemen komedi, anak-anak, dan imax juga direkomendasikan dengan skor tinggi, yang sangat sesuai dengan profil konten film input. Ini kembali menunjukkan kemampuan model CBF untuk menemukan item dengan karakteristik konten yang sangat serupa.

### Hasil Evaluasi Model CF

Evaluasi model CF (SVD) dilakukan menggunakan metrik prediksi rating (RMSE dan MAE) dan metrik kualitas peringkat (Precision@k dan Recall@k).

* **Metrik Prediksi Rating untuk Model SVD:**
    * **Model Baseline:**
        * RMSE = 0.8710
        * MAE = 0.6681
    * **Model Tuned (menggunakan parameter terbaik dari GridSearchCV untuk RMSE, yaitu `{'n_factors': 100, 'n_epochs': 30, 'lr_all': 0.005, 'reg_all': 0.06}` ):**
        * RMSE = 0.8690
        * MAE = 0.6672
    * **Interpretasi:**
        * Kedua model (baseline dan tuned) menunjukkan performa yang baik dalam memprediksi rating, dengan rata-rata kesalahan prediksi kurang dari 1 poin rating pada skala 0.5-5.0.
        * Proses *hyperparameter tuning* memberikan perbaikan yang lebih terlihat pada *test split* ini dibandingkan interpretasi sebelumnya. RMSE turun dari 0.8710 menjadi 0.8690, dan MAE turun dari 0.6681 menjadi 0.6672. Ini menunjukkan bahwa parameter yang ditemukan melalui `GridSearchCV` (RMSE terbaik pada CV: 0.8702 dengan parameter yang sedikit berbeda) berhasil meningkatkan akurasi prediksi model SVD.

* **Metrik Peringkat Top-N (CF - Model Tuned):**
    * (Menggunakan k=10 dan rating threshold relevansi >= 4.0 pada `testset`)
    * **Rata-rata Precision@10: 0.5656** 
    * **Rata-rata Recall@10: 0.6668** 
    * **Interpretasi:**
        * **Precision@10 ≈ 0.5656:** Dari 10 film yang direkomendasikan, rata-rata sekitar 56.56% (atau sekitar 5-6 film) adalah film yang memang dianggap relevan oleh pengguna (mendapat rating aktual >= 4.0 di `testset`). Ini adalah indikasi yang baik bahwa pengguna akan menemukan item yang mereka sukai di antara rekomendasi teratas.
        * **Recall@10 ≈ 0.6668:** Dari semua film yang relevan untuk pengguna di `testset`, model berhasil menangkap dan merekomendasikan sekitar 66.68% di antaranya dalam 10 rekomendasi teratas. Ini menunjukkan kemampuan model yang baik untuk menemukan kembali sebagian besar item yang disukai pengguna.

### Perbandingan Kinerja Model

| Fitur Perbandingan         | Content-Based Filtering (CBF)                                  | Collaborative Filtering (CF - SVD Tuned)                       |
| :------------------------- | :------------------------------------------------------------- | :------------------------------------------------------------- |
| **Input Utama** | Fitur konten film (genre, tag)                                 | Matriks interaksi pengguna-item (rating)                       |
| **Metrik Utama** | Cosine Similarity (kualitatif untuk relevansi konten)          | RMSE, MAE (akurasi prediksi rating), Precision@k, Recall@k (kualitas peringkat) |
| **Hasil Kuantitatif** | Skor kemiripan tinggi untuk item serupa ( >0.87 untuk rekomendasi "Kung Fu Panda") | RMSE ≈ 0.8690, MAE ≈ 0.6672, P@10 ≈ 0.5656, R@10 ≈ 0.6668        |
| **Interpretasi Rekomendasi** | Mudah dijelaskan (berdasarkan kesamaan genre/tag)              | Lebih sulit dijelaskan (berdasarkan pola preferensi laten)       |
| **Serendipity** | Rendah (cenderung merekomendasikan item yang sangat mirip)       | Potensi lebih tinggi (dapat menemukan item lintas genre)        |
| **Penanganan *Cold-Start*** | Baik untuk *item cold-start* (jika item baru punya metadata)      | Buruk untuk *user cold-start* dan *item cold-start* |
| **Ketergantungan Data** | Kualitas metadata konten                                       | Kuantitas dan kualitas data rating (rentan *sparsity*)         |

* **Analisis:**
    * CBF menghasilkan rekomendasi yang sangat relevan secara konten, seperti yang ditunjukkan  "Kung Fu Panda". Namun, ini bisa membatasi penemuan item baru yang berbeda secara signifikan.
    * CF (SVD) menunjukkan akurasi prediksi rating yang baik (RMSE ~0.8690) dan mampu menghasilkan daftar rekomendasi top-N yang relevan (P@10 ~0.56, R@10 ~0.67). Model ini berpotensi memberikan rekomendasi yang lebih beragam.
    * Perbaikan dari *hyperparameter tuning* pada CF (SVD) memberikan peningkatan yang lebih jelas pada *test split* ini dibandingkan interpretasi sebelumnya, menunjukkan manfaat dari proses optimasi.
    * Kedua model memiliki kekuatan komplementer. CBF baik untuk menjelaskan rekomendasi dan menangani item baru, sementara CF baik dalam menangkap preferensi implisit dan memberikan rekomendasi yang lebih beragam.

## Kesimpulan dan Saran

### Ringkasan Proyek dan Temuan Utama
Proyek ini telah berhasil mengembangkan dan mengevaluasi dua pendekatan utama dalam sistem rekomendasi film: Content-Based Filtering (CBF) dan Collaborative Filtering (CF) menggunakan algoritma Singular Value Decomposition (SVD).
1.  **Pemahaman Data:** EDA yang komprehensif mengungkap karakteristik dataset MovieLens, termasuk kualitas data yang baik, distribusi rating yang cenderung positif, dominasi genre tertentu, variasi tag yang kaya, dan adanya *data sparsity*.
2.  **Persiapan Data:** Data telah berhasil dibersihkan dan ditransformasi. Untuk CBF, `feature_soup` dari genre dan tag dibuat. Untuk CF, data rating disiapkan untuk library `Surprise`.
3.  **Kinerja CBF:** Model CBF (TF-IDF & Cosine Similarity) menghasilkan rekomendasi yang relevan secara konten, divalidasi dengan skor kemiripan tinggi untuk film dengan fitur serupa (misalnya, skor >0.87 untuk rekomendasi "Kung Fu Panda").
4.  **Kinerja CF (SVD):** Model SVD menunjukkan performa prediksi rating yang baik (RMSE baseline 0.8710, RMSE tuned 0.8690). Evaluasi peringkat top-N (P@10 ≈ 0.5656, R@10 ≈ 0.6668) mengindikasikan kemampuan model untuk menyajikan rekomendasi yang relevan.
5.  **Perbandingan:** CBF unggul dalam transparansi dan *item cold-start*, sementara CF lebih baik dalam *serendipity* meskipun rentan *cold-start* pengguna/item dan *sparsity*.

### Keterkaitan dengan Pemahaman Bisnis

Model yang dievaluasi memberikan dampak signifikan terhadap pemahaman bisnis dan tujuan proyek:

#### Jawaban terhadap Pernyataan Masalah
1.  **Bagaimana cara efektif menyarankan film berdasarkan konten (genre dan tag)?**
    * **Jawaban:** Model CBF yang diimplementasikan menggunakan TF-IDF pada `feature_soup` (gabungan genre dan tag) dan Cosine Similarity terbukti efektif. Contoh rekomendasi untuk "Kung Fu Panda (2008)" menghasilkan film-film animasi dengan genre serupa dan skor kemiripan yang tinggi (di atas 0.87), menunjukkan bahwa pendekatan ini berhasil menangkap kesamaan konten.
2.  **Bagaimana cara memanfaatkan data rating historis untuk rekomendasi?**
    * **Jawaban:** Model CF dengan algoritma SVD berhasil memanfaatkan data rating historis. Dengan mempelajari pola rating dari 610 pengguna terhadap 9724 film, model mampu memprediksi rating untuk film yang belum ditonton pengguna dan menghasilkan rekomendasi yang dipersonalisasi, seperti yang ditunjukkan pada contoh User 46.
3.  **Pendekatan mana (CBF vs CF) yang lebih akurat/relevan untuk dataset ini?**
    * **Jawaban:** Kedua pendekatan memiliki keunggulan masing-masing. CBF sangat relevan dari segi konten (skor kemiripan tinggi). CF (SVD tuned) menunjukkan akurasi prediksi rating yang baik (RMSE ≈ 0.8690) dan kualitas peringkat yang layak (P@10 ≈ 0.5656, R@10 ≈ 0.6668). CF lebih unggul dalam potensi *serendipity* dan tidak memerlukan metadata konten yang detail, namun CBF lebih mudah diinterpretasikan. "Lebih baik" akan tergantung pada tujuan spesifik (misalnya, akurasi prediksi rating vs. relevansi konten vs. penemuan baru).
4.  **Faktor apa saja yang paling berpengaruh terhadap kualitas rekomendasi?**
    * **Jawaban:**
        * **Untuk CBF:** Kualitas dan kelengkapan `feature_soup` (yaitu, genre dan tag) sangat berpengaruh. Semakin deskriptif dan akurat fitur konten, semakin baik rekomendasinya. Teknik representasi fitur (TF-IDF) juga memainkan peran.
        * **Untuk CF (SVD):** Jumlah data rating yang tersedia (mengatasi *sparsity*), kualitas data rating, dan pemilihan hiperparameter (seperti `n_factors`, `n_epochs`, `lr_all`, `reg_all` yang dioptimalkan melalui `GridSearchCV`) sangat berpengaruh. Hasil tuning menunjukkan bahwa penyesuaian parameter ini dapat memberikan sedikit peningkatan pada akurasi prediksi.

#### Pencapaian Tujuan Proyek (Goals)
1.  **Melakukan EDA komprehensif:** **Tercapai.** EDA mendalam telah dilakukan, mencakup analisis kualitas data, distribusi, univariat, dan multivariat, yang memberikan *insight* penting.
2.  **Melaksanakan pra-pemrosesan data:** **Tercapai.** Data telah dibersihkan dan ditransformasi, termasuk pembuatan `feature_soup` untuk CBF dan penyiapan data rating untuk CF.
3.  **Mengimplementasikan model CBF (TF-IDF & Cosine Similarity):** **Tercapai.** Model CBF berhasil diimplementasikan, menghasilkan matriks fitur dan fungsi rekomendasi.
4.  **Mengimplementasikan model CF (SVD & GridSearchCV):** **Tercapai.** Model CF SVD berhasil diimplementasikan, termasuk baseline dan versi yang di-tuning dengan `GridSearchCV`.
5.  **Mengevaluasi kedua model:** **Tercapai.** CBF dievaluasi secara kualitatif dengan skor kemiripan. CF dievaluasi dengan RMSE, MAE, Precision@k, dan Recall@k.
6.  **Menganalisis dan membandingkan hasil:** **Tercapai.** Perbandingan kinerja, kelebihan, dan kekurangan kedua model telah didiskusikan.

#### Dampak Pendekatan Solusi
1.  **Eksplorasi dan Pemahaman Data:** Pendekatan EDA yang detail sangat berdampak pada kualitas data yang digunakan untuk modeling. Identifikasi `(no genres listed)`, pemahaman distribusi rating, dan *sparsity* data mengarahkan langkah pra-pemrosesan dan pemilihan model yang lebih tepat.
2.  **Persiapan Data:** Pembuatan `feature_soup` yang cermat untuk CBF dan penyiapan data rating yang benar untuk CF memastikan model menerima input yang optimal. Penanganan `(no genres listed)` dan pengisian `NaN` pada `tags_aggregated` mencegah error dan meningkatkan kualitas fitur CBF.
3.  **Pengembangan Model CBF:** Penggunaan TF-IDF dan Cosine Similarity menghasilkan rekomendasi yang secara intuitif relevan berdasarkan konten, yang dapat meningkatkan kepercayaan pengguna pada sistem.
4.  **Pengembangan Model CF:** Implementasi SVD dan *hyperparameter tuning* (meskipun peningkatannya marginal pada *test split* ini) menunjukkan upaya untuk optimasi dan menghasilkan model CF dengan akurasi prediksi rating yang baik serta kualitas peringkat top-N yang layak. Kemampuan CF untuk menghasilkan rekomendasi yang lebih beragam (*serendipitous*) berpotensi meningkatkan *engagement* pengguna.
5.  **Evaluasi Model:** Penggunaan metrik yang beragam (kualitatif, prediksi rating, peringkat) memberikan pandangan yang holistik terhadap kinerja masing-masing model, memungkinkan pengambilan keputusan yang lebih informasional mengenai potensi penggunaan model di dunia nyata. Penjelasan metrik juga meningkatkan pemahaman terhadap hasil.

Secara keseluruhan, pendekatan solusi yang direncanakan telah berhasil diimplementasikan dan memberikan dampak positif dalam menjawab pernyataan masalah dan mencapai tujuan proyek.

## Referensi
1.  Ricci, F., Rokach, L., & Shapira, B. (2011). *Introduction to Recommender Systems Handbook*. Springer.
2.  Aggarwal, C. C. (2016). *Recommender Systems: The Textbook*. Springer.
3.  Sarwar, B., Karypis, G., Konstan, J., & Riedl, J. (2001). Item-based collaborative filtering recommendation algorithms. *Proceedings of the 10th international conference on World Wide Web*, 285-295.
4.  Su, X., & Khoshgoftaar, T. M. (2009). A survey of collaborative filtering techniques. *Advances in artificial intelligence*, *2009*.
5.  Pazzani, M. J., & Billsus, D. (2007). Content-based recommendation systems. In *The adaptive web* (pp. 325-341). Springer Berlin Heidelberg.
6.  Linden, G., Smith, B., & York, J. (2003). Amazon.com recommendations: Item-to-item collaborative filtering. *IEEE Internet Computing*, *7*(1), 76-80.
7.  Resnick, P., Iacovou, N., Suchak, M., Bergstrom, P., & Riedl, J. (1994). GroupLens: an open architecture for collaborative filtering of netnews. *Proceedings of the 1994 ACM conference on Computer supported cooperative work*, 175-186.
8.  Koren, Y., Bell, R., & Volinsky, C. (2009). Matrix factorization techniques for recommender systems. *Computer*, *42*(8), 30-37.
9.  Funk, S. (2006). *Netflix Update: Try This at Home*. [https://sifter.org/~simon/journal/20061211.html](https://sifter.org/~simon/journal/20061211.html)
10. Bennett, J., & Lanning, S. (2007). The Netflix prize. *Proceedings of KDD cup and workshop*.
11. Hug, N. (2020). Surprise: A Python library for recommender systems. *Journal of Open Source Software*, *5*(52), 2174. [https://joss.theoj.org/papers/10.21105/joss.02174](https://joss.theoj.org/papers/10.21105/joss.02174)
12. Harper, F. M., & Konstan, J. A. (2015). The MovieLens datasets: History and context. *ACM Transactions on Interactive Intelligent Systems (TiiS)*, *5*(4), 1-19.
13. GroupLens. (n.d.). *MovieLens Datasets*. University of Minnesota. Diakses dari [https://grouplens.org/datasets/movielens/](https://grouplens.org/datasets/movielens/)
14. Herlocker, J. L., Konstan, J. A., Terveen, L. G., & Riedl, J. T. (2004). Evaluating collaborative filtering recommender systems. *ACM Transactions on Information Systems (TOIS)*, *22*(1), 5-53.
15. Shani, G., & Gunawardana, A. (2011). Evaluating recommendation systems. In *Recommender systems handbook* (pp. 257-297). Springer US.
16. Dokumentasi Scikit-learn. *TfidfVectorizer*. Diakses dari [https://scikit-learn.org/stable/modules/generated/sklearn.feature_extraction.text.TfidfVectorizer.html](https://scikit-learn.org/stable/modules/generated/sklearn.feature_extraction.text.TfidfVectorizer.html)
17. Dokumentasi Scikit-learn. *cosine_similarity*. Diakses dari [https://scikit-learn.org/stable/modules/generated/sklearn.metrics.pairwise.cosine_similarity.html](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.pairwise.cosine_similarity.html)
18. Dokumentasi Surprise. *SVD algorithm*. Diakses dari [https://surprise.readthedocs.io/en/stable/matrix_factorization.html#surprise.prediction_algorithms.matrix_factorization.SVD](https://surprise.readthedocs.io/en/stable/matrix_factorization.html#surprise.prediction_algorithms.matrix_factorization.SVD)
19. Dokumentasi Surprise. *GridSearchCV*. Diakses dari [https://surprise.readthedocs.io/en/stable/model_selection.html#surprise.model_selection.search.GridSearchCV](https://surprise.readthedocs.io/en/stable/model_selection.html#surprise.model_selection.search.GridSearchCV)


