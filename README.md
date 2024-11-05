# Cara Kerja Login

# A. PHP API untuk Login
## 1. Koneksi Database
- File koneksi.php melakukan koneksi ke database coba-ionic dengan menggunakan mysqli.
- Header HTTP diatur untuk mendukung komunikasi antar-origin dan format JSON. 
## 2. Proses Login di login.php
- File login.php menerima data username dan password dalam bentuk JSON dari client, kemudian mendekode datanya.
- password yang diterima di-hash menggunakan MD5 untuk mencocokkan format yang sama dengan yang ada di database.
- Query dilakukan untuk mengecek apakah username dan password sesuai dengan data di database.
- Jika data ditemukan (jumlah != 0), maka login berhasil, dan server mengembalikan objek JSON berisi username, token, dan status_login bernilai "berhasil".
- Jika tidak ada kecocokan, server mengembalikan status_login "gagal".

# B. Pengaturan Aplikasi Ionic
## 1. Deklarasi HTTP Client di app.module.ts
- provideHttpClient() diimpor untuk memungkinkan aplikasi berkomunikasi dengan API.
## 2. Service untuk Autentikasi (authentication.service.ts)
- AuthenticationService menyediakan fungsi-fungsi untuk login, menyimpan data token dan username, dan mengelola status autentikasi.
- Jika login berhasil, token dan username disimpan menggunakan Capacitor Preferences.
- Metode postMethod() mengirimkan data login ke endpoint login.php.
- Jika logout dipanggil, data disetel ke kosong dan status autentikasi diatur menjadi false.
## 3. Guards untuk Pengaturan Akses (auth.guard.ts dan auto-login.guard.ts)
- authGuard: Mengizinkan akses ke halaman hanya jika status autentikasi adalah true.
- autoLoginGuard: Mengarahkan pengguna ke halaman home jika sudah login; jika tidak, tetap berada di halaman login.
## 4. Pengaturan Rute di app-routing.module.ts
- Menggunakan authGuard untuk halaman home agar hanya dapat diakses ketika pengguna sudah login.
- Menggunakan autoLoginGuard untuk halaman login agar pengguna yang sudah login diarahkan langsung ke halaman home.

# C. Tampilan dan Fungsi Login
## 1. Tampilan login.page.html
- Form login terdiri dari input username dan password, serta tombol untuk memanggil fungsi login().
## 2. Pengolahan Login di login.page.ts
- Fungsi login() memvalidasi input pengguna. Jika input valid, data dikirim ke API menggunakan AuthenticationService.
- Jika API merespons dengan status_login "berhasil", token dan username disimpan, dan pengguna diarahkan ke halaman home.
- Jika gagal, notifikasi ditampilkan dengan pesan kesalahan.
  
# D. Tampilan dan Fungsi Home
## 1. Tampilan home.page.html
- Menampilkan ucapan selamat datang dan tombol untuk logout.
## 2. Pengolahan Logout di home.page.ts
- Fungsi logout() menghapus data autentikasi dari storage dan mengembalikan pengguna ke halaman login.
