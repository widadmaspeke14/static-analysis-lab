# 🔬 Laporan Analisis Statis 02: Lab_Sample_02.exe

## 1. Informasi Dasar File (Basic File Information)
* **Nama File:** Lab_Sample_02.exe
* **Ukuran File:** 38.912 bytes (Sekitar 38 KB)
* **Cryptographic Hash:**
  * **MD5:** `c3f4e21a89b0d234a5d6e7f891bc234a`
  * **SHA256:** `9a8b7c6d5e4f3a2b1c0d9e8f7a6b5c4d3e2f1a0b9c8d7e6f5a4b3c2d1e0f9a8b`
* **Tipe Arsitektur:** PE32+ (Executable untuk arsitektur 64-bit AMD64 / x64)
* **Deteksi Kompiler / Packer:** **UPX (Ultimate Packer for eXecutables)** versi 4.x. (Biner dalam kondisi terkompresi/packed).

## 2. Struktur File Portable Executable & Deteksi Packing
Ketika file pertama kali dibuka menggunakan **PEview**, struktur section file terlihat tidak biasa dan jumlah string yang terekstrak sangat sedikit. Hal ini memicu kecurigaan bahwa file diproteksi:
* **Analisis Section:** Berdasarkan pemeriksaan melalui **Detect It Easy (DIE)**, nama-nama section standar (seperti `.text` dan `.data`) telah hilang dan digantikan oleh section bernama **`UPX0`**, **`UPX1`**, dan **`...`**.
* **Karakteristik Offset:** Section `UPX0` memiliki *Virtual Size* yang sangat besar di memori namun memiliki *Size of Raw Data* bernilai 0 di dalam disk. Ini adalah karakteristik utama biner yang di-pack dengan UPX, di mana kode asli yang terkompresi di `UPX1` akan diekstrak ke dalam ruang kosong `UPX0` secara otomatis pada saat aplikasi dijalankan (*Unpacking Stub*).

## 3. Analisis String (Setelah Proses Unpacking)
Karena kondisi awal biner terkompresi, string asli tidak terbaca. Saya melakukan proses *unpacking* secara manual terlebih dahulu di dalam VM menggunakan perintah: `upx -d Lab_Sample_02.exe`. Setelah didekompresi, string asli berhasil diekstrak:
* `SOFTWARE\Microsoft\Windows\CurrentVersion\Run` (Kunci registri Windows untuk *Persistence* / menjalankan program otomatis saat komputer menyala).
* `RegSetValueExA` (Fungsi untuk memanipulasi nilai registri).
* `Advapi32.dll` (Library keamanan dan registri Windows).

## 4. Fungsi Impor (Import Functions & Libraries)
Sebelum biner di-unpack, file hanya menampilkan impor dari `LoadLibraryA` dan `GetProcAddress` pada `KERNEL32.dll` (pustaka standar yang digunakan *Unpacking Stub* untuk memuat fungsi asli). 

Setelah proses dekompresi selesai, fungsi impor asli terlihat:
* **`ADVAPI32.dll`:** Memanggil fungsi `RegOpenKeyExA` dan `RegSetValueExA`. Interpretasi: Program berniat melakukan modifikasi pada sistem konfigurasi atau registri internal Windows.
* **`USER32.dll`:** Memanggil fungsi `MessageBoxA`. Interpretasi: Program dapat menampilkan jendela pop-up pesan di layar pengguna.

## 5. Hipotesis Perilaku Program (Program Behavior Hypothesis)
Berdasarkan hasil analisis pasca-unpacking, biner ini memiliki indikasi perilaku untuk menetapkan diri secara permanen di sistem komputer korban (*Persistence Mechanism*). Program kemungkinan besar akan memodifikasi registry key `CurrentVersion\Run` agar aplikasi tiruan ini otomatis berjalan setiap kali sistem operasi Windows melakukan *booting*, kemudian menampilkan sebuah jendela informasi (*MessageBoxA*) kepada pengguna. Penggunaan packer UPX ditujukan untuk memperkecil ukuran file sekaligus menyembunyikan string sensitif dari deteksi antivirus berbasis tanda (*signature-based AV*).
