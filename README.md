# Luca × Gean — E-Commerce WebApps

> **Dashboard-Glass** adalah fondasi awal dari WebApps E-Commerce milik Luca × Gean:
> sebuah *operating & intelligence layer* yang menghubungkan **data → analisis → keputusan → eksperimen → pembelajaran**.

Bukan sekadar dashboard yang menampilkan angka. Tujuannya membantu menjawab: apa yang terjadi, kenapa mungkin terjadi, apa yang harus dilakukan, apakah tindakan itu berhasil, dan apa yang dipelajari untuk keputusan berikutnya.

![status](https://img.shields.io/badge/status-in%20development-8ec5ff) ![type](https://img.shields.io/badge/type-single--file%20HTML-d6a7ff) ![theme](https://img.shields.io/badge/theme-dark%20glass-08090d)

---

## Fitur / Scene

| Scene | Fungsi |
|---|---|
| **Overview** | Command center: periode aktif, order, omzet, profit, temuan penting, eksperimen aktif. |
| **Performa Produk** | Traffic, order, conversion, margin per produk, plus Ringkasan Ads dan Campaign Performance. |
| **Dana Dilepas** | Sudut pandang settlement/cash flow: pendapatan, pengeluaran, adjustment, total dilepas. |
| **Komisi (Finance)** | Komisi/Affiliate sebagai bagian dari economics toko. |
| **Analisis** | Mengubah data menjadi temuan (NORMAL / PERHATIAN / EVALUASI / TINDAKAN), buffer margin, Return Tinggi, diagnosis iklan. |
| **Log** | Memory sistem: apa yang diubah, kapan, kenapa, hasilnya. |
| **Eksperimen** | Baseline → perubahan → hipotesis → periode → hasil → keputusan. |
| **Upload Excel** | Pusat data: unggah laporan Shopee, deteksi jenis, parsing, normalisasi. |

Pengaturan (multi-toko, tema, sinkronisasi) tersedia dari menu Settings.

## Cara Pakai

Tidak perlu build atau dependensi lokal.

1. Buka `Dasboard-Rev-5_Aibo.html` di browser modern (Chrome/Edge/Firefox), atau jalankan server statis:
   ```bash
   python3 -m http.server 8080
   # lalu buka http://127.0.0.1:8080/Dasboard-Rev-5_Aibo.html
   ```
2. Buka scene **Upload Excel** dan unggah laporan Shopee (Produk, Ads, Dana Dilepas, Komisi).
3. Dashboard membaca data dari penyimpanan lokal peramban dan menghitung ulang semua scene.

> Pembaca Excel memakai SheetJS yang dimuat dari CDN (`cdn.sheetjs.com`), jadi butuh koneksi internet saat pertama membuka halaman.

## Data & Penyimpanan

- **Sumber data:** laporan Shopee (file `.xlsx`). Excel hanyalah *input*, bukan produk utama.
- **Alur:** `Upload → Identifikasi → Parsing → Normalisasi → Data Store → Analysis Engine → UI`.
- **Penyimpanan:** `localStorage` peramban (kunci berawalan `luca_gean_*`), terpisah per toko.
- **Periode:** diambil dari nama file (mis. `…20261005_20261011.xlsx` → 05 Okt 2026 — 11 Okt 2026). Perbandingan selalu *periode sekarang vs periode sebelumnya*.
- **Sinkronisasi (opsional):** Google Sheets via Google Apps Script milik pemilik. Skrip server-nya sengaja tidak disertakan di repo ini.

## Prinsip Logika Bisnis

- Mengutamakan **profit efficiency**, tetapi tetap menjaga volume order dan perputaran stok.
- Profit/PCS = dana dilepas/PCS − (HPP + biaya off-system). Klasifikasi margin memakai satu *single source of truth* (`calculateAnalysisMargin`) yang dipakai semua scene.
- ROAS tinggi tidak otomatis berarti iklan menguntungkan. Selalu dibaca bersama HPP, fee, margin, dan profit.
- Analisis membedakan **fakta** dan **hipotesis**. Dashboard adalah *decision support*, keputusan tetap di tangan Luca.

## Arsitektur Singkat

```
DATA → CALCULATION ENGINE → ANALYSIS ENGINE → VIEW MODEL → RENDERER → UI
```

- Satu file HTML berisi style, markup, dan skrip. Scene diturunkan dari elemen `.app-scene[data-scene]`.
- Tema: dark (default), light, dan guardian.
- Prinsip perubahan: **patch kecil > rewrite besar**, backup sebelum perubahan berisiko, prototype terpisah sebelum integrasi.

## Struktur Repo

```
.
├── Dasboard-Rev-5_Aibo.html   # aplikasi utama
└── README.md
```

## Keamanan

- Kode rahasia sinkronisasi dan API key tidak disimpan di repo. Kode rahasia dimasukkan lewat Settings dan hanya tersimpan di peramban.
- Input eksternal (Excel, teks, pencarian) ditampilkan sebagai teks, bukan HTML mentah.

## Status & Arah

Status: **In Development**. Fokus saat ini: stabilisasi arsitektur, data engine, dan scene inti. Visual dan animasi bukan prioritas dibanding **data → logika → analisis → keputusan**.

Jangka panjang: platform modular (Analytics, Finance, Marketing, Product Intelligence, Experimentation, Decision Log, Operations, Automation).

---

*Build the system, not just the screen. Data first. Logic before decoration. Evidence before assumption. History becomes knowledge.*
