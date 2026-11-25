# Building Acoustics — Python / Google Colab Case Study

Repository ini mendukung **mini-project 1 minggu** Akustika Bangunan untuk mahasiswa Teknik Fisika. Fokusnya bukan sekadar menjalankan kode, tetapi menggunakan Python sebagai *virtual laboratory* untuk menghubungkan:

**mekanisme fisika -> model -> simulasi -> evidence -> keputusan engineering**.

## Kasus
Dua ruang kelas identik berukuran `8 x 6 x 3 m` dipisahkan partisi `12 m2`:

- dinding utama: `10 m2`, `R = 45 dB`;
- pintu: `2 m2`, `R = 20 dB`;
- sumber: `Lw = 95 dB`, `Q = 2`;
- receiver di ruang sumber: `r = 2 m`;
- target proyek: `Req >= 40 dB`, `T60 < 0.8 s`.

Angka kasus adalah **data didaktik**, bukan hasil pengukuran produk nyata.

## Struktur repository

```text
building-acoustics-colab-project/
├── notebooks/
│   └── building_acoustics_case_study.ipynb
├── src/building_acoustics/
│   ├── room.py
│   ├── partition.py
│   └── levels.py
├── data/
│   ├── case_500hz.csv
│   └── octave_absorption_example.csv
├── tests/
│   └── test_models.py
├── docs/
│   └── assignment-brief.md
├── .github/workflows/tests.yml
├── requirements.txt
├── pyproject.toml
└── README.md
```

## Jalankan di Google Colab

Buka notebook `notebooks/building_acoustics_case_study.ipynb`. Notebook sengaja memuat kode fungsi inti secara mandiri sehingga dapat dijalankan langsung di Colab tanpa instalasi package lokal.

Untuk penggunaan repository penuh di Colab:

```python
!git clone https://github.com/USERNAME/building-acoustics-colab-project.git
%cd building-acoustics-colab-project
!pip install -e .
```

Ganti `USERNAME` setelah repository dipublikasikan ke akun GitHub Anda.

## Model fisika

### Equivalent absorption area

```text
A = sum(S_i * alpha_i)
```

### Sabine

```text
T60 = (24 ln 10 / c) * V / A
```

untuk `c = 343 m/s`, koefisiennya sekitar `0.16111`.

### Eyring

```text
T_E = (24 ln 10 / c) * V / [-S ln(1-alpha_bar)]
```

### Composite partition

```text
tau_eq = sum(S_i * 10^(-R_i/10)) / sum(S_i)
Req = -10 log10(tau_eq)
```

**Penting:** indeks dB tidak boleh dirata-ratakan secara aritmetik.

## Hasil baseline yang dipakai sebagai benchmark

- `V = 144 m3`
- `S = 180 m2`
- `A = 16.32 m2`
- `T60 (Sabine) ≈ 1.42 s`
- `Req ≈ 27.71 dB`

Dinding yang dinaikkan dari `45 -> 55 dB` sementara pintu tetap `20 dB` hanya meningkatkan `Req` menjadi sekitar `27.77 dB`. Ini menunjukkan **weak-element dominance**.

## Tugas sulit mahasiswa

Notebook meminta mahasiswa untuk:

1. mereproduksi benchmark;
2. membandingkan alternatif A–D;
3. melakukan sweep rating pintu;
4. melakukan sweep absorpsi plafon;
5. menambahkan jalur flanking;
6. melakukan analisis cost-performance;
7. menulis keputusan engineering dan keterbatasan model.

## Pengujian lokal

```bash
python -m pip install -e . pytest
pytest -q
```

## Batas model

Model di repository ini adalah model pembelajaran satu-pita / statistik. Untuk bangunan nyata diperlukan data per pita, sambungan, jalur flanking, kondisi pemasangan, dan verifikasi pengukuran lapangan.
