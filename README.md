# HO1_MLOps_NabilahShamid - Data Collection & Preprocessing

## Pendahuluan

Pada Hands-On pertama ini, saya melakukan proses pengumpulan dan pembersihan data kursus atau pekerjaan dari sumber data publik. Data hasil *scraping* (atau dalam kasus ini, data publik yang disediakan) kemudian diproses dan dibersihkan melalui beberapa tahapan *preprocessing* teks dasar seperti normalisasi, penghilangan *stopwords*, serta *stemming*. Setelah data diproses hingga bersih, hasil akhirnya disimpan dalam format yang mudah digunakan untuk tahap pemodelan berikutnya (format `.csv` dan `.json`). Data ini akan digunakan sebagai *input* awal *pipeline* sistem rekomendasi di modul selanjutnya. 

## Data yang Digunakan

Dataset yang digunakan dalam pengerjaan Hands-On 1 ini adalah dataset publik yang diunduh dari Kaggle, berjudul "[Data Scientist Job Dataset Collections of data scientist and network engineer related job from jobstreet](https://www.kaggle.com/datasets/azraimahadan/data-scientist-jobstreet-scraped)" oleh Azrai Mahadan.

Dataset ini berisi informasi lowongan pekerjaan yang relevan dengan posisi Data Scientist dan Network Engineer dari JobStreet. Dataset ini memiliki **11 kolom** dan **606 baris** data, dengan fitur-fitur utama meliputi:
* `job_id`: ID unik untuk setiap lowongan pekerjaan.
* `job_title`: Judul atau nama posisi pekerjaan.
* `company`: Nama perusahaan yang membuka lowongan.
* `descriptions`: Deskripsi lengkap mengenai pekerjaan dan persyaratannya.
* `location`: Lokasi kantor atau tempat kerja.
* `category`: Kategori umum pekerjaan (misal: IT).
* `subcategory`: Sub-kategori pekerjaan yang lebih spesifik (misal: Data Science).
* `type`: Jenis pekerjaan (misal: Full-time, Permanent).
* `salary`: Informasi rentang gaji (jika tersedia).

## Proses Pengumpulan Data (Web Scraping)

Sebagai bagian dari tahapan Data Collection, awalnya saya berupaya melakukan *web scraping* secara mandiri untuk mengumpulkan data lowongan pekerjaan dari platform Glints menggunakan Selenium. Namun, dalam prosesnya, saya menghadapi kendala teknis yang signifikan. Halaman situs Glints tidak dapat dimuat dengan sempurna atau bahkan tidak merender konten lowongan sama sekali, mengindikasikan adanya mekanisme anti-*scraping* yang kuat. Nampaknya, Glints kemungkinan mendeteksi aktivitas Selenium sebagai bot, sehingga menghalangi akses saya untuk mengekstrak data yang dibutuhkan secara otomatis.
Oleh karena adanya hambatan teknis yang tidak dapat diatasi dalam waktu pengerjaan ini, saya kemudian memutuskan untuk menggunakan dataset publik yang tersedia di Kaggle sebagai alternatif. Meskipun penggunaan dataset publik ini akan berpengaruh pada penilaian, saya memprioritaskan untuk menunjukkan pemahaman dan implementasi pada tahapan *data processing* sesuai instruksi.

## Proses Data Preprocessing (Text Cleaning)

Data mentah yang diperoleh (dalam hal ini, dari dataset Kaggle) seringkali masih "kotor" dan memerlukan pembersihan sebelum dapat digunakan untuk analisis atau pemodelan Machine Learning. Proses *Data Preprocessing* atau *Text Cleaning* bertujuan untuk membersihkan data dari *noise* dan menstandarisasi formatnya.
Berikut adalah tahapan *text preprocessing* yang dilakukan:

1.  **Penghapusan Kolom `Unnamed: 0`**: Kolom ini merupakan indeks yang sering muncul secara otomatis saat membaca file CSV dan tidak memiliki relevansi analitis, sehingga dihapus.

2.  **Lowercasing**: Semua teks diubah menjadi huruf kecil. Ini penting untuk memastikan bahwa kata yang sama dengan kapitalisasi berbeda (misalnya, "Data" dan "data") dianggap sebagai entitas yang sama.

3.  **Penghapusan Noise**: Tahap ini bertujuan untuk menghilangkan elemen-elemen yang tidak memberikan nilai semantik dalam analisis teks. Noise yang dihapus meliputi:
    * **Tag HTML**: (`<p>`, `</p>`) yang sering menyertai teks yang di-*scrape*.
    * **URL**: Alamat web lengkap (misalnya `https://`, `www.`).
    * **Hashtag**: Kata-kata yang diawali dengan simbol `#`.
    * **Emoji**: Simbol-simbol visual.
    * **Angka**: Semua digit numerik.
    * **Tanda Baca**: Karakter non-alfanumerik seperti `!`, `?`, `.`, `,`, `-`, dll.
    * **Spasi Berlebih**: Mengganti beberapa spasi dengan satu spasi tunggal dan menghapus spasi di awal/akhir kalimat.

4.  **Penghapusan Stopwords**: Kata-kata umum Bahasa Indonesia (misalnya "di", "dan", "untuk") yang sering muncul namun tidak membawa makna signifikan dalam konteks tertentu akan dihapus. Ini membantu fokus pada kata kunci yang lebih informatif.

5.  **Stemming**: Kata-kata yang memiliki variasi bentuk (misalnya karena imbuhan) akan dikembalikan ke bentuk kata dasarnya (stem) menggunakan library Sastrawi. Contoh: "mewajibkan" menjadi "wajib", "pembelajaran" menjadi "ajar", "daftarkan" menjadi "daftar".

**Pemilihan Kolom untuk Preprocessing:**

* **Kolom Teks Bebas (Full Text Preprocessing):**
    * `job_title`: Diproses sepenuhnya untuk `cleaned_job_title`.
    * `descriptions`: Diproses sepenuhnya untuk `cleaned_descriptions`.
    Kolom-kolom ini mengandung teks naratif yang luas dan merupakan fokus utama analisis konten.

* **Kolom String Kategori (Normalisasi Ringan):**
    * `company`: Dinormalisasi menjadi `normalized_company`.
    * `location`: Dinormalisasi menjadi `normalized_location`.
    * `category`: Dinormalisasi menjadi `normalized_category`.
    * `subcategory`: Dinormalisasi menjadi `normalized_subcategory`.
    * `type`: Dinormalisasi menjadi `normalized_type`.
    Untuk kolom-kolom ini, hanya dilakukan *lowercasing* dan *strip whitespace* untuk menjaga konsistensi format tanpa mengubah makna kategoris.                                                                                                                                                                                                                                                                                                                                                                                                               | `normalized_location` | `kuala lumpur`                                                                                                                                                           |

## Refleksi Singkat (Kendala dan Solusi)

Selama pengerjaan tugas Hands-On 1 ini, saya menghadapi beberapa kendala teknis dan implementasi, yang berhasil saya atasi dengan solusi yang relevan:

* **Kendala Web Scraping**: Awalnya, saya berupaya melakukan *web scraping* secara mandiri untuk mengumpulkan data lowongan pekerjaan dari platform Glints menggunakan Selenium. Namun, dalam prosesnya, saya menghadapi kendala teknis yang signifikan: halaman situs Glints tidak dapat dimuat dengan sempurna atau bahkan tidak merender konten lowongan sama sekali. Ini mengindikasikan adanya mekanisme anti-*scraping* yang mendeteksi aktivitas Selenium sebagai bot, menghalangi akses untuk mengekstrak data.
* **Solusi Web Scraping**: Karena kendala teknis yang tidak dapat diatasi dalam waktu pengerjaan, saya memutuskan untuk menggunakan dataset publik dari Kaggle (`Data Scientist Job Dataset Collections`) sebagai *input* untuk tahapan *data processing*. Pendekatan ini memungkinkan saya untuk tetap menunjukkan pemahaman saya pada proses *text preprocessing* sesuai instruksi tugas, meskipun ada konsekuensi pengurangan poin karena tidak melakukan *scraping* mandiri.
* **Kendala Preprocessing (Regex)**: Saat pertama kali menerapkan *pipeline preprocessing*, saya menemukan bahwa kolom teks yang telah dibersihkan menghasilkan output yang tidak sesuai (misalnya, kata-kata menyatu tanpa spasi, atau hanya berisi karakter 's' dan 'w' yang berulang). Ini disebabkan oleh kesalahan penulisan *regular expression* di fungsi `clean_noise`.
* **Solusi Preprocessing (Regex)**: Saya melakukan *debugging* dan mengidentifikasi bahwa penggunaan *backslash* ganda (`\\w`, `\\s`) di dalam *raw string* (`r''`) menyebabkan *regex* salah menginterpretasikan karakter spesial. Setelah perbaikan menjadi *single backslash* (`\w`, `\s`), *pipeline preprocessing* berfungsi sebagaimana mestinya, menghasilkan teks yang bersih dan terpisah dengan benar.
* **Kendala Preprocessing (Nama Kolom & Cakupan)**: Saya juga sempat keliru dalam mengidentifikasi nama kolom (`descriptions` alih-alih `job_description`) dan belum sepenuhnya menormalisasi semua kolom *string* yang potensial.
* **Solusi Preprocessing (Nama Kolom & Cakupan)**: Saya melakukan inspeksi ulang `df.info()` untuk memastikan nama kolom yang benar dan memperluas cakupan *preprocessing* dengan menambahkan fungsi normalisasi ringan (`normalize_categorical_string`) pada kolom-kolom kategori (`company`, `location`, `category`, `subcategory`, `type`) untuk konsistensi data. Kolom indeks (`Unnamed: 0`) juga dihapus karena tidak dibutuhkan.

## *Deliverables*

Repositori GitHub ini berisi *deliverables* berikut:
* `HO1_MLO_[Nama Lengkap Anda].ipynb` (Jupyter Notebook dengan kode *scraping*/*preprocessing*).
* `data_scientist_jobstreet_scraped_v2.csv` (Dataset mentah dari Kaggle).
* `cleaned_job_data.csv` (Dataset yang sudah dibersihkan dalam format CSV).
* `cleaned_job_data.json` (Dataset yang sudah dibersihkan dalam format JSON).
* `README.md` (Laporan singkat ini).

---
