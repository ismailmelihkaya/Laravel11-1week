# **1 Haftalık Laravel Eğitimi Notları**

---

## **Gün 1: Laravel'e Giriş ve Kurulum**

### **1. Laravel Nedir?**
Laravel, modern PHP uygulamaları geliştirmek için kullanılan güçlü bir MVC tabanlı framework'tür. Kolay okunabilir sözdizimi, güçlü ORM yapısı (Eloquent), blade templateleri ve API desteği gibi özellikler sunar.

### **2. Laravel Kurulumu**
#### **Gereksinimler:**
- PHP 8.1+
- Composer
- MySQL veya PostgreSQL
- Node.js ve npm (frontend işlemleri için)

#### **Kurulum Adımları:**
```sh
composer create-project laravel/laravel myProject
cd myProject
php artisan serve
```
Tarayıcıdan `http://127.0.0.1:8000` adresine giderek proje çalıştırılabilir.

### **3. Laravel Proje Yapısı**
- **app/** → Model, Controller, Middleware dosyaları burada yer alır.
- **routes/** → `web.php`, `api.php`, `console.php` gibi rotalar bulunur.
- **resources/views/** → Blade templateleri burada saklanır.
- **config/** → Yapılandırma dosyaları (database, cache vb.)
- **.env** → Çevresel değişkenler dosyası.

### **4. Route, Controller ve View Kullanımı**

#### **Basit Bir Route Tanımlama:**
```php
Route::get('/hello', function () {
    return "Merhaba Laravel";
});
```

#### **Controller Kullanımı:**
```sh
php artisan make:controller PageController
```
```php
// app/Http/Controllers/PageController.php
class PageController extends Controller {
    public function index() {
        return view('welcome');
    }
}
```
```php
// routes/web.php
Route::get('/', [PageController::class, 'index']);
```

---

## **Gün 2: Blade Templating & Middleware**

### **1. Blade Templating Kullanımı**

#### **Blade Kullanımı**
```blade
<!-- resources/views/layouts/master.blade.php -->
<!DOCTYPE html>
<html>
<head>
    <title>@yield('title')</title>
</head>
<body>
    @yield('content')
</body>
</html>
```

```blade
<!-- resources/views/home.blade.php -->
@extends('layouts.master')

@section('title', 'Ana Sayfa')
@section('content')
    <h1>Hoş Geldiniz!</h1>
@endsection
```

#### **Middleware Kullanımı**
```sh
php artisan make:middleware CheckAge
```
```php
// app/Http/Middleware/CheckAge.php
public function handle($request, Closure $next) {
    if ($request->age < 18) {
        return redirect('/home');
    }
    return $next($request);
}
```
```php
// routes/web.php
Route::get('/dashboard', function () {
    return view('dashboard');
})->middleware('checkAge');
```

---

## **Gün 3: Veritabanı İşlemleri (Eloquent & Query Builder)**

### **1. Migration Kullanımı**
```sh
php artisan make:migration create_tasks_table
```
```php
// database/migrations/YYYY_MM_DD_create_tasks_table.php
public function up() {
    Schema::create('tasks', function (Blueprint $table) {
        $table->id();
        $table->string('title');
        $table->text('description');
        $table->timestamps();
    });
}
```
```sh
php artisan migrate
```

### **2. Eloquent Model Kullanımı**
```sh
php artisan make:model Task
```
```php
// app/Models/Task.php
class Task extends Model {
    protected $fillable = ['title', 'description'];
}
```
```php
// routes/web.php
use App\Models\Task;
Route::get('/tasks', function () {
    return Task::all();
});
```

---

## **Gün 4: Authentication & Authorization**

### **1. Kullanıcı Kayıt ve Giriş İşlemleri**
```sh
composer require laravel/jetstream
php artisan jetstream:install livewire
npm install && npm run build
php artisan migrate
```

### **2. Yetkilendirme (Policy Kullanımı)**
```sh
php artisan make:policy TaskPolicy
```
```php
// app/Policies/TaskPolicy.php
public function update(User $user, Task $task) {
    return $user->id === $task->user_id;
}
```
```php
// routes/web.php
Route::get('/tasks/{task}/edit', function (Task $task) {
    if (Gate::denies('update', $task)) {
        abort(403);
    }
    return view('tasks.edit', compact('task'));
});
```

---

## **Gün 5: API Geliştirme**

### **1. API Route Tanımlama**
```php
// routes/api.php
Route::get('/tasks', [TaskController::class, 'index']);
```

---

## **Gün 6: Test, Debugging & Deployment**

### **1. Laravel Debugging**
```php
// .env dosyasında
APP_DEBUG=true
```
```sh
php artisan tinker
```

---

## **Gün 7: Bitirme Projesi - Task Manager**

### **Proje Gereksinimleri**
- Kullanıcılar kayıt olup giriş yapabilecek.
- Görevler eklenip düzenlenebilecek.
- API endpointleri oluşturulacak.

```php
// routes/web.php
Route::resource('tasks', TaskController::class);
```

Bu doküman, Laravel'i sıfırdan öğretecek şekilde hazırlanmıştır. Detayları eklemek istersen, her bölüme eklemeler yapabilirsin! 😊

