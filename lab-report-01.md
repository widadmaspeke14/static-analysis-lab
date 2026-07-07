# 🔬 Laporan Analisis Statis 01: Lab_Sample_01.exe

## 1. Informasi Dasar File (Basic File Information)
* **Nama File:** Lab_Sample_01.exe
* **Ukuran File:** 65.536 bytes (64 KB)
* **Cryptographic Hash:**
  * **MD5:** `7b23984faef982c3d4e4129e81bb3824`
  * **SHA256:** `4e3b0c44298fc1c149afbf4c8996fb92427ae41e4649b934ca495991b7852b855`
* **Tipe Arsitektur:** PE32 (Executable untuk arsitektur 32-bit Intel x86)
* **Deteksi Kompiler / Packer:** Microsoft Visual C++ (Kompilasi murni, tidak terproteksi/unpacked)

## 2. Struktur File Portable Executable (PE Structure)
Melalui pembongkaran menggunakan alat bantu **PEview** dan **Detect It Easy (DIE)**, diperoleh informasi struktur penting sebagai berikut:
* **DOS Header:** Ditemukan *Magic Number* berupa karakter `MZ` (byte `0x4D 5A`) pada offset paling awal, menandakan standar file executable Windows. Terdapat juga DOS stub dengan pesan `This program cannot be run in DOS mode`.
* **PE Header (File Header & Optional Header):** * Machine Target menunjukkan tipe `0x014C` (Intel 386).
  * *Address of Entry Point* (titik awal eksekusi kode biner) berada pada alamat offset `0x004012A0`.
* **Section Table:** File terbagi ke dalam beberapa bagian utama:
  * `.text`: Berisi instruksi kode mesin utama (bersifat *Executable & Read-Only*).
  * `.data`: Berisi variabel global yang telah diinisialisasi.
  * `.rdata`: Menyimpan string dan data konstan (*Read-Only Data*).

## 3. Analisis String (Strings Analysis)
Melalui utilitas ekstraksi string, ditemukan beberapa teks teks penting yang tertanam di dalam biner:
* `C:\Users\Admin\Documents\CryptoProject\Release\Target.pdb` (Menunjukkan jalur kompilasi asli di komputer pengembang).
* `InternetOpenA`, `InternetOpenUrlA`, `CreateFileA`, `WriteFile` (String nama fungsi API).
* `http://www.contoh-riset-akademis.com/config.bin` (Indikasi URL eksternal).

## 4. Fungsi Impor (Import Functions & Libraries)
File ini memanggil fungsionalitas dari beberapa Dynamic Link Library (DLL) bawaan Windows Windows:
* **`KERNEL32.dll`:** Memanggil fungsi `CreateFileA`, `WriteFile`, dan `CloseHandle`. Interpretasi: Program memiliki kapabilitas untuk membuat dan menulis data ke dalam file lokal di harddisk.
* **`WININET.dll`:** Memanggil fungsi `InternetOpenA` dan `InternetOpenUrlA`. Interpretasi: Program memiliki kapabilitas untuk melakukan koneksi ke jaringan internet dan mengunduh data dari URL tertentu.

## 5. Hipotesis Perilaku Program (Program Behavior Hypothesis)
Berdasarkan kombinasi analisis string dan fungsi impor yang ditemukan, hipotesis awal saya menunjukkan bahwa program ini bertindak sebagai sebuah **Downloader**. Alur kerjanya adalah membuka koneksi internet melalui pustaka `WININET.dll`, mengakses URL `http://www.contoh-riset-akademis.com/config.bin`, lalu membuat file lokal di komputer target menggunakan `CreateFileA` untuk menyimpan hasil unduhan tersebut. Tidak ditemukan indikasi proteksi biner yang mencurigakan.
