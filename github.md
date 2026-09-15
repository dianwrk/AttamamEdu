# Panduan Lengkap Instalasi & Clone Proyek AttamamEdu
> **Untuk Siswi / Pemula:** Ikuti langkah-langkah di bawah ini secara berurutan mulai dari Bagian 1 sampai aplikasi berhasil berjalan di browser laptop kamu!

---

## 📋 Daftar Isi
1. [Bagian 1: Persiapan Software (Prasyarat)](#bagian-1-persiapan-software-prasyarat)
2. [Bagian 2: Clone Repositori ke Laptop](#bagian-2-clone-repositori-ke-laptop)
3. [Bagian 3: Install Dependensi PHP (Composer Install / Update)](#bagian-3-install-dependensi-php-composer-install--update)
4. [Bagian 4: Konfigurasi File Environment (.env) & App Key](#bagian-4-konfigurasi-file-environment-env--app-key)
5. [Bagian 5: Membuat Database di phpMyAdmin](#bagian-5-membuat-database-di-phpmyadmin)
6. [Bagian 6: Migrasi & Seeding Database (Data Awal)](#bagian-6-migrasi--seeding-database-data-awal)
7. [Bagian 7: Install Dependensi Frontend (Node.js / NPM)](#bagian-7-install-dependensi-frontend-nodejs--npm)
8. [Bagian 8: Menjalankan Aplikasi (Penting: 2 Terminal)](#bagian-8-menjalankan-aplikasi-penting-2-terminal)
9. [Bagian 9: Akses & Akun Login Default](#bagian-9-akses--akun-login-default)
10. [Bagian 10: Alur Kerja Git Sehari-hari (Tarik Pembaruan / Kirim Tugas)](#bagian-10-alur-kerja-git-sehari-hari)
11. [Bagian 11: Troubleshooting & Solusi Masalah Sering Terjadi](#bagian-11-troubleshooting--solusi-masalah-sering-terjadi)

---

## Bagian 1: Persiapan Software (Prasyarat)

Pastikan aplikasi-aplikasi berikut sudah terpasang di laptop masing-masing sebelum memulai:

| No | Software | Versi Minimal | Fungsi | Cara Cek di Terminal |
|---|---|---|---|---|
| 1 | **Git** | Bebas | Mengunduh dan mengelola kode | `git --version` |
| 2 | **PHP** | 8.2 ke atas (8.3 / 8.4 direkomendasikan) | Menjalankan backend Laravel | `php -v` |
| 3 | **Composer** | 2.x ke atas | Package manager untuk library PHP Laravel | `composer --version` |
| 4 | **Node.js & NPM**| Node v18+ / v20+ / v22+ | Menjalankan build asset frontend & Vite | `node -v` dan `npm -v` |
| 5 | **Database MySQL**| XAMPP / Laragon | Menyimpan data aplikasi | Buka control panel XAMPP / Laragon |
| 6 | **Code Editor** | VS Code / Cursor | Mengedit file kode proyek | `code .` |

> 💡 **Tips Pengguna Windows:**
> Jika kamu menggunakan **XAMPP** atau **Laragon**, pastikan servis **Apache** dan **MySQL** sudah berada dalam status **Start / Running**.

---

## Bagian 2: Clone Repositori ke Laptop

1. **Buka Terminal / PowerShell / Git Bash.**
2. Pindah ke folder tempat kamu biasa menyimpan tugas (contoh di drive `D:\Dev` atau `C:\xampp\htdocs` atau folder `Documents`):
   ```bash
   cd D:\Dev
   ```
3. Ketik perintah berikut untuk mengunduh (*clone*) repositori ke laptop kamu:
   ```bash
   git clone https://github.com/dianwrk/AttamamEdu.git
   ```
4. Masuk ke dalam folder proyek yang baru saja di-clone:
   ```bash
   cd AttamamEdu
   ```
5. Buka proyek tersebut di VS Code:
   ```bash
   code .
   ```

---

## Bagian 3: Install Dependensi PHP (Composer Install / Update)

> ⚡ **PENTING: MENGAPA INI HARUS DI AWAL?**
> Folder `vendor` (kumpulan library inti Laravel) sengaja **tidak disertakan** di GitHub agar ukuran unduhan ringan. 
> Tanpa menjalankan `composer install` terlebih dahulu, perintah Laravel seperti `php artisan` **tidak akan bisa berjalan sama sekali** dan akan error `autoload.php not found`.

Buka terminal di dalam VS Code (tekan shortcut keyboard ``Ctrl + ` ``):

1. **Jalankan perintah instalasi Composer:**
   ```bash
   composer install
   ```
   > 📌 **Penjelasan untuk Siswi:**
   > - **`composer install` (Wajib saat pertama kali clone):** Mengunduh pustaka persis sesuai versi yang sudah dikunci oleh guru di file `composer.lock` sehingga tidak akan terjadi error bentrok versi.
   > - **`composer update` (Gunakan hanya jika diinstruksikan oleh guru):** Mengunduh dan memperbarui paket-paket ke versi terbaru yang cocok dengan `composer.json`.

2. **Jika terjadi kendala saat `composer install`:**
   - *Jika muncul error perbedaan versi PHP:*
     ```bash
     composer install --ignore-platform-reqs
     ```
   - *Jika autoloader belum terbaca:*
     ```bash
     composer dump-autoload
     ```

---

## Bagian 4: Konfigurasi File Environment (`.env`) & App Key

Laravel memerlukan file bernama `.env` untuk konfigurasi lokal (kunci aplikasi dan koneksi database).

1. **Salin template file `.env.example` menjadi file `.env`:**
   - **PowerShell (Terminal VS Code):**
     ```powershell
     Copy-Item .env.example .env
     ```
   - **Git Bash / Command Prompt:**
     ```bash
     cp .env.example .env
     ```

2. **Generate Kunci Aplikasi (*Application Key*):**
   Karena `composer install` sudah selesai di Bagian 3, sekarang perintah `php artisan` sudah siap digunakan:
   ```bash
   php artisan key:generate
   ```
   *(Perintah ini otomatis mengisi nilai acak pada `APP_KEY` di file `.env`)*

---

## Bagian 5: Membuat Database di phpMyAdmin

Sebelum tabel database dibuat oleh Laravel, kita siapkan wadah database kosongnya:

1. Buka browser (Chrome / Edge / Firefox).
2. Akses alamat: **`http://localhost/phpmyadmin`**
3. Klik menu **Baru** / **New** di panel sebelah kiri.
4. Pada kolom nama basis data (*Database name*), ketik persis:
   ```text
   attamamedu
   ```
5. Klik tombol **Buat** / **Create**.

6. **Periksa file `.env` di VS Code:**
   Buka file `.env`, pastikan baris koneksi database (sekitar baris 20-25) sudah sesuai:
   ```env
   DB_CONNECTION=mysql
   DB_HOST=127.0.0.1
   DB_PORT=3306
   DB_DATABASE=attamamedu
   DB_USERNAME=root
   DB_PASSWORD=
   ```
   > ⚠️ **Catatan Password:**
   > - **XAMPP default:** Password adalah **kosong** (`DB_PASSWORD=`).
   > - **Laragon default:** Password biasanya **kosong**.
   > - Jika MySQL di laptopmu memiliki password pribadi, isi di sebelah kanan tanda `=`.

---

## Bagian 6: Migrasi & Seeding Database (Data Awal)

Langkah ini akan membuat seluruh tabel yang dibutuhkan serta mengisinya dengan data awal (berita, data jurusan, profil sekolah, dan akun admin):

1. **Jalankan Migrasi & Seeding sekaligus:**
   ```bash
   php artisan migrate --seed
   ```
   > ✅ Pastikan semua baris berstatus `DONE`.

2. **Buat Tautan Folder Penyimpanan (*Storage Link*):**
   Agar foto/berkas yang diupload nanti dapat tampil di browser:
   ```bash
   php artisan storage:link
   ```

---

## Bagian 7: Install Dependensi Frontend (Node.js / NPM)

Proyek ini menggunakan React, Tailwind CSS v4, dan Vite untuk tampilan antarmuka modern.

Di terminal VS Code, jalankan:
```bash
npm install
```
Tunggu proses instalasi selesai sampai folder `node_modules` terbentuk.

---

## Bagian 8: Menjalankan Aplikasi (PENTING: Gunakan 2 Terminal!)

Aplikasi ini membutuhkan **dua terminal aktif** yang berjalan bersamaan:
1. **Terminal 1: Server Laravel** (Menangani backend PHP & database)
2. **Terminal 2: Server Vite** (Menangani frontend React & styling)

### 🔹 Terminal 1: Jalankan Laravel
Di terminal VS Code pertama, ketik:
```bash
php artisan serve
```
*Output sukses:*
```text
INFO Server running on [http://127.0.0.1:8000].
```

### 🔹 Terminal 2: Jalankan Vite Dev Server
Buka terminal kedua (klik ikon **`+`** atau **Split Terminal** di panel terminal VS Code), lalu ketik:
```bash
npm run dev
```
*Output sukses:*
```text
VITE v7.x.x ready in ... ms
➜ Local: http://localhost:5173/
```

> 🚨 **PERINGATAN KERAS:**
> **JANGAN menutup Terminal 2 (`npm run dev`)!**
> Jika Terminal 2 tidak jalan, website akan langsung error:
> `Vite manifest not found at: .../public/build/manifest.json`.

---

## Bagian 9: Akses & Akun Login Default

Setelah kedua terminal berjalan, buka halaman berikut di browsermu:

| Halaman | Alamat URL | Keterangan |
|---|---|---|
| **Website Utama** | [http://localhost:8000](http://localhost:8000) | Beranda web sekolah untuk umum |
| **Formulir PPDB** | [http://localhost:8000/ppdb](http://localhost:8000/ppdb) | Portal pendaftaran peserta didik baru |
| **Cek Status PPDB** | [http://localhost:8000/ppdb/cek-status](http://localhost:8000/ppdb/cek-status) | Pelacakan status pendaftar |
| **Panel Admin (Portal)** | [http://localhost:8000/portal](http://localhost:8000/portal) | Panel login admin Filament |

### 🔐 Akun Login Admin Bawaan (Hasil Seeder):

| Peran (Role) | Email Login | Password | Akses Fitur |
|---|---|---|---|
| **Super Admin** | `superadmin@attamam.sch.id` | `password123` | Hak akses penuh seluruh sistem |
| **Admin CMS** | `cms@attamam.sch.id` | `password123` | Kelola berita, agenda, galeri & pengumuman |
| **Admin PPDB** | `ppdb@attamam.sch.id` | `password123` | Seleksi berkas & verifikasi calon siswa |
| **Editor Akademik** | `akademik@attamam.sch.id` | `password123` | Kelola data jurusan & guru/staf |

---

## Bagian 10: Alur Kerja Git Sehari-hari

### 1. Mengambil Pembaruan Terbaru dari Guru
Jika guru mengumumkan ada update materi atau fitur baru di GitHub:
```bash
git pull origin main
```
*Jika ada penambahan database atau library baru setelah pull:*
```bash
composer install
npm install
php artisan migrate
```

### 2. Membuat Branch Sendiri untuk Pengerjaan Tugas
Agar kode tugas kamu rapi dan tidak mengganggu branch utama:
```bash
# Contoh format: git checkout -b tugas-nama-kamu
git checkout -b tugas-siti-rahma
```

### 3. Menyimpan Perubahan Pekerjaan Kamu (Commit)
```bash
# 1. Cek file apa saja yang diubah
git status

# 2. Masukkan semua perubahan ke staging
git add .

# 3. Simpan commit dengan catatan jelas
git commit -m "feat: mengerjakan halaman kontak sekolah"
```

### 4. Mengirim Hasil Tugas ke Repositori (Push)
```bash
git push -u origin tugas-siti-rahma
```

---

## Bagian 11: Troubleshooting & Solusi Masalah Sering Terjadi

### ❌ 1. Error: `Vite manifest not found at: .../manifest.json`
- **Penyebab:** Server Vite (`npm run dev`) belum dinyalakan.
- **Solusi:** Buka terminal baru dan jalankan `npm run dev`. Jangan ditutup selama membuka website.

---

### ❌ 2. Error: `Failed opening required '.../vendor/autoload.php'`
- **Penyebab:** Perintah `composer install` belum dijalankan setelah clone.
- **Solusi:** Jalankan `composer install` di terminal.

---

### ❌ 3. Error: `SQLSTATE[HY000] [1049] Unknown database 'attamamedu'`
- **Penyebab:** Database belum dibuat di MySQL.
- **Solusi:** Buka `http://localhost/phpmyadmin`, buat database bernama `attamamedu`, lalu jalankan kembali `php artisan migrate --seed`.

---

### ❌ 4. Error: `SQLSTATE[HY000] [1045] Access denied for user 'root'@'localhost'`
- **Penyebab:** Password MySQL salah.
- **Solusi:** Buka file `.env`, untuk XAMPP default pastikan kosong: `DB_PASSWORD=`.

---

### ❌ 5. Error: `No application encryption key has been specified`
- **Penyebab:** Kunci enkripsi belum di-generate.
- **Solusi:** Jalankan `php artisan key:generate`.

---

### ❌ 6. Error PowerShell: `... File cannot be loaded because running scripts is disabled on this system`
- **Penyebab:** Security policy PowerShell Windows membatasi script.
- **Solusi:** Buka PowerShell sebagai Administrator, lalu ketik:
  ```powershell
  Set-ExecutionPolicy -Scope CurrentUser RemoteSigned
  ```
  Pilih `Y`, kemudian restart VS Code.

---

### ❌ 7. Error: `Failed to listen on 127.0.0.1:8000` (Port sudah terpakai)
- **Solusi:** Jalankan di port alternatif:
  ```bash
  php artisan serve --port=8080
  ```
  Lalu buka `http://localhost:8080`.

---

### ❌ 8. Error PHP Extension Missing (`pdo_mysql`, `fileinfo`, `gd`, `zip`, `intl`)
- **Solusi (XAMPP):**
  1. Di XAMPP Control Panel, klik tombol **Config** di baris Apache -> pilih **PHP (php.ini)**.
  2. Cari baris berikut dan **hapus tanda titik koma (`;`)** di depannya:
     ```ini
     extension=pdo_mysql
     extension=fileinfo
     extension=gd
     extension=zip
     extension=intl
     ```
  3. Simpan file (`Ctrl + S`), lalu **Stop** dan **Start** kembali Apache.

---

## 🚀 Ringkasan Perintah Cepat (Cheatsheet)

Bagi yang sudah terbiasa, berikut alur perintah cepat dari nol:

```bash
# 1. Clone & masuk folder
git clone https://github.com/dianwrk/AttamamEdu.git
cd AttamamEdu

# 2. Install dependensi PHP (Wajib Pertama!)
composer install

# 3. Setup environment & key
cp .env.example .env
php artisan key:generate

# 4. Buat DB 'attamamedu' di phpMyAdmin, lalu migrasi & seed
php artisan migrate --seed
php artisan storage:link

# 5. Install dependensi frontend
npm install

# 6. Jalankan aplikasi (2 terminal aktif)
# Terminal 1:
php artisan serve

# Terminal 2:
npm run dev
```

*Selamat belajar dan berkarya bersama AttamamEdu!* ✨
