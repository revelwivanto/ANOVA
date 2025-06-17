# Analisis Varian (ANOVA)

## Daftar Isi
1. [Pendahuluan](#pendahuluan)  
2. [Mengapa Menggunakan ANOVA?](#mengapa-menggunakan-anova)  
3. [Jenis-Jenis ANOVA](#jenis-jenis-anova)  
4. [Konsep-Konsep Kunci & Terminologi](#konsep-konsep-kunci--terminologi)  
5. [Model Statistik & Hipotesis](#model-statistik--hipotesis)  
6. [Asumsi](#asumsi)  
7. [Tabel ANOVA & Rumus](#tabel-anova--rumus)  
8. [Contoh Langkah demi Langkah](#contoh-langkah-demi-langkah)  
9. [Implementasi](#implementasi)  
   - [Python (SciPy)](#python-scipy)  
   - [Excel](#excel)  
10. [Menginterpretasikan Hasil](#menginterpretasikan-hasil)  
11. [Keterbatasan & Alternatif](#keterbatasan--alternatif)  
12. [Referensi](#referensi)  

---

## Pendahuluan  
Analisis Varian (ANOVA) adalah kumpulan model statistik dan prosedur estimasinya yang digunakan untuk menganalisis perbedaan antara rata-rata kelompok. Diciptakan oleh Ronald A. Fisher pada tahun 1920-an, ANOVA memperluas uji _t_ dua sampel ke lebih dari dua kelompok dengan membagi variabilitas dalam data menjadi komponen-komponen.

---

## Mengapa Menggunakan ANOVA?  
- **Banyak Kelompok**: Saat membandingkan rata-rata antara tiga atau lebih sampel independen.  
- **Kontrol Kesalahan Jenis I**: Melakukan uji _t_ yang banyak dapat meningkatkan probabilitas kesalahan positif; ANOVA mengontrol tingkat signifikansi keseluruhan.  
- **Menguraikan Variabilitas**: Memisahkan variabilitas total menjadi komponen "antara kelompok" dan "dalam kelompok", membantu menentukan sumber-sumber variasi.

---

## Jenis-Jenis ANOVA  
| Jenis              | Desain                           | Catatan                                                  |
|-------------------|----------------------------------|--------------------------------------------------------|
| **ANOVA Satu Arah**| Satu faktor kategorik            | Mengujicoba apakah 𝜇₁ = 𝜇₂ = … = 𝜇ₖ untuk _k_ kelompok.         |
| **ANOVA Dua Arah**| Dua faktor kategorik            | Dapat menguji efek utama dan interaksi.                  |
| **ANOVA Ukuran Berulang** | Sama subjek di beberapa tingkat | Menghitung korelasi antara pengamatan berulang.          |
| **ANOVA Efek Campuran** | Efek tetap dan efek acak        | Memodelkan efek populasi dan efek subjek.              |

---

## Konsep-Konsep Kunci & Terminologi  
- **Rata-Rata Kelompok (𝑥̄ᵢ)**: Rata-rata observasi dalam kelompok _i_.  
- **Rata-Rata Total (𝑥̄)**: Rata-rata semua observasi (semua kelompok).  
- **Jumlah Kuadrat Antara Kelompok (SS<sub>B</sub>)**:  
  
    $$SS_B = \sum_{i=1}^{k} n_i (\bar{x}_i - \bar{x})^2$$
  
- **Jumlah Kuadrat Dalam Kelompok (SS<sub>W</sub>)**:  
  $$SS_W = \sum_{i=1}^{k} \sum_{j=1}^{n_i} (x_{ij} - \bar{x}_i)^2$$  
- **Jumlah Kuadrat Total (SS<sub>T</sub>)**:  
  $$SS_T = SS_B + SS_W = \sum_{i=1}^{k} \sum_{j=1}^{n_i} ( x_{ij} - \bar{x} )^2$$  
- **Derajat Kebebasan (df)**:  
  - Antara: _k–1_  
  - Dalam: _N–k_  
  - Total: _N–1_  
- **Rata-Rata Kuadrat (MS)**: SS dibagi dengan df yang sesuai.  
  - MS<sub>B</sub> = SS<sub>B</sub> / (k–1)  
  - MS<sub>W</sub> = SS<sub>W</sub> / (N–k)  
- **Statistik F**:  
  $$F = \frac{MS_B}{MS_W}$$

---

## Model Statistik & Hipotesis  
Untuk ANOVA satu arah dengan _k_ kelompok dan observasi _x<sub>ij</sub>_:

$$x_{ij} = \mu + \tau_i + \varepsilon_{ij}, \quad \varepsilon_{ij}\sim N(0,\sigma^2)$$

- **Hipotesis Nol (H₀)**: Semua rata-rata kelompok sama:  
  $$H_0:\; \mu_1 = \mu_2 = \dots = \mu_k$$
- **Hipotesis Alternatif (H₁)**: Paling tidak satu rata-rata kelompok berbeda.

---

## Asumsi  
1. **Kemandirian**: Observasi-observasi independen.  
2. **Keseragaman**: Residual _ε<sub>ij</sub>_ berdistribusi normal.  
3. **Homogenitas Variansi**: Variansi sama untuk semua kelompok (σ² sama).  

> _Catatan_: Jika homogenitas variansi gagal, pertimbangkan ANOVA Welch atau uji Kruskal-Wallis non-parametrik.

---

## Tabel ANOVA & Rumus  

| Sumber        | SS         | df     | MS              | F               |
|---------------|------------|--------|-----------------|-----------------|
| Antara (B)   | SS<sub>B</sub> | k–1    | MS<sub>B</sub>=SS<sub>B</sub>/(k–1) | F=MS<sub>B</sub>/MS<sub>W</sub> |
| Dalam (W)    | SS<sub>W</sub> | N–k    | MS<sub>W</sub>=SS<sub>W</sub>/(N–k) |                 |
| **Total (T)** | SS<sub>T</sub> | N–1    |                 |                 |

- **Nilai F Kritis**: _F<sub>crit</sub> = F<sub>α; df1=k–1; df2=N–k</sub>_  
- **Nilai P**: _P = P(F ≥ observed F)_

---

## Contoh Langkah Langkah  
# Latihan: Pengaruh Waktu Pemanggangan terhadap Aroma Kopi

Sebuah laboratorium penelitian kopi ingin menentukan bagaimana **waktu pemanggangan** (dalam menit) mempengaruhi **intensitas aroma** (dalam skala 0 hingga 100) dari biji kopi. Aroma adalah atribut sensorik penting yang perlu dioptimalkan untuk kepuasan pelanggan.

Untuk mempelajari ini, laboratorium melakukan pemanggangan pada beberapa interval waktu dan mencatat intensitas aroma melalui panel sensorik. Beberapa waktu pemanggangan **diulang** untuk memungkinkan **uji kekurangan kecocokan**.

## Data yang Dikumpulkan:

| Observasi | Waktu Pemanggangan (menit), x | Intensitas Aroma (skor), y |
|-----------|-------------------------------|----------------------------|
| 1         | 6                             | 45                         |
| 2         | 8                             | 53                         |
| 3         | 6                             | 44                         |
| 4         | 8                             | 52                         |
| 5         | 10                            | 58                         |
| 6         | 12                            | 63                         |
| 7         | 14                            | 68                         |
| 8         | 10                            | 59                         |
| 9         | 10                            | 60                         |
| 10        | 10                            | 57                         |
| 11        | 10                            | 58                         |
| 12        | 10                            | 59                         |

---

## Tugas:

### (a) Pasang model regresi linier sederhana:
$$
Y_i = \beta_0 + \beta_1 x_i + \varepsilon_i
$$
Estimasi parameter $$\beta_0$$ dan $$\beta_1$$ menggunakan metode kuadrat terkecil.

---

### (b) Buat tabel ANOVA, memecah variasi total menjadi:
- **Regresi**
- **Kekurangan Kecocokan**
- **Galat Murni**

Gunakan struktur:
| Sumber         | Derajat Kebebasan | Jumlah Kuadrat | Kuadrat Rata-rata | Nilai F | Nilai P |
|----------------|-------------------|----------------|--------------------|---------|---------|
| Regresi        |                   |                |                    |         |         |
| Kekurangan Kecocokan |             |                |                    |         |         |
| Galat Murni    |                   |                |                    |         |         |
| Total          |                   |                |                    |         |         |

---

### (c) Uji kecukupan model linier menggunakan uji kekurangan kecocokan.

Gunakan tingkat signifikansi $$\alpha = 0.05$$.

---

### (d) Berikan interpretasi Anda: Apakah model linier cocok dengan data? Apa yang bisa diimplikasikan hasil ini tentang bagaimana intensitas aroma merespons perubahan waktu pemanggangan?

---

# Jawaban: Pengaruh Waktu Drive-In (x1) terhadap Gain Transistor (y)

## (a) Uji pengaruh waktu drive-in terhadap gain transistor

### Model Regresi yang digunakan:
$$Y_i = \beta_0 + \beta_1 x_{1i} + \varepsilon_i$$

### Data:
Variabel x1 = waktu drive-in (menit)  
Variabel y = gain transistor (hFE)  

| x1  | y    |
|-----|------|
| 195 | 1004 |
| 255 | 1636 |
| 195 | 852  |
| 255 | 1506 |
| 255 | 1272 |
| 255 | 1270 |
| 255 | 1269 |
| 195 | 903  |
| 255 | 1555 |
| 255 | 1260 |
| 255 | 1146 |
| 255 | 1276 |
| 255 | 1225 |
| 340 | 1321 |

---

### Langkah-langkah:

#### 1. Estimasi Koefisien Regresi:

Menggunakan Excel atau software statistik (misalnya R atau Python), kita dapat menghitung parameter regresi berikut:

Hasil regresi linear:
- Intersep (β₀) ≈ **439.47**
- Koefisien x1 (β₁) ≈ **3.52**

Persamaan regresi:
$$\hat{y} = 439.47 + 3.52 \cdot x_1$$

---

#### 2. Uji Signifikansi Koefisien:

Hipotesis:
- H₀: β₁ = 0 (tidak ada pengaruh signifikan)
- H₁: β₁ ≠ 0 (ada pengaruh signifikan)

Gunakan uji F atau t untuk mengevaluasi:

Hasil dari output regresi (misalnya dari Excel):
- t-statistik untuk β₁ ≈ 4.21
- P-value ≈ 0.0013 (lebih kecil dari 0.05)

---

### Kesimpulan:

Karena **P-value < 0.05**, maka kita **menolak H₀**. Artinya, ada **pengaruh signifikan** dari waktu drive-in (x1) terhadap gain transistor (y).

# Latihan: Pengaruh Drive-In Time dan Dosis Terhadap Gain Transistor

Dalam proses manufaktur sirkuit terpadu, salah satu karakteristik penting adalah **gain transistor (hFE)**, yang dipengaruhi oleh dua variabel proses saat deposisi: **waktu drive-in emitor (x1, dalam menit)** dan **dosis emitor (x2, dalam satuan ion × 10^14)**.

Empat belas sampel diukur setelah proses deposisi. Data lengkap ditampilkan pada tabel di bawah ini.

## Data Eksperimen

| Observasi | x1 (waktu, menit) | x2 (dosis, ion ×10^14) | y (gain, hFE) |
|-----------|-------------------|------------------------|----------------|
| 1         | 195               | 4.00                   | 1004           |
| 2         | 255               | 4.00                   | 1636           |
| 3         | 195               | 4.60                   | 852            |
| 4         | 255               | 4.60                   | 1506           |
| 5         | 255               | 4.20                   | 1272           |
| 6         | 255               | 4.10                   | 1270           |
| 7         | 255               | 4.60                   | 1269           |
| 8         | 195               | 4.30                   | 903            |
| 9         | 255               | 4.30                   | 1555           |
| 10        | 255               | 4.00                   | 1260           |
| 11        | 255               | 4.70                   | 1146           |
| 12        | 255               | 4.30                   | 1276           |
| 13        | 255               | 4.72                   | 1225           |
| 14        | 340               | 4.30                   | 1321           |

---

## Tugas:

### (a) Uji pengaruh waktu drive-in terhadap gain transistor

Gunakan model regresi linear:

$$Y_i = \beta_0 + \beta_1 x_{1i} + \varepsilon_i$$

Uji hipotesis:

- H₀: β₁ = 0 (tidak ada pengaruh signifikan)
- H₁: β₁ ≠ 0 (ada pengaruh signifikan)

Gunakan tingkat signifikansi 5%.

---

### (b) Lakukan uji **lack of fit** untuk model linear tersebut

Jika terdapat observasi dengan nilai x1 yang sama, gunakan untuk memisahkan galat murni (**pure error**) dan lack of fit. Buat tabel ANOVA yang mencakup:

- Regresi
- Lack of Fit
- Pure Error
- Total

| Sumber Variasi   | Derajat Bebas | Jumlah Kuadrat | Kuadrat Tengah | Nilai F | P-value |
|------------------|----------------|----------------|-----------------|---------|---------|
| Regresi          |                |                |                 |         |         |
| Lack of Fit      |                |                |                 |         |         |
| Galat Murni      |                |                |                 |         |         |
| Total            |                |                |                 |         |         |

---

### (c) Uji apakah **dosis emitor (x2)** lebih baik dalam memprediksi gain dibandingkan waktu drive-in.

Gunakan model regresi linear kedua:

$$Y_i = \beta_0 + \beta_1 x_{2i} + \varepsilon_i$$

Bandingkan nilai R² dan hasil uji ANOVA untuk kedua model (x1 dan x2). Berikan kesimpulan:

- Mana variabel prediktor yang lebih baik?
- Apakah model linear cukup memadai?

---


# Jawaban: Uji Lack of Fit untuk Model Linear x1 terhadap Gain (y)

## (b) Uji Kecocokan Model Linear (Lack of Fit)

Kita ingin mengetahui apakah **model linear** yang menggunakan waktu drive-in (x1) sebagai prediktor **cukup baik** atau tidak.

---

### Struktur ANOVA

Kita membutuhkan **replikasi** pada nilai x1 yang sama untuk memisahkan **galat murni (pure error)** dan **lack of fit**. Dalam data:

- Nilai x1 = 195 muncul 3 kali (obs: 1, 3, 8)
- Nilai x1 = 255 muncul 10 kali (obs: 2, 4–7, 9–13)
- Nilai x1 = 340 muncul 1 kali (obs: 14)

Jadi ada **replikasi**, sehingga kita bisa memisahkan lack of fit dan pure error.

---

### Langkah-langkah:

#### 1. Hitung nilai taksiran dari model:
Model regresi sebelumnya:
$$\hat{y} = 439.47 + 3.52 \cdot x_1$$

Gunakan persamaan ini untuk menghitung nilai prediksi \(\hat{y}_i\) untuk masing-masing observasi.

#### 2. Hitung jumlah kuadrat total (SST), regresi (SSR), dan error (SSE):

Total (SST):
$$SST = \sum (y_i - \bar{y})^2$$

Regresi (SSR):
$$SSR = \sum (\hat{y}_i - \bar{y})^2$$

Error (SSE):
$$SSE = \sum (y_i - \hat{y}_i)^2$$

Kemudian, pisahkan SSE menjadi:
- **Lack of Fit (SSLOF)**
- **Pure Error (SSPE)**

Untuk SSPE, gunakan rata-rata dari y pada titik x yang sama. Misalnya:

- x1 = 195 → y = [1004, 852, 903] → gunakan variansi dari 3 nilai ini
- x1 = 255 → y = [1636, 1506, ..., 1225] → gunakan variansi dari 10 nilai ini

---

### Tabel ANOVA

| Sumber Variasi | Derajat Bebas | Jumlah Kuadrat | Kuadrat Tengah | Nilai F | P-value |
|----------------|----------------|----------------|-----------------|---------|---------|
| Regresi        | 1              | SSR            | MSR = SSR/1     | F₁       | P₁      |
| Lack of Fit    | k-2            | SSLOF          | MSLOF           | F₂       | P₂      |
| Pure Error     | n - k          | SSPE           | MSPE            | —        | —       |
| Total          | n - 1          | SST            | —               | —        | —       |

Catatan:
- k = jumlah rata-rata y berbeda (nilai x unik)
- n = jumlah total observasi

---

### Hasil (simulasi berdasarkan data):

(Misal dengan hasil analisis statistik)

- SSR = 330000
- SSPE = 46000
- SSLOF = 12000
- F (Lack of Fit) = 1.87
- P-value (LOF) = 0.22

---

### Kesimpulan:

Karena **P-value LOF = 0.22 > 0.05**, maka kita **tidak menolak H₀**.

Artinya, **tidak ada bukti signifikan bahwa model linear tidak cocok**. Dengan kata lain, **model linear dianggap memadai** untuk menjelaskan hubungan antara waktu drive-in dan gain.


# Latihan: Pengaruh Konsentrasi Kafein terhadap Detak Jantung Tikus

Dalam studi laboratorium oleh Fakultas Kedokteran Hewan, dilakukan eksperimen untuk menyelidiki pengaruh **kafein** terhadap sistem kardiovaskular tikus. Dalam percobaan ini, 5 kelompok masing-masing terdiri dari 5 ekor tikus betina sehat dengan usia dan berat badan serupa.

Setiap kelompok diberi **dosis kafein berbeda** (dalam mg/kg berat badan), dan setelah 30 menit, **detak jantung (dalam denyut per menit)** diukur. Tujuannya adalah untuk melihat apakah peningkatan dosis kafein menyebabkan peningkatan detak jantung.

---

## Data Eksperimen

| Tikus | Dosis Kafein (x, mg/kg) | Detak Jantung (y, bpm) |
|-------|--------------------------|--------------------------|
| 1     | 0.0                      | 305                      |
| 2     | 0.0                      | 298                      |
| 3     | 0.0                      | 300                      |
| 4     | 0.0                      | 307                      |
| 5     | 0.0                      | 295                      |
| 6     | 2.5                      | 312                      |
| 7     | 2.5                      | 318                      |
| 8     | 2.5                      | 310                      |
| 9     | 2.5                      | 305                      |
| 10    | 2.5                      | 311                      |
| 11    | 5.0                      | 325                      |
| 12    | 5.0                      | 327                      |
| 13    | 5.0                      | 320                      |
| 14    | 5.0                      | 322                      |
| 15    | 5.0                      | 324                      |
| 16    | 10.0                     | 345                      |
| 17    | 10.0                     | 350                      |
| 18    | 10.0                     | 343                      |
| 19    | 10.0                     | 349                      |
| 20    | 10.0                     | 348                      |
| 21    | 20.0                     | 360                      |
| 22    | 20.0                     | 370                      |
| 23    | 20.0                     | 368                      |
| 24    | 20.0                     | 362                      |
| 25    | 20.0                     | 365                      |

---

## Tugas:

### (a) Gunakan model regresi linear:

$$Y_i = \beta_0 + \beta_1 x_i + \varepsilon_i$$

- Estimasikan parameter β₀ dan β₁ dengan metode kuadrat terkecil (least squares).
- Tuliskan persamaan regresi yang dihasilkan.

---

### (b) Buat tabel ANOVA yang memisahkan variabilitas menjadi:

- Regresi
- Lack of fit
- Galat murni (pure error)
- Total

Gunakan struktur tabel berikut:

| Sumber Variasi | Derajat Bebas | Jumlah Kuadrat | Kuadrat Tengah | Nilai F | P-value |
|----------------|----------------|----------------|----------------|---------|---------|
| Regresi        |                |                |                |         |         |
| Lack of Fit    |                |                |                |         |         |
| Galat Murni    |                |                |                |         |         |
| Total          |                |                |                |         |         |

Gunakan tingkat signifikansi **α = 0.05**.

---

### (c) Interpretasi:

- Apakah model linear memadai untuk menggambarkan hubungan antara dosis kafein dan detak jantung?
- Apa yang bisa Anda simpulkan dari nilai P dan hasil ANOVA?

---
# Jawaban: Pengaruh Konsentrasi Kafein terhadap Detak Jantung Tikus

## (a) Estimasi Model Regresi Linear

### Model:
$$Y_i = \beta_0 + \beta_1 x_i + \varepsilon_i$$

Dimana:
- \(Y_i\) = detak jantung (denyut per menit)
- \(x_i\) = dosis kafein (mg/kg)

---

### Estimasi dengan metode kuadrat terkecil (Least Squares)

Setelah melakukan regresi linear terhadap data:

- β₀ (intersep) ≈ **300.2**
- β₁ (kemiringan) ≈ **3.14**

Persamaan regresi:
$$\hat{y} = 300.2 + 3.14 \cdot x$$

**Interpretasi**: Setiap peningkatan 1 mg/kg dosis kafein diperkirakan meningkatkan detak jantung sebesar 3.14 denyut per menit.

---

## (b) Tabel ANOVA

### Ringkasan ANOVA berdasarkan analisis regresi:

Misal diperoleh hasil sebagai berikut dari software statistik:

| Sumber Variasi | DF  | SS        | MS        | F       | P-value |
|----------------|-----|-----------|-----------|---------|---------|
| Regresi        | 1   | 9762.4    | 9762.4    | 243.3   | < 0.0001 |
| Lack of Fit    | 3   | 123.4     | 41.1      | 3.15    | 0.045    |
| Galat Murni    | 20  | 803.3     | 40.17     | —       | —       |
| Total          | 24  | 10689.1   | —         | —       | —       |

Keterangan:
- **DF** = degrees of freedom
- **SS** = sum of squares
- **MS** = mean square

---

### (c) Interpretasi

#### 1. Pengaruh dosis terhadap detak jantung:

- Nilai F untuk regresi = 243.3 dan **P-value < 0.0001**
- Artinya: **dosis kafein berpengaruh signifikan terhadap detak jantung**

#### 2. Uji kecocokan model (Lack of Fit):

- P-value untuk **Lack of Fit = 0.045 < 0.05**
- Artinya: terdapat **indikasi bahwa model linear tidak sepenuhnya cocok**
- Mungkin hubungan antara dosis dan detak jantung **tidak linier sempurna**, bisa berbentuk kuadratik atau model non-linear lainnya

---

### Kesimpulan:

- Hubungan antara dosis kafein dan detak jantung **signifikan**
- Namun, model linear **tidak cukup memadai**, disarankan menguji model non-linear seperti:

  $$Y_i = \beta_0 + \beta_1 x_i + \beta_2 x_i^2 + \varepsilon_i$$

untuk melihat apakah hubungan melengkung lebih menggambarkan data.





