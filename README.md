# 📊 Scraping Ulasan Shopee + Text Processing

Otomasi pengambilan ulasan produk dari Shopee dan pembersihan teks untuk analisis NLP (Natural Language Processing).

---

## 🎯 Fitur Utama

✅ **Scraping Otomatis** - Ambil ulasan dari halaman Shopee tanpa login manual  
✅ **Bypass Bot Detection** - Menggunakan undetected ChromeDriver  
✅ **Text Processing** - Pembersihan teks dengan Sastrawi (Indonesian Stemmer)  
✅ **Export Data** - Hasil tersimpan dalam format CSV dan Excel  
✅ **Flexible Configuration** - Dapat menyesuaikan jumlah halaman dan parameter

---

## 📋 Prerequisites

### Sistem Operasi
- Windows, macOS, atau Linux

### Browser
- Google Chrome (versi terbaru)

### Python
- Python 3.7 atau lebih baru

---

## 🚀 Instalasi

### 1. Clone atau Download Project
```bash
cd "Scrapping komentar shopee"
```

### 2. Install Dependencies
```bash
pip install undetected-chromedriver pandas selenium webdriver-manager sastrawi openpyxl
```

Atau gunakan file requirements.txt (jika ada):
```bash
pip install -r requirements.txt
```

---

## 📖 Cara Penggunaan

### Tahap 1: Scraping Ulasan

**File:** `scrapping.ipynb` - Cell 1

**Langkah-langkah:**
1. Ganti `URL_TARGET` dengan link produk Shopee yang ingin di-scrape
2. Set `total_halaman` sesuai kebutuhan (default: 3)
3. Jalankan cell pertama
4. **PENTING:** Saat browser terbuka:
   - Scroll manual sampai melihat ulasan (minimal 5 ulasan)
   - Kembali ke terminal/notebook
   - Ketik `y` dan tekan Enter
5. Tunggu proses selesai

**Input:**
```python
URL_TARGET = "https://shopee.co.id/Ms-Glow-for-Men-..."
scrape_shopee_tembak_dalam(URL_TARGET, total_halaman=3)
```

**Output:**
- File: `dataset_ulasan.csv`
- Kolom: `Username`, `Rating`, `Komentar`
- Contoh:
  ```
  Username,Rating,Komentar
  RiskyStore,5,Produk bagus sangat puas
  BudiToko,4,Pengiriman cepat packaging rapi
  ```

---

### Tahap 2: Text Processing & Pembersihan

**File:** `scrapping.ipynb` - Cell 2

**Input File:** `dataset_ulasan.csv` (atau ubah nama file di variable `file_excel`)

**Langkah:**
1. Pastikan file `dataset_ulasan.csv` sudah ada di folder
2. Jalankan cell kedua
3. Proses pembersihan teks otomatis berjalan

**Proses Pembersihan:**

| No | Step | Input | Output |
|---|------|-------|--------|
| 1 | Case Folding | "MANTAP!" | "mantap!" |
| 2 | Cleaning | "Bagus!! 😍 Link: http..." | "bagus" |
| 3 | Stopword Removal | "yang di sini pun" | "sini" |
| 4 | Stemming | "pengiriman kualitas" | "kirim kualitas" |

**Contoh Transformasi Lengkap:**
```
Input:  "BARANGNYA MANTAP!! Pengiriman cepat banget 😍👍 Buruan beli sekarang juga!"
Output: "barang mantap pengiriman cepat buruan beli"
```

**Output:**
- File: `Dataset_NLP_Siap_Olah.xlsx`
- Kolom: `Username`, `Rating`, `Komentar`, `Komentar_Bersih`
- Data siap untuk analisis sentimen, topic modeling, atau machine learning

---

## 🔧 Konfigurasi

### Scraping Configuration
```python
# File: scrapping.ipynb - Cell 1

URL_TARGET = "https://shopee.co.id/..."  # Ubah dengan link produk
total_halaman = 3                         # Jumlah halaman
```

### Text Processing Configuration
```python
# File: scrapping.ipynb - Cell 2

file_excel = 'dataset_ulasan.csv'         # Input file
file_hasil = 'Dataset_NLP_Siap_Olah.xlsx' # Output file
```

### Class Selectors (Jika Shopee Update)
Jika scraping gagal, Shopee mungkin sudah update struktur HTML:

1. Buka halaman Shopee di browser
2. Right-click → Inspect Element
3. Cari class untuk:
   - Username: Ganti `.InK5kS` 
   - Komentar: Ganti `.YNedDV`
4. Update di code:
   ```python
   users = driver.find_elements(By.CLASS_NAME, "CLASS_BARU_USERNAME")
   contents = driver.find_elements(By.CLASS_NAME, "CLASS_BARU_KOMENTAR")
   ```

---

## 📊 Arsitektur Data Flow

```
┌─────────────────────────────┐
│   Shopee Product URL        │
└──────────────┬──────────────┘
               │
               ▼
      ┌────────────────────┐
      │  Scrape Ulasan     │ ◄─── Browser + Manual Scroll
      │  (undetected-cd)   │
      └────────────────────┘
               │
               ▼
      ┌────────────────────────┐
      │  dataset_ulasan.csv    │ ◄─── Raw Data (Username, Rating, Komentar)
      └────────────────────────┘
               │
               ▼
      ┌────────────────────────────┐
      │    Text Processing         │
      │  (Sastrawi Stemmer +       │
      │   Stopword Remover)        │
      └────────────────────────────┘
               │
               ▼
      ┌────────────────────────────────┐
      │ Dataset_NLP_Siap_Olah.xlsx     │ ◄─── Ready for NLP/ML
      │ (Username, Rating, Komentar,   │
      │  Komentar_Bersih)              │
      └────────────────────────────────┘
```

---

## 🐛 Troubleshooting

| Error | Penyebab | Solusi |
|-------|----------|--------|
| `WebDriver error` | Chrome version mismatch | Update Chrome ke versi terbaru |
| `0 ulasan ter-scrape` | Selectors sudah berubah di Shopee | Check class selectors dengan inspect element |
| `Connection timeout` | IP di-block Shopee | Gunakan VPN atau hotspot HP lain |
| `FileNotFoundError` | File input tidak ditemukan | Pastikan file ada di folder yang sama |
| `Module not found` | Library belum terinstall | `pip install -r requirements.txt` |
| `Memory error` | Data terlalu besar | Kurangi `total_halaman` |
| `Excel file error` | openpyxl belum terinstall | `pip install openpyxl` |

---

## 📝 Contoh Penggunaan Lengkap

```python
# ===== CELL 1: SCRAPING =====
URL_TARGET = "https://shopee.co.id/Ms-Glow-for-Men-Paket-Bundling-Perawatan-Skincare-Cowok-Facial-Wash-Serum-Cream-Maskulin-Sunscreen-i.20896488.18129438655"
scrape_shopee_tembak_dalam(URL_TARGET, total_halaman=5)

# Output: dataset_ulasan.csv dengan 100+ ulasan

# ===== CELL 2: TEXT PROCESSING =====
# (Baca dataset_ulasan.csv → bersihkan teks → simpan ke Excel)
# Otomatis di-run, tunggu selesai

# Output: Dataset_NLP_Siap_Olah.xlsx siap untuk analisis
```

---

## 📁 Struktur File

```
Scrapping komentar shopee/
├── README.md                          # File ini
├── scrapping.ipynb                    # Jupyter Notebook (main file)
├── dataset_ulasan.csv                 # Output scraping (auto-generated)
└── Dataset_NLP_Siap_Olah.xlsx        # Output text processing (auto-generated)
```

---

## ⚙️ Technical Stack

| Komponen | Library | Fungsi |
|----------|---------|--------|
| **Web Scraping** | undetected-chromedriver | Bypass deteksi bot |
| | Selenium | Automation browser |
| | webdriver-manager | Auto download ChromeDriver |
| **Data Handling** | Pandas | Proses DataFrame/CSV |
| **Text Processing** | Sastrawi | Stemming & Stopword (Indonesian) |
| | RegEx (re) | Pattern matching |
| **Export** | openpyxl | Export ke Excel |

---

## 🔐 Security & Ethics

⚠️ **Disclaimer:**
- Pastikan scraping sesuai dengan ToS Shopee
- Jangan gunakan untuk spam atau harassment
- Batasi jumlah request untuk tidak membebani server
- Gunakan dengan bijak untuk keperluan penelitian atau analisis

---

## 📚 Resources & Referensi

- [Selenium Documentation](https://selenium.dev/documentation/)
- [Sastrawi - Indonesian Text Processing](https://github.com/har07/PySastrawi)
- [Pandas Documentation](https://pandas.pydata.org/)
- [Shopee Terms of Service](https://shopee.co.id/)

---

## 🤝 Support & Kontribusi

Jika ada pertanyaan atau issue:
1. Check bagian Troubleshooting di atas
2. Verify class selectors di halaman Shopee
3. Pastikan semua dependencies sudah terinstall

---

## 📄 License

Project ini dibuat untuk keperluan edukasi dan penelitian.

---

**Last Update:** 22 Februari 2026  
**Status:** Active & Maintained
