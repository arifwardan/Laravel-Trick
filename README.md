# Laravel Trik
Kumpulan trik berbahasa indonesia untuk menggunakan framework laravel.

_Berisi: **9** trik._

**Terakhir diupdate 25 Oktober 2020**

> Berikan pull request untuk memberikan manfaat lebih banyak !

# Daftar Isi :

- [DB Models dan Eloquent](#db-models-dan-eloquent) (2 trik).
- [Perintah `artisan`](#perintah-artisan) (1 trik).
- [Package](#package) (7 trik).
- [Templating](#templating) (1 trik).
- [Basis Data (Database)](#basis-data-database) (1 trik).
- [Middleware](#middleware)(1 trik).
- [Lain - lain](#lain-lain) (1 trik).

## DB Models dan Eloquent

⬆️ [Ke Atas](#laravel-trik) ➡️ [Berikutnya (Perintah `artisan`)](#perintah-artisan)

- [Cara mengubah format output `created_at` dan `updated_at` lewat model](#cara-mengubah-format-output-created_at-dan-updated_at-lewat-model)

- [Cara Mengubah format text `created_at` dan `updated_at` menjadi `tgl_dibuat` dan `tgl_diupdate` lewat model](#cara-mengubah-format-text-created_at-dan-updated_at-menjadi-tgl_dibuat-dan-tgl_diupdate-lewat-model)
---

### **Cara mengubah format output `created_at` dan `updated_at` lewat model**

- **created_at**

Untuk mengubah format `created_at`, tambahkan method berikut di dalam model:

```php
public function getCreatedAtFormattedAttribute()
{
   return $this->created_at->format('H:i d, M Y');
}
```
Method ini bisa digunakan dengan cara seperti ini : `$entry->created_at_formatted`.

Dan akan menampilkan hasil seperti ini: `04:19 23, Aug 2020`.

- **updated_at**

Untuk mengubah format `updated_at`, tambahkan method berikut di dalam model:

```php
public function getUpdatedAtFormattedAttribute()
{
   return $this->updated_at->format('H:i d, M Y');
}
```
Method ini bisa digunakan dengan cara seperti ini : `$entry->updated_at_formatted`.

Dan akan menampilkan hasil seperti ini: `04:19 23, Aug 2020`.

### **Cara Mengubah format text created_at dan updated_at menjadi tgl_dibuat dan tgl_diupdate lewat model**

Caranya sangat mudah 

- **created_at**

Untuk created_at cukup menambahkan :

     const CREATED_AT = 'tgl_dibuat';

- **updated_at**

dan untuk  updated_at cukup menambahkan:

    const UPDATED_AT = 'tgl_diupdate';

## Perintah `artisan`

⬆️ [Ke Atas](#laravel-trik) ➡️ [Berikutnya (Package)](#package)

- [Cara membuat model, controller, migration sekaligus](#cara-membuat-model-controller-migration-sekaligus)
---

### **Cara membuat model, controller, migration sekaligus**

Untuk membuat `model`, `controller`, dan `migration` sekaligus cukup jalankan perintah untuk membuat model dengan tambahan `-mc`, seperti berikut :

```php
php artisan make:model User -mc
```

Jika kita ingin membuat `controller` dengan resource (default method dari laravel). Gunakan perintah ini (dengan tambahan huruf `r`):
```php
php artisan make:model User -mcr
```
## Package

⬆️ [Ke Atas](#laravel-trik) ➡️ [Berikutnya (Templating)](#templating)

- [Install Spatie Role Permission](#install-spatie-role-permission)
- [Cara Menggunakan Role](#cara-menggunakan-role)
- [Cara Menggunakan Permission](#cara-menggunakan-permission)
- [Cara Menggunakan Permission Via Role](#cara-menggunakan-permission-via-role)
- [Cara Menggunakan Spatie di Blade](#cara-menggunakan-spatie-di-blade)
- [Cara Menggunakan Spatie di Middleware](#cara-menggunakan-spatie-di-middleware)
- [Scaffolding Menggunakan Jetstream](#scaffolding-menggunakan-jetstream)
---
### **Install Spatie Role Permission**

1. Install package

 `composer require spatie/laravel-permission`
 
2. edit app/config.php
```php
'providers' => [
    // ...
    Spatie\Permission\PermissionServiceProvider::class,
];
```
3. publish migration di terminal

`php artisan vendor:publish --provider="Spatie\Permission\PermissionServiceProvider"`

4. clear config & cache

 `php artisan optimize:clear`
 **or**
 `php artisan config:clear`
 
5. terakhir jalankan migration

 `php artisan migrate`

---

### **Cara menggunakan Role**
1. membuat role 
```php
use Spatie\Permission\Models\Role;

$role = Role::create(['name' => 'writer']);
```

2. Menambahkan role kepada user
```php
$user->assignRole($role);
```
3. Menghapus role pada user
```php
$user->revokeRoleTo($role); //$role = nama rolenya apa
```
---

### **Cara Menggunakan Permission**

1. Menambahkan Permission
```php
// Tambah permission ke user
$user->givePermissionTo('edit articles');

// Tambah permission melalui role
$user->assignRole('writer');

$role->givePermissionTo('edit articles');

// Tambahkan role sekaligus
$user->givePermissionTo('edit articles', 'delete articles');

// Tambahkan role sekaligus menggunakan array
$user->givePermissionTo(['edit articles', 'delete articles']);
```

2. Menghapus Permission
```php
// Hapus permissioin dari user
$user->revokePermissionTo('edit articles');

// Hapus dan tamabah sekaligus (update)
$user->syncPermissions(['edit articles', 'delete articles']);
```

3. Mengecek Apakah User Mempunyai Permissioin
```php
$user->hasPermissionTo('edit articles');

// Cek use mengguakan id
$user->hasPermissionTo('1');
$user->hasPermissionTo(Permission::find(1)->id);
$user->hasPermissionTo($somePermission->id);

// Periksa user yang mempunya beberapa permission
$user->hasAnyPermission(['edit articles', 'publish articles', 'unpublish articles']);

// alterntif lain
$user->hasAllPermissions(['edit articles', 'publish articles', 'unpublish articles']);

// Periksa user yang mempunya beberapa permission menggunakan integer
$user->hasAnyPermission(['edit articles', 1, 5]);
```

---

### **Cara Menggunakan Permission Via Role**
1. Menerapkan Role ke User manapun
```php
$user->assignRole('writer');

// Memberikan role lebih dari satu
$user->assignRole('writer', 'admin');
// alternatif lain menggunakan array
$user->assignRole(['writer', 'admin']);
```
2. Menghapus Role dari User
```php
$user->removeRole('writer');
```
3. Update Role dari User
```php
// Semua role saat ini akan di hapus dari use dan di gantikan dari array yang di berikan
$user->syncRoles(['writer', 'admin']);
```
4. Memeriksa User mempunyai Role, biasanya di gunakan untuk validasi
```php
// Semua role saat ini akan di hapus dari use dan di gantikan dari array yang di berikan
$user->syncRoles(['writer', 'admin']);

// Memeriksa user yang mempunyai beberapa role
$user->hasAnyRole(['writer', 'reader']);
// atau
$user->hasAnyRole('writer', 'reader');

// Cek jika user punya semua role
$user->hasAllRoles(Role::all());
```
---
### **Cara Menggunakan Spatie di Blade**
1. Permission pada Blade
```php
@can('edit articles')
  //
@endcan
// atau

@if(auth()->user()->can('edit articles') && $some_other_condition)
  //
@endif
// Bisa juga menggunakan @can, @cannot, @canany, dan @guest
```
2. Role pada Blade
```php
// Persiksa sebuah role
@role('writer')
    Saya seorang penulis!
@else
    Saya bukan penulis
@endrole
// bisa juga menggunakan ini
@hasrole('writer')
    Saya seorang penulis!
@else
    Saya bukan penulis
@endhasrole


// Cek beberapa Role 
@hasanyrole($collectionOfRoles)
    Saya punya satu atau lebih dari satu role
@else
    Saya tidak punya role...
@endhasanyrole
// atau
@hasanyrole('writer|admin')
    Saya penulis atau admin atau keudannya!
@else
    Saya tidak punya role...
@endhasanyrole


// Cek Semua Role
@hasallroles($collectionOfRoles)
    Saya punya semua role!!
@else
    Saya tidak memiliki semua role...
@endhasallroles
// atau
@hasallroles('writer|admin')
    Saya penulis dan juga admin!!
@else
    Saya tidak memiliki semua role...
@endhasallroles

// Alternatif lain, @unlessrole 
@unlessrole('does not have this role')
   Saya tidak punya role
@else
    Saya punya role
@endunlessrole
```

---

### **Cara Menggunakan Spatie di Middleware**
1. Tambahkan Kodingan di bawah ini pada **app/Http/Kernel.php**
```php
protected $routeMiddleware = [
    // ...
    'role' => \Spatie\Permission\Middlewares\RoleMiddleware::class,
    'permission' => \Spatie\Permission\Middlewares\PermissionMiddleware::class,
    'role_or_permission' => \Spatie\Permission\Middlewares\RoleOrPermissionMiddleware::class,
];
```

2. Cara memvalidasi Route
```php
Route::group(['middleware' => ['role:super-admin']], function () {
    //
});

Route::group(['middleware' => ['permission:publish articles']], function () {
    //
});

Route::group(['middleware' => ['role:super-admin','permission:publish articles']], function () {
    //
});

Route::group(['middleware' => ['role_or_permission:super-admin|edit articles']], function () {
    //
});

Route::group(['middleware' => ['role_or_permission:publish articles']], function () {
    //
});
```

3. Memvalidasi Route lebih dari satu **role** atau **permission** menggunakan **| (pipa) karakter**
```php
Route::group(['middleware' => ['role:super-admin|writer']], function () {
    //
});

Route::group(['middleware' => ['permission:publish articles|edit articles']], function () {
    //
});

Route::group(['middleware' => ['role_or_permission:super-admin|edit articles']], function () {
    //
});
```

4. Memvalidasi Middleware di Controller

```php
public function __construct()
{
    $this->middleware(['role:super-admin','permission:publish articles|edit articles']);
}
public function __construct()
{
    $this->middleware(['role_or_permission:super-admin|edit articles']);
}
```
 ---
 
 - [Scaffolding Menggunakan Jetstream](#scaffolding-menggunakan-jetstream)
 
### Laravel8 Auth Scaffolding dengan jetstream

1 install laravel dengan ditambahkan --jet di akhir nama project

```
laravel new project-name --jet
```

ketik 0 lalu enter jika ingin menggunakan livewire
keitk 1 lalu enter jika ingin menggunakna inertia

setelah instalasi selesai, atur file .env lalu jalankan perintah

```
php artisan migrate
```

installasi selesai, jika sudah terlajur install laravel tanpa --jet di akhir, bisa gunakan composer

2 alternatif install package laravel jetstream melalui composer

```
composer require laravel/jetstream
```

3 jika ingin menggunakan livewire gunakan perintah berikut

```
php artisan jetstream:install livewire --teams
```

atau juga bisa menggunakan inertia dengan perintah

```
php artisan jetstream:install inertia --teams
```

4 Setelah menginstal Jetstream, Anda harus menginstal dan membangun dependensi NPM Anda dan memigrasi database Anda:

```
npm install && npm run dev

php artisan migrate
```
installasi selesai.
untuk 

## Templating

⬆️ [Ke Atas](#laravel-trik) ➡️ [Berikutnya (Basis Data / Database)](#basis-data-database)

- [Html To Pdf](#html-to-pdf)
---

### **Html To Pdf**
- Sebuah fungsi untuk mengubah string html menjadi string pdf menggunakan wkhtmltopdf. Fungsi ini tidak menggunakan file sementara apa pun dan tidak bergantung pada plugin. untuk info update silahkan lihat <a href="https://github.com/meertensm/HtmlToPdf">meertensm/HtmlToPdf</a>

```php
function toPdf($html, $landscape = false)
{
    
    $wkhtmltopdf = '/usr/local/bin/wkhtmltopdf';

    // manfaatkan variables laravel's environment 
    // $wkhtmltopdf = env('WKHTMLTOPDF');

    $descriptorspec = [
        0 => ['pipe', 'r'], // stdin
        1 => ['pipe', 'w'], // stdout
        2 => ['pipe', 'w']  // stderr
    ];
    
    //Hapus DYLD_LIBRARY_PAth sebagai solusi errors ketika bekerja di system operasi MAC menggunakan MAMP
    $process = proc_open('unset DYLD_LIBRARY_PATH ;' . $wkhtmltopdf . ' ' . ( $landscape ? '-O landscape' : '' ) . ' -q - -', $descriptorspec, $pipes);

    // kirim HTML di stdin
    fwrite($pipes[0], $html);
    fclose($pipes[0]);

    // Baca hasilnya
    $pdf = stream_get_contents($pipes[1]);
    $errors = stream_get_contents($pipes[2]);

    // Tutup Prosesnya
    fclose($pipes[1]);
    $return_value = proc_close($process);

    if ($errors){
        dd($errors);
    }else{
        header('Content-Type: application/pdf'); 
        echo $pdf;
    }
}
```

## **Basis Data (Database)**

[Ke Atas](#laravel-trik) ➡️ [Berikutnya (Middleware)](#middleware)

- [Koneksi Banyak Basis Data (Multiple-connection Database)](#koneksi-banyak-basis-data-multiple-connection-database)
---

### Koneksi Banyak Basis Data (Multiple-connection Database)

- biasa di gunakan untuk aplikasi menengah ke atas

- cara pemasangan :

1. Tambahkan baris kode ini, di dalam file `.env`.

```php
DB_CONNECTION2=mysql
DB_HOST2=127.0.0.1
DB_PORT2=3306
DB_DATABASE2=database ke 2
DB_USERNAME2=root
DB_PASSWORD2=password kamu
```

2. Di dalam file `config/database.php` tambahkan baris kode berikut:


```php
 'mysql2' => [
            'driver' => 'mysql',
            'url' => env('DATABASE_URL'),
            'host' => env('DB_HOST2', '127.0.0.1'),
            'port' => env('DB_PORT2', '3306'),
            'database' => env('DB_DATABASE2', 'forge'),
            'username' => env('DB_USERNAME2', 'forge'),
            'password' => env('DB_PASSWORD2', ''),
            'unix_socket' => env('DB_SOCKET2', ''),
            'charset' => 'utf8mb4',
            'collation' => 'utf8mb4_unicode_ci',
            'prefix' => '',
            'prefix_indexes' => true,
            'strict' => true,
            'engine' => null,
            'options' => extension_loaded('pdo_mysql') ? array_filter([
                PDO::MYSQL_ATTR_SSL_CA => env('MYSQL_ATTR_SSL_CA'),
            ]) : [],
        ],
```

3. Untuk default databasenya adalah `firstdb`, jika ingin menggunakan database ke dua ubah connection yang ada di migration

```php
  public function up()
    {
        Schema::connection(mysql2)->create('users', function (Blueprint $table) { // <= perhatikan connection nya
            $table->id();
            $table->string('name');
            $table->string('email')->unique();
            $table->timestamp('email_verified_at')->nullable();
            $table->string('password');
            $table->rememberToken();
            $table->timestamps();
        });
    }
```

4. Jalankan migration satu persatu

```php
php artisan migrate --database=mysql
php artisan migrate --database=mysql2
```

5. selesai

## Middleware

⬆️ [Ke Atas](#laravel-trik) ➡️ [Berikutnya (Lain -lain)](#lain-lain)
- [Validasi per menit menggunakan throttle](#Validasi-per-menit-menggunakan-throttle)
---

### **Validasi per menit menggunakan throttle**
1. Membuat class middleware menggunakan artisan


		php artisan make:middleware Report

	
2. Tambahkan Class middleware tadi ke dalam middlewareGroups yang ada di kernel.php

		protected $middlewareGroups = [

			'api' => [

				'report' => \App\Http\Middleware\Report::class,

			]

		];


3. Masuk Ke Web.php. dan Tambahkan Route middleware Group

		Route::middleware('report', 'throttle:1,1440')->group(function () {

			Route::post('laporan', "Dashboard\ReportController@store")->name('laporan');

		});

		//'report' -> nama middleware yg sudah terdaftar di kernel

		//'throttle' -> library untuk Rate Limit

		//'1,1440' -> 1 kali validasi dalam 24 jam/1440 menit



**Cara Merubah pesan error throttle**

Masuk ke  `Exceptions/Handler.php`, masukkan kondisi di dalam function render.

Contoh:

	if ($exception instanceof ThrottleRequestsException) {
	
	    return response()->json(abort(429, 'Upaya Hari Ini Sudah Habis'));
	    
	}else {
	
	    return parent::render($request, $exception);            
	    
	}


## Lain-lain
⬆️ [Ke Atas](#laravel-trik)

- [Penulisan singkat `if-else` dan `if-elseif-else` dengan ternary](#penulisan-singkat-if-else-dan-if-elseif-else-dengan-ternary)
---

### **Penulisan singkat if-else dan if-elseif-else dengan ternary**

- **if-else**

Penulisan singkat if-else yaitu: 

    $role = 0;
    echo $role == 0 ? 'Admin' : 'Pengunjung';

- **if-elseif-else**

**Hindari** penulisan seperti ini: 

> $role = 0;

> echo $role == 0 ? 'admin' : $role == 1 ? 'guru' : 'santri';
	
Jika mengikuti penulisan di atas maka akan muncul pesan error
**`Unparenthesized a ? b : c ? d : e is deprecated. Use either (a ? b : c) ? d : e or a ? b : (c ? d : e)`**

Yang benar adalah seperti ini:
    
    $role = 0;
    echo $role == 0 ? 'admin' : ($role == 1 ? 'guru' : 'santri'); 


