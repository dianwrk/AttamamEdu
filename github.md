# Panduan Lengkap Git: Clone, Remote & Push ke GitHub

Dokumentasi ini berisi panduan komprehensif mulai dari cara melakukan clone repositori, menghubungkan repositori ke remote baru (repo sendiri), hingga melakukan push commit ke GitHub.

---

## Daftar Isi
1. [Bagian I: Panduan Git Clone](#bagian-i-panduan-git-clone)
   - [1.1 Apa itu `git clone`?](#11-apa-itu-git-clone)
   - [1.2 Prasyarat](#12-prasyarat)
   - [1.3 Cara Mendapatkan URL Repositori](#13-cara-mendapatkan-url-repositori)
   - [1.4 Metode Clone (HTTPS vs SSH vs CLI)](#14-metode-clone-https-vs-ssh-vs-cli)
   - [1.5 Variasi Perintah `git clone`](#15-variasi-perintah-git-clone)
2. [Bagian II: Panduan Mengubah Remote & Push ke Repositori Baru](#bagian-ii-panduan-mengubah-remote--push-ke-repositori-baru)
   - [2.1 Skenario: Clone Repo Sumber lalu Push ke Akun Sendiri](#21-skenario-clone-repo-sumber-lalu-push-ke-akun-sendiri)
   - [2.2 Cek Remote Saat Ini](#22-cek-remote-saat-ini)
   - [2.3 Mengubah Remote Origin ke Repositori Baru](#23-mengubah-remote-origin-ke-repositori-baru)
   - [2.4 Menambahkan Perubahan (Git Add & Commit)](#24-menambahkan-perubahan-git-add--commit)
   - [2.5 Melakukan Push ke GitHub (`git push`)](#25-melakukan-push-ke-github-git-push)
3. [Bagian III: Langkah Setelah Clone (Setup Proyek Laravel & Node)](#bagian-iii-langkah-setelah-clone-setup-proyek-laravel--node)
4. [Bagian IV: Troubleshooting & Penanganan Masalah Umum](#bagian-iv-troubleshooting--penanganan-masalah-umum)
5. [Bagian V: Cheatsheet Perintah Lengkap](#bagian-v-cheatsheet-perintah-lengkap)

---

## Bagian I: Panduan Git Clone

### 1.1 Apa itu `git clone`?
`git clone` adalah perintah Git yang digunakan untuk menduplikasi/mengunduh repositori remote (seperti di GitHub) beserta seluruh riwayat commit, branch, dan versinya ke komputer lokal.

### 1.2 Prasyarat
Sebelum melakukan clone, pastikan:
1. **Git telah terpasang**:
   ```bash
   git --version
   ```
   Unduh di [git-scm.com](https://git-scm.com/) jika belum terpasang.
2. **Terminal / Shell**: Gunakan Command Prompt, PowerShell, Git Bash, atau terminal IDE.
3. **Akses ke Repositori**: Jika repositori privat, pastikan akun Anda memiliki izin akses.

### 1.3 Cara Mendapatkan URL Repositori
1. Buka halaman repositori di GitHub (misal: `https://github.com/username/repository`).
2. Klik tombol hijau bertuliskan **`<> Code`**.
3. Pilih protokol (**HTTPS** atau **SSH**) lalu klik tombol **Copy**.

### 1.4 Metode Clone (HTTPS vs SSH vs CLI)

| Protokol | Contoh URL | Kelebihan | Catatan |
|---|---|---|---|
| **HTTPS** | `https://github.com/user/repo.git` | Paling mudah, langsung jalan | Membutuhkan Personal Access Token (PAT) saat autentikasi push |
| **SSH** | `git@github.com:user/repo.git` | Sangat aman, tidak perlu input sandi | Butuh mendaftarkan SSH Key di akun GitHub |
| **GitHub CLI** | `gh repo clone user/repo` | Sangat praktis via terminal | Memerlukan instalasi GitHub CLI (`gh`) |

### 1.5 Variasi Perintah `git clone`

- **Clone Standar (Membuat Folder Baru Sesuai Nama Repo):**
  ```bash
  git clone https://github.com/username/repository.git
  cd repository
  ```

- **Clone ke Folder dengan Nama Kustom:**
  ```bash
  git clone https://github.com/username/repository.git nama-folder-tujuan
  ```

- **Clone Langsung ke Folder Saat Ini (`.`):**
  *(Gunakan saat sudah berada di dalam folder proyek kosong)*
  ```bash
  git clone https://github.com/username/repository.git .
  ```

- **Clone Branch Tertentu Saja:**
  ```bash
  git clone -b nama-branch https://github.com/username/repository.git
  ```

- **Shallow Clone (Hemat Kuota, Hanya Commit Terakhir):**
  ```bash
  git clone --depth 1 https://github.com/username/repository.git
  ```

- **Clone dengan Submodule:**
  ```bash
  git clone --recurse-submodules https://github.com/username/repository.git
  ```

---

## Bagian II: Panduan Mengubah Remote & Push ke Repositori Baru

### 2.1 Skenario: Clone Repo Sumber lalu Push ke Akun Sendiri
Misalkan Anda meng-clone proyek dari:
`https://github.com/M-FARID-RASYAD-F/redesign.git`
Lalu ingin menyimpan seluruh riwayat dan perubahan ke repositori akun Anda sendiri:
`https://github.com/dianwrk/AttamamEdu.git`

### 2.2 Cek Remote Saat Ini
Periksa alamat remote remote yang aktif:
```bash
git remote -v
```
Output contoh:
```text
origin  https://github.com/M-FARID-RASYAD-F/redesign.git (fetch)
origin  https://github.com/M-FARID-RASYAD-F/redesign.git (push)
```

### 2.3 Mengubah Remote Origin ke Repositori Baru

Ada dua cara untuk mengubah remote origin:

#### Cara A: Mengubah Langsung URL Remote `origin` (Paling Sederhana)
```bash
git remote set-url origin https://github.com/dianwrk/AttamamEdu.git
```

#### Cara B: Menyimpan Repo Asal sebagai `upstream` (Rekomendasi untuk Fork/Sync)
Jika Anda tetap ingin bisa mengambil pembaruan dari repositori sumber di masa depan:
```bash
# Ubah nama origin lama menjadi upstream
git remote rename origin upstream

# Tambahkan repositori Anda sebagai origin baru
git remote add origin https://github.com/dianwrk/AttamamEdu.git
```

Verifikasi kembali dengan:
```bash
git remote -v
```

### 2.4 Menambahkan Perubahan (Git Add & Commit)

1. Cek status file:
   ```bash
   git status
   ```

2. Tambahkan file baru atau yang telah diubah ke area staging:
   ```bash
   git add .
   ```

3. Simpan commit dengan pesan deskriptif:
   ```bash
   git commit -m "docs: tambahkan panduan lengkap github clone dan push di github.md"
   ```

### 2.5 Melakukan Push ke GitHub (`git push`)

1. Pastikan nama branch utama adalah `main`:
   ```bash
   git branch -M main
   ```

2. Kirim (*push*) commit ke repositori baru dan set tracking upstream:
   ```bash
   git push -u origin main
   ```
   > `-u` (atau `--set-upstream`) berfungsi agar perintah push berikutnya cukup mengetik `git push`.

3. Jika repositori di GitHub sudah memiliki commit atau file README/LICENSE tersendiri dan terjadi penolakan (*non-fast-forward*), gunakan:
   ```bash
   # Opsi 1: Tarik dan gabungkan
   git pull origin main --allow-unrelated-histories --rebase
   git push origin main

   # Opsi 2: Timpa total dengan isi lokal (Gunakan dengan hati-hati)
   git push -u origin main --force
   ```

---

## Bagian III: Langkah Setelah Clone (Setup Proyek Laravel & Node)

1. **Buat file `.env` dari `.env.example`:**
   - PowerShell: `Copy-Item .env.example .env`
   - Linux/Git Bash: `cp .env.example .env`
2. **Install dependensi PHP (Composer):**
   ```bash
   composer install
   php artisan key:generate
   ```
3. **Install dependensi JavaScript & CSS (NPM):**
   ```bash
   npm install
   ```
4. **Jalankan aplikasi:**
   ```bash
   # Terminal 1
   php artisan serve

   # Terminal 2
   npm run dev
   ```

---

## Bagian IV: Troubleshooting & Penanganan Masalah Umum

### 🔴 Masalah 1: `Permission to username/repo.git denied to user`
- **Penyebab**: Akun yang tersimpan di sistem tidak memiliki izin write ke repositori target.
- **Solusi**:
  - Gunakan Personal Access Token (PAT) dengan scope `repo`.
  - Hapus kredensial lama di **Windows Credential Manager** (`Control Panel` -> `Credential Manager` -> `Windows Credentials` -> cari `git:https://github.com` dan perbarui).

### 🔴 Masalah 2: `failed to push some refs to 'https://github.com/...'`
- **Penyebab**: Repositori GitHub target sudah memiliki commit yang belum ada di lokal (misal dibuat dengan inisialisasi README di web GitHub).
- **Solusi**:
  ```bash
  git pull origin main --rebase
  git push origin main
  ```

### 🔴 Masalah 3: `destination path '.' already exists and is not an empty directory`
- **Penyebab**: Menjalankan `git clone <url> .` di dalam folder yang sudah ada file-nya.
- **Solusi**: Kosongkan folder terlebih dahulu atau jalankan `git clone <url>` tanpa tanda titik agar otomatis membuat folder baru.

---

## Bagian V: Cheatsheet Perintah Lengkap

```bash
# === CLONE ===
git clone <URL>                        # Clone standar
git clone <URL> <folder>               # Clone ke folder kustom
git clone <URL> .                      # Clone ke folder aktif
git clone -b <branch> <URL>            # Clone branch tertentu
git clone --depth 1 <URL>              # Shallow clone

# === REMOTE & BRANCH ===
git remote -v                          # Cek daftar remote
git remote set-url origin <URL_BARU>   # Ganti URL origin
git remote add origin <URL_BARU>       # Tambah origin baru
git branch -M main                     # Set nama branch ke main

# === COMMIT & PUSH ===
git status                             # Cek status file
git add .                              # Stage semua file
git commit -m "pesan commit"           # Commit perubahan
git push -u origin main                # Push pertama kali
git push                               # Push selanjutnya
git pull                               # Ambil pembaruan terbaru
```
