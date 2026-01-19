# SIORMAWA - Sistem Informasi Manajemen Organisasi Mahasiswa

![Laravel](https://img.shields.io/badge/Laravel-12.x-FF2D20?style=for-the-badge&logo=laravel)
![Tailwind CSS](https://img.shields.io/badge/Tailwind_CSS-3.x-38B2AC?style=for-the-badge&logo=tailwind-css)
![Vite](https://img.shields.io/badge/Vite-6.x-646CFF?style=for-the-badge&logo=vite)
![MySQL](https://img.shields.io/badge/MySQL-8.0-4479A1?style=for-the-badge&logo=mysql)

![SS AWAL](./siormawa.png)

**SIORMAWA** adalah platform digital komprehensif yang dirancang untuk memodernisasi dan menyederhanakan manajemen organisasi kemahasiswaan di lingkungan kampus. Aplikasi ini memfasilitasi pengelolaan data anggota, administrasi kegiatan, arsip dokumen, dan distribusi informasi secara terpusat, efisien, dan transparan.

## 🚧 Status Pengembangan

**Status:** Tahap Awal (80%)

Proyek ini masih dalam tahap pengembangan aktif. Fitur utama sedang dibangun dan disempurnakan.

---

## 🎯 Konteks Penggunaan

Aplikasi ini dirancang fleksibel untuk berbagai tingkat organisasi pendidikan:

### 🎓 Universitas (Skala Masif)
*   **1 Admin Sistem (Staff IT)**: Mengelola sistem secara keseluruhan.
*   **15+ Admin Organisasi**: Ketua/Sekretaris tiap Himpunan/UKM.
*   **500+ Member**: Mahasiswa aktif.
*   *Fokus*: Setup awal, maintenance, monitoring kegiatan rutin, dan rekrutmen.

### 🏫 Sekolah / SMA (Skala Menengah)
*   **1 Admin Sistem (Guru TI)**: Pengawas sistem.
*   **9+ Admin Organisasi**: Pembina OSIS & Ekstrakurikuler.
*   **200+ Member**: Siswa.
*   *Fokus*: Koordinasi kegiatan sekolah, manajemen anggota ekskul, distribusi info.

---

## 🚀 Fitur & Hak Akses

### 👑 Admin Sistem (Superadmin)
*   ✅ **Manajemen Pengguna**: CRUD seluruh akun pengguna.
*   ✅ **Manajemen Organisasi**: Setup organisasi (Himpunan, BEM, UKM).
*   ✅ **Monitoring**: Dashboard statistik aktivitas.
*   ✅ **Pengumuman Global**: Broadcast info ke semua user.

### 🏛️ Admin Organisasi
*   ✅ **Kelola Internal**: CRUD data organisasinya sendiri.
*   ✅ **Manajemen Anggota**: Rekrutmen dan pendataan anggota.
*   ✅ **Kegiatan & Event**: Membuat dan mempublikasikan acara.
*   ✅ **Arsip Dokumen**: Upload proposal, LPJ, surat.
*   ❌ *Pembatasan*: Tidak bisa mengakses data organisasi lain.

### 👤 Member (Mahasiswa)
*   ✅ **Lihat Data**: Akses profil dan info organisasi.
*   ✅ **Download**: Mengunduh dokumen publik/internal.
*   ✅ **Partisipasi**: Mendaftar pada kegiatan/event.
*   ❌ *Pembatasan*: Tidak bisa mengubah data inti (Read-Only/Limited).

---

## 💾 Struktur & Relasi Database

Desain database dirancang agar setiap organisasi memiliki privasi data masing-masing, namun tetap dalam satu sistem terintegrasi.

### 1. Entitas & Tabel

Berikut adalah penjelasan fungsi setiap tabel dalam database:

*   **Tabel `users` (Pengguna)**
    *   Berfungsi untuk menyimpan akun login (Email & Password).
    *   Memiliki atribut `role` untuk membedakan hak akses: *Admin Sistem*, *Admin Organisasi*, atau *Member*.
    *   Khusus *Admin Organisasi*, akun ini akan terhubung langsung ke organisasi tertentu.

*   **Tabel `organizations` (Organisasi)**
    *   Merupakan jantung dari sistem ini.
    *   Menyimpan profil organisasi seperti Nama, Singkatan (HMTI/BEM), dan Kategori.
    *   Semua data lain (Kegiatan, Anggota, Dokumen) "menempel" pada tabel ini.

*   **Tabel `members` (Anggota)**
    *   Menyimpan biodata lengkap mahasiswa seperti NIM, Nama Lengkap, Jabatan, dan Tahun Masuk.
    *   **Penting:** Tabel ini terpisah dari tabel `users`. Artinya, seorang mahasiswa bisa terdaftar sebagai anggota organisasi meskipun dia tidak memiliki akun login.

*   **Tabel `events` (Kegiatan)**
    *   Menyimpan jadwal kegiatan organisasi.
    *   Fitur utama: Judul acara, Tanggal pelaksanaan, dan Status (Akan datang/Selesai).

*   **Tabel `documents` (Arsip)**
    *   Menyimpan file proposal, LPJ, atau surat yang diunggah oleh pengurus.
    *   Setiap dokumen diberi label kategori untuk memudahkan pencarian.

### 2. Hubungan Antar Data (Relasi)

Sistem menggunakan konsep "Multi-Tenancy" sederhana:

1.  **Satu Organisasi Memiliki Banyak Data**
    *   Satu organisasi (misal: HMTI) memiliki banyak **Anggota**.
    *   Satu organisasi memiliki banyak **Kegiatan**.
    *   Satu organisasi memiliki banyak **Dokumen**.

2.  **Privasi Data**
    *   Ketika Admin Login, sistem akan mengecek ID Organisasinya.
    *   Sistem **hanya** akan menampilkan data (Anggota/Event/Dokumen) yang memiliki ID Organisasi yang sama dengan Admin tersebut.
    *   Akibatnya, Admin HMTI tidak akan bisa melihat atau mengedit data milik BEM.

3.  **Koneksi User & Member**
    *   Jika seorang *Member* (Mahasiswa) ingin login, admin akan membuatkan akun di tabel `users`.
    *   Akun di `users` tersebut akan dihubungkan dengan biodata aslinya di tabel `members` menggunakan Nomor Induk Mahasiswa (NIM).

---

## 🛠️ Teknologi yang Digunakan

*   **Framework**: [Laravel 12](https://laravel.com)
*   **Styling**: [Tailwind CSS](https://tailwindcss.com) (Vanilla CSS philosophy)
*   **Build Tool**: [Vite](https://vitejs.dev)
*   **Database**: MySQL

> **Catatan Teknis (Laravel 11/12)**:
> File `app/Http/Kernel.php` sudah tidak digunakan lagi untuk daftar route middleware. Semua konfigurasi middleware sekarang dipusatkan di file `bootstrap/app.php`.

---

## ⚙️ Instalasi

1.  **Clone & Install**:
    ```bash
    git clone https://github.com/username/siormawa.git
    cd siormawa
    composer install && npm install
    ```

2.  **Environment**:
    ```bash
    cp .env.example .env
    # Edit .env sesuaikan database
    php artisan key:generate
    ```

3.  **Migrate & Seed**:
    ```bash
    php artisan migrate:fresh --seed
    ```

4.  **Run**:
    ```bash
    npm run build
    php artisan serve
    ```

---

## 🔑 Akun Default

| Role | Email | Password | Keterangan |
| :--- | :--- | :--- | :--- |
| **Admin Sistem** | `admin@siormawa.ac.id` | `password` | Full Akses |
| **Org Admin (HMTI)** | `hmti.admin@university.ac.id` | `password` | Admin Himpunan |
| **Org Admin (BEM)** | `bem.admin@university.ac.id` | `password` | Admin BEM |
| **Member** | *(Cek di database)* | `password` | User biasa |

---

## 📜 Lisensi

Aplikasi ini direncanakan akan menggunakan lisensi **Open Source**. Detail lisensi akan ditetapkan saat rilis stabil.