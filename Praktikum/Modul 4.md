# Basic Routing dan Migration

# Langkah Percobaan
## 1. GET
Untuk menambahkan endpoint dengan method GET pada aplikasi kita, kita dapat mengunjungi file web.php pada folder routes. Kemudian tambahkan baris ini pada akhir file
```javascript
...

$router->get('/get', function () {
    return 'GET';
});
```
![Screenshot connection](../Laprak4/1.png) <br><br>

Setelah itu coba jalankan aplikasi dengan command,
```javascript
php -S localhost:8000 -t public
```
![Screenshot connection](../Laprak4/1.1.png) <br><br>

## 2. POST, PUT, PATCH, DELETE, dan OPTIONS
Sama halnya saat menambahkan method GET, kita dapat menambahkan methode POST, PUT, PATCH, DELETE, dan OPTIONS pada file web.php dengan code seperti ini,
```javascript
...
$router->post('/post', function () {
    return 'POST';
});

$router->put('/put', function () {
    return 'PUT';
});

$router->patch('/patch', function () {
    return 'PATCH';
});

$router->delete('/delete', function () {
    return 'DELETE';
});

$router->options('/options', function () {
    return 'OPTIONS';
});
```
![Screenshot connection](../Laprak4/2.png) <br><br>
Setelah selesai menambahkan route untuk method POST, PUT, PATCH, DELETE, dan OPTIONS, kita dapat menjalankan server seperti pada saat percobaan GET. Setelah server berhasil menyala, kita dapat membuka aplikasi Postman atau Insomnia atau kita juga dapat menggunakan PowerShell (Windows) / Terminal (Linux atau Mac) untuk
melakukan request ke server. Namun, pada percobaan kali ini kita akan menggunakan extensions pada VSCode yaitu Thunder Client.

### a. Kita dapat menginstall ekstensi dengan membuka panel extensions lalu mencari thunder client
![Screenshot connection](../Laprak4/2.1.png) <br><br>

### b. Setelah menginstall Thunder Client, kita akan melihat logo seperti petir pada activity bar kita (sebelah kiri).
![Screenshot connection](../Laprak4/2.2.png) <br><br>

### c. Kita dapat membuat request dengan menekan "New Request" pada ekstensi
![Screenshot connection](../Laprak4/2.9.png) <br><br>

### d. Setelah itu kita dapat memasukkan method dan url yang dituju
![Screenshot connection](../Laprak4/2.10.png) <br><br>

### e. Akses url yang baru saja ditambahkan pada aplikasi dengan methodnya
![Screenshot connection](../Laprak4/2.3.png) <br><br>
![Screenshot connection](../Laprak4/2.4.png) <br><br>
![Screenshot connection](../Laprak4/2.5.png) <br><br>
![Screenshot connection](../Laprak4/2.6.png) <br><br>
![Screenshot connection](../Laprak4/2.7.png) <br><br>
![Screenshot connection](../Laprak4/2.8.png) <br><br>

## 3. Migrasi Database
### a. Sebelum melakukan migrasi database pastikan server database aktif kemudian pastikan sudah membuat database dengan nama ```lumenapi```
![Screenshot connection](../Laprak4/3.1.png) <br><br>

### b. Kemudian ubah konfigurasi database pada file .env menjadi seperti ini
```javascript
DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=lumenapi
DB_USERNAME=root
DB_PASSWORD=<<password masing-masing>>
```
![Screenshot connection](../Laprak4/3.2.png) <br><br>

### c. Setelah mengubah konfigurasi pada file .env, kita juga perlu menghidupkan beberapa library bawaan dari lumen dengan membuka file app.php pada folder bootstrap dan mengubah baris ini,
```javascript
//$app->withFacades();
//$app->withEloquent();
```

### Menjadi, 
```javascript
$app->withFacades();
$app->withEloquent();
```
![Screenshot connection](../Laprak4/3.3.png) <br><br>

### d. Setelah itu jalankan command berikut untuk membuat file migration,
```javascript
php artisan make:migration create_users_table # membuat migrasi untuk tabel users
php artisan make:migration create_products_table # membuat migrasi untuk tabel products
```
![Screenshot connection](../Laprak4/3.4.png) <br><br>

Setelah menjalankan 2 syntax diatas akan terbuat 2 file pada folder ```database/migrations``` dengan format YYYY_MM_DD_HHmmss_nama_migrasi. Pada file migrasi kita akan menemukan fungsi up() dan fungsi down(), fungsi up() akan digunakan pada saat kita melakukan migrasi, fungsi down() akan digunakan saat kita ingin me-rollback migrasi

### e. Ubah fungsi up pada file migrasi ```create_users_table```
```javascript
# sebelumnya
...
    public function up()
    {
        Schema::create('users', function (Blueprint $table) {
            $table->id();
            $table->timestamps();
        });
    }
...

# diubah menjadi
...
    public function up()
    {
        Schema::create('users', function (Blueprint $table) {
            $table->id();
            $table->timestamps();
            $table->string('name');
            $table->string('email');
            $table->string('password');
        });
    }
...
```
![Screenshot connection](../Laprak4/3.5.png) <br><br>

### f. Ubah fungsi up pada file migrasi ```create_products_table```
```javascript
# sebelumnya
...
    public function up()
    {
        Schema::create('products', function (Blueprint $table) {
            $table->id();
            $table->timestamps();
        });
    }
...

# diubah menjadi
...
    public function up()
    {
        Schema::create('products', function (Blueprint $table) {
            $table->id();
            $table->timestamps();
            $table->string('name');
            $table->integer('category_id');
            $table->string('slug');
            $table->integer('price');
            $table->integer('weight');
            $table->text('description');
        });
    }
...
```
![Screenshot connection](../Laprak4/3.6.png) <br><br>

### g. Kemudian jalankan command,
```javascript
php artisan migrate
```
![Screenshot connection](../Laprak4/3.7.png) <br><br>
![Screenshot connection](../Laprak4/3.8.png) <br><br>