# Konfigurasi Dasar Switch Huawei

## Daftar isi
1. Konfigurasi Management interface
2. Konfigurasi SSH dan User Login
3. Konfigurasi RSA Key
4. Verifikasi Konfigurasi
5. Troubleshooting
---

## 1. Konfigurasi management interface
Konfigurasi management interface bertujuan untuk:
- Mengaktifkan akses remote via VTY (Telnet/SSH)
- Mengatur interface manajemen (MEth0/0/0)

### Perintah Utama
```sh
user-interface maximum-vty 21       # Maksimal 21 sesi remote
user-interface vty 0 20             # Konfigurasi sesi VTY 0-20
idle-timeout 0                      # Nonaktifkan auto-disconnect
authentication-mode aaa             # Wajibkan login via AAA
protocol inbound all                # Izinkan Telnet & SSH

interface Meth 0/0/0                # Interface manajemen out-of-band
ip address 192.168.30.1 24          # Set IP manajemen
```
### Note
- Untuk ip manajemen bebas

## 2. Konfigurasi SSH dan User login
- Membuat user lokal dengan autentikasi AAA
- Mengaktifkan SSH untuk akses aman

### Perintah utama
```sh
undo local-user policy security-enhance                  # Nonaktifkan kompleksitas password
local-user admin password irreversible-cipher admin2025  # Buat user 'admin'
local-user admin service-type ssh telnet                 # Izinkan akses SSH & Telnet
local-user admin level 3                                 # Hak akses level admin
local-user admin user-group manage-ug                    # Masukkan ke grup manajemen

stelnet server enable                   # Aktifkan SSH server
snetconf server enable                  # Aktifkan NETCONF over SSH
ssh authorization-type default aaa      # Gunakan AAA untuk otorisasi
```
### Note
- irreversible-cipher menggunakan hash SHA-256 untuk penyimpanan password.
- Level 3 = Hak akses tertinggi (bisa diubah ke level 1 untuk user biasa).

## 3. Konfigurasi RSA Key
- Membuat key pair RSA untuk enkripsi SSH

### Perintah utama
```sh
rsa local-key-pair create       # Generate key RSA 2048-bit
```
### Note
- Key RSA wajib untuk koneksi SSH yang aman.
- Key disimpan dengan nama default HUAWEI_Host.

## 4. Verifikasi Konfigurasi
Mengecek Konfigurasi yang Sedang Berjalan (Running Config):
```sh
<HUAWEI> display current-configuration
```

Filter hasil:
```sh
display current-configuration | include username  # Cari user tertentu
display current-configuration | begin interface   # Mulai dari section interface
```

Memeriksa Konfigurasi yang Tersimpan (Startup Config):
```sh
<HUAWEI> display startup
```

Output penting:
- Startup saved-configuration file: Pastikan tidak NULL
- Next startup saved-configuration file: File yang akan dipakai saat reboot

Melihat Daftar Interface Aktif:
```sh
<HUAWEI> display interface brief
```

## 5. Troubleshooting
Cek konektivitas manajemen:
```sh
ping 192.168.30.1  # Test IP manajemen
```
Verifikasi service aktif:
```sh
display telnet server status
display ssh server session
```

Log error:
```sh
display logbuffer  # Tampilkan log sistem
```

### Note
Simpan konfigurasi secara permanen:
```sh
<HUAWEI>save  ##Pastikan berada di user-view bukan di system-view
```