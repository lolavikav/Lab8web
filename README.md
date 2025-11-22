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
```
CREATE TABLE data_barang (
id_barang int(10) auto_increment Primary Key,
kategori varchar(30),
nama varchar(30),
gambar varchar(100),
harga_beli decimal(10,0),
harga_jual decimal(10,0),
stok int(4)
);
```

## C. Menambah Data
```
VALUES ('Elektronik', 'HP Samsung Android', 'hp_samsung.jpg', 2000000, 2400000, 5), 
('Elektronik', 'HP Xiaomi Android', 'hp_xiaomi.jpg', 1000000, 1400000, 5), 
('Elektronik', 'HP OPPO Android', 'hp_oppo.jpg', 1800000, 2300000, 5);
```

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

Membuat folder lab8_php_database pada root directory web server (d:\xampp\htdocs) dengan file didalamnya koneksi.php. File koneksi.php berfungsi untuk menghubungkan aplikasi PHP dengan database MySQL. Di dalam file ini mengatur server seperti host, username, password, dan nama database yang digunakan, yaitu latihan1. Proses koneksi dilakukan menggunakan fungsi mysql_connect(), dan jika koneksi gagal maka program akan menampilkan pesan "koneksi ke server gagal" lalu menghentikan proses. Dengan adanya file ini, aplikasi dapat berjalan dengan baik dan terhubung secara langsung ke MySQL.
Berikut Hasilnya:
<img width="964" height="308" alt="image" src="https://github.com/user-attachments/assets/9b13127a-7b73-4397-9a26-bb3faf4f199a" />

## 5.Membuat file index untuk menampilkan data (Read)
```
<?php
include("koneksi.php");

// query untuk menampilkan data
$sql = "SELECT * FROM data_barang";
$result = mysqli_query($conn, $sql);
?>
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<title>Data Barang</title>
<link href="style.css" rel="stylesheet" type="text/css">
</head>
<body>
<div class="container">
    <h1>Data Barang</h1>
    <a href="tambah.php">+ Tambah Barang</a>
    <br><br>
    <table border="1" cellpadding="10" cellspacing="0">
        <tr>
            <th>Gambar</th>
            <th>Nama Barang</th>
            <th>Kategori</th>
            <th>Harga Jual</th>
            <th>Harga Beli</th>
            <th>Stok</th>
            <th>Aksi</th>
        </tr>
        <?php if($result && mysqli_num_rows($result) > 0): ?>
            <?php while($row = mysqli_fetch_assoc($result)): ?>
                <tr>
                    <td>
                        <?php if($row['gambar']): ?>
                            <img src="<?= $row['gambar'];?>" width="80" alt="<?= $row['nama'];?>">
                        <?php else: ?>
                            -
                        <?php endif; ?>
                    </td>
                    <td><?= $row['nama'];?></td>
                    <td><?= $row['kategori'];?></td>
                    <td><?= $row['harga_jual'];?></td>
                    <td><?= $row['harga_beli'];?></td>
                    <td><?= $row['stok'];?></td>
                    <td>
                        <a href="ubah.php?id=<?= $row['id_barang'];?>">Ubah</a> | 
                        <a href="hapus.php?id=<?= $row['id_barang'];?>" onclick="return confirm('Yakin ingin dihapus?')">Hapus</a>
                    </td>
                </tr>
            <?php endwhile; ?>
        <?php else: ?>
            <tr>
                <td colspan="7">Belum ada data</td>
            </tr>
        <?php endif; ?>
    </table>
</div>
</body>
</html><?php
```

Membuat file baru di VSCode dengan nama `index.php`. File ini digunakan untuk menampilkan seluruh data barang dari database. File ini memanggil koneksi database, mengambil data menggunakan query `SELECT *FROM data_barang`, lalu menampilkannnya dalam bentuk tabel. setiap barang ditampilkan lengkap dengan gambar, nama, katerori, harga jual, harga beli, serta tombol ubah dan hapus untuk mengelola data. Jika tidak ada data, halaman akan menampilkan pesan " Belum ada data".

Berikut Hasilnya:
<img width="1918" height="518" alt="image" src="https://github.com/user-attachments/assets/ff6b9e69-3b28-418b-b515-981935db0e19" />

## 6. Menambah Data (Create)
```
<?php
include_once 'koneksi.php';

if (isset($_POST['submit'])) {
    $nama = $_POST['nama'];
    $kategori = $_POST['kategori'];
    $harga_jual = $_POST['harga_jual'];
    $harga_beli = $_POST['harga_beli'];
    $stok = $_POST['stok'];

    $gambar = null;
    if (isset($_FILES['file_gambar']) && $_FILES['file_gambar']['error'] == 0) {
        $filename = str_replace(' ', '_', $_FILES['file_gambar']['name']);
        $destination = 'gambar/' . $filename;
        if (move_uploaded_file($_FILES['file_gambar']['tmp_name'], $destination)) {
            $gambar = $destination;
        }
    }

    $sql = "INSERT INTO data_barang (nama, kategori, harga_jual, harga_beli, stok, gambar)
            VALUES ('$nama', '$kategori', '$harga_jual', '$harga_beli', '$stok', '$gambar')";
    mysqli_query($conn, $sql);
    header('Location: index.php');
    exit;
}
?>
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<title>Tambah Barang</title>
<link href="style.css" rel="stylesheet" type="text/css" />
</head>
<body>
<div class="container">
    <h1>Tambah Barang</h1>
    <form method="post" action="" enctype="multipart/form-data">
        <label>Nama Barang</label><br>
        <input type="text" name="nama" required><br><br>

        <label>Kategori</label><br>
        <select name="kategori" required>
            <option value="Komputer">Komputer</option>
            <option value="Elektronik">Elektronik</option>
            <option value="Hand Phone">Hand Phone</option>
        </select><br><br>

        <label>Harga Jual</label><br>
        <input type="number" name="harga_jual" required><br><br>

        <label>Harga Beli</label><br>
        <input type="number" name="harga_beli" required><br><br>

        <label>Stok</label><br>
        <input type="number" name="stok" required><br><br>

        <label>File Gambar</label><br>
        <input type="file" name="file_gambar"><br><br>

        <input type="submit" name="submit" value="Simpan">
    </form>
</div>
</body>
</html>
```

Membuat file baru di VSCode dengan nama `tambah.php`. File ini digunakan untuk menambahkan data barang baru ke dalam database. Pada bagian awal file, sistem memanggil koneksi database dan memeriksa apakah tombol submit telah di tekan. Jika form dikirim, data nama seperti nama barang, kategori, harga jual, harga beli, dan stokk akan diambil dari input pengguna. File ini juga menggunakan proses upload gambar, jika pengguna mengunggah file dan proses upload berhasil, maka gambar akan disimpan ke folder `gambar/` dan namanya disimpan ke database.
Berikut Hasilnya:
<img width="1918" height="666" alt="image" src="https://github.com/user-attachments/assets/9625c447-41a1-44ca-bfa5-3d52df7c0328" />
<img width="1919" height="653" alt="image" src="https://github.com/user-attachments/assets/51680fdb-c451-496a-9c7f-03e6c2f83fdd" />

## 7. Mengubah Data ( UPDATE)
```
<?php
include_once 'koneksi.php';

$id = $_GET['id'] ?? null;
if (!$id) {
    die("ID tidak ditemukan");
}

// ambil data lama
$sql = "SELECT * FROM data_barang WHERE id_barang='$id'";
$result = mysqli_query($conn, $sql);
$data = mysqli_fetch_assoc($result);

if (!$data) die("Data tidak tersedia");

if (isset($_POST['submit'])) {
    $nama = $_POST['nama'];
    $kategori = $_POST['kategori'];
    $harga_jual = $_POST['harga_jual'];
    $harga_beli = $_POST['harga_beli'];
    $stok = $_POST['stok'];

    $gambar = $data['gambar'];
    if (isset($_FILES['file_gambar']) && $_FILES['file_gambar']['error'] == 0) {
        $filename = str_replace(' ', '_', $_FILES['file_gambar']['name']);
        $destination = 'gambar/' . $filename;
        if (move_uploaded_file($_FILES['file_gambar']['tmp_name'], $destination)) {
            $gambar = $destination;
        }
    }

    $sql_update = "UPDATE data_barang SET 
                    nama='$nama', 
                    kategori='$kategori', 
                    harga_jual='$harga_jual', 
                    harga_beli='$harga_beli', 
                    stok='$stok', 
                    gambar='$gambar'
                   WHERE id_barang='$id'";
    mysqli_query($conn, $sql_update);
    header('Location: index.php');
    exit;
}

// fungsi untuk selected option
function is_select($var, $val) {
    return $var == $val ? 'selected' : '';
}
?>
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<title>Ubah Barang</title>
<link href="style.css" rel="stylesheet" type="text/css">
</head>
<body>
<div class="container">
    <h1>Ubah Barang</h1>
    <form method="post" action="" enctype="multipart/form-data">
        <label>Nama Barang</label><br>
        <input type="text" name="nama" value="<?= $data['nama']; ?>" required><br><br>

        <label>Kategori</label><br>
        <select name="kategori" required>
            <option value="Komputer" <?= is_select($data['kategori'], 'Komputer'); ?>>Komputer</option>
            <option value="Elektronik" <?= is_select($data['kategori'], 'Elektronik'); ?>>Elektronik</option>
            <option value="Hand Phone" <?= is_select($data['kategori'], 'Hand Phone'); ?>>Hand Phone</option>
        </select><br><br>

        <label>Harga Jual</label><br>
        <input type="number" name="harga_jual" value="<?= $data['harga_jual']; ?>" required><br><br>

        <label>Harga Beli</label><br>
        <input type="number" name="harga_beli" value="<?= $data['harga_beli']; ?>" required><br><br>

        <label>Stok</label><br>
        <input type="number" name="stok" value="<?= $data['stok']; ?>" required><br><br>

        <label>File Gambar</label><br>
        <input type="file" name="file_gambar"><br>
        <?php if($data['gambar']): ?>
            <img src="<?= $data['gambar']; ?>" width="100" alt="<?= $data['nama']; ?>">
        <?php endif; ?><br><br>

        <input type="submit" name="submit" value="Simpan">
    </form>
</div>
</body>
</html>
```

Membuat file baru di VSCode dengan nama `ubah.php`. File ini berfungsi sebagai halaman untuk meperbarui data barang berdasarkan ID yang dipilih dari halaman utama. Pengguna dapat mengubah nama barang, kategori, harga jual, harga beli, stok dan juga menggati gambar.

Hasilnya:
<img width="1917" height="932" alt="image" src="https://github.com/user-attachments/assets/89162d51-dde6-4f47-bf54-1590e02efb94" />
<img width="1910" height="696" alt="image" src="https://github.com/user-attachments/assets/173c3d7d-2bfd-497d-a47a-4714b5efaf9d" />

## 8. Menghapus Data (DELETE)
```
<?php
error_reporting(E_ALL);
ini_set('display_errors', 1);

include_once 'koneksi.php';

// Ambil ID dari URL
$id = $_GET['id'] ?? null;

if (!$id) {
    die("Error: ID tidak ditemukan di URL");
}

// Cek apakah data dengan ID tersebut ada
$cek = mysqli_query($conn, "SELECT * FROM data_barang WHERE id_barang='$id'");
if (!$cek) {
    die("Error query cek data: " . mysqli_error($conn));
}
if (mysqli_num_rows($cek) == 0) {
    die("Error: Data dengan ID $id tidak ditemukan");
}

// Hapus data
$sql = "DELETE FROM data_barang WHERE id_barang='$id'";
$result = mysqli_query($conn, $sql);

if (!$result) {
    die("Error menghapus data: " . mysqli_error($conn));
}

// Jika berhasil, redirect ke index.php
header('Location: index.php');
exit;
?>
```

Membuat file baru di VSCode dengan nama `hapus.php`. File ini digunakan untuk menghapus data barang dari database berdasarkan ID yang dikirim melalui URL. Pada bagian awal, file ini mengambil ID dan melakukan pengecekan untuk memastikan ID tersebut benar-benar ada dalam database. Jika data ditemukan, sistem menjalankan query `DELETE` untuk menghapus baris data sesuai ID yang dipilih. File ini juga melengkapi dengan pengecekkan error, sehingga apabila terjadin kessalahan pada proses query, pesan error akan langsung ditampilkan.


Hasilnya:
<img width="1917" height="721" alt="image" src="https://github.com/user-attachments/assets/d9e1d946-9584-4028-8e5d-b87e0391e196" />
<img width="1919" height="695" alt="image" src="https://github.com/user-attachments/assets/f8b8ae12-8a71-48dd-ac7e-8708fe880a25" />
<img width="1919" height="599" alt="image" src="https://github.com/user-attachments/assets/067927a1-b5a2-4174-ba68-21610b8f08a1" />










