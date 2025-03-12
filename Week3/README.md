<div align="center">
    <h1 style="text-align: center;font-weight: bold">Laporan Workshop Administrasi Jaringan<br></h1>
    <h2 style="text-align: center;">Instalasi Debian <br></h2>
    <h4 style="text-align: center;">Dosen Pengampu : Dr. Ferry Astika Saputra, S.T., M.Sc.</h4>
</div>
<br />
<div align="center">
    <img src="assets/Logo_PENS.png" alt="Logo PENS">
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

![Install NTP Client](image/1.png)

### 2. Nama NTP Server
Nama NTP server yang harus dirujuk adalah NTP server Indonesia.

Referensi:
- [Server World - Debian 12 NTP](https://www.server-world.info/en/note?os=Debian_12&p=ntp&f=1)
- [NTP Pool - Indonesia](https://www.ntppool.org/en/zone/id)

**Package terkait:** `ntp`, `ntpssec`

![NTP Server](image/2.png)
![Konfigurasi NTP](image/3.png)
![NTP Status](image/4.png)

### 3. Periksa Sinkronisasi

![Cek Sinkronisasi](image/5.png)

---

## B. Instalasi dan Konfigurasi Samba

### 1. Membuat Public Shared Folder
Folder ini harus bisa diakses melalui Windows Client dan Linux Client via file manager.

#### Instalasi Samba

![Install Samba](image/6.png)

#### Percobaan Fully Access dan Konfigurasi Samba

![Konfigurasi Samba](image/7.png)

#### Membuat File di Direktori `/home/share`

![Buat File](image/8.png)
![Cek File](image/9.png)
![Verifikasi File](image/10.png)

### 2. Membuat Limited Shared Folder

#### Konfigurasi Samba untuk Limited Share

![Konfigurasi Limited Share](image/11.png)

#### Membuat File di Direktori `/home/share01`

![Buat File Limited Share](image/12.png)

### 3. Menambahkan User Baru

![Menambahkan User](image/13.png)

### 4. Menambahkan User Baru ke Grup `fanigroup`

![Tambah User ke Grup](image/14.png)

### 5. Menambahkan User Valid ke Samba

![Tambah User Valid Samba](image/15.png)

### 6. Akses ke Folder Share dari CLI Client
Referensi:
- [Server World - Debian 12 Samba](https://www.server-world.info/en/note?os=Debian_12&p=samba&f=1)

![Akses dari CLI](image/16.png)

**Package terkait:** `samba`, `smbclient`, `cifs-tools`

---

## C. Rangkuman Package Management

### Package Management
Package management adalah sistem yang mengatur instalasi, pemutakhiran, konfigurasi, dan penghapusan paket perangkat lunak pada sistem operasi Debian. Sistem ini dapat digunakan melalui antarmuka grafis (GUI) maupun baris perintah (CLI).

### Advanced Package Tool (APT)
APT (Advanced Package Tool) adalah package manager berbasis CLI yang umum digunakan dalam sistem Debian. APT memungkinkan pengguna untuk mencari, menginstal, memperbarui, dan menghapus paket perangkat lunak secara efisien.

#### 1. Perintah untuk Mencari dan Menampilkan Informasi Paket

![Mencari Paket](image/17.png)

#### 2. Perintah untuk Mengelola Paket dalam Sistem
Perintah ini hanya dapat dijalankan oleh administrator sistem dengan mode root (`su -`).

![Kelola Paket](image/18.png)
![APT Update](image/19.png)

### Software (Simplified Package Manager)
Software adalah sekumpulan perintah atau instruksi yang menjalankan program komputer.

![Software Manager](image/20.png)

### Discover: KDE Package Manager
Discover adalah pusat perangkat lunak KDE yang menyediakan antarmuka grafis untuk mengelola aplikasi dan pembaruan sistem.

![Discover](image/21.png)

Discover mendukung berbagai backend, termasuk PackageKit, Flatpak, dan Snap, memungkinkan integrasi dengan berbagai sistem manajemen paket. Dengan menggunakan teknologi Kirigami, Discover menawarkan antarmuka yang responsif dan adaptif untuk berbagai perangkat.

### Synaptic: Pengelola Paket yang Komprehensif
Synaptic adalah antarmuka grafis dari manajer paket Debian yang memberikan tampilan menyeluruh dari semua paket yang tersedia.

![Synaptic](image/22.png)
![Synaptic Interface](image/23.png)

**Fitur utama Synaptic:**
- Memiliki fungsi yang sama seperti `apt`.
- Memerlukan kata sandi administrator untuk digunakan.
- Koneksi internet aktif diperlukan untuk instalasi atau pembaruan perangkat lunak.
