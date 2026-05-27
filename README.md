# 📊 Panduan Integrasi Google Sheets & Deployment GitHub - NUSANTARA POS

Sistem Kasir (POS) **NUSANTARA POS** ini memiliki kemampuan sinkronisasi data cloud multi-cabang (Lahat & Pagar Alam) menggunakan **Google Sheets** sebagai database gratis tanpa biaya server (*Zero Cost Serverless Backend*). 

Berikut adalah langkah-langkah lengkap untuk mempersiapkan Google Sheets dan merilis Web POS ini agar bisa diakses oleh kasir Anda secara online.

---

## 🛠️ Langkah 1: Persiapan Google Sheets & Apps Script

Aplikasi ini menggunakan Google Apps Script untuk menghubungkan antarmuka kasir ke Google Sheets.

1. **Buat Google Spreadsheet Baru**:
   - Buka [Google Sheets](https://sheets.google.com/) dan buat dokumen baru.
   - Beri nama spreadsheet Anda, misalnya: `Database Nusantara POS`.

2. **Buka Editor Apps Script**:
   - Pada Google Sheets Anda, klik menu **Ekstensi** > **Apps Script**.
   - Hapus semua baris kode bawaan (*default*) yang ada di dalam editor `Kode.gs`.

3. **Salin dan Tempel Kode Apps Script**:
   - Buka file `/apps-script/Code.gs` pada repositori proyek ini.
   - Salin seluruh kode yang ada dalam file tersebut dan tempelkan (*paste*) ke dalam editor Google Apps Script. 

4. **Inisialisasi Database (Membuat Sheet Otomatis)**:
   - Pilih fungsi `initializePOSDatabase` dari menu drop-down fungsi di atas layar editor.
   - Klik tombol ▶️ **Jalankan** (*Run*).
   - Saat pop-up perizinan muncul, klik **Tinjau Izin** (*Review Permissions*), pilih akun Google Anda, klik **Lanjutan** (*Advanced*) > **Buka POS Kasir Backend (tidak aman)**, lalu klik **Izinkan** (*Allow*).
   - Setelah selesai berjalan, Google Sheets Anda secara otomatis akan terisi dengan dua buah lembar kerja (*sheet*): 
     * **`Products`** (Berisi parameter kolom: *id, sku, name, price, category, stock, barcode* lengkap beserta contoh isinya).
     * **`Transactions`** (Berisi parameter kolom: *id, timestamp, items, totalPrice, cashPaid, changeGiven, paymentMethod, cashierName*).

---

## 🚀 Langkah 2: Deploy Google Apps Script sebagai Web App

Setelah database siap, kita perlu mempublikasikan script tersebut sebagai Web App agar dapat diakses oleh aplikasi React Anda.

1. Di pojok kanan atas halaman Apps Script, klik tombol **Terapkan** (*Deploy*) > **Penerapan baru** (*New deployment*).
2. Klik ikon ⚙️ (*gigi*) di samping "Pilih tipe" dan pilih **Aplikasi Web** (*Web App*).
3. Isi konfigurasi sebagai berikut:
   - **Deskripsi**: `Nusantara POS API v1`
   - **Jalankan sebagai** (*Execute as*): **Saya (email_anda@gmail.com)**
   - **Siapa yang memiliki akses** (*Who has access*): **Siapa saja** (*Anyone*) *(Langkah ini aman karena endpoint hanya melayani pengiriman data transaksi dan lookup produk saja).*
4. Klik **Terapkan** (*Deploy*).
5. Jika diminta verifikasi ulang, berikan izin akses yang sama seperti langkah sebelumnya.
6. **Simpan URL Aplikasi Web**:
   - Salin URL Aplikasi Web yang muncul di layar (formatnya seperti: `https://script.google.com/macros/s/AKfycb.../exec`).
   - URL ini yang akan kita simpan di konfigurasi POS.

---

## 💻 Langkah 3: Menghubungkan Google Sheets ke Aplikasi POS

1. Jalankan aplikasi kasir Nusantara POS Anda.
2. Masuk menggunakan akun **Admin** atau **Owner** (Klik tombol pintasan akses demo "Admin Super" di halaman login).
3. Buka menu **Keamanan & Sinkronisasi** (Atau klik menu paling bawah pada sidebar kiri).
4. Pilih tab **"Database Cloud / Google Sheets"** (atau "Google Sheets API" di tab atas).
5. Tempelkan URL yang sudah Anda salin di Langkah 2 ke kolom input yang berbunyi: `https://script.google.com/macros/s/.../exec`.
6. Klik **Simpan Endpoint** kemudian klik **Pindai / Test Connection**.
7. Jika sukses, bar hijau bertuliskan **"Apps Script Terhubung & Sinkronisasi Aktif"** akan menyala! Kini setiap transaksi kasir, input produk baru, transfer stok, ataupun asisten analitis AI akan dibongkar-pasang real-time menuju Google Sheets Anda.

---

## 🌐 Langkah 4: Publikasi Web POS ke GitHub Pages

Untuk membagikan aplikasi ini agar kasir di cabang Lahat (Aisyah) dan cabang Pagar Alam (Beni) bisa menggunakannya secara online dan serentak:

1. Buat **Repositori Baru** di akun GitHub Anda (misal: `nusantara-pos`).
2. Masukkan semua kode proyek ini (kecuali folder `node_modules` dan build artifacts yang sudah diabaikan di `.gitignore`) ke repositori GitHub Anda.
3. Jalankan build statis untuk produksi dengan perintah:
   ```bash
   npm run build
   ```
4. Di repository GitHub Anda, masuk ke **Settings** > **Pages**.
5. Di bagian **Build and deployment**, pilih Source: **GitHub Actions** atau deploy via cabang `gh-pages` / direktori `/dist` using the `gh-pages` npm dependency:
   - Pasang paket deployment: `npm install gh-pages --save-dev`
   - Tambahkan skrip `"predeploy": "npm run build"` dan `"deploy": "gh-pages -d dist"` di file `package.json` Anda.
   - Jalankan perintah: `npm run deploy`
6. Selamat! POS Anda kini terpublikasi secara publik di URL GitHub Pages milik Anda (misal: `https://username-anda.github.io/nusantara-pos/`) dan siap digunakan online sepenuhnya 100% tanpa sepeserpun biaya server!

---

### 🔑 Informasi Pintasan Login Kasir & Akun Demo:
- **Cabang Lahat**: Login Username `kasir1` / Sandi `kasir1` -> Atas nama kasir **Aisyah**
- **Cabang Pagar Alam**: Login Username `kasir2` / Sandi `kasir2` -> Atas nama kasir **Beni**
- **Management (Owner)**: Login Username `owner` / Sandi `owner` -> Atas nama **ndy**
