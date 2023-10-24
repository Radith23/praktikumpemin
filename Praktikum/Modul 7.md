# Relasi One-to-Many dan Many-to-Many

# Dasar Teori
## Relasi
Hubungan antar tabel yang dilakukan dengan pencocokan primary key dengan foreign key untuk mengombinasikan data dari satu tabel dengan tabel lainnya.

## Foreign Key
Properti yang digunakan untuk menandai hubungan dua tabel atau lebih. Foreign key
pada tabel anak (child) akan menunjuk tabel induk (parent) yang menjadi referensinya (reference).

## One-to-Many
Relasi yang menunjukkan hubungan antar tabel dimana baris pada tabel induk dapat
terhubung dengan satu atau lebih baris di tabel anak. Sementara baris pada tabel anak hanya dapat terhubung dengan satu baris di tabel induk.
Contoh penerapan one-to-many
<li>Satu tutorial dapat memiliki banyak komentar, namun satu komentar hanya dapat
berada di satu tutorial</li>
<li>Satu dosbing dapat memiliki banyak mahasiswa, namun mahasiswa hanya dapat
dibimbing satu dosen</li>

![Screenshot connection](../Laprak7/Teori_1.png) <br><br>

## Many-to-Many
Relasi yang menunjukkan hubungan antar tabel dimana baris pada tabel induk dapat
terhubung dengan satu atau lebih baris di tabel anak. Berlaku sebaliknya pada tabel anak yang dapat terhubung dengan satu atau lebih baris di tabel induk.

Contoh penerapan many-to-many
<li>Satu mahasiswa dapat mengambil banyak mata kuliah, namun satu mata kuliah
dapat diambil banyak mahasiswa</li>
<li>Postingan dapat memiliki banyak tag, namun satu tag dapat dimiliki banyak
postingan.</li>

![Screenshot connection](../Laprak7/Teori_2.png) <br><br>
Kombinasi baris pada relasi many-to-many diatur dengan junction table.

## Junction Table
Tabel yang digunakan untuk mengatur kombinasi baris pada relasi many-to-many.
Junction table berisi foreign key dari kedua tabel yang memiliki relasi many-to-many. Contoh penerapan junction table adalah tabel Enrollment pada relasi many-to-many di atas. <br><br>

# Langkah Percobaan
## Pembuatan Tabel
Berikut adalah tabel yang akan digunakan pada percobaan ini
<table>
 	<tr>
 		<td> posts </td>
        <td> comments </td>
        <td> tags </td>
        <td> post_tag </td>
 	</tr>
 	<tr>
 		<td> content (STRING) </td>
        <td> review (STRING) </td>
        <td> name </td>
        <td> tagId </td>
 	</tr>

 </table>
1. Sebelum membuat migrasi database atau membuat tabel pastikan server database
aktif kemudian pastikan sudah membuat database dengan nama ```lumenpost```

![Screenshot connection](../Laprak7/1.png) <br><br>

2. Kemudian ubah konfigurasi database pada file .env menjadi seperti berikut
```javascript
DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=lumenpost
DB_USERNAME=root
DB_PASSWORD=
```

![Screenshot connection](../Laprak7/1.1.png) <br><br>

3. Setelah mengubah konfigurasi pada file .env, kita juga perlu menghidupkan
beberapa library bawaan dari lumen dengan membuka file app.php pada folder
bootstrap dan mengubah baris ini
```javascript
// $app->withFacades();
// $app->withEloquent();
```
![Screenshot connection](../Laprak7/1.2.png) <br><br>
menjadi
```javascript
$app->withFacades();
$app->withEloquent();
```
![Screenshot connection](../Laprak7/1.2.1.png) <br><br>

4. Setelah itu jalankan command berikut untuk membuat file migration
```javascript
php artisan make:migration create_posts_table
php artisan make:migration create_comments_table
php artisan make:migration create_tags_table
php artisan make:migration create_post_tag_table
```
![Screenshot connection](../Laprak7/1.3.png) <br><br>

5. Ubah fungsi ```up()``` pada file ```migrasi create_posts_table```
```javascript
#sebelumnya
...
public function up()
{
    Schema::create('posts', function (Blueprint $table) {
        $table->id();
        $table->timestamps();
    });
}
...

#diubah menjadi
...
public function up()
{
    Schema::create('posts', function (Blueprint $table) {
        $table->id();
        $table->timestamps();
        $table->string('content');
    });
}
...
```
![Screenshot connection](../Laprak7/1.4.1.png) <br><br>
![Screenshot connection](../Laprak7/1.4.2.png) <br><br>

6. Ubah fungsi ```up()``` pada file ```create_comments_table```
```javascript
#sebelumnya
...
public function up()
{
    Schema::create('comments', function (Blueprint $table) {
        $table->id();
        $table->timestamps();
    });
}
...

#diubah menjadi
...
public function up()
{
    Schema::create('comments', function (Blueprint $table) {
        $table->id();
        $table->timestamps();
        $table->string('review');
        $table->foreignId('postId')->unsigned();
    });
}
...
```
![Screenshot connection](../Laprak7/1.5.1.png) <br><br>
![Screenshot connection](../Laprak7/1.5.2.png) <br><br>

7. Ubah fungsi ```up()``` pada file ```create_tags_table```
```javascript
#sebelumnya
...
public function up()
{
    Schema::create('tags', function (Blueprint $table) {
        $table->id();
        $table->timestamps();
    });
}
...

#diubah menjadi
...
public function up()
{
    Schema::create('tags', function (Blueprint $table) {
        $table->id();
        $table->timestamps();
        $table->string('name');
    });
}
...
```
![Screenshot connection](../Laprak7/1.6.1.png) <br><br>
![Screenshot connection](../Laprak7/1.6.2.png) <br><br>

8. Ubah fungsi ```up()``` pada file ```create_post_tag_table```
```javascript
#sebelumnya
...
public function up()
{
    Schema::create('post_tag', function (Blueprint $table) {
        $table->id();
        $table->timestamps();
    });
}
...

#diubah menjadi
...
public function up()
{
    Schema::create('post_tag', function (Blueprint $table) {
        $table->id();
        $table->timestamps();
        $table->foreignId('postId')->unsigned();
        $table->foreignId('tagId')->unsigned();
    });
}
...
```
![Screenshot connection](../Laprak7/1.7.1.png) <br><br>
![Screenshot connection](../Laprak7/1.7.2.png) <br><br>

9. Kemudian jalankan command
```javascript
php artisan migrate
```
![Screenshot connection](../Laprak7/1.8.png) <br><br>

## Pembuatan Model
1. Buatlah file dengan nama Post.php dan isi dengan baris kode berikut
```javascript
<?php

namespace App\Models;

use Illuminate\Database\Eloquent\Model;

class Post extends Model
{
    /**
     * * The attributes that are mass assignable.
     * *
     * * @var string[]
     * */
    protected $fillable = [
        'content'
    ];
    
    /**
     * * The attributes excluded from the model's JSON form.
     * *
     * * @var string[]
     * */
    protected $hidden = [];
}
```
![Screenshot connection](../Laprak7/2.1.png) <br><br>

2. Buatlah file dengan nama Comment.php dan isi dengan baris kode berikut
```javascript
<?php

namespace App\Models;

use Illuminate\Database\Eloquent\Model;

class Comment extends Model
{
    /**
     * * The attributes that are mass assignable.
     * *
     * * @var string[]
     * */
    protected $fillable = [
        'review'
    ];
    
    /**
     * * The attributes excluded from the model's JSON form.
     * *
     * * @var string[]
     * */
    protected $hidden = [];
}
```
![Screenshot connection](../Laprak7/2.2.png) <br><br>

3. Buatlah file dengan nama Tag.php dan isi dengan baris kode berikut
```javascript
<?php

namespace App\Models;

use Illuminate\Database\Eloquent\Model;

class Tag extends Model
{
    /**
     * * The attributes that are mass assignable.
     * *
     * * @var string[]
     * */
    protected $fillable = [
        'name'
    ];
    
    /**
     * * The attributes excluded from the model's JSON form.
     * *
     * * @var string[]
     * */
    protected $hidden = [];
}
```
![Screenshot connection](../Laprak7/2.3.png) <br><br>

## Relasi One-to-Many
1. Tambahkan fungsi comments() pada file Post.php
```javascript
<?php

namespace App\Models;

use Illuminate\Database\Eloquent\Model;

class Post extends Model
{
    ...
    
    // fungsi comments
    public function comments()
    {
        return $this->hasMany(Comment::class, 'postId');
    }
}
```
![Screenshot connection](../Laprak7/3.1.png) <br><br>

2. Tambahkan fungsi post() dan atribut postId pada $fillable pada file Comment.php
```javascript
<?php

namespace App\Models;

use Illuminate\Database\Eloquent\Model;

class Comment extends Model
{
    ...
    
    protected $fillable = [
        'review',
        'postId' // atribut postId
    ];
    
    /**
     * * The attributes excluded from the model's JSON form.
     * *
     * * @var string[]
     * */
    protected $hidden = [];
    
    public function post()
    {
        return $this->belongsTo(Post::class, 'postId');
    }
}
```
![Screenshot connection](../Laprak7/3.2.png) <br><br>

3. Buatlah file PostController.php dan isilah dengan baris kode berikut
```javascript
<?php

namespace App\Http\Controllers;

use App\Models\Post;
use Illuminate\Http\Request;

class PostController extends Controller
{
    /**
     * * Create a new controller instance.
     * *
     * * @return void
     * */
    public function __construct()
    {
        //
    }
    
    //
    public function createPost(Request $request)
    {
        $post = Post::create([
            'content' => $request->content,
        ]);
        
        return response()->json([
            'success' => true,
            'message' => 'New post created',
            'data' => [
                'post' => $post
            ]
        ]);
    }
    
    public function getPostById(Request $request)
    {
        $post = Post::find($request->id);
        
        return response()->json([
            'success' => true,
            'message' => 'All post grabbed',
            'data' => [
                'post' => [
                    'id' => $post->id,
                    'content' => $post->content,
                    'comments' => $post->comments,
                ]
            ]
        ]);
    }
}
```
![Screenshot connection](../Laprak7/3.3.1.png) <br><br>
![Screenshot connection](../Laprak7/3.3.2.png) <br><br>

4. Buatlah file CommentController.php dan isilah dengan baris kode berikut
```javascript
<?php

namespace App\Http\Controllers;

use App\Models\Comment;
use Illuminate\Http\Request;

class CommentController extends Controller
{
    /**
     * * Create a new controller instance.
     * *
     * * @return void
     * */
    public function __construct()
    {
        //
    }

    //
    public function createComment(Request $request)
    {
        $comment = Comment::create([
            'review' => $request->review,
            'postId' => $request->postId,
        ]);
        
        return response()->json([
            'success' => true,
            'message' => 'New comment created',
            'data' => [
                'comment' => $comment
            ]
        ]);
    }
}
```
![Screenshot connection](../Laprak7/3.4.png) <br><br>

5. Tambahkan baris berikut pada routes/web.php
```javascript
<?php

...

$router->group(['prefix' => 'posts'], function () use ($router) {
    $router->post('/', ['uses' => 'PostController@createPost']);
    $router->get('/{id}', ['uses' => 'PostController@getPostById']);
});

$router->group(['prefix' => 'comments'], function () use ($router) {
    $router->post('/', ['uses' => 'CommentController@createComment']);
});
```
![Screenshot connection](../Laprak7/3.5.png) <br><br>

6. Buatlah satu post menggunakan Postman
![Screenshot connection](../Laprak7/3.6.png) <br><br>

7. Buatlah satu comment menggunakan Postman
![Screenshot connection](../Laprak7/3.7.png) <br><br>

8. Tampilkan post menggunakan Postman
![Screenshot connection](../Laprak7/3.8.png) <br><br>

## Relasi Many-to-Many
1. Tambahkan fungsi tags() pada file Post.php
```javascript
<?php

namespace App\Models;

use Illuminate\Database\Eloquent\Model;

class Post extends Model
{
    ...
    
    public function tags()
    {
        return $this->belongsToMany(Tag::class, 'post_tag', 'postId', 'tagId');
    }
}
```
![Screenshot connection](../Laprak7/4.1.png) <br><br>

2. Tambahkan fungsi posts() pada file Tag.php
```javascript
<?php

namespace App\Models;

use Illuminate\Database\Eloquent\Model;

class Tag extends Model
{
    ...
    
    public function posts()
    {
        return $this->belongsToMany(Post::class, 'post_tag', 'tagId', 'postId');
    }
}
```
![Screenshot connection](../Laprak7/4.2.png) <br><br>

3. Buatlah file TagController.php dan isilah dengan baris kode berikut
```javascript
<?php

namespace App\Http\Controllers;

use App\Models\Tag;
use Illuminate\Http\Request;

class TagController extends Controller
{
    /**
     * * Create a new controller instance.
     * *
     * * @return void
     * */

    public function __construct()
    {
        //
    }

    //
    public function createTag(Request $request)
    {
        $tag = Tag::create([
            'name' => $request->name
        ]);
        
        return response()->json([
            'success' => true,
            'message' => 'New tag created',
            'data' => [
                'tag' => $tag
            ]
        ]);
    }
}
```
![Screenshot connection](../Laprak7/4.3.png) <br><br>

4. Tambahkan fungsi addTag dan response tags pada PostController.php
```javascript
<?php

namespace App\Http\Controllers;

use App\Models\Post;
use Illuminate\Http\Request;

class PostController extends Controller
{
    ...
    
    public function getPostById(Request $request)
    {
        $post = Post::find($request->id);
        
        return response()->json([
            'success' => true,
            'message' => 'All post grabbed',
            'data' => [
                'post' => [
                    'id' => $post->id,
                    'content' => $post->content,
                    'comments' => $post->comments,
                    'tags' => $post->tags, //response tags
                ]
            ]
        ]);
    }

    public function addTag(Request $request)
    {
        $post = Post::find($request->id);

        $post->tags()->attach($request->tagId);
        
        return response()->json([
            'success' => true,
            'message' => 'Tag added to post',
        ]);
    }
}
```
![Screenshot connection](../Laprak7/4.4.png) <br><br>

5. Tambahkan baris berikut pada routes/web.php
```javascript
$router->group(['prefix' => 'posts'], function () use ($router) {
    $router->post('/', ['uses' => 'PostController@createPost']);
    $router->get('/{id}', ['uses' => 'PostController@getPostById']);
    $router->put('/{id}/tag/{tagId}', ['uses' => 'PostController@getPostById']); //
});

...

$router->group(['prefix' => 'tags'], function () use ($router) {
    $router->post('/', ['uses' => 'TagController@createTag']);
});
```
![Screenshot connection](../Laprak7/4.5.png) <br><br>

6. Buatlah satu tag menggunakan Postman
![Screenshot connection](../Laprak7/4.6.png) <br><br>

7. Tambahkan tag “jadul” pada post “disana engkau berdua”
![Screenshot connection](../Laprak7/4.7.png) <br><br>

8. Tampilkan post “disana engkau berdua” menggunakan Postman
![Screenshot connection](../Laprak7/4.8.png) <br><br>

9. Buatlah postingan “tanpamu apa artinya” menggunakan Postman
![Screenshot connection](../Laprak7/4.9.png) <br><br>

10. Tambahkan tag “jadul” pada postingan “tanpamu apa artinya”
![Screenshot connection](../Laprak7/4.10.png) <br><br>

11. Buatlah tag “lagu” menggunakan Postman
![Screenshot connection](../Laprak7/4.11.png) <br><br>

12. Tambahkan tag “lagu” pada postingan “tanpamu apa artinya”
![Screenshot connection](../Laprak7/4.12.png) <br><br>

13. Tampilkan post pertama
![Screenshot connection](../Laprak7/4.13.png) <br><br>

14. Tampilkan post kedua
![Screenshot connection](../Laprak7/4.14.png) <br><br>