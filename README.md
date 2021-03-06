# Lab8Web

| Nama      | Faris Syahluthfi |
| ----------- | ----------- |
| NIM     | 312010034       |
| Kelas   | TI.20.A.1        |

## Langkah langkah praktikum 8

## 1. Persiapan
Untuk memulai membuat aplikasi CRUD sederhana, yang perlu disiapkan adalah
database server menggunakan MySQL. Pastikan MySQL Server sudah dapat dijalankan
melalui XAMPP.

## 2. Menjalankan MySQL Server
Untuk menjalankan MySQL Server dari menu XAMPP Contol.
![xampp](screenshot/xampp.png)

## 3. Mengakses MySQL Client menggunakan PHP MyAdmin
Pastikan webserver Apache dan MySQL server sudah dijalankan. Kemudian buka
melalui browser: http://localhost/phpmyadmin/


## 4. Membuat Database: Studi Kasus Data Barang

![tabel](screenshot/tabel.png)

## 5. Membuat Database
![database](screenshot/database.png)

## 6. Membuat Tabel
![tabel1](screenshot/tabel1.png)
![tabel2](screenshot/tabel2.png)

## 7. Menambahkan Data
![data1](screenshot/data1.png)
![data2](screenshot/data2.png)

## 8. Membuat Program CRUD
Buat folder lab8_php_database pada root directory web server (d:\xampp\htdocs)
![folder](screenshot/folder.png)

Kemudian untuk mengakses direktory tersebut pada web server dengan mengakses URL:
http://localhost/lab8_php_database/

![gambar1](screenshot/gambar1.png)

## 9. Membuat file koneksi database
Buat file baru dengan nama koneksi.php

```php

<?php
$host = "localhost";
$user = "root";
$pass = "";
$db = "latihan1";
$conn = mysqli_connect($host, $user, $pass, $db);
if ($conn == false)
{
    echo "Koneksi ke server gagal.";
    die();
} else echo "Koneksi berhasil";
?>

```

Buka melalui browser untuk menguji koneksi database (untuk menyampilkan pesan
koneksi berhasil, uncomment pada perintah echo “koneksi berhasil”;

![koneksi](screenshot/koneksi.png)

## 10. Membuat file index untuk menampilkan data (Read)
Buat file baru dengan nama index.php

```php

<?php
include("koneksi.php");

// query untuk menampilkan data
$sql = 'SELECT * FROM data_barang';
$result = mysqli_query($conn, $sql);

?>
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <link href="style.css" rel="stylesheet" type="text/css" />
    <title>Data Barang</title>
</head>
<body>
    <div class="container">
        <h1>Data Barang</h1>
        <div class="main">
            <a href="tambah.php">Tambah Barang</a>
            <table>
            <tr>
                <th>Gambar</th>
                <th>Nama Barang</th>
                <th>Katagori</th>
                <th>Harga Jual</th>
                <th>Harga Beli</th>
                <th>Stok</th>
                <th>Aksi</th>
            </tr>
            <?php if($result): ?>
            <?php while($row = mysqli_fetch_array($result)): ?>
            <tr>
                <td><img src="gambar/<?= $row['gambar'];?>" alt="<?=$row['nama'];?>"></td>
                <td><?= $row['nama'];?></td>
                <td><?= $row['kategori'];?></td>
                <td><?= $row['harga_jual'];?></td>
                <td><?= $row['harga_beli'];?></td>
                <td><?= $row['stok'];?></td>
                <td>
                    <a href="ubah.php?id=<?= $row['id_barang'];?>">Ubah</a>
                    <a href="hapus.php?id=<?= $row['id_barang'];?>">Hapus</a> 
                </td>
            </tr>
            <?php endwhile; else: ?>
            <tr>
                <td colspan="7">Belum ada data</td>
            </tr>
            <?php endif; ?>
            </table>
        </div>
    </div>
</body>
</html>

```
# outputnya:
![index](screenshot/index.png)

## 11. Menambah Data (Create)
Buat file baru dengan nama tambah.php

```php

<?php
error_reporting(E_ALL);
include_once 'koneksi.php';

if (isset($_POST['submit']))
{
    $nama = $_POST['nama'];
    $kategori = $_POST['kategori'];
    $harga_jual = $_POST['harga_jual'];
    $harga_beli = $_POST['harga_beli'];
    $stok = $_POST['stok'];
    $file_gambar = $_FILES['file_gambar'];
    $gambar = null;

    if ($file_gambar['error'] == 0)
    {
        $filename = str_replace(' ', '_',$file_gambar['name']);
        $destination = dirname(__FILE__) .'/gambar/' . $filename;
        if(move_uploaded_file($file_gambar['tmp_name'], $destination))
        {
            $gambar = 'gambar/' . $filename;;
        }
    }
    
    $sql = 'INSERT INTO data_barang (nama, kategori, harga_jual, harga_beli, stok, gambar) ';
    $sql .= "VALUE ('{$nama}', '{$kategori}','{$harga_jual}','{$harga_beli}', '{$stok}', '{$gambar}')";
    $result = mysqli_query($conn, $sql);
    header('location: index.php');
}
?>

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <link href="style.css" rel="stylesheet" type="text/css" />
    <title>Tambah Barang</title>
</head>
<body>
<div class="container">
    <h1>Tambah Barang</h1>
    <div class="main">
        <form method="post" action="tambah.php" enctype="multipart/form-data">
            <div class="input">
                <label>Nama Barang</label>
                <input type="text" name="nama" style="margin-left: 40px;" />
            </div>
            <div class="input">
                <label>Kategori</label>
                <select name="kategori" style="margin-left: 40px;" >
                    <option value="Komputer">Komputer</option>
                    <option value="Elektronik">Elektronik</option>
                    <option value="Hand Phone">Hand Phone</option>
                </select>
            </div>
            <div class="input">
                <label>Harga Jual</label>
                <input type="text" name="harga_jual" style="margin-left: 40px;" />
            </div>
            <div class="input">
                <label>Harga Beli</label>
                <input type="text" name="harga_beli" style="margin-left: 40px;" />
            </div>
            <div class="input">
                <label>Stok</label>
                <input type="text" name="stok" style="margin-left: 40px;" />
            </div>
            <div class="input">
                <label>File Gambar</label>
                <input type="file" name="file_gambar" />
            </div>
            <div class="submit">
                <input type="submit" name="submit" value="Simpan" />
            </div>
        </form>
    </div>
</div>
</body>
</html>

```

# Outputnya:
![tambah](screenshot/tambah.png)

## 12. Mengubah Data (Update)
Buat file baru dengan nama ubah.php

```php

<?php
error_reporting(E_ALL);
include_once 'koneksi.php';

if (isset($_POST['submit']))
{
    $id = $_POST['id'];
    $nama = $_POST['nama'];
    $kategori = $_POST['kategori'];
    $harga_jual = $_POST['harga_jual'];
    $harga_beli = $_POST['harga_beli'];
    $stok = $_POST['stok'];
    $file_gambar = $_FILES['file_gambar'];
    $gambar = null;
 
    if ($file_gambar['error'] == 0)
    {
        $filename = str_replace(' ', '_', $file_gambar['name']);
        $destination = dirname(__FILE__) . '/gambar/' . $filename;
        if (move_uploaded_file($file_gambar['tmp_name'], $destination))
        {
            $gambar = 'gambar/' . $filename;;
        }
    }
    $sql = 'UPDATE data_barang SET ';
    $sql .= "nama = '{$nama}', kategori = '{$kategori}', ";
    $sql .= "harga_jual = '{$harga_jual}', harga_beli = '{$harga_beli}', stok = '{$stok}' ";
    if (!empty($gambar))
    $sql .= ", gambar = '{$gambar}' ";
    $sql .= "WHERE id_barang = '{$id}'";
    $result = mysqli_query($conn, $sql);

    header('location: index.php');
}
   
$id = $_GET['id'];
$sql = "SELECT * FROM data_barang WHERE id_barang = '{$id}'";
$result = mysqli_query($conn, $sql);
if (!$result) die('Error: Data tidak tersedia');
$data = mysqli_fetch_array($result);

function is_select($var, $val) {
    if ($var == $val) return 'selected="selected"';
    return false;
}

?>
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <link href="style.css" rel="stylesheet" type="text/css" />
    <title>Ubah Barang</title>
</head>
<body>
<div class="container">
    <h1>Ubah Barang</h1>
    <div class="main">
        <form method="post" action="ubah.php"
enctype="multipart/form-data">
<div class="input">
    <label>Nama Barang</label>
    <input type="text" name="nama" value="<?php echo $data['nama'];?>" />
</div>
<div class="input">
    <label>Kategori</label>
    <select name="kategori">
    <option <?php echo is_select
    ('Komputer', $data['kategori']);?> value="Komputer">Komputer</option>
    <option <?php echo is_select
    ('Komputer', $data['kategori']);?> value="Elektronik">Elektronik</option>
    <option <?php echo is_select
    ('Komputer', $data['kategori']);?> value="Hand Phone">Hand Phone</option>
    </select>
</div>
<div class="input">
    <label>Harga Jual</label>
    <input type="text" name="harga_jual" value="<?php echo $data['harga_jual'];?>" />
    </div>
    <div class="input">
        <label>Harga Beli</label>
        <input type="text" name="harga_beli" value="<?php echo $data['harga_beli'];?>" />
    </div>
    <div class="input">
        <label>Stok</label>
        <input type="text" name="stok" value="<?php echo $data['stok'];?>" />
    </div>
    <div class="input">
        <label>File Gambar</label>
        <input type="file" name="file_gambar" />
    </div>
    <div class="submit">
        <input type="hidden" name="id" value="<?php echo $data['id_barang'];?>" />
        <input type="submit" name="submit" value="Simpan" />
    </div>
</form>
</div>
</div>
</body>
</html>

```
# Outputnya:
![ubah](screenshot/ubah.png)

## 13. Menghapus Data (Delete)
Buat file baru dengan nama hapus.php

```php

<?php
include_once 'koneksi.php';
$id = $_GET['id'];
$sql = "DELETE FROM data_barang WHERE id_barang = '{$id}'";
$result = mysqli_query($conn, $sql);
header('location: index.php');
?>

```
# Outputnya:
![hapus](screenshot/hapus.png)






