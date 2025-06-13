# Laporan Proyek Machine Learning - Satria Adi Firmansyah
## Domain Proyek

**Latar Belakang**
Mesin milling (disebut juga mesin frais) adalah mesin perkakas yang digunakan untuk membentuk benda kerja dengan menggunakan alat potong yang berputar. Proses ini menghasilkan permukaan datar atau bentuk yang presisi pada benda kerja. Sebab cara kerjanya inilah, mesin milling sangat rawan terhadap kerusakan yang disebabkan oleh beban kinetik yang dihasilkan dari gesekan antara alat potong dengan benda yang dibentuk. Terlebih lagi, apabila mesin milling ini rusak, sangat berpotensi untuk menghambat jalannya produksi.

Beberapa faktor yang berkontribusi kepada kerusakan mesin milling ini termasuk suhu, torsi, kecepatan rotasi alat potong, usia pemakaian alat, hingga kualitas alat potong. Oleh sebab itu, sebuah metode yang mampu memprediksi kapan alat potong akan mengalami kerusakan berdasarkan faktor faktor tersebut, dapat membantu dalam mencegah kerusakan alat sebelum terjadi. 

Metode untuk prediksi ini bisa dibuat berbasis algoritma prediksi. Proyek ini bertujuan untuk meneliti algoritma prediksi apa yang mampu dengan tepat memprediksi kerusakan mesin milling. Nantinya, diharapkan algoritma prediksi ini mampu digunakan untuk memprediksi dengan tepat kapan kerusakan mesin milling akan terjadi, yang kemudian mampu mencegah kerusakan mesin milling sebelum terjadi. 

## Business Understanding
Proyek ini diharapkan dapat memberi dampak signifikan baik bagi dunia bisnis dan dunia produksi. Dalam sudut pandang bisnis, diharapkan proyek ini dapat memberikan solusi teknologi yang inovatif bagi lembaga produsen yang menggunakan mesin milling, dan lembaga produsen mesin milling itu sendiri. Dalam sudut pandang dunia produksi, proyek ini berpotensi membantu produsen yang menggunakan mesin milling dalam mencegah kerusakan mesinnya sebelum terjadi, serta membantu produsen mata potong mesin milling ini dalam mengembangkan produk yang berkualitas lebih tinggi lagi.

### Problem Statement
1. Bagaimana mengembangkan algoritma prediksi yang efektif untuk klasifikasi risiko kerusakan mesin milling berdasarkan faktor fisis?
2. Bagaimana implementasi algoritma prediksi dalam meningkatkan deteksi potensi kerusakan mesin milling berdasarkan teknologi yang akan dibangun?

### Goals
1. Mengembangkan algoritma dengan tingkat akurasi prediksi yang cukup akurat untuk klasifikasi risiko kerusakan mesin milling.
2. Mengimplementasikan algoritma prediksi dalam meningkatkan deteksi dini serangan jantung berdasarkan teknologi yang akan dibangun.

### Solution
1. Mengumpulkan data kerusakan mesin milling yang terekam beserta faktornya dan memproses data tersebut agar bisa diolah oleh algoritma prediksi.
2. Mengembangkan model prediksi menggunakan beberapa algoritma pilian seperti Random Forest Classifier, XGBoost Classifier, dan Multi Layer Perceptron Classifier
3. Melakukan analisa terhadap kinerja masing masing model dengan metrik evaluasi seperti Accuracy memilih model dengan metrik evaluasi terbaik

## Data Understanding
Dalam membangun model prediksi, proyek ini akan menggunakan Dataset Predictive Maintenance Dataset (AI4I 2020) oleh Stephan Matzka yang diambil dari Kaggle pada link [berikut](https://www.kaggle.com/datasets/stephanmatzka/predictive-maintenance-dataset-ai4i-2020)

Dataset dipilih karena jumlah fitur yang dinilai cukup untuk mewakili kategori-kategori yang berpotensi menjadi penyebab kerusakan mesin milling. Seleksi dataset dilakukan setelah melakukan evaluasi terhadap beberapa dataset yang tersedia di Kaggle.

Dataset terkait memiliki 10000 data dengan 14 fitur, yang mana 8 fitur akan diseleksi dalam proses analisis data selanjutnya untuk digunakan dalam algoritma prediksi, dan 6 fitur target yaitu Machine failure, TWF, HDF, PWF, OSF, dan RNF. Variabel dalam dataset mencakup:
1. UDI: nomor identifikasi (object)
2. product ID: nomor serial mesin, berisikan huruf L, M, dan H yang mengindikasikan kualitas mesin (object)
3. type: Tipe kualitas produk sesuai kolom 2 (object)
4. air temperature [K]: Suhu udara (numerikal)
5. process temperature [K]: Suhu proses (numerikal)
6. rotational speed [rpm]: kecepatan rotasi dari alat potong (numerikal)
7. torque [Nm]: torsi dari alat potong, (numerikal)
8. tool wear [min]: Menit penggunaan alat,(numerikal)
9. machine failure: terjadinya kerusakan alat potong (kategorikal: ya/tidak)

Serta detail lebih lanjut mengenai tipe kerusakan yang tercakup pada dataset ini yaitu:
1. tool wear failure (TWF): kerusakan akibat usia pemakaian alat, setelah sekitar 200-240 menit (kategorikal)
2. heat dissipation failure (HDF): kerusakan ketika perbedaan antara suhu udara dengan proses di bawah 8.6 K dan kecepatan rotasi alat di bawah 1380 rpm. (kategorikal)
3. power failure (PWF): kerusakan akibat kesamaan antara daya yang dibutuhkan untuk proses dengan torsi dan kecepatan rotasi. Jika daya ini di bawah 3500 W dan di atas 9000 W (kategorikal)
4. overstrain failure (OSF): kerusakan akibat usia pemakaian  dan torsi, yang melebihi 11000 Nm untuk tipe L, 12000 Nm untuk tipe M, dan 13000 untuk tipe H. (kategorikal)
5. random failures (RNF): kerusakan yang terjadi diluar parameter kerusakan apapun, dengan potensi kerusakan seperti ini untuk semua alat hanya sebesar 0.1%. (kategorikal)

Hasil analisis data tidak menemukan adanya data duplikat, maupun kosong dalam dataset. Kemudian, untuk Outlier/Pencilan, hanya ditemukan pada fitur `Rotational Speed [rpm]` serta `Torque [Nm]`.
Kemudian, untuk analisis data multivariat, ditemukan:
- Paling banyak kerusakan terjadi pada mesin dengan tipe kualitas L (low), terbanyak kedua ada pada tipe M (medium) dan paling sedikit pada tipe H (high)
- Terdapat paling banyak kerusakan terjadi pada rentang suhu 300 K hingga sekitar 303 K, serta ada sedikit outlier kerusakan yang terjadi pada suhu di bawah 300 K untuk mesin tipe M dan H
- Terdapat paling banyak kerusakan terjadi pada rentang suhu 309 K hingga sekitar 311 K, serta ada beberapa outlier kerusakan yang terjadi pada suhu di bawah 308 K untuk semua tipe mesin
- Mayoritas kerusakan terjadi pada rentang kecepatan rotasi 1250 rpm hingga sekitar 1500 rpm, serta ada beanyak outlier kerusakan yang terjadi pada rotasi di atas 1500 rpm pada semua tipe mesin
- Mayoritas kerusakan terjadi pada rentang torsi 40 nm hingga sekitar 70 Nm, serta ada banyak outlier kerusakan yang terjadi pada torsi di bawah 30 Nm pada semua tipe mesin
- Mayoritas kerusakan terjadi pada rentang torsi 40 nm hingga sekitar 70 Nm, serta ada banyak outlier kerusakan yang terjadi pada torsi di bawah 30 Nm pada semua tipe mesin
- Mayoritas kerusakan terjadi pada rentang selisih suhu 8 K hingga sekitar 9 K

## Data Preparation
1. Hapus dahulu kolom yang tidak akan digunakan. Kolom yang tidak akan digunakan adalah `UDI`, `Product ID`, serta segala jenis spesifik kerusakan mesin, dari `TWF` hingga `RNF`, karena kita akan terfokus ke kerusakan mesin keseluruhan.
2. Terapkan Label Encoding ke fitur `Type` agar bisa dilihat korelasinya dengan data lain
3. Periksa korelasi antar fitur dalam dataset. Ditemukan korelasi tertinggi dengan `Machine failure` ada dengan data `Torque [Nm]`, `Tool wear [min]`, serta `Temp Diff [K]`
4. Standarisasi fitur yang memiliki standar deviasi tinggi yaitu `Rotational speed [rpm]`, dan `Tool wear [min]`
5. Train Test Split, dimana data akan dipecah menjadi data pelatihan model (80% dari keseluruhan) dan data pengujian model (20% dari keseluruhan). Fitur yang akan digunakan utamanya untuk menghasilkan prediksi adalah data yang berkorelasi paling tinggi dengan kerusakan mesin milling, yaitu `Torque [Nm]`, `Tool wear [min]`, serta `Temp Diff [K]`. 
6. Periksa shape dari data latih dan data uji. Terdapat 8000 baris data latih, dan 2000 baris data uji.

## Modelling
### XGBoost Classifier
XGBoost CLassifier merupakan salah satu algoritma yang terbaru dalam dunia machine learning, berbasis gradien ekstrim. Keunggulan XGBoost mencakup akurasi tinggi, skalabilitas, dan fleksibilitas, serta regularisasi bawaan XGBoost membantu mencegah overfitting dan meningkatkan generalisasi.

Parameter XGBoost yang akan digunakan adalah:
- **booster**: booster yang digunakan oleh XGBoost (sesuai namanya dalam algoritmanya, Boost). Defaultnya adalah 'gbtree', yang menggunakan booster berbasis pohon. 
- **tree_method**: struktur pohon yang digunakan oleh booster, khusus booster tipe pohon. Jenis 'exact' menggunakan algoritma tipe serakah (greedy), yang mengenumerasikan semua kandidat.

Setelah pelatihan, model mencapai akurasi sebesar 99.7% terhadap data latih. Saat dievaluasi terhadap data uji, akurasinya turun menjadi 97.6%, tidak terlalu signifikan. 

### Random Forest Classifier
Random Forest Classifier layak dicoba karena merupakan salah satu algoritma berbasis Ensemble Learning yang umum digunakan sejak dahulu. Keunggulan Random Forest mencakup kinerja yang baik dalam berbagai jenis masalah klasifikasi, kemampuan bawaan untuk mengurangi overfitting.

Parameter Random Forest Classifier yang digunakan adalah: 
- n_estimators: Jumlah pohon dalam forest. (Nilai Default = 100)
- max_depth: Kedalaman atau panjang pohon. Ini merupakan ukuran seberapa banyak pohon dapat membelah (splitting) untuk membagi setiap node ke dalam jumlah pengamatan yang diinginkan.
- random_state: Digunakan untuk mengontrol random number generator yang digunakan.

Setelah pelatihan, model mencapai akurasi sebesar 99.97% terhadap data latih. Saat dievaluasi terhadap data uji, akurasinya turun menjadi 98%, tidak terlalu signifikan. 

### Multi Layer Perceptron (MLP) Classifier
MLP Classsifier adalaha algoritma klasifikasi berbasis neural network (perceptron). Keunggulan MLP Classifier mencakup keunggulan dalam pemodelan hubungan non-linier, mempelajari fitur secara otomatis, dan menangani kumpulan data besar dengan skalabilitas.

Parameter yang digunakan dalam MLP Classifier mencakup:
- **activation**: fungsi aktivasi dari neuron. Defaultnya adalah 'relu', tapi karena proyek klasifikasi ini hanya mencakup klasifikasi biner, maka digunakan aktivasi 'logistic'.
- **solver**: solver untuk optimisasi neuron. Defaultnya adalah 'adam'
- **max_iter**: membatasi iterasi yang dilaksanakan oleh solver. 

Setelah pelatihan, model mencapai akurasi sebesar 97.2% terhadap data latih. Saat dievaluasi terhadap data uji, akurasinya tetap di angka 97.2%.

## Evaluasi
### Metrik Akurasi

Metrik evaluasi "accuracy" adalah ukuran yang digunakan untuk mengukur seberapa akurat model klasifikasi dalam mengklasifikasikan data dengan benar, dihitung dari jumlah prediksi yang benar dibagi dengan jumlah total prediksi yang dilakukan oleh model. Rumusnya adalah: 

Accuracy = Jumlah Prediksi Benar/Jumlah Total Prediksi

dimana: 
- Jumlah Prediksi Benar adalah jumlah sampel yang diklasifikasikan dengan benar oleh model.
- Jumlah Total Prediksi adalah jumlah total sampel yang dievaluasi oleh model.
Accuracy memberikan gambaran tentang seberapa baik model klasifikasi dapat mengklasifikasikan semua kelas dengan benar. 

Meskipun accuracy adalah metrik yang penting, namun pada beberapa kasus, accuracy tidak bisa menjadi patokan satu-satunya dalam mengevaluasi performa model.

### Hasil Prediksi dan Kesimpulan
Berikut adalah hasil pengujian model prediksi menggunakan XGBoost, Random Forest, dan Multi Layer Perceptron, masing-masing 1 kali percobaan.

|   | Model                     | Accuracy Test | Accuracy Train |
|---|---------------------------|---------------|----------------|
| 1 | XGBoost                   | 0.976      | 0.997       |
| 2 | Random Forest             | 0.980      | 0.999       |
| 3 | Multi Layer Perceptron    | 0.972      | 0.972       |


Hasil Penelitian diatas mendapatkan kesimpulan :

1. Random Forest bekerja paling baik dengan data latih (99.9% akurasi) dan data uji (98% akurasi)
2. XGBoost bekerja sangat baik dengan data latih (99.7% akurasi), meskipun kalah tipis baiknya dari Random Forest (97.6% akurasi)
3. Multi Layer Perceptron bekerja paling konsisten, dengan akurasi yang persis dengan data latih dan data uji (keduanya di 97.2% akurasi)

Dari hasil di atas, bisa disimpulkan bahwa Random Forest layak digunakan untuk diimplementasikan dalam memprediksi kerusakan mesin milling, karena akurasinya yang tinggi. Metode ini layak untuk diimplementasikan ke lapangan karena mampu membantu mencegah kerusakan mesin milling yang berpotensi berdampak kepada laju produksi barang, atau berdampak kepada kemajuan kualitas alat potong mesin milling di masa depan. 
