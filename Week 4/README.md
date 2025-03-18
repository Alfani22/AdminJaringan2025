<div align="center">
    <h1 style="text-align: center;font-weight: bold">Tugas Review Deskripsi<br>Workshop Administrasi Jaringan</h1>
    <h4 style="text-align: center;">Dosen Pengampu : Dr. Ferry Astika Saputra, S.T., M.Sc.</h4>
</div>
<br />
<div align="center">
    <img src="Week4/Assets/Logo_PENS.png" alt="Logo PENS">
    <h3 style="text-align: center;">Disusun Oleh : </h3>
    <p style="text-align: center;">
        <strong>Marieta Nona Alfani</strong><br>
        <strong>3123500026 / 2 D3 IT A</strong><br>
    </p>

<h3>Politeknik Elektronika Negeri Surabaya<br>Departemen Teknik
Informatika Dan Komputer<br>Program Studi Teknik Informatika<br>2024/2025</h3>
    <hr>
    <hr>
</div>

# Konfigurasi BIND untuk Internal Network dan Zone Files

## A. Configure for Internal Network

### 1. Instal BIND
<img src="Week4/Assets/1.png" alt="image1.png">

### 2. Tambahkan Konfigurasi Internal
<img src="Week4/Assets/2.png" alt="image2.png">
<img src="Week4/Assets/3.png" alt="image3.png">
**Edit konfigurasi opsi:**
<img src="Week4/Assets/4.png" alt="image4.png">

### 3. Tambahkan Zona Internal
<img src="Week 4/Assets/5.png" alt="image5.png">
<img src="Week 4/Assets/6.png" alt="image6.png">

### 4. Konfigurasi Penggunaan IPv4
<img src="Week 4/Assets/7.png" alt="image7.png">
<img src="Week 4/Assets/8.png" alt="image8.png">

## B. Configure Zone Files

### 1. Buat Zone Files untuk Resolusi Forward (Domain ke IP)
<img src="Week 4/Assets/9.png" alt="image9.png">
<img src="Week 4/Assets/10.png" alt="image10.png">

### 2. Buat Zone Files untuk Resolusi Reverse (IP ke Domain)
<img src="Week 4/Assets/11.png" alt="image11.png">
<img src="Week 4/Assets/12.png" alt="image12.png">

## C. BIND Verify Resolution

### 1. Restart BIND untuk Menerapkan Perubahan
<img src="Week 4/Assets/13.png" alt="image13.png">

### 2. Ubah Pengaturan DNS ke Server Sendiri
<img src="Week 4/Assets/14.png" alt="image14.png">
<img src="Week 4/Assets/15.png" alt="image15.png">

### 3. Verifikasi Resolusi Nama dan Alamat
Gunakan perintah `dig` untuk menguji apakah domain dapat di-resolve ke alamat IP:

#### (a) Verifikasi Resolusi Nama ke IP
<img src="Week 4/Assets/16.png" alt="image16.png">

**Error:** "communications error to 10.0.0.30#53 timed out" menunjukkan bahwa sistem tidak bisa berkomunikasi dengan server DNS pada 10.0.0.30.

### Langkah-Langkah Troubleshooting
1. **Pastikan BIND9 berjalan di server**  
   <img src="Week 4/Assets/17.png" alt="image17.png">

2. **Pastikan Port 53 tidak diblokir**  
   <img src="Week 4/Assets/18.png" alt="image18.png">

3. **Cek konektivitas jaringan**  
   <img src="Week 4/Assets/19.png" alt="image19.png">
   
   **Coba ping server DNS:**  
   <img src="Week 4/Assets/20.png" alt="image20.png">

4. **Pastikan konfigurasi BIND9 benar**  
   **Edit file named.conf.options:**  
   <img src="Week 4/Assets/21.png" alt="image21.png">
   <img src="Week 4/Assets/22.png" alt="image22.png">

**Ulang Verifikasi Resolusi Nama ke IP:**  
<img src="Week 4/Assets/22.png" alt="image23.png">

Dari hasil `dig dlp.kelompok3.home`, terlihat bahwa status yang dikembalikan adalah **NXDOMAIN**, yang berarti nama domain tersebut tidak ditemukan dalam server DNS yang digunakan.

### Penyebab Kemungkinan & Solusi:
1. **Zona Tidak Terdefinisi di Bind9**
   Pastikan sudah menambahkan zona untuk `kelompok3.home` di file konfigurasi Bind9 (`named.conf.local` atau `named.conf.default-zones`).
   <img src="Week 4/Assets/23.png" alt="image24.png">
   <img src="Week 4/Assets/24.png" alt="image25.png">

2. **File Zona Tidak Ada atau Salah Format**
   Pastikan file zona `/etc/bind/db.kelompok3.home` ada dan berisi konfigurasi yang benar.
   <img src="Week 4/Assets/25.png" alt="image26.png">

3. **Restart Bind9 Setelah Perubahan**
   <img src="Week 4/Assets/26.png" alt="image27.png">

4. **Uji Query Kembali**
   **(a) Verifikasi Resolusi Nama ke IP**  
   <img src="Week 4/Assets/27.png" alt="image28.png">

   **Penjelasan hasil output:**
   - **Status NOERROR** → Domain `dlp.kelompok3.home` berhasil ditemukan oleh server DNS.
   - **ANSWER SECTION** → `dlp.kelompok3.home` memiliki alamat IP `36.86.63.182`, artinya server DNS telah menyelesaikan pencarian domain dengan benar.
   - **AUTHORITY SECTION** → Ada informasi SOA (Start of Authority), menandakan bahwa server DNS bertanggung jawab atas domain ini.
   - **Server DNS yang Digunakan** → Query dilakukan ke `10.0.2.3`, yang merupakan server DNS lokal.

   **(b) Verifikasi Resolusi IP ke Nama**
   <img src="Week 4/Assets/28.png" alt="image29.png">

   **Penjelasan hasil output:**
   1. **Header:**
      - `dig` dijalankan menggunakan versi **DiG 9.18.33-1** pada sistem Debian.
      - **Status NOERROR**, artinya permintaan berhasil diproses tanpa kesalahan.
      - ID kueri: 33936 (ID unik untuk permintaan ini).
   2. **QUESTION SECTION:**
      - `30.0.0.10.in-addr.arpa. IN PTR`
      - Ini menunjukkan bahwa permintaan adalah reverse lookup untuk IP `10.0.0.30`.
   3. **ANSWER SECTION:**
      - `30.0.0.10.in-addr.arpa. 300 IN A 36.86.63.182`
        - Ini berarti alamat IP privat `10.0.0.30` dipetakan ke alamat IP publik `36.86.63.182`.
        - Ini biasanya menunjukkan bahwa `10.0.0.30` diterjemahkan melalui NAT ke IP publik.
   4. **AUTHORITY SECTION:**
      - `prisoner.iana.org` adalah server yang menangani domain `.arpa`, yang digunakan untuk DNS reverse lookup.

---

**Kesimpulan:** Dengan mengikuti langkah-langkah di atas, konfigurasi BIND dapat digunakan untuk resolusi nama domain ke IP dan sebaliknya dengan troubleshooting yang sesuai.


