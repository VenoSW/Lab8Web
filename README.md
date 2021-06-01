# Praktikum 8 - Pemrograman Web
```
Veno Setyoaji Wiratama
311910363
TI.19.A.2
Universitas Pelita Bangsa
```

## LANGKAH 1
### Jalankan XAMPP dan akses http://localhost/phpmyadmin/ untuk membuat database.
![LANGKAH 1](https://user-images.githubusercontent.com/22215113/120314094-10db6a80-c305-11eb-8d26-02289dd27c1c.png)

## LANGKAH 2
### Membuat database `latihan1` dan buat tabel
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
![LANGKAH 2 MEMBUAT TABEL](https://user-images.githubusercontent.com/22215113/120314335-55ff9c80-c305-11eb-837e-b557b2a05623.png)
![LANGKAH 2 MEMBUAT TABEL 2](https://user-images.githubusercontent.com/22215113/120314260-3ff1dc00-c305-11eb-812b-bd8f478b2f73.png)

## LANGKAH 3
### Menambahkan data
```
INSERT INTO data_barang (kategori, nama, gambar, harga_beli, harga_jual, stok)
VALUES ('Elektronik', 'HP Samsung Android', 'hp_samsung.jpg', 2000000, 2400000, 5),
('Elektronik', 'HP Xiaomi Android', 'hp_xiaomi.jpg', 1000000, 1400000, 5),
('Elektronik', 'HP OPPO Android', 'hp_oppo.jpg', 1800000, 2300000, 5);
```
![LANGKAH 3 MENAMBAHKAN DATA](https://user-images.githubusercontent.com/22215113/120314535-8b0bef00-c305-11eb-99d5-e54f67c7d926.png)
![LANGKAH 3 MENAMBAHKAN DATA 2](https://user-images.githubusercontent.com/22215113/120314576-95c68400-c305-11eb-9dc8-b12b0b2d05a3.png)

## LANGKAH 4
### Membuat file `lab8_php_database` pada root directory web server `(d:\xampp\htdocs)`
![LANGKAH 4 MEMBUAT FILE PADA HTDOCS](https://user-images.githubusercontent.com/22215113/120314812-d2927b00-c305-11eb-8dd5-e3ef18619b57.png)
![LANGKAH 4 MEMBUAT FILE PADA HTDOCS 2](https://user-images.githubusercontent.com/22215113/120314831-d7efc580-c305-11eb-8074-ef3e4ed85aec.png)

## LANGKAH 5
### Membuat `koneksi.php` pada folder `(d:\xampp\htdocs\lab8_php_database)`
```
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
} #else echo "Koneksi berhasil";
?>
```
![LANGKAH 5 MEMBUAT KONEKSI PHP](https://user-images.githubusercontent.com/22215113/120314943-01a8ec80-c306-11eb-9af5-50cba02bc02c.png)

## LANGKAH 6
### Membuat index menu awal. Buat file php dengan nama `index.php` pada folder `(d:\xampp\htdocs\lab8_php_database)`
* Coding
```
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
![LANGKAH 6 MEMBUAT INDEX PHP (coding 1)](https://user-images.githubusercontent.com/22215113/120315218-4c2a6900-c306-11eb-91c1-f89ab2b62784.png)
![LANGKAH 6 MEMBUAT INDEX PHP (coding 2)](https://user-images.githubusercontent.com/22215113/120315226-4d5b9600-c306-11eb-8ef6-2fad30649f7c.png)
* Hasil
![LANGKAH 6 MEMBUAT INDEX PHP](https://user-images.githubusercontent.com/22215113/120315228-4df42c80-c306-11eb-8d47-01aa4a2e2494.png)

## LANGKAH 7
### Membuat menu tambah barang. Buat file php dengan nama `tambah.php` pada folder `(d:\xampp\htdocs\lab8_php_database)`
* Coding
```
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
                <input type="text" name="nama" style="margin-left: 20px;" />
            </div>
            <div class="input">
                <label>Kategori</label>
                <select name="kategori" style="margin-left: 58px;" >
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
                <input type="text" name="harga_beli" style="margin-left: 43px;" />
            </div>
            <div class="input">
                <label>Stok</label>
                <input type="text" name="stok" style="margin-left: 85px;" />
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
![LANGKAH 7 MENAMBAHKAN BARANG (CODING 1)](https://user-images.githubusercontent.com/22215113/120315340-7419cc80-c306-11eb-98b8-c98b089b534e.png)
![LANGKAH 7 MENAMBAHKAN BARANG (CODING 2)](https://user-images.githubusercontent.com/22215113/120315346-75e39000-c306-11eb-9e1e-7dcf71bdfde8.png)
![LANGKAH 7 MENAMBAHKAN BARANG (CODING 3)](https://user-images.githubusercontent.com/22215113/120315353-77ad5380-c306-11eb-8749-d79cf6e0369d.png)
* Hasil
![LANGKAH 7 MENAMBAHKAN BARANG](https://user-images.githubusercontent.com/22215113/120315358-7845ea00-c306-11eb-8a1f-99852e160348.png)

## LANGKAH 8
### Membuat menu ubah barang. Buat file php dengan nama `ubah.php` pada folder `(d:\xampp\htdocs\lab8_php_database)`
* Coding
```
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
        <form method="post" action="ubah.php" enctype="multipart/form-data">
            <div class="input">
                <label>Nama Barang</label>
                <input type="text" name="nama" value="<?php echo $data['nama'];?>" style="margin-left: 20px;"/>
            </div>
            <div class="input">
                <label>Kategori</label>
                <select name="kategori" style="margin-left: 58px;">
                    <option <?php echo is_select ('Komputer', $data['kategori']);?> value="Komputer">Komputer</option>
                    <option <?php echo is_select ('Komputer', $data['kategori']);?> value="Elektronik">Elektronik</option>
                    <option <?php echo is_select ('Komputer', $data['kategori']);?> value="Hand Phone">Hand Phone</option>
                </select>
            </div>
            <div class="input">
                <label>Harga Jual</label>
                <input type="text" name="harga_jual" value="<?php echo $data['harga_jual'];?>" style="margin-left: 40px;"/>
            </div>
            <div class="input">
                <label>Harga Beli</label>
                <input type="text" name="harga_beli" value="<?php echo $data['harga_beli'];?>" style="margin-left: 43px;"/>
            </div>
            <div class="input">
                <label>Stok</label>
                <input type="text" name="stok" value="<?php echo $data['stok'];?>" style="margin-left: 85px;"/>
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
![LANGKAH 8 UBAH BARANG (CODING 1)](https://user-images.githubusercontent.com/22215113/120315752-e25e8f00-c306-11eb-924a-0cbfdf36b8e7.png)
![LANGKAH 8 UBAH BARANG (CODING 2)](https://user-images.githubusercontent.com/22215113/120315756-e38fbc00-c306-11eb-8238-92c888794542.png)
![LANGKAH 8 UBAH BARANG (CODING 3)](https://user-images.githubusercontent.com/22215113/120315760-e4285280-c306-11eb-9212-976e2c2dd536.png)
* Hasil
sebelum diubah
![LANGKAH 8 UBAH BARANG 1](https://user-images.githubusercontent.com/22215113/120315792-eb4f6080-c306-11eb-8e01-239584777327.png)\
sesudah diubah
![LANGKAH 8 UBAH BARANG 2](https://user-images.githubusercontent.com/22215113/120315926-0fab3d00-c307-11eb-8e21-2c5de0e8e49d.png)

## LANGKAH 9
### Membuat menu hapus barang. Buat file php dengan nama `hapus.php` pada folder `(d:\xampp\htdocs\lab8_php_database)`
* Coding
```
<?php
include_once 'koneksi.php';
$id = $_GET['id'];
$sql = "DELETE FROM data_barang WHERE id_barang = '{$id}'";
$result = mysqli_query($conn, $sql);
header('location: index.php');
?>
```
![LANGKAH 9 MENGHAPUS BARANG (CODING)](https://user-images.githubusercontent.com/22215113/120316071-38cbcd80-c307-11eb-8e2b-ddb986e5ed51.png)
* Hasil
sebelum dihapus
![LANGKAH 9 MENGHAPUS BARANG 1](https://user-images.githubusercontent.com/22215113/120316108-42edcc00-c307-11eb-8a55-3bcee279b245.png)
sesudah dihapus
![LANGKAH 9 MENGHAPUS BARANG 2](https://user-images.githubusercontent.com/22215113/120316114-44b78f80-c307-11eb-8e04-94d8c4ad71ed.png)

## LANGKAH 10
### Tambahkan CSS agar tampilan menarik
```
body{
    font-family: sans-serif;
}
table{
    border-collapse: collapse;
}
th, td{
    border: 1px solid black;
    font-size: 16px;
    padding: 7px 9px;
}

/* Tambah Barang */
.input{
    padding: 5px;
}
```
