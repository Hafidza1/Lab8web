# Lab8web

**LANGKAH-LANGKAH PRATIKUM**

**uji koneksi database**
```
<?php
$host = "localhost";
$user = "root";
$pass = "";
$db = "latihan1";
$conn = mysqli_connect('localhost', 'root','', 'latihan1');

if ($conn == false)
{
	echo "Koneksi ke server gagal.";
	die();
} #else echo "Koneksi berhasil";
?>
```
![1](https://github.com/Hafidza1/Lab8web/assets/115520666/f2b50d04-b63a-4658-8c53-ee3fc11373d3)

**Membuat file index untuk menampilkan data(Read)**

```
<?php
include("koneksi.php");
$no = 1;
// query untuk menampilkan data
$sql = 'SELECT * FROM data_barang';
$result = mysqli_query($conn, $sql);

?>
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <link rel="stylesheet" href="css/bootstrap.min.css" />
    <script src="js/bootstrap.min.js"></script>
    <title>Data Barang</title>
</head>
<body>
    <div class="container">
        <h1 align="center">Data Barang</h1>
		<table class="table table-striped table-sm">
			<td><td>
		</table>
        <div class="main">
		<div class="btn-toolbar mb-2 mb-md-10">
           <a class="btn btn-primary" href="tambah.php" role="button">Tambah Barang</a>
		</div> 
			<div class="table-responsive">
				<table class="table table-striped table-hover">
				<thead>
					<tr>
						<th>No</th>
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
						<td><?php echo $no++ ?></td>
						<td ><img src="gambar/<?= $row['gambar'];?>" alt="<?=$row['nama'];?>"></td>
						<td><?= $row['nama'];?></td>
						<td><?= $row['kategori'];?></td>
						<td><?= $row['harga_jual'];?></td>
						<td><?= $row['harga_beli'];?></td>
						<td><?= $row['stok'];?></td>
						<td>
						<a class="btn btn-success" href="ubah.php?id=<?= $row['id_barang'];?>" role="button">Ubah</a>
						<a class="btn btn-danger" href="hapus.php?id=<?= $row['id_barang'];?>" role="button">Hapus</a>
					</td>
					</tr>
					<?php endwhile; else: ?>
					<tr>
						<td colspan="7">Belum ada data</td>
					</tr>
					<?php endif; ?>
				</thead>
            </table>
        </div>
    </div>
</div>
</body>
</html>
```
![2](https://github.com/Hafidza1/Lab8web/assets/115520666/0b7cf4b2-32b7-4745-8504-38185ad218c5)

**Mengubah Data (Update)**
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
	$sql = 'INSERT INTO data_barang (nama, kategori,harga_jual, harga_beli, stok, gambar) ';
	$sql .= "VALUE ('{$nama}', '{$kategori}','{$harga_jual}', '{$harga_beli}', '{$stok}', '{$gambar}')";
	$result = mysqli_query($conn, $sql);
	header('location: index.php');
}
?>

<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<link rel="stylesheet" href="css/bootstrap.min.css" />
    <script src="js/bootstrap.min.js"></script>
	<title>Tambah Barang</title>
	<style>
	form div > label {
			display: inline-block;
			width: 100px;
			height: 30px;
	}
	form div > label {
			display: inline-block;
			width: 100px;
			height: 50px;
	}
	form input[type="text"], form textarea {
			border: 1px solid;
	}
	form input[type="submit"] {
		border: 1px solid #2E8B57;
		background-color: #2E8B57;
		color: #ffffff;
		font-weight: bold;
		padding: 5px 15px;
	}
	</style>
</head>
<body>
	<div class="container">
	<h1 align="center">Tambah Barang</h1>
		<table class="table table-striped table-sm">
			<td><td>
		</table>
	<div class="main">
		<form method="post" action="tambah.php" enctype="multipart/form-data">
			<div class="input">
				<label>Nama Barang</label>
				<input type="text" name="nama" />
			</div>
			<div class="input">
				<label>Kategori</label>
					<select name="kategori">
					<option value="Komputer">Komputer</option>
					<option value="Elektronik">Elektronik</option>
					<option value="Hand Phone">Hand Phone</option>
				</select>
			</div>
			<div class="input">
				<label>Harga Jual</label>
				<input type="text" name="harga_jual" />
			</div>
			<div class="input">
				<label>Harga Beli</label>
				<input type="text" name="harga_beli" />
			</div>
			<div class="input">
				<label>Stok</label>
				<input type="text" name="stok" />
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
</fieldset>
</body>
</html>
```
![3](https://github.com/Hafidza1/Lab8web/assets/115520666/67296edc-b575-4484-afd8-477871479932)

![4](https://github.com/Hafidza1/Lab8web/assets/115520666/facbe7f4-297a-437f-9989-c31ba41c8a92)

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
        $destination = dirname(_FILE_) . '/gambar/' . $filename;
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
	<link rel="stylesheet" href="css/bootstrap.min.css" />
    <script src="js/bootstrap.min.js"></script>
    <title>Ubah Barang</title>
	<style>
	form div > label {
			display: inline-block;
			width: 100px;
			height: 30px;
	}
	form div > label {
			display: inline-block;
			width: 100px;
			height: 50px;
	}
	form input[type="text"], form textarea {
			border: 1px solid;
	}
	form input[type="submit"] {
		border: 1px solid #2E8B57;
		background-color: #2E8B57;
		color: #ffffff;
		font-weight: bold;
		padding: 5px 15px;
	}
	</style>
</head>
<body>
<div class="container">
    <h1 align="center">Ubah Barang</h1>
		<table class="table table-striped table-sm">
			<td><td>
		</table>
    <div class="main">
        <form method="post" action="ubah.php" enctype="multipart/form-data">
            <div class="input">
                <label>Nama Barang</label>
                <input type="text" name="nama" value="<?php echo $data['nama'];?>"/>
            </div>
            <div class="input">
                <label>Kategori</label>
                <select name="kategori">
                    <option <?php echo is_select ('Komputer', $data['kategori']);?> value="Komputer">Komputer</option>
                    <option <?php echo is_select ('Komputer', $data['kategori']);?> value="Elektronik">Elektronik</option>
                    <option <?php echo is_select ('Komputer', $data['kategori']);?> value="Hand Phone">Hand Phone</option>
                </select>
            </div>
            <div class="input">
                <label>Harga Jual</label>
                <input type="text" name="harga_jual" value="<?php echo $data['harga_jual'];?>"/>
            </div>
            <div class="input">
                <label>Harga Beli</label>
                <input type="text" name="harga_beli" value="<?php echo $data['harga_beli'];?>"/>
            </div>
            <div class="input">
                <label>Stok</label>
                <input type="text" name="stok" value="<?php echo $data['stok'];?>"/>
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
![5](https://github.com/Hafidza1/Lab8web/assets/115520666/61ac7a74-b54a-4410-94ac-8107daf1c73f)

![6](https://github.com/Hafidza1/Lab8web/assets/115520666/5454435d-88c6-4cdd-843d-755d2e3df9a0)

**Menghapus Data(Delete)**

```
<?php
include_once 'koneksi.php';
$id = $_GET['id'];
$sql = "DELETE FROM data_barang WHERE id_barang = '{$id}'";
$result = mysqli_query($conn, $sql);
header('location: index.php');
?>
```
![7](https://github.com/Hafidza1/Lab8web/assets/115520666/97d78b77-f9fb-4e16-80b3-52fac2c798e8)
