<div align="center">
  <h1 style="text-align: center;font-weight: bold">LAPORAN RESMI<br>WORKSHOP ADMINISTRASI JARINGAN</h1>
  <h4 style="text-align: center;">Dosen Pengampu : Dr. Ferry Astika Saputra, S.T., M.Sc.</h4>
</div>
<br />
<div align="center">
  <img src="https://upload.wikimedia.org/wikipedia/id/4/44/Logo_PENS.png" alt="Logo PENS">
  <h3 style="text-align: center;">Disusun Oleh : </h3>
  <p style="text-align: center;">
    <strong>Marieta Nona Alfani (3123500026) </strong><br>
  </p>
<h3 style="text-align: center;line-height: 1.5">Politeknik Elektronika Negeri Surabaya<br>Departemen Teknik Informatika Dan Komputer<br>Program Studi Teknik Informatika<br>2024/2025</h3>
  <hr><hr>
</div>

## Daftar Isi
- [Daftar Isi](#daftar-isi)
- [Instalasi Virtual Machine No-GUI (VM 1)](#instalasi-virtual-machine-no-gui-vm-1)
- [Konfigurasi Virtual Machine No-GUI (VM 1)](#konfigurasi-virtual-machine-no-gui-vm-1)
- [Instalasi NTP Client](#instalasi-ntp-client)
- [Instalasi Samba](#instalasi-samba)
- [Instalasi Bind9 (DNS)](#instalasi-bind9-dns)

## Instalasi Virtual Machine No-GUI (VM 1)
Langkah 1:<br>
Unduh file ZIP virtual machine no-GUI di `https://drive.google.com/drive/folders/1Rnv4prB4BbsOebonX2GTHu6f1k6oQEZ4`.
<br>Gambar:
<br><div style=width:500;>![ss](assets/insng1.png)</div>

Langkah 2:<br>
Ekstrak file ZIP yang telah diunduh, dan masuk ke dalam folder debian12-10-nogui > debidora-nogui.
<br>Gambar:
<br><div style=width:500;>![ss](assets/insng2.png)</div>
<br><div style=width:500;>![ss](assets/insng3.png)</div>
<br><div style=width:500;>![ss](assets/insng4.png)</div>

Langkah 3:<br>
Buka file debidora-nogui yang memiliki ikon biru, lalu virtual box akan otomatis akan terbuka dan virtual machine no-GUI akan terbuat.
<br>Gambar:
<br><div style=width:500;>![ss](assets/insng5.png)</div>
<br><div style=width:500;>![ss](assets/insng6.png)</div>

Langkah 4:<br>
Tekan settings dengan memilih debidora-nogui, lalu menuju menu network. Ubah adapter 1 menjadi bridged network dan adapter 2 menjadi internal network. Tekan OK untuk menyimpan perubahan.
<br>Gambar:
<br><div style=width:500;>![ss](assets/insng7.png)</div>
<br><div style=width:500;>![ss](assets/insng8.png)</div>
<br><div style=width:500;>![ss](assets/insng9.png)</div>

## Konfigurasi Virtual Machine No-GUI (VM 1)
Langkah 1:<br>
Login dengan menggunakan student sebagai username dan password.
<br>Gambar:
<br><div style=width:500;>![ss](assets/confng1.png)</div>

Langkah 2:<br>
Jalankan perintah `ip a` untuk melihat ip address dan ambil ip dari interface enp0s3. Buka terminal atau command prompt pada window host, jalankan command ssh student@\[IP-Address-enp0s3] dan login seperti dalam VM untuk menggunakan VM no-GUI dalam terminal windows.
<br>Gambar:
<br><div style=width:500;>![ss](assets/confng2.png)</div>
<br><div style=width:500;>![ss](assets/confng3.png)</div>

Langkah 3:<br>
Setelah itu, Konfigurasikan jaringan dan cek dengan menggunakan perintah `nano -l -w /etc/network/interfaces`.
<br>Gambar:
<br><div style=width:500;>![ss](assets/confng4.png)</div>
<br><div style=width:500;>![ss](assets/confng5.png)</div>

Langkah 4:<br>
Konfigurasi `sysctl.conf` untuk menghapus comment pada bagian `net.ipv4.ip_forward=1` dengan menggunakan perintah `nano -l -w /etc/sysctl.conf`.
<br>Gambar:
<br><div style=width:500;>![ss](assets/confng6.png)</div>
<br><div style=width:500;>![ss](assets/confng7.png)</div>

Langkah 5:<br>
Unduh package `iptables` dan `iptables-persistent` dengan menggunakan perintah `sudo apt-get install iptables iptables-persistent`.
<br>Gambar:
<br><div style=width:500;>![ss](assets/confng8.png)</div>

Langkah 6:<br>
tambahkan baris kode berikut kedalam `/etc/iptables/rules.v4` menggunakan perintah `sudo nano -l -w /etc/iptables/rules.v4`.
```bash
*nat
 -A POSTROUTING -o enp0s3 -j MASQUERADE
 COMMIT

 *filter
 -A INPUT -i lo -j ACCEPT
 # allow ssh, so that we do not lock ourselves
 -A INPUT -i enp0s3 -p tcp -m tcp --dport 22 -j ACCEPT
 # allow incoming traffic to the outgoing connections,
 # et al for clients from the private network
 -A INPUT -m state --state RELATED,ESTABLISHED -j ACCEPT
 # prohibit everything else incoming
 -A INPUT -i enp0s3 -j DROP
 COMMIT
```
<br>Gambar:
<br><div style=width:500;>![ss](assets/confng9.png)</div>
<br><div style=width:500;>![ss](assets/confng10.png)</div>

Langkah 7:<br>
Jalankan perintah `sudo iptables-restore < /etc/iptables/rules.v4` untuk mengembalikan aturan-aturan iptables dari file konfigurasi yang telah disimpan sebelumnya.
<br>Gambar:
<br><div style=width:500;>![ss](assets/confng11.png)</div>

Langkah 8:
Lakukan reboot VM dengan menggunakan perintah `sudo reboot`.
<br>Gambar:
<br><div style=width:500;>![ss](assets/confng12.png)</div>

## Instalasi NTP Client
Langkah 1:
Instalasi NTP Client menggunakan perintah `sudo apt -y install ntpsec`.
<br>Gambar:
<br><div style=width:500;>![ss](assets/ntpng1.png)</div>

Langkah 2:
Setelah selesai instalasi, gunakan perintah `sudo nano /etc/ntpsec/ntp.conf` untuk mengakses file konfigurasi NTP Client.
<br>Gambar:
<br><div style=width:500;>![ss](assets/ntpng2.png)</div>

Langkah 3:
Pada dalam ntp.conf, beri komen (#) pada pool ke empat pool yang ada dan masukkan server Indonesia yang dapat disalin dari web `https://www.ntppool.org/en/zone/id`.
<br>Gambar:
<br><div style=width:500;>![ss](assets/ntpng3.png)</div>

Langkah 4:
Setelah konfigurasi, lakukan restart untuk NTP Client untuk menggunakan konfigurasi yang baru dengan perintah `sudo systemctl restart ntpsec`.
<br>Gambar:
<br><div style=width:500;>![ss](assets/ntpng4.png)</div>
 
Langkah 5:
Untuk melihat jika sudah tersambung dengan server, gunakan perintah `ntpq -p`.
<br>Gambar:
 <br><div style=width:500;>![ss](assets/ntpng5.png)</div>

Langkah 6:
Periksa waktu NTP Client dengan menggunakan perintah `sudo systemctl status ntpsec`.
<br>Gambar:
<br><div style=width:500;>![ss](assets/ntpng6.png)</div>

## Instalasi Samba
Langkah 1:<br>
unduh package samba dengan menggunakan perintah `sudo apt -y install samba`.
<br>Gambar:
<br><div style=width:500;>![ss](assets/smbng1.png)</div>

Langkah 2:<br>
Membuat direktori dengan menggunaakn perintah `sudo mkdir /home/public`.
<br>Gambar:
<br><div style=width:500;>![ss](assets/smbng2.png)</div>

Langkah 3:<br>
Merubah akses agar semua dapat write, read, dan excute dengan perintah `sudo chmod 777 /home/public`.
<br>Gambar:
<br><div style=width:500;>![ss](assets/smbng3.png)</div>

Langkah 4:<br>
Konfigurasi file samba menggunakan perintah `sudo nano /etc/samba/smb.conf`.
<br>Gambar:
<br><div style=width:500;>![ss](assets/smbng4.png)</div>

Langkah 5:<br>
Menambahkan `unix charset = UTF-8` untuk kompatibilitas dengan sistem yang mendukung pengkodean UTF-8.
<br>Gambar:
<br><div style=width:500;>![ss](assets/smbng5.png)</div>

Langkah 6:<br>
Menambahkan interface sesuai dengan alamat IP yang akan digunakan.
<br>Gambar:
<br><div style=width:500;>![ss](assets/smbng6.png)</div>

Langkah 7:<br>
Menambahkan konfigurasi folder public.
<br>Gambar:
<br><div style=width:500;>![ss](assets/smbng7.png)</div>

Langkah 8:<br>
Melakukan restart dengan perintah `sudo systemctl restart smbd`.
<br>Gambar:
<br><div style=width:500;>![ss](assets/smbng8.png)</div>

Langkah 9:<brd>
Lakukan pengecekan akses samba dalam VM 1 yang menggunakan IP Statik [smb://192.168.200.1] dengan menggunakan VM 2.
<br>Gambar:
<br><div style=width:500;>![ss](assets/smbng9.png)</div>
<br><div style=width:500;>![ss](assets/smbng10.png)</div>

## Instalasi Bind9 (DNS)
Langkah 1:<br>
Instalasi BIND menggunakan perintah `sudo apt -y install bind9 bind9utils`.
<br>Gambar:
<br><div style=width:500;>![ss](assets/dnsng1.png)</div>

Langkah 2:<br>
Konfigurasi BIND untuk network internal dalam named.conf menggunakan perintah `sudo nano /etc/bind/named.conf` untuk menambahkan `include "/etc/bind/named.conf.internal-zones";`.
<br>Gambar:
<br><div style=width:500;>![ss](assets/dnsng2.png)</div>

Langkah 3:<br>
Konfigurasi BIND untuk network internal dalam named.conf.options menggunakan perintah `sudo nano /etc/bind/named.conf.options` untuk menambahkan: 
```bash
        acl internal-network {
                192.168.200.0/24;
        };
...
...
        allow-query { localhost; internal-network; };
        allow-transfer { localhost; };
        listen-on port 53 { any; };
        recursion yes;
```
Gambar:
<br><div style=width:500;>![ss](assets/dnsng3.png)</div>
<br><div style=width:500;>![ss](assets/dnsng4.png)</div>

Langkah 4:<br>
Konfigurasi BIND untuk network internal dalam named.conf.internal-zones menggunakan perintah `sudo nano /etc/bind//etc/bind/named.conf.internal-zones` untuk menambahkan:<br>
```bash
zone "kelompok2.home" IN {
        type master;
        file "/etc/bind/kelompok2.home.lan";
        allow-update { none; };
};
zone "200.168.192.in-addr.arpa" IN {
        type master;
        file "/etc/bind/200.168.192.db";
        allow-update { none; };
};
```
Gambar:
<br><div style=width:500;>![ss](assets/dnsng5.png)</div>

Langkah 5:<br>
Konfigurasi BIND untuk network internal dalam /default/named menggunakan perintah `sudo nano /etc/default/named` untuk menambahkan:<br>
```bash
# add
OPTIONS="-u bind -4"
```
Gambar:
<br><div style=width:500;>![ss](assets/dnsng6.png)</div>

Langkah 6:<br>
Membuat file zona yang digunakan server untuk menyelesaikan alamat IP dari nama domain, menggunakan perintah `sudo nano /etc/bind/kelompok2.home.lan`.
<br>Gambar:
<br><div style=width:500;>![ss](assets/dnsng7.png)</div>

Langkah 7:<br>
Buat file zona yang memungkinkan server mengubah nama domain menjadi alamat IP menggunakan perintah `sudo nano /etc/bind/200.168.192.db`.
<br>Gambar:
<br><div style=width:500;>![ss](assets/dnsng8.png)</div>

Langkah 8:<br>
Restart BIND untuk menyimpan perubahan menggunakan `sudo systemctl restart named`.
<br>Gambar:
<br><div style=width:500;>![ss](assets/dnsng9.png)</div>

Langkah 9:<br>
Merubah pengaturan DNS untuk merujuk ke DNS sendiri pada `sudo nano /etc/resolv.conf`.
<br>Gambar:
<br><div style=width:500;>![ss](assets/dnsng10.png)</div>

Langkah 10:<br>
Pada VM 2, ubah adapter 1 di setting > Network menjadi internal network.
<br>Gambar:
<br><div style=width:500;>![ss](assets/dnsng11.png)</div>

Langkah 11:<br>
Konfigurasi jaringan Wired menjadi IP statik seperti yang tertera pada gambar. Jika sudah, klik apply untuk menyimpan perubahan.
<br>Gambar:
<br><div style=width:500;>![ss](assets/dnsng12.png)</div>

Langkah 12:<br>
Jika VM 2 sudah terhubung dengan VM 1 melalui internal network, lakukan pengecekan DNS yang ada pada VM 1 dengan menggunakan `dig ns.kelompok2.home.` dan `dig -x 192.168.200.1`. Indikasi berhasilnya DNS pada VM 1 dapat dilihat dari flag `ANSWER: 1` atau lebih dan dengan `status: NOERROR`
<br>Gambar:
<br><div style=width:500;>![ss](assets/dnsng13.png)</div>
<br><div style=width:500;>![ss](assets/dnsng14.png)</div>
