<div align="center">
    <h1 style="text-align: center;font-weight: bold">Laporan Workshop Administrasi Jaringan<br></h1>
    <h2 style="text-align: center;">Chapter 4 <br></h2>
    <h4 style="text-align: center;">Dosen Pengampu : Dr. Ferry Astika Saputra, S.T., M.Sc.</h4>
</div>
<br />
<div align="center">
    <img src="images/Logo_PENS.png" alt="Logo PENS">
    <h3 style="text-align: center;">Disusun Oleh :</h3>
    <p style="text-align: center;">
        <strong>Marieta Nona Alfani (3123500025)</strong>
    </p>
    <h3 style="text-align: center;line-height: 1.5">Politeknik Elektronika Negeri Surabaya<br>Departemen Teknik Informatika Dan Komputer<br>Program Studi Teknik Informatika<br>2025/2026</h3>
    <hr>
</div>
<br>

# Chapter 4: Process Control

<img src="images/c41.png" alt="Proces Control">

# Komponen Proses

Sebuah proses terdiri dari **ruang alamat** dan **struktur data dalam kernel**. Ruang alamat adalah sekumpulan halaman memori yang digunakan untuk menyimpan kode, data, dan stack proses. Kernel menggunakan struktur data internal untuk melacak status, prioritas, serta parameter penjadwalan proses.

## Sumber Daya Proses
Proses dapat dianggap sebagai wadah yang mengelola berbagai sumber daya, seperti:
- Halaman memori untuk kode dan data  
- Deskriptor file untuk file yang terbuka  
- Informasi tentang status, penggunaan CPU/memori, serta akses jaringan  

Selain itu, kernel menyimpan data penting tentang proses, seperti:
- Peta ruang alamat proses  
- Status saat ini (berjalan, tidur, dll.)  
- Prioritas proses  
- Sumber daya yang telah digunakan  
- Sinyal yang diblokir  
- Pemilik proses (user ID)  

## Thread dalam Proses
**Thread** adalah konteks eksekusi dalam suatu proses. Beberapa thread dalam satu proses berbagi ruang alamat dan sumber daya yang sama, sehingga lebih efisien dibandingkan proses baru.

### Contoh Penggunaan Thread
Dalam server web, satu proses menangani banyak permintaan dengan membuat thread baru untuk setiap koneksi. Hal ini memungkinkan server menangani banyak permintaan secara bersamaan.

## PID, PPID, UID, dan EUID

- **PID (Process ID)**: Setiap proses memiliki **ID unik** yang diberikan oleh kernel saat proses dibuat. PID digunakan dalam pemanggilan sistem, seperti mengirim sinyal ke proses.  
- **Namespaces** memungkinkan beberapa proses memiliki PID yang sama dalam lingkungan yang terisolasi (seperti **container**).  

- **PPID (Parent Process ID)**: Setiap proses memiliki **proses induk**, yang menciptakannya. **PPID** adalah PID dari proses induk, digunakan dalam sistem panggilan untuk berkomunikasi dengan induknya.  

- **UID (User ID) & EUID (Effective User ID)**:  
  - **UID** adalah ID pengguna yang menjalankan proses.  
  - **EUID** menentukan izin akses proses terhadap sumber daya seperti file dan jaringan.

## Siklus Hidup Proses

### Pembuatan Proses
- Proses baru dibuat menggunakan **fork**, yang membuat salinan identik dari proses induk dengan PID berbeda.
- Linux menggunakan **clone**, yang merupakan versi lebih canggih dari fork, menangani thread dan fitur tambahan.

### Proses Sistem
- Saat sistem **booting**, kernel secara otomatis menciptakan beberapa proses, termasuk **init** atau **systemd** (PID 1).
- Proses ini menjalankan skrip startup sistem dan menjadi induk dari semua proses lain, kecuali yang dibuat langsung oleh kernel.

## Sinyal dalam Proses

Sinyal adalah mekanisme untuk memberi notifikasi ke suatu proses saat terjadi suatu peristiwa. Ada sekitar 30 jenis sinyal yang digunakan dalam berbagai situasi:

- **Komunikasi antarproses**
- **Menghentikan atau menangguhkan proses** melalui terminal (misalnya dengan tombol tertentu)
- **Administrator mengelola proses** menggunakan perintah `kill`
- **Kernel mengirim sinyal** saat terjadi kesalahan, seperti pembagian oleh nol
- **Kernel memberi notifikasi** tentang kondisi tertentu, misalnya proses anak yang selesai atau data yang tersedia pada saluran I/O
![Signals](https://liujunming.top/images/2018/12/71.png)

### Jenis Sinyal dalam Proses
Meskipun beberapa sinyal terdengar mirip, masing-masing memiliki fungsi berbeda:

- **KILL (SIGKILL):**
  - Tidak dapat diblokir atau ditangani oleh proses
  - Menghentikan proses langsung di tingkat kernel

- **INT (SIGINT):**
  - Dikirim oleh terminal saat pengguna menekan `Ctrl+C`
  - Meminta proses berhenti; program sederhana seharusnya keluar, sementara program interaktif bisa membersihkan dan menunggu input baru

- **TERM (SIGTERM):**
  - Permintaan untuk menghentikan proses secara bersih
  - Memberi kesempatan bagi proses untuk melakukan cleanup sebelum keluar

- **HUP (SIGHUP):**
  - Dikirim saat terminal yang mengontrol proses ditutup
  - Awalnya untuk "hang up" telepon, sekarang sering digunakan untuk me-restart daemon agar memuat ulang konfigurasi

- **QUIT (SIGQUIT):**
  - Mirip dengan SIGTERM, tetapi jika tidak ditangani, akan menghasilkan core dump
  - Beberapa program menggunakannya dengan makna khusus

1. **Menjalankan proses `sleep` di latar belakang**  
   - User menjalankan tiga proses `sleep 1000 &`, dengan PID:  
     - 4349  
     - 4350  
     - 4351  

2. **Melihat daftar proses `sleep` yang berjalan**  
   - Menggunakan `ps aux | grep sleep` untuk melihat proses yang berjalan.  

3. **Menghentikan proses dengan `kill`**  
   - `kill 4349` digunakan untuk menghentikan proses dengan PID 4349.  
   - Setelah dicek lagi (`ps aux | grep sleep`), proses 4349 sudah hilang, tetapi 4350 dan 4351 masih berjalan.  

4. **Menghentikan semua proses `sleep` dengan `killall`**  
   - `killall sleep` digunakan untuk menghentikan semua proses `sleep`.  
   - Hasilnya, proses 4350 dan 4351 dihentikan.  

5. **Menggunakan `pkill` sebagai alternatif**  
   - `pkill sleep` dijalankan, tetapi karena semua proses `sleep` sudah dihentikan sebelumnya, tidak ada proses tambahan yang dihentikan.  

6. **Memverifikasi proses setelah `kill` commands**  
   - `ps aux | grep sleep` menunjukkan bahwa tidak ada lagi proses `sleep` yang berjalan, hanya proses `grep sleep` yang muncul.  

### **Kesimpulan**  
User berhasil menguji perintah `kill`, `killall`, dan `pkill` untuk menghentikan proses `sleep`.

- **`kill`** digunakan untuk menghentikan satu proses berdasarkan PID.  
- **`killall`** menghentikan semua proses berdasarkan nama.  
- **`pkill`** juga menghentikan proses berdasarkan nama tetapi lebih fleksibel dengan opsi tambahan.

## **Perintah Monitoring Proses (`ps`, `pgrep`, `pidof`)**
### **`ps aux`**
Menampilkan semua proses yang berjalan di sistem.
- `a` → Menampilkan proses dari semua pengguna.
- `u` → Menampilkan informasi detail proses.
- `x` → Menampilkan proses yang tidak terkait dengan terminal.

**Contoh:**  
```bash
ps aux | head -8
```
Menampilkan 8 baris pertama dari daftar proses.

### **`ps lax`**
Alternatif `ps aux` yang lebih cepat karena tidak perlu menyelesaikan nama pengguna dan grup.

**Contoh:**
```bash
ps lax | head -8
```

### **Mencari Proses Tertentu dengan `grep`**
```bash
ps aux | grep -v grep | grep firefox
```

### **Menemukan PID dengan `pgrep` dan `pidof`**
```bash
pgrep firefox
pidof /usr/bin/firefox
```
## Monitoring Interaktif dengan `top` dan `htop`
### 1. `top` – Monitoring Proses Secara Real-Time
- Menampilkan informasi seperti:
  - Load average
  - Penggunaan CPU dan memori
  - Daftar proses yang sedang berjalan
- Data diperbarui setiap 1-2 detik secara default.
- Navigasi dalam `top`:
  - `q` → Keluar dari `top`.
  - `h` → Menampilkan bantuan.
  - `k` → Menghentikan proses berdasarkan PID.
  - `r` → Mengubah prioritas proses (nice value).
  - `M` → Mengurutkan berdasarkan penggunaan memori.
  - `P` → Mengurutkan berdasarkan penggunaan CPU.

### 2. `htop` – Alternatif `top` dengan Tampilan Lebih Interaktif
- Menampilkan full command line dari setiap proses.
- Bisa scroll secara vertikal dan horizontal.
- Menampilkan penggunaan CPU dan memori dalam bentuk grafik bar.
- Contoh penggunaan: `htop`

### Kesimpulan
- Gunakan `top` untuk monitoring default yang tersedia di semua sistem Linux.
- Gunakan `htop` jika ingin tampilan yang lebih interaktif dan user-friendly.
# Mengubah Prioritas Proses dengan `nice` dan `renice`

## 1. Apa Itu Niceness?
Niceness adalah nilai yang menentukan seberapa "ramah" sebuah proses terhadap proses lain dalam persaingan CPU.

- **Niceness tinggi (+19)** → Proses mendapat prioritas rendah (lebih sedikit CPU time).
- **Niceness rendah (-20)** → Proses mendapat prioritas tinggi (lebih banyak CPU time).

### Rentang Nilai Niceness:
- **Linux**: `-20` (prioritas tinggi) hingga `+19` (prioritas rendah).
- **FreeBSD**: `-20` hingga `+20`.

### Formula:
```plaintext
priority_value = 20 + nice_value
```

- Nilai prioritas sistem Linux berkisar dari `0` hingga `139`.
  - `0-99` → Prioritas real-time.
  - `100-139` → Prioritas user-space.

---

## 2. Menggunakan `nice` untuk Menjalankan Proses dengan Prioritas Tertentu
`nice` digunakan untuk memulai proses dengan nilai niceness tertentu.

### Sintaks:
```bash
nice -n [nice_value] [command]
```

### Contoh:
Menjalankan `infinite.sh` dengan niceness `10` (prioritas rendah):
```bash
nice -n 10 sh infinite.sh &
```
> Jika tidak menggunakan `-n`, default `nice_value` adalah `10`.

---

## 3. Menggunakan `renice` untuk Mengubah Prioritas Proses yang Sedang Berjalan
Jika sebuah proses sudah berjalan, kita bisa mengubah niceness-nya menggunakan `renice`.

### Sintaks:
```bash
renice -n [nice_value] -p [PID]
```

### Contoh:
Mengubah niceness proses dengan PID `1234` menjadi `10`:
```bash
renice -n 10 -p 1234
```

### Catatan:
- **Hanya root** yang bisa mengatur nilai niceness negatif (prioritas tinggi).
- **Pengguna biasa** hanya bisa meningkatkan niceness (menurunkan prioritas).

---

## 4. Penjelasan Mengubah Prioritas Proses dengan `nice` dan `renice` Berdasarkan Percobaan

### 1. Menjalankan Proses dengan `nice`
Pada percobaan pertama, dijalankan perintah berikut:
```bash
nice -n 10 sh infinite.sh &
```
Perintah ini bertujuan untuk menjalankan skrip `infinite.sh` dengan prioritas nice sebesar `10`.
Namun, terjadi error:
```plaintext
sh: 0: cannot open infinite.sh: No such file
```
Yang berarti skrip tidak ditemukan atau tidak ada di direktori kerja saat ini.

### 2. Menampilkan PID dan Prioritas Proses
Untuk mengecek apakah proses benar-benar berjalan, dilakukan pencarian menggunakan:
```bash
ps -o pid,ni,comm -p 2232
```
Namun, proses dengan PID `2232` sudah tidak ada, yang kemungkinan besar terjadi karena skrip tidak ditemukan dan proses gagal berjalan.

### 3. Mengecek Proses yang Sedang Berjalan
Dilakukan pengecekan menggunakan:
```bash
top
```
Dari hasil tampilan `top`, terlihat daftar proses yang sedang berjalan, dengan beberapa PID termasuk `2234` yang menjalankan perintah `top`.

### 4. Mencari Proses `infinite.sh` yang Berjalan
Karena `infinite.sh` tidak berjalan sebelumnya, dilakukan pencarian ulang menggunakan:
```bash
ps aux | grep infinite.sh
```
Hasilnya menunjukkan proses dengan PID `2447` sedang berjalan.

Untuk mengecek prioritasnya, digunakan:
```bash
ps -o pid,ni,comm -p 2447
```
Namun, hasilnya kosong, yang berarti proses mungkin telah berakhir atau tidak memiliki prioritas yang bisa ditampilkan.

### 5. Mengubah Prioritas dengan `renice`
Dicoba mengubah prioritas proses dengan:
```bash
renice -n 5 -p 2452
```
Hasilnya:
```plaintext
renice: failed to get priority for 2452 (process ID): No such process
```
Yang berarti proses `2452` tidak ditemukan.

Dicoba lagi dengan PID `2426`:
```bash
renice -n 5 -p 2426
```
Hasilnya menunjukkan perubahan prioritas dari `0` ke `5`, yang berarti perubahan berhasil dilakukan.

---

## 6. Kesimpulan
- `nice` digunakan saat pertama kali menjalankan proses, di mana bisa menentukan prioritas awalnya.
- `renice` digunakan untuk mengubah prioritas proses yang sedang berjalan, dengan syarat proses masih aktif.
- Percobaan awal dengan `nice` gagal karena skrip `infinite.sh` tidak ditemukan.
- `renice` berhasil mengubah prioritas proses dengan PID `2426` dari `0` ke `5`.
- Saat menggunakan `renice`, pastikan PID yang digunakan valid dan proses masih berjalan.

# Filesystem `/proc` di Linux

## Apa Itu `/proc`?
`/proc` adalah pseudo-filesystem yang digunakan oleh kernel Linux untuk menyediakan berbagai informasi sistem dalam bentuk file yang dapat diakses pengguna. Meskipun namanya `/proc`, filesystem ini tidak hanya berisi data tentang proses, tetapi juga statistik sistem lainnya.

## Struktur `/proc`
Setiap proses yang berjalan di sistem direpresentasikan sebagai direktori dengan nama sesuai **Process ID (PID)-nya**. Di dalam direktori tersebut terdapat berbagai file yang menyimpan informasi terkait proses, seperti:
- **`/proc/[PID]/cmdline`** → Command line yang digunakan untuk menjalankan proses.
- **`/proc/[PID]/environ`** → Variabel lingkungan yang digunakan oleh proses.
- **`/proc/[PID]/fd/`** → Direktori berisi symbolic link ke file descriptor yang dibuka oleh proses.
- **`/proc/[PID]/status`** → Informasi umum tentang status proses, seperti UID, GID, dan penggunaan memori.

## Contoh Penggunaan `/proc`
1. **Melihat command line proses tertentu**
   ```bash
   cat /proc/1234/cmdline

# Strace dan Truss

## Pengantar
`strace` di Linux dan `truss` di FreeBSD digunakan untuk menelusuri **system call** dan **sinyal** dari suatu proses. Perintah ini berguna untuk debugging atau memahami bagaimana suatu program berinteraksi dengan sistem.

## Instalasi
### Linux (strace)
```bash
sudo apt install strace  # Debian/Ubuntu
sudo yum install strace  # CentOS/RHEL
sudo dnf install strace  # Fedora
```

### FreeBSD (truss)
```bash
sudo pkg install truss
```

## Menggunakan `strace`
### Menjalankan Program dengan `strace`
Untuk melacak system call dari suatu perintah:
```bash
strace ls
```

### Melacak Proses Berjalan dengan PID
```bash
strace -p 5810
```
Output akan menunjukkan aktivitas proses, misalnya membaca waktu, membuka direktori `/proc`, atau mengakses file `/proc/1/stat` untuk mendapatkan informasi tentang proses init.

## Menggunakan `truss`
### Menjalankan Program dengan `truss`
```bash
truss ls
```

### Melacak Proses Berjalan dengan PID
```bash
truss -p 5810
```

## Opsi Tambahan
| Opsi       | Deskripsi |
|------------|------------------------------------------------|
| `-c`       | Menampilkan ringkasan jumlah dan waktu sistem call |
| `-o file`  | Menyimpan output ke file |
| `-e trace` | Menyaring jenis system call tertentu (misal `-e open,read`) |

## Contoh Output `strace`
```bash
open("/etc/ld.so.cache", O_RDONLY) = 3
open("/lib/x86_64-linux-gnu/libc.so.6", O_RDONLY) = 3
read(3, "\177ELF\002\001\001"..., 832) = 832
```

## Kesimpulan
- **`strace`** untuk Linux, **`truss`** untuk FreeBSD.
- Membantu dalam debugging dan analisis performa aplikasi.
- Dapat digunakan untuk melacak system call dari proses yang berjalan.
- Hati-hati saat digunakan pada sistem produksi, karena dapat memperlambat proses yang dipantau.

# Mengatasi Runaway Process di Linux

Runaway process adalah proses yang tidak merespons sistem dan menggunakan CPU secara berlebihan, menyebabkan komputer berjalan lambat atau tidak responsif.

## Berdasarkan percobaan yang dilakukan:

### 1. Menggunakan `lsof -p 547`
   - Perintah ini digunakan untuk melihat file yang dibuka oleh proses dengan **PID 547** (`dbus-daemon`).  
   - Hasilnya menunjukkan bahwa beberapa file tidak dapat diakses karena **"Permission denied"** (izin ditolak).

### 2. Menjalankan `df -h` untuk Melihat Penggunaan Disk
   - Perintah ini digunakan untuk melihat kapasitas penyimpanan yang tersedia dan terpakai pada sistem.  
   - Terlihat bahwa `/dev/sda1` memiliki total 19G dengan 6.1G digunakan, sedangkan `/storage` menggunakan `/dev/sda2`.

### 3. Menjalankan `du -ah / | sort -rh | head -10` untuk Melihat File Terbesar
   - Perintah ini digunakan untuk mencari file atau direktori terbesar di sistem.  
   - Namun, ada beberapa direktori di `/tmp/systemd-private-...` yang tidak dapat diakses karena keterbatasan izin.

### 4. Menjalankan `ps aux --sort=-%cpu | head -10` untuk Menampilkan Proses dengan Penggunaan CPU Tertinggi
   - Perintah ini menampilkan daftar proses yang sedang berjalan, diurutkan berdasarkan penggunaan CPU tertinggi.  
   - Hasilnya menunjukkan bahwa proses dengan **PID 2516** menggunakan **200% CPU**, yang kemungkinan besar adalah runaway process.  
   - Proses lain seperti `gnome-shell`, `ibus-daemon`, dan `fwupd` juga terlihat dalam daftar dengan penggunaan CPU yang lebih kecil.

### 5. Mencoba Menghentikan Proses `dbus-daemon` dengan `kill 547`
   - Perintah ini gagal dengan pesan **"Operation not permitted"**, yang berarti pengguna tidak memiliki izin untuk menghentikan proses ini.  
   - Kemungkinan, proses ini berjalan sebagai sistem atau pengguna lain, sehingga membutuhkan `sudo` untuk menghentikannya.

## **Kesimpulan:**
- Sistem mengalami proses yang menggunakan CPU secara berlebihan (`PID 2516`).  
- Proses `dbus-daemon` (`PID 547`) tidak dapat dihentikan karena keterbatasan izin.  
- Ada beberapa direktori di `/tmp/systemd-private-...` yang tidak bisa diakses.  
- Untuk menghentikan proses runaway, kemungkinan diperlukan perintah dengan `sudo`.  

# Strace and Truss

Untuk mengetahui aktivitas suatu proses, Anda bisa menggunakan `strace` di Linux atau `truss` di FreeBSD. Perintah ini menelusuri system call dan sinyal, berguna untuk debugging atau memahami cara kerja suatu program.

## Contoh penggunaan `strace` pada proses dengan PID 5810:

```bash
$ strace -p 5810
```

Outputnya menunjukkan proses `top` membaca waktu saat ini, membuka directory `/proc`, lalu mengakses file `/proc/1/stat` untuk mendapatkan informasi tentang proses `init`.

---

# Mengatasi Runaway Process di Linux

Runaway process adalah proses yang tidak merespons sistem dan menggunakan CPU secara berlebihan, menyebabkan komputer berjalan lambat atau tidak responsif.

## Berdasarkan percobaan yang dilakukan:

### 1. Menggunakan `lsof -p 547`
   - Perintah ini digunakan untuk melihat file yang dibuka oleh proses dengan **PID 547** (`dbus-daemon`).  
   - Hasilnya menunjukkan bahwa beberapa file tidak dapat diakses karena **"Permission denied"** (izin ditolak).

### 2. Menjalankan `df -h` untuk Melihat Penggunaan Disk
   - Perintah ini digunakan untuk melihat kapasitas penyimpanan yang tersedia dan terpakai pada sistem.  
   - Terlihat bahwa `/dev/sda1` memiliki total 19G dengan 6.1G digunakan, sedangkan `/storage` menggunakan `/dev/sda2`.

### 3. Menjalankan `du -ah / | sort -rh | head -10` untuk Melihat File Terbesar
   - Perintah ini digunakan untuk mencari file atau direktori terbesar di sistem.  
   - Namun, ada beberapa direktori di `/tmp/systemd-private-...` yang tidak dapat diakses karena keterbatasan izin.

### 4. Menjalankan `ps aux --sort=-%cpu | head -10` untuk Menampilkan Proses dengan Penggunaan CPU Tertinggi
   - Perintah ini menampilkan daftar proses yang sedang berjalan, diurutkan berdasarkan penggunaan CPU tertinggi.  
   - Hasilnya menunjukkan bahwa proses dengan **PID 2516** menggunakan **200% CPU**, yang kemungkinan besar adalah runaway process.  
   - Proses lain seperti `gnome-shell`, `ibus-daemon`, dan `fwupd` juga terlihat dalam daftar dengan penggunaan CPU yang lebih kecil.

### 5. Mencoba Menghentikan Proses `dbus-daemon` dengan `kill 547`
   - Perintah ini gagal dengan pesan **"Operation not permitted"**, yang berarti pengguna tidak memiliki izin untuk menghentikan proses ini.  
   - Kemungkinan, proses ini berjalan sebagai sistem atau pengguna lain, sehingga membutuhkan `sudo` untuk menghentikannya.

## **Kesimpulan:**
- Sistem mengalami proses yang menggunakan CPU secara berlebihan (`PID 2516`).  
- Proses `dbus-daemon` (`PID 547`) tidak dapat dihentikan karena keterbatasan izin.  
- Ada beberapa direktori di `/tmp/systemd-private-...` yang tidak bisa diakses.  
- Untuk menghentikan proses runaway, kemungkinan diperlukan perintah dengan `sudo`.  

---

# Proses Periodik dan Cron di Linux

Cron adalah daemon yang menjalankan perintah pada jadwal yang telah ditentukan. Cron dimulai saat sistem booting dan berjalan terus selama sistem aktif.

## Format Crontab

Crontab adalah file konfigurasi yang berisi jadwal eksekusi perintah. Formatnya:

```plaintext
*     *     *     *     *  perintah
-     -     -     -     -
|     |     |     |     |
|     |     |     |     +---- hari dalam seminggu (0-6, Minggu=0)
|     |     |     +-------- bulan (1-12)
|     |     +---------- tanggal (1-31)
|     +------------ jam (0-23)
+-------------- menit (0-59)
```

## Percobaan:

### 1. Cek Status Cron
Pengguna menjalankan perintah berikut untuk mengecek status layanan cron:

```bash
systemctl status cron
```

Hasilnya menunjukkan bahwa layanan cron aktif dan berjalan (`active (running)`). Ini berarti cron siap menjalankan tugas yang dijadwalkan.

### 2. Membuka Crontab
Pengguna mencoba mengedit crontab dengan perintah:

```bash
crontab -e
```

Namun, karena belum ada crontab untuk pengguna `alfani26`, sistem membuat crontab kosong.

### 3. Pemilihan Editor
Sistem menawarkan dua pilihan editor:
- **Nano** (lebih mudah digunakan, default)
- **Vim Tiny**

Pengguna memilih `1` (Nano).

### 4. Pengeditan Crontab
Setelah memilih editor, pengguna kembali menjalankan `crontab -e`.
Sistem mengonfirmasi bahwa crontab baru sedang dibuat dengan pesan:

```plaintext
crontab: installing new crontab
```
# Systemd Timer vs Cron Jobs

## **Pengantar**
**Systemd Timer** adalah unit konfigurasi dalam sistem Linux yang namanya berakhiran `.timer`. Systemd timers dapat digunakan sebagai alternatif dari cron jobs dengan fleksibilitas dan kontrol yang lebih besar.

## **Cara Kerja Systemd Timer**
- Unit **timer** mengaktifkan unit **service** pada waktu yang ditentukan.
- Dapat dijalankan berdasarkan waktu tertentu, saat sistem boot, atau berdasarkan suatu kejadian.
- Untuk melihat daftar timer aktif, gunakan perintah berikut:

```bash
systemctl list-timers
```

## **Contoh Systemd Timer**
Unit `logrotate.timer` menjalankan `logrotate.service` setiap tengah malam:

```ini
[Timer]
OnCalendar=daily
AccuracySec=1h
Persistent=true
```

### **Penjelasan Konfigurasi**
- `OnCalendar=daily` → Menjadwalkan eksekusi setiap hari.
- `AccuracySec=1h` → Memberikan toleransi waktu eksekusi hingga 1 jam.
- `Persistent=true` → Menjalankan tugas yang terlewat saat sistem mati.

## **Penggunaan Umum Systemd Timer & Cron Jobs**
1. **Mengirim Email** → Mengirim laporan otomatis melalui email.
2. **Membersihkan Filesystem** → Menghapus file lama di direktori sampah setiap hari.
3. **Rotasi Log** → Membagi log berdasarkan ukuran atau tanggal agar lebih terorganisir.
4. **Menjalankan Batch Jobs** → Memproses antrian pesan atau data dalam jumlah besar.
5. **Backup & Mirroring** → Menyalin direktori ke sistem lain secara otomatis menggunakan `rsync`.

## **Perbedaan Systemd Timer dan Cron Jobs**
| Fitur | Systemd Timer | Cron Jobs |
|-------|--------------|-----------|
| Fleksibilitas | Tinggi (bisa berdasarkan event) | Terbatas pada waktu |
| Logging | Dapat dipantau melalui `journalctl` | Harus dikonfigurasi manual |
| Pemulihan | Bisa menjalankan tugas yang terlewat | Tidak mendukung |

## **Kesimpulan**
Systemd timers lebih fleksibel dibandingkan cron, mendukung eksekusi berbasis kejadian, serta memiliki fitur pemulihan tugas yang terlewat.
