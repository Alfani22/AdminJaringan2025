<div align="center">
  <h1 style="text-align: center;font-weight: bold">Laporan<br>Workshop Administrasi Jaringan</h1>
  <h4 style="text-align: center;">Dosen Pengampu : Dr. Ferry Astika Saputra, S.T., M.Sc.</h4>
</div>
<br />
<div align="center">
  <img src="https://upload.wikimedia.org/wikipedia/id/4/44/Logo_PENS.png" alt="Logo PENS">
  <h3 style="text-align: center;">Disusun Oleh :</h3>
  <p style="text-align: center;">
    <strong>Marieta Nona Alfani</strong><br>
    <strong>3123500026</strong>
  </p>

<h3 style="text-align: center;line-height: 1.5">Politeknik Elektronika Negeri Surabaya<br>Departemen Teknik Informatika Dan Komputer<br>Program Studi Teknik Informatika<br>2024/2025</h3>
  <hr><hr>
</div>

## Daftar Isi

1. [Tugas](#tugas)
2. [Hasil Percobaan](#hasil-percobaan)

## Tugas 

![App Screenshot](img/tugas.png)

1. Melakukan Instalisasi Winbox 
2. Konfigurasi Layer Network
   Percobaan awal yang dilakukan berdasarkan skema di atas adalah menghubungkan perangkat MikroTik antar kelompok. Tujuan dari langkah ini adalah agar setiap laptop yang tergabung dalam jaringan LAN dapat saling berkomunikasi, khususnya melakukan uji koneksi (ping) ke IP kelompok lainnya. 
   
   Untuk melakukan hal tersebut, salah satu laptop dari setiap kelompok perlu dihubungkan ke jaringan LAN dan menjalankan perintah ping guna menguji konektivitas. Pada percobaan ini, digunakan rentang alamat IP 10.252.108.5x, di mana x mewakili nomor kelompok masing-masing. Karena saya kelompok 3, maka IP MikroTik yang digunakan adalah 10.252.108.53. 
   
   Setelah perangkat berhasil terhubung ke jaringan melalui kabel LAN, pengujian koneksi dapat dilakukan melalui Command Prompt di sistem operasi Windows dengan menggunakan perintah ping terhadap IP tujuan.
3. Ping device kelompok lain sesuai networknya

## Hasil Percobaan

1. Install Wine
   
   ![App Screenshot](img/install_wine.png)

   Instalasi ini dibutuhkan agar aplikasi Winbox, yang hanya tersedia untuk Windows, bisa dijalankan di Linux. Untuk itu, digunakan Wine, sebuah tool yang memungkinkan aplikasi Windows berjalan di sistem operasi Linux tanpa perlu emulator penuh atau virtual machine.

2. Install Winbox melalui SFTP – Kelompok 5

   ![App Screenshot](img/sftp_10_252_108_110.png)

   ![App Screenshot](img/get_winbox.png)

3. Mencoba ping network 10.252.108.53

   ![App Screenshot](img/ping.png)

   Berdasarkan hasil pengujian, jaringan milik Kelompok 3 berhasil terdeteksi saat perangkat terhubung ke jaringan LAN. Alamat IP yang teridentifikasi adalah 10.252.108.53.

4. Hubungkan Winbox ke Mikrotik menggunakan IP address 10.252.108.53.
    
   ![App Screenshot](img/connect_network_winbox_kel3.png)

   ![App Screenshot](img/berhasil_connect.png)

5. Lakukan pengujian koneksi (ping) ke jaringan 192.168.1.0/24, yang dimiliki oleh Kelompok 1.

   ![App Screenshot](img/ping_kel_7.png)

   Untuk melakukan routing, buka menu IP > Routes pada Winbox. Tambahkan entri baru dengan destination network: 192.168.1.0/24, lalu isi kolom gateway dengan IP address 10.252.108.51 (dalam contoh ini, gateway merujuk pada IP milik Kelompok 7).

Setelah konfigurasi selesai, klik tombol OK untuk menyimpan perubahan.

Agar tidak membingungkan, sembunyikan IP yang tidak terhubung langsung ke perangkat Mikrotik. Setelah itu, lakukan pengujian koneksi dengan menjalankan perintah ping 10.252.108.51 melalui Command Prompt.

Sebagai referensi, gambar di bawah menunjukkan hasil ping yang berhasil menuju jaringan 192.168.1.0/24.

   ![App Screenshot](img/hasil_ping_kel_1.png)

  Jika pengujian ping seperti pada langkah sebelumnya berhasil dilakukan, dan setiap koneksi tidak menunjukkan status Request Timed Out atau pesan kesalahan lainnya, maka dapat disimpulkan bahwa koneksi antar perangkat dari kelompok yang berbeda telah berhasil terjalin dengan baik.
   
