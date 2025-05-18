# Unila World Class University
## Deskripsi

Proyek ini adalah Pengembangan Platform Digital dalam Peningkatan Reputasi Unila Menuju World Class University.

## Fitur

- Login otomatis ke akun Sinta menggunakan kredensial dari file .env
- Scraping data jurnal dari halaman yang ditentukan
- Penyimpanan data dalam format CSV
- Pencegahan duplikasi data dengan mekanisme normalisasi judul
- Penanganan error dan percobaan ulang otomatis
- Penyimpanan kemajuan secara berkala
- Preprocessing data CSV untuk analisis lebih lanjut
- Preprocessing NLP pada judul artikel untuk analisis teks
- Labeling otomatis SDGs pada data artikel menggunakan model machine learning

## Teknologi yang Digunakan

- **Python 3.x** - Bahasa pemrograman utama
- **Selenium** - Framework otomatisasi web
- **BeautifulSoup4** - Library parsing HTML
- **Chrome WebDriver** - Driver browser untuk Selenium
- **Pandas** - Library untuk manipulasi dan analisis data
- **NumPy** - Library untuk operasi numerik
- **NLTK** - Natural Language Toolkit untuk pemrosesan bahasa alami
- **scikit-learn** - Library untuk machine learning dan vektorisasi teks
- **Sastrawi** - Library stemming untuk Bahasa Indonesia
- **langdetect** - Library untuk deteksi bahasa
- **googletrans** - Library untuk terjemahan teks
- **Python Libraries**:
  - csv - Untuk operasi file CSV
  - os - Untuk operasi file dan direktori
  - re - Untuk penggunaan regular expressions
  - time - Untuk pengaturan waktu tunggu
  - python-dotenv - Untuk manajemen variabel lingkungan
  - argparse - Untuk parsing argumen command line
  - joblib - Untuk menyimpan dan memuat model scikit-learn

## Persyaratan Sistem

- Python 3.6 atau lebih tinggi
- Google Chrome Browser
- Koneksi internet yang stabil

## Instalasi

1. Clone repositori ini:

```bash
git clone https://github.com/username/HLS_Project.git
cd HLS_Project
```

2. Instal dependensi:

```bash
pip install selenium beautifulsoup4 python-dotenv pandas numpy nltk scikit-learn langdetect googletrans==4.0.0-rc1 Sastrawi joblib tqdm
```

3. Download dan instal Chrome WebDriver:

   - Kunjungi: https://sites.google.com/chromium.org/driver/
   - Unduh versi yang sesuai dengan Chrome yang diinstal
   - Letakkan file executable di lokasi yang terdaftar di PATH sistem

4. Buat file `.env` di direktori root proyek:

```
SINTA_EMAIL=email_anda@example.com
SINTA_PASSWORD=password_anda
```

## Penggunaan

### Konfigurasi Dasar

1. Jalankan aplikasi dengan konfigurasi default (halaman 2503-3336):

```bash
python main.py
```

2. Atau tentukan rentang halaman:

```bash
python main.py --start 2503 --end 3336
```

3. Hasil scraping akan disimpan dalam file CSV dengan format: `sinta_articles_[START_PAGE]_to_[END_PAGE].csv`

### Preprocessing Data

Setelah melakukan scraping, Anda dapat melakukan preprocessing pada data hasil scraping dengan dua cara:

#### Cara 1: Menjalankan scraping dan preprocessing sekaligus

```bash
python main.py --preprocess
```

Atau dengan rentang halaman kustom:

```bash
python main.py --start 2503 --end 3336 --preprocess
```

#### Cara 2: Hanya menjalankan preprocessing pada file CSV yang sudah ada

```bash
python main.py --only-preprocess data/csv/sinta_articles_2503_to_3336.csv
```

Preprocessing data mencakup:

- Penanganan nilai kosong
- Normalisasi kolom judul dan penulis
- Standarisasi format tahun
- Konversi sitasi ke nilai numerik
- Penghapusan duplikat
- Pengurutan berdasarkan tahun dan jumlah sitasi

Hasil preprocessing akan disimpan dengan format: `[namafile]_processed.csv`

### Preprocessing NLP pada Judul Artikel

Anda dapat melakukan preprocessing NLP (Natural Language Processing) pada judul artikel untuk persiapan analisis teks lanjutan:

#### Cara 1: Menjalankan scraping, preprocessing data, dan NLP sekaligus

```bash
python main.py --preprocess --nlp
```

#### Cara 2: Hanya menjalankan NLP pada file CSV yang sudah ada

```bash
python main.py --only-nlp data/csv/sinta_articles_2503_to_3336.csv
```

#### Cara 3: Menjalankan preprocessing data dan NLP pada file yang sudah ada

```bash
python main.py --only-preprocess data/csv/sinta_articles_2503_to_3336.csv --nlp
```

Untuk menerjemahkan judul dalam bahasa asing ke Bahasa Indonesia (lambat), tambahkan flag `--translate`:

```bash
python main.py --only-nlp data/csv/sinta_articles_2503_to_3336.csv --translate
```

Preprocessing NLP mencakup:

1. **Case folding** - Mengubah semua teks menjadi huruf kecil
2. **Punctuation removal** - Menghapus tanda baca dan simbol
3. **Language detection** - Mendeteksi bahasa dari judul artikel
4. **Translation** (opsional) - Menerjemahkan judul non-Indonesia ke Bahasa Indonesia
5. **Tokenization** - Memecah teks menjadi token (kata-kata)
6. **Stopword removal** - Menghapus kata-kata umum yang tidak informatif
7. **Stemming** - Mengubah kata menjadi bentuk dasar (menggunakan Sastrawi untuk Bahasa Indonesia)
8. **Vectorization** - Mengubah teks menjadi representasi vektor menggunakan TF-IDF

Hasil preprocessing NLP akan disimpan dengan format: `[namafile]_nlp.csv`, dan
vektorisasi disimpan sebagai file terpisah: `[namafile]_nlp_tfidf_vectorizer.pkl` dan `[namafile]_nlp_tfidf_features.pkl`.

### Labeling Otomatis SDGs

Fitur ini melakukan labeling otomatis pada data artikel Sinta menggunakan model machine learning berbasis RandomForest dan TF-IDF. Data latih diambil dari `data/label_sdgs.csv`. Hasil prediksi label SDGs disimpan di `data/sinta_articles_2503_to_3336_labeled.csv`. Data yang belum terlabeli otomatis diekspor ke `data/unlabeled_for_review.csv` untuk labeling manual. Script pembersihan label multi-SDG (format `{"choices":...}`) tersedia, hasil bersih di `data/2503_to_3336_labeled_clean.csv`.

#### Cara Menjalankan Labeling Otomatis

1. Jalankan script labeling otomatis:

```bash
python interfaces/label_sdgs.py
```

2. Hasil labeling otomatis akan tersimpan di `data/sinta_articles_2503_to_3336_labeled.csv`.
3. Untuk data yang belum terlabeli, cek file `data/unlabeled_for_review.csv` dan lakukan labeling manual jika diperlukan.
4. Untuk membersihkan format label multi-SDG, jalankan script yang sama, hasil bersih ada di `data/2503_to_3336_labeled_clean.csv`.

#### Tips Peningkatan Akurasi

- Tambahkan data latih baru ke `label_sdgs.csv` dari hasil labeling manual.
- Lakukan retraining model dengan data latih yang diperbarui.
- Gunakan fitur prediksi top-N SDGs untuk membantu labeling manual.

### Opsi Tambahan

- **Penyesuaian Waktu Tunggu**: Edit nilai `time.sleep()` di `usecases/scraper.py` untuk mengatur kecepatan scraping
- **Rentang Halaman**: Sesuaikan `START_PAGE` dan `END_PAGE` di `main.py` untuk mengubah jangkauan scraping

## Struktur Proyek

```
HLS_Project/
├── .env                     # File kredensial (jangan commit ke repositori)
├── .gitignore               # File konfigurasi git
├── main.py                  # File utama untuk menjalankan aplikasi (scraping & preprocessing)
├── entities/
│   └── article.py           # Entitas artikel (model data)
├── interfaces/
│   ├── fetcher_selenium.py  # Interface untuk mengambil data menggunakan Selenium
│   ├── writer.py            # Interface untuk menulis data ke CSV
│   ├── csv_preprocessor.py  # Interface untuk preprocessing data CSV
│   └── nlp_processor.py     # Interface untuk preprocessing NLP pada judul artikel
└── usecases/
    └── scraper.py           # Implementasi logika utama scraping
```

## Arsitektur Aplikasi

Proyek ini mengikuti prinsip Clean Architecture dengan pemisahan:

1. **Entities Layer** - Representasi data (artikel jurnal)
2. **Interfaces Layer** - Mekanisme akses data (fetcher), output (writer), dan processing (csv_preprocessor)
3. **Use Cases Layer** - Logika bisnis untuk scraping dan pengolahan data
