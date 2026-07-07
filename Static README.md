# 🔬 Static Analysis Lab - Portable Executable (PE) Binaries

Repositori ini berfungsi sebagai laboratorium digital untuk mendokumentasikan hasil analisis statis mendalam terhadap minimal 2 file biner berformat *Portable Executable* (PE / file `.exe` Windows). Analisis dilakukan murni menggunakan metode statis tanpa mengeksekusi biner, guna memahami struktur, metadata, fungsi eksternal, dan potensi perilaku program secara aman.

## 🎯 Target Kompetensi Praktikum
* Mampu mengidentifikasi informasi dasar biner seperti ukuran file, tipe arsitektur (PE32/PE64), dan nilai cryptographic hash (MD5/SHA256).
* Memahami tata letak dan struktur internal format Portable Executable (DOS Header `MZ`, PE Header, dan Section Table).
* Mampu mengekstrak dan menganalisis string (*Strings Analysis*) serta fungsi pustaka (*Import Functions/Libraries*) untuk mendeteksi indikasi mencurigakan atau memprediksi perilaku program (*heuristic hypothesis*).

## 📊 Ringkasan Hasil Analisis Laboratorium
Berikut adalah daftar file biner yang telah dianalisis secara statis dalam laboratorium ini:

| No | Nama File Biner | Arsitektur Target | Jenis Kompiler / Packer | Tautan Laporan Lengkap |
|----|-----------------|-------------------|-------------------------|------------------------|
| 1  | **Lab_Sample_01.exe** | PE32 (32-bit x86) | Microsoft Visual C++    | [Baca Laporan Lab 1](lab-report-01.md) |
| 2  | **Lab_Sample_02.exe** | PE32+ (64-bit x64)| GCC / UPX Packed        | [Baca Laporan Lab 2](lab-report-02.md) |

## 🧰 Metodologi & Alat Bantu Analisis
Proses pembedahan struktur biner pada lab ini memanfaatkan beberapa perangkat lunak khusus RE berikut:
* **File Signature & Packer Identifier:** Detect It Easy (DIE) & Pestudio
* **PE Header Structure Viewer:** PEview & PE-bear
* **Hexadecimal Raw Data Viewer:** HxD Hex Editor
* **Strings Extractor:** Utilitas `strings` / Sysinternals Strings

---

## 📚 Referensi & Dokumentasi Teknis
* **Spesifikasi Standar:** [Microsoft Portable Executable and Common Object File Format Specification](https://learn.microsoft.com/en-us/windows/win32/debug/pe-format).
* **Panduan Analisis Statis:** Panduan *Group Discussion Reverse Engineering File Executable* via Waskita Amikom.
* **Buku Acuan:** *Practical Malware Analysis: The Hands-On Guide to Dissecting Malicious Software* (Bab Analisis Statis Dasar).

---

## 🏁 Kata Penutup
Praktikum analisis statis ini memberikan fondasi yang sangat kuat bagi saya dalam memahami anatomi file *executable* Windows. Saya menyadari bahwa sebuah file biner bukanlah kotak hitam yang misterius; dengan alat dan pemahaman struktur PE yang tepat, kita dapat memetakan kapabilitas, dependensi library (seperti `kernel32.dll` atau `user32.dll`), hingga niat dari pembuat aplikasi tersebut bahkan sebelum program itu dijalankan satu detik pun. 

Disiplin analisis statis ini sangat krusial dalam dunia siber, terutama untuk melakukan triase awal terhadap berkas tidak dikenal secara cepat dan aman.

---
> ⚠️ **PENTING - ETIKA DAN LEGALITAS (DISCLAIMER):**
> Semua analisis dilakukan di lingkungan laboratorium yang terisolasi secara ketat (*Virtual Machine*). **TIDAK ADA MALWARE AKTIF ATAU BERBAHAYA** yang diunggah ke dalam repositori publik ini. Semua sampel yang digunakan adalah biner uji coba tiruan (*mock samples*) yang aman dan bertujuan murni untuk memenuhi tugas akademis serta riset edukasi.
