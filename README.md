# Proyek Akhir Data Science — Kelompok 50 Next Level

## Judul
**Analisis Kondisi Armada Operasional Damkar DKI Jakarta Menggunakan Metode Exploratory Data Analysis (EDA) dan Random Forest**

Program Studi Informatika, Fakultas Teknik Industri, Universitas Pembangunan Nasional “Veteran” Yogyakarta.

## Anggota kelompok
- Dwirizki Adithya Putra — 123240021
- Satya Dio Putranto — 123240121
- Sebastian Alvaro Huller — 123240162
- Paksi Dwirangga — 123240172

## Gambaran proyek
Proyek ini mengkaji data kendaraan operasional penanggulangan kebakaran dan penyelamatan DKI Jakarta untuk periode 2025. Analisis dilakukan dengan melihat karakteristik dan sebaran kondisi kendaraan, menyiapkan data untuk pemodelan, lalu membandingkan hasil Random Forest dengan SVM kernel linear.

Kolom `Kondisi` merupakan status yang tercatat pada dataset periode tersebut. Hasil analisis tidak dapat dianggap sebagai informasi kesiapan armada secara real-time atau pengganti pemeriksaan teknis.

## Sumber data
[Satu Data Jakarta — Data Kendaraan Operasional Penanggulangan Kebakaran dan Penyelamatan](https://satudata.jakarta.go.id/open-data/detail?kategori=dataset&page_url=data-kendaraan-operasional-penanggulangan-kebakaran-dan-penyelamatan&data_no=1)

Salinan sumber yang diperiksa berisi 1.153 baris dan 10 kolom. Berkas sumber berekstensi `.xls`, tetapi isinya terbaca sebagai tabel HTML. Kolom `plat_nomor` dihilangkan dari CSV analisis karena berfungsi sebagai pengenal kendaraan. Ringkasan pemeriksaan sumber tersedia pada `data/source_audit.json`, termasuk lima nilai kosong pada kolom tersebut dan tidak ditemukannya baris duplikat identik pada sumber lengkap.

**Lisensi:** Halaman umum Satu Data Jakarta menyatakan bahwa data pada portal dapat dimanfaatkan dan digunakan kembali. Lisensi dataset ini bersifat **Terbuka** dan telah berhasil diverifikasi melalui metadata dataset yang dapat diakses. Atribusi kepada Satu Data Jakarta tetap dicantumkan. Berkas yang disertakan adalah CSV sanitasi, bukan salinan sumber yang memuat `plat_nomor`.

## Struktur folder
```text
50_IF-E_NextLevel_Revisi/
├── README.md
├── requirements.txt
├── data/
│   ├── processed/kendaraan_2025_sanitized.csv
│   └── source_audit.json
├── notebooks/Analisis_Damkar_EDA_RandomForest.ipynb
├── outputs/
└── laporan/Ringkasan_Proyek.md
```

## Menjalankan proyek
Disarankan menggunakan Python 3.10–3.12.

```bash
python -m venv .venv

# macOS/Linux
source .venv/bin/activate

# Windows PowerShell
.venv\Scripts\Activate.ps1

python -m pip install --upgrade pip
pip install -r requirements.txt
jupyter lab
```

Buka `notebooks/Analisis_Damkar_EDA_RandomForest.ipynb`, kemudian jalankan sel secara berurutan dari bagian awal. Notebook dirancang agar dapat dijalankan dari direktori utama proyek maupun dari folder `notebooks`.

## Ringkasan hasil
Pengujian menggunakan pembagian data latih dan data uji dengan rasio 80:20. SVM linear memperoleh accuracy 87,88% dan macro-F1 0,6966. Random Forest memperoleh accuracy 81,39% dan macro-F1 0,6955. Untuk kelas `RUSAK`, recall Random Forest sebesar 0,6471, lebih tinggi dibandingkan SVM linear sebesar 0,3529. Dengan demikian, SVM memiliki accuracy sedikit lebih tinggi, sedangkan Random Forest lebih banyak mengenali catatan yang berlabel `RUSAK`.

## Catatan kualitas data
Setelah kolom `plat_nomor` dikeluarkan, terdapat sejumlah baris yang memiliki nilai sama pada seluruh kolom tersisa. Baris tersebut tetap dipertahankan karena kesamaan atribut tidak membuktikan bahwa catatan berasal dari kendaraan yang sama. Penghapusan tanpa pengenal kendaraan dapat menyebabkan jumlah unit yang tercatat berkurang secara keliru.
