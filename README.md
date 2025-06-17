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
  \[
    SS_W = \sum_{i=1}^{k} \sum_{j=1}^{n_i} (x_{ij} - \bar{x}_i)^2
  \]  
- **Jumlah Kuadrat Total (SS<sub>T</sub>)**:  
  \[
    SS_T = SS_B + SS_W = \sum_{i=1}^{k} \sum_{j=1}^{n_i} ( x_{ij} - \bar{x} )^2
  \]  
- **Derajat Kebebasan (df)**:  
  - Antara: _k–1_  
  - Dalam: _N–k_  
  - Total: _N–1_  
- **Rata-Rata Kuadrat (MS)**: SS dibagi dengan df yang sesuai.  
  - MS<sub>B</sub> = SS<sub>B</sub> / (k–1)  
  - MS<sub>W</sub> = SS<sub>W</sub> / (N–k)  
- **Statistik F**:  
  \[
    F = \frac{MS_B}{MS_W}
  \]

---

## Model Statistik & Hipotesis  
Untuk ANOVA satu arah dengan _k_ kelompok dan observasi _x<sub>ij</sub>_:

\[
  x_{ij} = \mu + \tau_i + \varepsilon_{ij}, \quad \varepsilon_{ij}\sim N(0,\sigma^2)
\]

- **Hipotesis Nol (H₀)**: Semua rata-rata kelompok sama:  
  \[
    H_0:\; \mu_1 = \mu_2 = \dots = \mu_k
  \]
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

## Contoh Langkah demi Langkah  
Misalkan tiga pupuk (A, B, C) diuji terhadap hasil panen (kg) pada _n = 5_ plot masing-masing:

| Plot | A   | B   | C   |
|------|-----|-----|-----|
| 1    | 30  | 28  | 35  |
| 2    | 32  | 26  | 33  |
| 3    | 31  | 27  | 36  |
| 4    | 29  | 25  | 34  |
| 5    | 33  | 29  | 37  |

1. **Hitung rata-rata kelompok**:  
   - \(\bar{x}_A = 31\), \(\bar{x}_B = 27\), \(\bar{x}_C = 35\)  
   - Rata-rata total \(\bar{x} = \frac{31+27+35}{3} = 31\)  

2. **Hitung SS<sub>B</sub>**:  
   \[
     SS_B = 5[(31-31)^2 + (27-31)^2 + (35-31)^2]
           = 5[0 + 16 + 16] = 160
   \]

3. **Hitung SS<sub>W</sub>**:  
   \[
     SS_W = \sum_i \sum_j (x_{ij} - \bar{x}_i)^2
           = \ldots = 40  \quad(\text{details omitted for brevity})
   \]

4. **Derajat kebebasan**:  
   - df<sub>B</sub> = 3–1 = 2  
   - df<sub>W</sub> = 15–3 = 12  

5. **Rata-rata kuadrat**:  

