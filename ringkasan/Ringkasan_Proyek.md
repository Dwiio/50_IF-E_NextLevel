# Ringkasan Proyek Data Science

## Identitas kelompok
Kelompok 50 — Next Level, kelas IF-E, Program Studi Informatika, Universitas Pembangunan Nasional “Veteran” Yogyakarta.

**Anggota:** Dwirizki Adithya Putra (123240021), Satya Dio Putranto (123240121), Sebastian Alvaro Huller (123240162), dan Paksi Dwirangga (123240172).

## Judul
Analisis Kondisi Armada Operasional Damkar DKI Jakarta Menggunakan Metode Exploratory Data Analysis (EDA) dan Random Forest.

## Sumber dan karakteristik data
Dataset diperoleh dari Satu Data Jakarta dan berisi catatan kendaraan operasional penanggulangan kebakaran serta penyelamatan untuk periode 2025. Hasil pemeriksaan terhadap salinan sumber menunjukkan 1.153 baris dan 10 kolom. Terdapat lima nilai kosong pada `plat_nomor`, sedangkan baris duplikat identik tidak ditemukan pada sumber lengkap. Kolom `plat_nomor` tidak disertakan dalam berkas analisis karena merupakan pengenal kendaraan.

Setelah kolom tersebut dihilangkan, beberapa catatan memiliki nilai yang sama pada seluruh atribut tersisa. Catatan tersebut dipertahankan karena tidak dapat dipastikan sebagai kendaraan yang sama. Menghapusnya berisiko mengurangi jumlah unit yang sebenarnya tercatat.

**Lisensi:** Halaman umum Satu Data Jakarta menyatakan bahwa data pada portal dapat dimanfaatkan dan digunakan kembali. Lisensi dataset ini bersifat **Terbuka** dan telah berhasil diverifikasi melalui metadata dataset yang dapat diakses. Atribusi sumber tetap dicantumkan.

## Tahapan analisis
Analisis dimulai dengan meninjau struktur, kelengkapan, dan sebaran kategori pada dataset. Selanjutnya dilakukan perapian teks, penyeragaman label kondisi, pemeriksaan nilai kosong dan duplikasi, serta pemilihan fitur yang akan digunakan. Fitur kategorikal diubah menggunakan One-Hot Encoding.

Kolom `Kondisi` menjadi target klasifikasi, sedangkan fitur yang digunakan adalah `wilayah`, `jenis_kendaraan`, `klasifikasi`, dan `fungsi`. Data dibagi menjadi 80% untuk pelatihan dan 20% untuk pengujian dengan stratifikasi kelas serta `random_state=42`. Random Forest digunakan sebagai model utama dan SVM kernel linear sebagai pembanding.

## Hasil evaluasi

| Model | Accuracy | Precision macro | Recall macro | Macro-F1 | Precision RUSAK | Recall RUSAK | F1 RUSAK |
|---|---:|---:|---:|---:|---:|---:|---:|
| SVM Linear | 0.8788 | 0.7817 | 0.6612 | 0.6966 | 0.6667 | 0.3529 | 0.4615 |
| Random Forest | 0.8139 | 0.6738 | 0.7448 | 0.6955 | 0.4151 | 0.6471 | 0.5057 |

Random Forest akhir menggunakan 100 pohon, kedalaman maksimum 10, kriteria Gini, dan `class_weight='balanced'`. Pengaturan bobot kelas digunakan agar kelas `RUSAK` yang jumlahnya lebih sedikit tetap mendapat perhatian dalam proses pembelajaran. SVM linear menggunakan StandardScaler di dalam Pipeline setelah fitur kategorikal diubah menjadi One-Hot.

SVM linear menghasilkan accuracy dan macro-F1 yang sedikit lebih tinggi. Di sisi lain, Random Forest memperoleh recall kelas `RUSAK` yang lebih tinggi, yaitu 0,6471 dibandingkan 0,3529 pada SVM. Hasil ini menunjukkan bahwa kedua model memiliki keunggulan yang berbeda. Jika perhatian utama adalah menemukan lebih banyak catatan berlabel `RUSAK`, hasil recall Random Forest menjadi pertimbangan penting.

## Feature importance
Nilai kepentingan fitur dari Random Forest setelah penggabungan kembali kategori One-Hot adalah sebagai berikut:

| Fitur | Importance |
|---|---:|
| fungsi | 0.3630 |
| wilayah | 0.3077 |
| jenis_kendaraan | 0.2208 |
| klasifikasi | 0.1085 |

Nilai tersebut menunjukkan kontribusi relatif fitur dalam proses pemisahan kelas pada model. Nilai importance tidak membuktikan bahwa suatu fitur menyebabkan kendaraan berada dalam kondisi tertentu.

## Keterbatasan dan tindak lanjut
Dataset hanya mewakili satu periode, sementara informasi usia kendaraan, riwayat perawatan, tanggal inspeksi, jam operasi atau odometer, serta tingkat kerusakan tidak tersedia. Definisi rinci label kondisi juga tidak dijelaskan pada berkas yang dianalisis. Selain itu, pengujian dilakukan melalui satu pembagian data latih dan data uji, sehingga hasilnya masih perlu diperiksa menggunakan data dari periode lain.

Pengembangan berikutnya dapat dilakukan dengan melengkapi riwayat inspeksi dan perawatan, memperbarui data secara berkala, serta menguji model pada periode yang berbeda. Hasil klasifikasi sebaiknya digunakan untuk membantu membaca pola data dan bukan sebagai pengganti pemeriksaan teknis maupun keputusan petugas.
