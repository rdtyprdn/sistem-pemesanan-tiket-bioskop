# Sistem Pemesanan Tiket Bioskop

> **Tugas Kelompok --- Pemrograman Berorientasi Objek**\
> **Kelas A, Kelompok 1**\
> Java + JavaFX + MySQL

------------------------------------------------------------------------

## 1. Latar Belakang

Beli tiket bioskop langsung di loket itu makan waktu, apalagi kalau
lagi ramai pengunjung.

Sistem ini dibuat untuk memindahkan proses tersebut ke aplikasi
desktop. Dari lihat film yang tayang, sampai pilih kursi dan
menyelesaikan pemesanan, tanpa harus antre.

Tujuannya sederhana: mempermudah admin mengelola data bioskop, dan
mempermudah customer memesan tiket.

------------------------------------------------------------------------

## 2. Dua Peran, Satu Sistem

Aplikasi ini punya dua jenis pengguna.

**Admin** mengurus bagian belakang layar:

-   kelola data film;
-   kelola studio dan kapasitasnya;
-   atur jadwal tayang;
-   kelola data kursi;
-   lihat data pemesanan;
-   lihat laporan penjualan.

**Customer** menggunakan aplikasi untuk transaksi:

-   registrasi dan login;
-   lihat daftar dan detail film;
-   pilih jadwal tayang;
-   pilih kursi;
-   konfirmasi pemesanan;
-   lihat riwayat pemesanan.

Sebelum masuk ke fitur apa pun, pengguna wajib login atau registrasi
dulu. Sistem baru menampilkan dashboard yang sesuai setelah tahu
perannya, admin atau customer.

------------------------------------------------------------------------

## 3. Kenapa JavaFX, Bukan Swing

Java Swing sebenarnya bisa saja dipakai. Sudah lama ada, komponennya
lengkap.

Tapi tampilan bawaannya cenderung terasa lama, dan butuh penyesuaian
ekstra kalau mau terlihat modern.

JavaFX dipilih karena tiga alasan konkret:

1.  mendukung CSS, jadi warna dan gaya tampilan gampang disesuaikan;
2.  mendukung FXML, jadi struktur tampilan bisa dipisah dari logika
    program;
3.  pemisahan itu bikin kode lebih terorganisir, Controller cukup
    fokus mengurus interaksi, bukan campur dengan tampilan.

Perancangan antarmuka dilakukan lewat NetBeans IDE dengan GUI Builder,
sementara data dikelola lewat MySQL.

------------------------------------------------------------------------

## 4. Struktur Kelas

``` text
User (superclass)
 ├── Admin
 └── Customer

Film ── Pemesanan ── Kursi
           │
        Studio
```

`User` jadi kelas induk yang menyimpan atribut dasar: id, username,
password, dan role.

`Admin` dan `Customer` mewarisi itu semua, lalu menambahkan fungsinya
masing-masing sesuai tanggung jawabnya.

`Pemesanan` jadi kelas penghubung. Satu transaksi terkait ke film yang
dipilih, kursi yang diambil, dan customer yang memesan.

------------------------------------------------------------------------

## 5. Alur Aplikasi

``` text
Buka aplikasi
     ↓
Login / Register
     ↓
Validasi data
     ↓
Admin?  →  Dashboard Admin
     │
Customer  →  Lihat film → Pilih jadwal → Pilih kursi → Konfirmasi
```

Saat customer konfirmasi pemesanan, sistem mengecek ulang apakah
kursinya masih tersedia.

Kalau sudah dipesan orang lain, sistem menampilkan pesan kursi tidak
tersedia dan customer diminta pilih kursi lain.

Kalau masih kosong, data pemesanan disimpan ke database, status kursi
berubah jadi terpesan, dan tiket ditampilkan sebagai bukti transaksi.

------------------------------------------------------------------------

## 6. Empat Pilar OOP

### Enkapsulasi

Atribut kelas `Film` seperti `idFilm`, `judul`, `genre`, dan `durasi`
dibuat private.

Akses dilakukan lewat method seperti `getJudul()` dan `setJudul()`.
Khusus `setDurasi()`, ada validasi tambahan: durasi film harus lebih
dari 0 menit.

### Pewarisan

`Admin` dan `Customer` sama-sama mewarisi atribut dan perilaku dari
`User`. Atribut umum seperti id, nama, dan password tidak perlu
ditulis ulang di dua kelas itu.

### Polimorfisme

Method `tampilkanMenu()` di-override berbeda oleh `Admin` dan
`Customer`.

Nama methodnya sama, tapi hasilnya beda. Di `Admin`, menu yang muncul
untuk kelola data. Di `Customer`, menu yang muncul untuk pesan tiket.

### Abstraksi

Untuk menghitung total harga, kelas antarmuka cukup memanggil
`totalHarga()`.

Tidak perlu tahu bagaimana perkalian harga berdasarkan jadwal
dilakukan, atau bagaimana sistem mengecek ketersediaan kursi di
belakang layar.

------------------------------------------------------------------------

## 7. Rancangan Database

Tujuh entitas utama: **Pengguna, Admin, Pelanggan, Film, Studio,
Pemesanan, Kursi**.

Beberapa relasi kuncinya:

-   1 Admin bisa mengelola banyak Film dan Studio (one-to-many);
-   1 Pelanggan bisa membuat banyak Pemesanan (one-to-many);
-   Pemesanan dan Kursi berelasi many-to-many, karena satu transaksi
    bisa mencakup beberapa kursi, dan satu kursi secara fisik bisa
    dipesan lagi di jadwal yang berbeda.

------------------------------------------------------------------------

## 8. Tim Pengembang

  Nama                                 NIM
  ------------------------------------ -----------
  Visvani Bhavanti Jyothi              2508561003
  I Putu Aditya Widiantara Putra       2508561006
  Putu Alia Devi Junita Lestari        2508561009
  I Gusti Agung Gde Raditya Pradana    2508561011

**Dosen Pembimbing:** I Made Satria Bimantara, S.Kom., M.Kom.\
**Program Studi Informatika, Universitas Udayana --- 2026**
