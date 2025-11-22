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


