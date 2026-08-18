# 📦 apt-local

**apt-local** adalah sebuah script Bash (Package Manager Wrapper) yang memungkinkan Anda untuk menginstal paket aplikasi Debian/Ubuntu (`.deb`) secara lokal di direktori user (`~/.local`) **tanpa memerlukan akses root/sudo**. 

Script ini sangat berguna untuk pengguna *Shared Hosting*, *Container*, atau *Non-Root Environment* yang ingin memasang aplikasi atau library secara mandiri tanpa mengganggu sistem operasi utama.

---

## ✨ Fitur Utama

- **Instalasi Tanpa Root**: Semua file diekstrak dan ditempatkan ke dalam `~/.local`.
- **Resolusi Dependensi Otomatis**: Otomatis mencari dan mengunduh dependensi yang dibutuhkan oleh sebuah paket (menggunakan cache repositori host).
- **Sistem Keamanan (Whitelist & Blacklist)**: Mencegah instalasi paket-paket inti OS (seperti `libc6`, `systemd`, `gcc`) yang berpotensi merusak lingkungan lokal.
- **Manajemen Paket Sederhana**: Mendukung perintah `install`, `remove`, `search`, `update`, dan `list`.
- **Pelacakan (Tracking) Instalasi**: Mencatat file apa saja yang diinstal sehingga dapat di-uninstall/remove hingga bersih.
- **Konfigurasi Environment Otomatis**: Menyediakan perintah `env` untuk otomatis menambahkan `$PATH` dan `$LD_LIBRARY_PATH` ke `.bashrc`.

---

## 🚀 Instalasi Script

1. Unduh atau salin script `apt-local` ke server/komputer Anda.
2. Beri hak akses eksekusi pada script:
   ```bash
   chmod +x apt-local