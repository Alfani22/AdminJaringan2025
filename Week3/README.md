<div align="center">
    <h1 style="text-align: center;font-weight: bold">Laporan Workshop Administrasi Jaringan<br></h1>
    <h2 style="text-align: center;">Instalasi Debian <br></h2>
    <h4 style="text-align: center;">Dosen Pengampu : Dr. Ferry Astika Saputra, S.T., M.Sc.</h4>
</div>
<br />
<div align="center">
    <img src="images/Logo_PENS.jpg.png" alt="Logo PENS">
    <h3 style="text-align: center;">Disusun Oleh :</h3>
    <p style="text-align: center;">
        <strong>Marieta Nona Alfani (3123500025)</strong>
    </p>
    <h3 style="text-align: center;line-height: 1.5">Politeknik Elektronika Negeri Surabaya<br>Departemen Teknik Informatika Dan Komputer<br>Program Studi Teknik Informatika<br>2025/2026</h3>
    <hr>
</div>
<br>

# Laporan Instalasi dan Konfigurasi NTP Client dan Samba

## A. Instalasi NTP Client

### 1. Install dan Konfigurasi NTP Client
Instal dan konfigurasi NTP client agar host memiliki waktu yang sinkron dengan NTP server di Indonesia.

<img src="image/1.png" alt="Install NTP Client">

### 2. Nama NTP Server
Nama NTP server yang harus dirujuk adalah NTP server Indonesia.

Referensi:
- [Server World - Debian 12 NTP](https://www.server-world.info/en/note?os=Debian_12&p=ntp&f=1)
- [NTP Pool - Indonesia](https://www.ntppool.org/en/zone/id)

**Package terkait:** `ntp`, `ntpssec`

<img src="image/2.png" alt="NTP Server">
<img src="image/3.png" alt="Konfigurasi NTP">
<img src="image/4.png" alt="NTP Status">

### 3. Periksa Sinkronisasi

<img src="image/5.png" alt="Cek Sinkronisasi">

---

## B. Instalasi dan Konfigurasi Samba

### 1. Membuat Public Shared Folder
Folder ini harus bisa diakses melalui Windows Client dan Linux Client via file manager.

#### Instalasi Samba

<img src="image/6.png" alt="Install Samba">

#### Percobaan Fully Access dan Konfigurasi Samba

<img src="image/7.png" alt="Konfigurasi Samba">

#### Membuat File di Direktori `/home/share`

<img src="image/8.png" alt="Buat File">
<img src="image/9.png" alt="Cek File">
<img src="image/10.png" alt="Verifikasi File">

### 2. Membuat Limited Shared Folder

#### Konfigurasi Samba untuk Limited Share

<img src="image/11.png" alt="Konfigurasi Limited Share">

#### Membuat File di Direktori `/home/share01`

<img src="image/12.png" alt="Buat File Limited Share">

### 3. Menambahkan User Baru

<img src="image/13.png" alt="Menambahkan User">

### 4. Menambahkan User Baru ke Grup `fanigroup`

<img src="image/14.png" alt="Tambah User ke Grup">

### 5. Menambahkan User Valid ke Samba

<img src="image/15.png" alt="Tambah User Valid Samba">

### 6. Akses ke Folder Share dari CLI Client
Referensi:
- [Server World - Debian 12 Samba](https://www.server-world.info/en/note?os=Debian_12&p=samba&f=1)

<img src="image/16.png" alt="Akses dari CLI">

**Package terkait:** `samba`, `smbclient`, `cifs-tools`

---

## C. Rangkuman Package Management

### Package Management
Package management adalah sistem yang mengatur instalasi, pemutakhiran, konfigurasi, dan penghapusan paket perangkat lunak pada sistem operasi Debian. Sistem ini dapat digunakan melalui antarmuka grafis (GUI) maupun baris perintah (CLI).

### Advanced Package Tool (APT)
APT (Advanced Package Tool) adalah package manager berbasis CLI yang umum digunakan dalam sistem Debian. APT memungkinkan pengguna untuk mencari, menginstal, memperbarui, dan menghapus paket perangkat lunak secara efisien.

#### 1. Perintah untuk Mencari dan Menampilkan Informasi Paket

<img src="image/17.png" alt="Mencari Paket">

#### 2. Perintah untuk Mengelola Paket dalam Sistem
Perintah ini hanya dapat dijalankan oleh administrator sistem dengan mode root (`su -`).

<img src="image/18.png" alt="Kelola Paket">
<img src="image/19.png" alt="APT Update">

### Software (Simplified Package Manager)
Software adalah sekumpulan perintah atau instruksi yang menjalankan program komputer.

<img src="image/20.png" alt="Software Manager">

### Discover: KDE Package Manager
Discover adalah pusat perangkat lunak KDE yang menyediakan antarmuka grafis untuk mengelola aplikasi dan pembaruan sistem.

<img src="image/21.png" alt="Discover">

Discover mendukung berbagai backend, termasuk PackageKit, Flatpak, dan Snap, memungkinkan integrasi dengan berbagai sistem manajemen paket. Dengan menggunakan teknologi Kirigami, Discover menawarkan antarmuka yang responsif dan adaptif untuk berbagai perangkat.

### Synaptic: Pengelola Paket yang Komprehensif
Synaptic adalah antarmuka grafis dari manajer paket Debian yang memberikan tampilan menyeluruh dari semua paket yang tersedia.

<img src="image/22.png" alt="Synaptic">
<img src="image/23.png" alt="Synaptic Interface">

**Fitur utama Synaptic:**
- Memiliki fungsi yang sama seperti `apt`.
- Memerlukan kata sandi administrator untuk digunakan.
- Koneksi internet aktif diperlukan untuk instalasi atau pembaruan perangkat lunak.
