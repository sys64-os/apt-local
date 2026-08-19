# 📦 apt-local

**apt-local** adalah sebuah script Bash (Package Manager Wrapper) yang memungkinkan Anda untuk menginstal paket aplikasi Debian/Ubuntu (`.deb`) secara lokal di direktori user (`~/.local`) **tanpa memerlukan akses root/sudo**. 

Script ini sangat berguna untuk pengguna *Shared Hosting*, *Container*, atau *Non-Root Environment* yang ingin memasang aplikasi atau library secara mandiri tanpa mengganggu sistem operasi utama.

---

## ✨ Fitur Utama

- **Instalasi Tanpa Root**: Semua file diekstrak dan ditempatkan ke dalam `~/.local`.
- **Resolusi Dependensi Otomatis**: Otomatis mencari dan mengunduh dependensi yang dibutuhkan oleh sebuah paket (menggunakan cache repositori host).
- **Sistem Keamanan (Whitelist & Blacklist)**: Mencegah instalasi paket-paket inti OS (seperti `libc6`, `systemd`, `gcc`) yang berpotensi merusak lingkungan lokal.
- **Autoremove Dependensi**: Saat menghapus paket, script akan melacak dan menawarkan untuk menghapus *orphaned dependencies* (dependensi yang tidak lagi digunakan oleh aplikasi mana pun).
- **Simulasi (Dry-Run)**: Fitur `--dry-run` untuk melihat pratinjau file apa saja yang akan diekstrak dan kode *post-install* apa yang akan dimanipulasi tanpa melakukan modifikasi pada sistem.
- **Eksekusi Post-Install Aman**: Otomatis mengekstrak skrip kontrol (`postinst`), menyesuaikan *hardcoded absolute paths* (contoh: `/usr` menjadi `~/.local/usr`), memblokir perintah khas *root* (`chown`, `systemctl`), lalu menjalankannya secara *silent* di *background*.
- **App-Specific Tweaks (Quirk Fixes)**: Menangani keanehan pada paket tertentu secara otomatis (misalnya membuat *wrapper script* untuk `nano` agar membaca berkas *rcfile* konfigurasi dari `.local`).
- **Pelacakan (Tracking) Instalasi**: Mencatat file apa saja yang diinstal sehingga dapat di-uninstall/remove hingga bersih.
- **Mode Reset Bersih (Clean Sweep)**: Perintah `reset` untuk menghapus seluruh aplikasi dan *symlink* yang pernah dipasang melalui apt-local (dilengkapi peringatan kata sandi *Danger Zone*).
- **Konfigurasi Environment Otomatis**: Menyediakan perintah `env` untuk otomatis menambahkan `$PATH` dan `$LD_LIBRARY_PATH` ke `.bashrc`.

---

## 🚀 Instalasi Script

1. Unduh atau salin script `apt-local` ke server/komputer Anda.
2. Beri hak akses eksekusi pada script:
   ```bash
   chmod +x apt-local
3. Pindahkan script ke folder bin lokal agar bisa dipanggil dari mana saja (opsional):
   ```bash
   mkdir -p ~/.local/bin
   mv apt-local ~/.local/bin/

📖 Panduan Penggunaan

Gunakan perintah selayaknya menggunakan `apt` pada umumnya.

| Perintah | Deskripsi | Contoh |
| :--- | :--- | :--- |
| `update` | Memperbarui daftar repositori ke direktori lokal | `apt-local update` |
| `search` | Mencari paket yang tersedia beserta versinya | `apt-local search htop` |
| `install` | Mengunduh, mengekstrak, dan memasang paket | `apt-local install htop nano` |
| `remove` | Menghapus paket yang sebelumnya diinstal oleh apt-local | `apt-local remove htop` |
| `list` | Menampilkan seluruh paket di repo | `apt-local list` |
| `list --installed` | Menampilkan paket yang sudah diinstal di lokal | `apt-local list --installed` |
| `env` | Cek status & pasang Environment Variable (`$PATH`, dll) | `apt-local env` |
| `install --dry-run` | Menampilkan simulasi ekstraksi & skrip post-install | `apt-local install --dry-run curl`|

⚙️ Konfigurasi (apt-local.conf)

Saat pertama kali dijalankan, script akan membuat file konfigurasi secara otomatis di `~/.local/etc/apt-local/apt-local.conf`.

Anda dapat mengedit file ini untuk mengatur Whitelist dan Blacklist.
Ini, TOML

> 1. Daftar Izin (Whitelist)
> Gunakan tanda bintang (*) untuk mengizinkan semua, atau array: whitelist=[htop, nano]
> whitelist=*
> 
> 2. Daftar Blokir (Blacklist)
> Paket yang tertulis di sini TIDAK AKAN BISA diinstal untuk melindungi OS host.
> blacklist=[gcc, g++, make, libc6, systemd, apt, dpkg, docker.io, dll...]

🛠️ Persiapan Environment (Penting!)

Agar aplikasi yang diinstal di `~/.local` dapat berjalan normal (terbaca sistem dan library-nya terhubung), Anda harus mengatur Environment Variables.

Cukup jalankan perintah:
Bash

    apt-local env

Script akan mengecek kelengkapan `$PATH, $LD_LIBRARY_PATH, dan $XDG_DATA_DIRS`. Jika ada yang kurang, script akan menawarkan untuk menambahkannya ke `~/.bashrc` Anda secara otomatis.

📂 Struktur Direktori

Semua aktivitas apt-local terisolasi pada folder `~/.local`.

> ~/.local/bin & ~/.local/usr/bin : Tempat aplikasi (.exe/binary) berada.
> 
> ~/.local/usr/lib : Tempat library (.so) berada.
> 
> ~/.local/var/lib/apt-local/deb-install : Tempat file tracking instalasi (.conf).

> ~/.local/tmp/apt-local : Folder temporary untuk mengunduh .deb sebelum diekstrak.

⚠️ Peringatan & Keterbatasan

> Bukan Pengganti APT Asli: Script ini menggunakan dpkg -x (ekstrak) dan tidak menjalankan skrip pre/post instalasi (preinst/postinst) dari paket Debian. Paket > yang membutuhkan setup daemon atau system service mungkin tidak berjalan sempurna.
> 
> Kompabilitas C-Library: Jika paket yang Anda instal membutuhkan versi libc6 yang lebih baru dari yang ada di OS Utama (Host), aplikasi tersebut mungkin akan mengalami Segmentation Fault.

`Lisensi: MIT License (Gunakan dengan risiko ditanggung sendiri).`