# Lab3Web
## Profil
| #               | Biodata              |
| --------------- | -------------------- |
| **Nama**        | Nabilah Ananda Putri |
| **NIM**         | 312110263            |
| **Kelas**       | TI.21.A.1            |
| **Mata Kuliah** | Pemrograman Web 2    |

## Menjalankan MySQL Server
- Untuk menjalankan MySQL Server dari menu XAMPP Control.

![Screenshot (8)](https://user-images.githubusercontent.com/92380488/227859131-0a99465a-ae67-49bf-8997-56c59f1cb175.png)

- Pastikan Web server Apache dan MySQL Server sudah dijalankan. Kemudian buka melalui browser: http://localhost/phpmyadmin/

## Membuat Database
1. Pilih menu `SQL` lalu jalankan perintah berikut.

```sql
CREATE DATABASE latihan1;
```

2. Membuat Tabel

```sql
USE latihan1;
CREATE TABLE data_barang(
    id_barang int(11) PRIMARY KEY AUTO_INCREMENT,
    nama_barang varchar(30) NOT NULL,
    kategori_barang varchar(30) NOT NULL,
    gambar_barang text NOT NULL,
    harga_beli decimal(10,0) NOT NULL,
    harga_jual decimal(10,0) NOT NULL,
    stok int(4) NOT NULL
);
```
![Screenshot (29)](https://user-images.githubusercontent.com/92380488/227858601-35ac0914-f437-47ce-9765-1f29af70dbbc.png)

3. Menambahkan Data

```sql
INSERT INTO `data_barang` (`id_barang`, `nama_barang`, `kategori_barang`, `gambar_barang`, `harga_beli`, `harga_jual`, `stok`) VALUES (NULL, 'Iphone 13', 'Elektronik', 'img/IP13.jpg', '18000000', '17500000', '1'), (NULL, 'Samsung S21 Ultra', 'Elektronik', 'img/SamsungS21.jpg', '20000000', '19800000', '2');
```

## Membuat Koneksi database
- Buat file baru dengan nama `koneksi.php`

```php
<?php
$koneksi = mysqli_connect('localhost', 'root', '', 'latihan1');

if ($koneksi == false) {
    echo "Koneksi gagal";
    die();
} 

// var_dump($koneksi);
```

## Membuat file index untuk menampilkan data (*Read*)
- Buatlah file baru dengan nama `index.php`

```php
<?php
include "koneksi.php";

$query = "SELECT * FROM data_barang";
$result = mysqli_query($koneksi, $query);
?>

<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta http-equiv="X-UA-Compatible" content="IE=edge" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>CRUD Sederhana</title>

    <link
      rel="stylesheet"
      href="https://cdn.jsdelivr.net/npm/bootstrap@5.2.0-beta1/dist/css/bootstrap.min.css"
    />
  </head>

  <body>
    <div>
      <h4 class="py-2 px-3" style="background-color: #667fa0; color: white;">
      <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" fill="currentColor" class="bi bi-clipboard-check my-2" viewBox="0 0 16 16">
     </svg> Data Barang</h4>
    </div>
    <div class="container">
      <a href="tambah.php" class="btn btn-success btn-sm mb-4 float-end">Tambah Barang</a>

      <table class="table table-sm table-bordered">
        <tr class="text-center fw-bold text-uppercase">
          <td>No</td>
          <td>Gambar</td>
          <td>Nama</td>
          <td>Kategori</td>
          <td>Harga Beli</td>
          <td>Harga Jual</td>
          <td>Stok</td>
          <td>Aksi</td>
        </tr>
        <?php
        if ($result->num_rows > 0) { 
          
          // die(); 
          $no = 1; 
          while ($data = mysqli_fetch_array($result)) { 
            // var_dump($data['nama_barang']); 
            ?>
        <tr>
          <td class="text-center"><?= $no++ ?></td>
          <td class="text-center">
            <img
              src="gambar/<?= $data['gambar_barang'] ?>"
              alt="<?= $data['nama_barang'];?>"
              width="100px" />
          </td>
          <td><?= $data['nama_barang'] ?></td>
          <td><?= $data['kategori_barang'] ?></td>
          <td>
            Rp.<?= $data['harga_beli'] ?>
          </td>
          <td>
            Rp.<?= $data['harga_jual'] ?>
          </td>
          <td><?= $data['stok'] ?></td>
          <td class="text-center">
            <a
              href="ubah.php?id_barang=<?= $data['id_barang'] ?>"
              class="btn btn-warning btn-sm mx-1">Edit</a>
            <a
              href="proses.php?id_barang=<?= $data['id_barang'] ?>&aksi=hapus"
              class="btn btn-danger btn-sm mx-1">Delete</a>
          </td>
        </tr>
        <?php
                }
            } else {
                ?>
        <tr>
          <td colspan="8" class="text-center">Data Kosong</td>
        </tr>
        <?php
            }
            ?>
      </table>
    </div>
  </body>
</html>
```

## Menambahkan Data (*Create*)
- Buatlah file baru dengan nama `tambah.php`

```php
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta http-equiv="X-UA-Compatible" content="IE=edge" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>CRUD Sederhana</title>

    <link
      rel="stylesheet"
      href="https://cdn.jsdelivr.net/npm/bootstrap@5.2.0-beta1/dist/css/bootstrap.min.css"
    />
  </head>

  <body>
    <div class="container">
      <div class="row m-0">
        <div class="col-md-5 mx-auto">
          <div class="card mt-3">
            <div class="card-header text-center">
              <h3>Tambah Barang</h3>
            </div>
            <div class="card-body">
              <form
                action="proses.php"
                method="post"
                enctype="multipart/form-data"
              >
                <div class="mb-3">
                  <label for="nama_barang" class="form-label"
                    >Nama Barang</label
                  >
                  <input
                    type="text"
                    name="nama_barang"
                    id="nama_barang"
                    placeholder="Masukan nama barang"
                    class="form-control"
                  />
                </div>
                <div class="mb-3">
                  <label for="kategori_barang" class="form-label"
                    >Kategori Barang</label
                  >
                  <input
                    type="text"
                    name="kategori_barang"
                    id="kategori_barang"
                    placeholder="Masukan kategori barang"
                    class="form-control"
                  />
                </div>

                <label for="harga_beli" class="form-label">Harga Beli</label>
                <div class="input-group mb-3">
                  <span class="input-group-text">Rp.</span>
                  <input
                    type="number"
                    name="harga_beli"
                    id="harga_beli"
                    placeholder="Masukan Harga Beli"
                    class="form-control"
                  />
                </div>

                <label for="harga_jual" class="form-label">Harga Jual</label>
                <div class="input-group mb-3">
                  <span class="input-group-text">Rp.</span>
                  <input
                    type="number"
                    name="harga_jual"
                    id="harga_jual"
                    placeholder="Masukan Harga Jual"
                    class="form-control"
                  />
                </div>

                <div class="mb-3">
                  <label for="stok" class="form-label">Stok</label>
                  <input
                    type="number"
                    class="form-control"
                    name="stok"
                    placeholder="Masukan Stok Barang"
                  />
                </div>

                <div class="mb-3">
                  <label for="gambar" class="form-label">Gambar</label>
                  <input
                    type="file"
                    name="gambar_barang"
                    id="gambar"
                    class="form-control form-control-sm"
                  />
                </div>

                <a href="index.php" class="btn btn-secondary">Kembali</a>
                <button class="btn btn-success" name="tambah" type="submit">
                  Tambah
                </button>
              </form>
            </div>
          </div>
        </div>
      </div>
    </div>
  </body>
</html>
```

![Screenshot (34)](https://user-images.githubusercontent.com/92380488/227861222-63eacd70-c6f7-4ec0-969f-6a43f9de05b7.png)

## Mengubah Data (*Update)
- Buatlah file baru dengan nama `ubah.php`

```php
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta http-equiv="X-UA-Compatible" content="IE=edge" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>CRUD Sederhana</title>

    <link
      rel="stylesheet"
      href="https://cdn.jsdelivr.net/npm/bootstrap@5.2.0-beta1/dist/css/bootstrap.min.css"
    />
  </head>

  <body>
    <div class="container">
      <div class="row m-0">
        <div class="col-md-5 mx-auto">
          <div class="card mt-3">
            <div class="card-header text-center">
              <h3>Ubah Barang</h3>
            </div>
            <div class="card-body">
              <?php
                        include "koneksi.php";

                        $id = $_GET['id_barang'];
                        $query = "SELECT * FROM data_barang ";
                        $query .= "WHERE id_barang = $id";

                        $result = mysqli_query($koneksi, $query);
                        $data = mysqli_fetch_array($result);
                        ?>
              <form
                action="proses.php"
                method="post"
                enctype="multipart/form-data"
              >
                <input type="hidden" name="id_barang" value="<?= $id ?>" />
                <div class="mb-3">
                  <label for="nama_barang" class="form-label"
                    >Nama Barang</label
                  >
                  <input
                    type="text"
                    name="nama_barang"
                    id="nama_barang"
                    placeholder="Masukan nama barang"
                    class="form-control"
                    value="<?= $data['nama_barang'] ?>"
                  />
                </div>
                <div class="mb-3">
                  <label for="kategori_barang" class="form-label"
                    >Kategori Barang</label
                  >
                  <input
                    type="text"
                    name="kategori_barang"
                    id="kategori_barang"
                    placeholder="Masukan kategori barang"
                    class="form-control"
                    value="<?= $data['kategori_barang']?>"
                  />
                </div>

                <label for="harga_beli" class="form-label">Harga Beli</label>
                <div class="input-group mb-3">
                  <span class="input-group-text">Rp.</span>
                  <input
                    type="number"
                    name="harga_beli"
                    id="harga_beli"
                    placeholder="Masukan Harga Beli"
                    class="form-control"
                    value="<?= $data['harga_beli']?>"
                  />
                </div>

                <label for="harga_jual" class="form-label">Harga Jual</label>
                <div class="input-group mb-3">
                  <span class="input-group-text">Rp.</span>
                  <input
                    type="number"
                    name="harga_jual"
                    id="harga_jual"
                    placeholder="Masukan Harga Jual"
                    class="form-control"
                    value="<?= $data['harga_jual']?>"
                  />
                </div>

                <div class="mb-3">
                  <label for="stok" class="form-label">Stok</label>
                  <input
                    type="number"
                    class="form-control"
                    name="stok"
                    placeholder="Masukan Stok Barang"
                    value="<?= $data['stok']?>"
                  />
                </div>

                <div class="mb-3">
                  <label for="gambar" class="form-label">Gambar</label>
                  <input
                    type="file"
                    name="gambar_barang"
                    id="gambar"
                    class="form-control form-control-sm"
                    value="<?= $data['gambar_barang']?>"
                  />
                </div>

                <a href="index.php" class="btn btn-secondary">Kembali</a>
                <button class="btn btn-warning" name="ubah" type="submit">
                  Edit
                </button>
              </form>
            </div>
          </div>
        </div>
      </div>
    </div>
  </body>
</html>
```
![Screenshot (35)](https://user-images.githubusercontent.com/92380488/227861509-31fa4914-4116-4366-a489-80998d52f0dc.png)


## Menghapus Data (Delete)
- Pada button delete, Ubah hrefnya ke `proses.php` dengan membawa 2 parameter yaitu id_barang dan aksi.
- Tambahkan kode berikut ke dalam file `proses.php`.

```php
    // -- Hapus barang --
} else if (isset($_GET['id_barang']) && $_GET['aksi'] == "hapus") {

    $id = $_GET['id_barang'];
    $query = "DELETE FROM data_barang WHERE id_barang = $id";

    $result = mysqli_query($koneksi, $query);

    header('location:index.php');
}
```

