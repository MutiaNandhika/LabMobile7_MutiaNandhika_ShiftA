# LabMobile7_MutiaNandhika_ShiftA

### Screenshot
<img src="https://github.com/user-attachments/assets/0ebd22cf-39c3-4bc9-a16b-f1224e6473fb" alt="Screenshot" width="300"/>
<img src="https://github.com/user-attachments/assets/6e1ed4cb-6be5-44a9-aff9-7b7469407366" alt="Screenshot" width="300"/>

## Penjelasan Cara Kerja Login

### 1. Backend (Database dan PHP API)
- **Database**: Dibuat tabel `user` pada database `coba-ionic` untuk menyimpan data pengguna. Data disimpan dalam bentuk hash dengan menggunakan MD5 untuk keamanan kata sandi.
- **API PHP**:
  - **koneksi.php**: Menghubungkan aplikasi dengan database menggunakan fungsi `mysqli_connect`. File ini memungkinkan akses data dari database saat proses login.
  - **login.php**: Mengambil data JSON yang dikirim dari frontend (username dan password), kemudian memverifikasi apakah data tersebut cocok dengan data di database.
    - Jika cocok, API mengembalikan JSON dengan status `berhasil`, nama pengguna, dan token waktu (untuk simulasi).
    - Jika tidak cocok, API mengembalikan status `gagal`.

### 2. Frontend (Ionic dengan Angular)
- **Service Authentication (authentication.service.ts)**:
  - `postMethod`: Mengirimkan data login (username dan password) ke API login melalui `http.post`.
  - `saveData`: Menyimpan token dan username ke dalam penyimpanan lokal menggunakan `Preferences.set`. Ini juga mengubah `isAuthenticated` menjadi `true`, yang menandakan pengguna sudah login.
  - `logout`: Menghapus data token dan username dari penyimpanan lokal dan mengubah `isAuthenticated` menjadi `false`, yang menandakan pengguna sudah logout.
  - `authenticationState`: Ini adalah `Observable` yang memantau status login pengguna dan memungkinkan komponen lain mengetahui apakah pengguna sudah login atau belum.

- **Guard (authGuard dan autoLoginGuard)**:
  - `authGuard`: Memeriksa apakah pengguna sudah login. Jika sudah, pengguna bisa mengakses halaman yang dilindungi seperti `/home`. Jika belum login, pengguna diarahkan ke halaman `/login`.
  - `autoLoginGuard`: Memastikan pengguna yang sudah login tidak diarahkan kembali ke halaman login, tetapi langsung diarahkan ke halaman `/home`.

- **Login Page (login.page.ts)**:
  - Pada halaman login, saat pengguna mengisi data dan menekan tombol login, fungsi `login` dipanggil.
  - Fungsi `login` memanggil `postMethod` dari `AuthenticationService` dengan data username dan password yang diinput pengguna.
  - Jika login berhasil (`status_login == "berhasil"`), token dan username disimpan, lalu pengguna diarahkan ke halaman `/home`.
  - Jika login gagal, muncul notifikasi “Username atau Password Salah”.

- **Home Page (home.page.ts)**:
  - Menampilkan halaman utama dan informasi username pengguna yang sudah login (`{{ nama }}`).
  - Terdapat fungsi `logout` yang memanggil `logout` dari `AuthenticationService`, membersihkan data login, dan mengarahkan pengguna kembali ke halaman login.

### Cara Kerja Terhubungnya Komponen
1. **Database & API**: Mengambil data dari database dan mengirim respons JSON dengan status login.
2. **Service & Guard di Ionic**: Service di Ionic berfungsi sebagai jembatan untuk komunikasi antara API dan komponen Ionic. Guard menjaga aksesibilitas halaman berdasarkan status login.
3. **Komponen Login & Home**: Masing-masing komponen memanfaatkan `AuthenticationService` untuk login dan logout. Halaman login akan memvalidasi login dan menyimpan data pengguna, sedangkan halaman home akan menampilkan informasi pengguna yang telah login.
   
