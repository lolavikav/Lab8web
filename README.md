# Lab8web

# nama : Lola Seftyliani 
# kelas: TI.24.A.4
# NIM :312410339

## DESKRIPSI
Praktikum ini adalah Program sederhana menggunakan PHP dan MySQL untuk mengelola data barang, pengguna bisa menambah, melihat, mengubah dan menghapus data barang, termasuk upload gambar barang.

## STRUKTUR FOLDER
lab8_php_database/ │── index.php │── tambah.php │── ubah.php │── hapus.php │── koneksi.php └── gambar/

## Langkah- Langkah Praktikum & Penjelasan

## 1. Persiapan Environment
- Text editor: Visual Studio Code
- Web server: XAMPP
- Buat folder lab8_php_database di htdocs

## 2. Menjalankan MySQL Server & phpMyAdmin
- Start Apache dan MySQL dari XAMPP Control Panel
- Akses: http://localhost/phpmyadmin

## 3. Membuat Database, Tabel, Menambah Data
## A. Membuat Database
`CREATE DATABASE latihan1;`

## B. Membuat Tabel
`CREATE TABLE data_barang (
 id_barang int(10) auto_increment Primary Key,
 kategori varchar(30),
 nama varchar(30),
 gambar varchar(100),
 harga_beli decimal(10,0),
 harga_jual decimal(10,0),
 stok int(4)
);`

## C. Menambah Data
`VALUES ('Elektronik', 'HP Samsung Android', 'hp_samsung.jpg', 2000000, 2400000, 5), 
('Elektronik', 'HP Xiaomi Android', 'hp_xiaomi.jpg', 1000000, 1400000, 5), 
('Elektronik', 'HP OPPO Android', 'hp_oppo.jpg', 1800000, 2300000, 5);`

Berikut Hasilnya:
<img width="1076" height="409" alt="image" src="https://github.com/user-attachments/assets/0c2ccc8c-febc-4f93-a585-071784a3829d" />

## 4. Membuat file koneksi database
```
<?php 
$host = "localhost"; 
$user = "root"; 
$pass = ""; 
$db   = "latihan1"; 

$conn = mysqli_connect($host, $user, $pass, $db); 
if ($conn == false) 
{ 
   echo "Koneksi ke server gagal."; 
   die(); 
} #else echo "Koneksi berhasil"; 
?>
```


