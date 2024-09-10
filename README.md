# Marquette

Marquette merupakan proyek Django untuk tugas mata kuliah Pemrograman Berbasis Platform Ganjil 2024/2025 oleh Steven Setiawan dengan NPM 2306152260.

PWS Link : http://steven-setiawan-marquette.pbp.cs.ui.ac.id/

## Daftar Isi
- [README.md Tugas 2](#tugas-2)
  - [Implementasi Checklist Tugas 2](#jelaskan-bagaimana-cara-kamu-mengimplementasikan-checklist-di-atas-secara-step-by-step-bukan-hanya-sekadar-mengikuti-tutorial)
  - [Bagan request client ke web aplikasi berbasis Django](#buatlah-bagan-yang-berisi-request-client-ke-web-aplikasi-berbasis-django-beserta-responnya-dan-jelaskan-pada-bagan-tersebut-kaitan-antara-urlspy-viewspy-modelspy-dan-berkas-html)
  - [Fungsi Git dalam pengembangan perangkat lunak](#jelaskan-fungsi-git-dalam-pengembangan-perangkat-lunak)
  - [Alasan framework Django dijadikan permulaan pembelajaran](#menurut-anda-dari-semua-framework-yang-ada-mengapa-framework-django-dijadikan-permulaan-pembelajaran-pengembangan-perangkat-lunak)
  - [Mengapa model Django disebut sebagai ORM?](#mengapa-model-pada-django-disebut-sebagai-orm)

## Tugas 2

## Jelaskan bagaimana cara kamu mengimplementasikan checklist di atas secara step-by-step (bukan hanya sekadar mengikuti tutorial).

### Membuat sebuah proyek Django baru
Untuk membuat proyek Django baru, langkah pertama yang saya lakukan adalah membuat folder dengan nama proyek yang saya inginkan.

Untuk melakukan itu, saya menjalankan command berikut:

```
mkdir marquette
cd marquette
```

Setelah itu, saya membuat dan mengaktifkan virtual environment pada folder tersebut dengan menggunakan command berikut:

```
python -m venv env
env\Scripts\activate
```

Kemudian, saya membuat file `requirements.txt` dan mengisinya dengan dependency proyek yang saya butuhkan. Lalu, saya menjalankan command berikut untuk menginstall seluruh module tersebut di virtual environment saya.

```
pip install -r requirements.txt
```

Setelah melakukan penginstallan dependency, saya membuat sebuah proyek Django baru dengan nama `marquette` menggunakan command berikut:

```
django-admin startproject marquette .
```

Di dalam folder marquette, saya menambahkan `ALLOWED_HOSTS` dalam `settings.py` agar dapat dilakukan deployment secara local.

```py
ALLOWED_HOSTS = ["localhost", "127.0.0.1"]
```

Setelah menambahkan `ALLOWED_HOSTS` tersebut, saya melakukan deployment secara local dengan menggunakan command:

```
python manage.py migrate
python manage.py runserver
```

Deployment berhasil dan dapat diakses melalui http://127.0.0.1:8000/

### Membuat aplikasi main pada proyek
Setelah deployment berhasil dilakukan di local, saya membuat aplikasi `main` pada proyek dengan menjalankan:

```
python manage.py startapp main
```

Terlihat ada folder baru pada root folder dengan nama `main`. Hal ini menandakan bahwa aplikasi main berhasil ditambahkan pada proyek.

### Melakukan routing pada proyek agar dapat menjalankan aplikasi `main`
Selanjutnya, agar aplikasi main dapat dijalankan, kita perlu terlebih dahulu melakukan routing pada proyek. Hal ini dilakukan dengan membuka file `settings.py` pada direktori proyek `marquette` dan menambahkan `main` pada variabel `INSTALLED_APPS`.

```py
INSTALLED_APPS = [
    ...,
    'main'
]
```

### Membuat model pada aplikasi main
Setelah melakukan routing agar dapat menjalankan aplikasi `main`, selanjutnya kita akan membuat model. Hal ini dilakukan dengan memodifikasi file `models.py` pada folder `main`. Berikut adalah kode yang saya tambahkan:

```py
class Product(models.Model) :
    name = models.CharField(max_length=255)
    price = models.IntegerField()
    description = models.TextField()
```

Sebelum melanjutkan proyek ini, terlebih dahulu saya melakukan migration pada models yang saya buat dengan menjalankan:

```
python manage.py makemigrations
python manage.py migrate
```

Migration dilakukan untuk merefleksikan perubahan dalam model ke database schema (memastikan model dicatat dalam database schema).

### Membuat sebuah fungsi pada views.py
Kemudian, saya membuat folder baru pada main dengan nama templates dan membuat file baru bernama main.html. main.html berfungsi sebagai display tampilan dari aplikasi main saya. Berikut adalah kode pada main.html saya:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>{{ app_name }}</title>

    <h1>Welcome to {{ app_name }}</h1>
    <h2>We're selling all Coquette items y'all ever need!</h2>
    <h4>Made by {{ name }} from {{ class }} class</h4>
</head>
<body>
    
</body>
</html>
```

Untuk mengisi nilai/value pada template variables app_name, name, dan class, saya kemudian membuat fungsi pada views.py di folder `main`. Berikut adalah kode yang saya tambahkan:

```py
def show_main(request) :
    context = {
        'app_name' : 'Marquette',
        'name' : 'Steven Setiawan',
        'class' : 'PBP D'
    }
    return render(request, "main.html", context)
```

### Membuat routing pada urls.py aplikasi main
Selanjutnya, saya melakukan konfigurasi routing urls.py agar aplikasi `main` dapat diakses melalui peramban ketika proyek dijalankan. Saya membuat file baru bernama `urls.py` pada direktori `main` dan menambahkan kode berikut:

```py
from django.urls import path
from main.views import show_main

app_name = 'main'

urlpatterns = [
    path('', show_main, name='show_main'),
]
```

Setelahnya, saya menambahkan routingan URL dari aplikasi main ke `urls.py` pada direktori proyek. Berikut adalah file `urls.py` setelah dilakukan modifikasi:

```py
from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path('admin/', admin.site.urls),
    path('', include('main.urls')),
]
```

### Melakukan deployment ke PWS
Setelah proyek berhasil dibuat, saya akan menyimpannya pada repository github serta mendeploynya pada PWS.

- **GitHub**

  Untuk menyimpan program yang telah saya buat dalam GitHub, terlebih dahulu saya membuat file `.gitignore` yang berisikan nama-nama file/folder yang tidak ingin saya simpan di dalam repository GitHub.

  Kemudian, saya membuat repository baru bernama `marquette` melalui web https://github.com/ dan kemudian menjalankan command berikut:

  ```
  git init
  git branch -M main
  git remote add origin https://github.com/setiawans/marquette.git
  git add .
  git commit -m "initial commit"
  git push -u origin main
  ```

- **PWS**
  
  Setelah menyimpan program di GitHub, selanjutnya saya akan melakukan deployment menggunakan PWS. Terlebih dahulu saya menambahkan `ALLOWED_HOSTS` pada `settings.py` di direktori proyek.

  ```py
  ALLOWED_HOSTS = ["localhost", "127.0.0.1", "steven-setiawan-marquette.pbp.cs.ui.ac.id"]
  ```

  Selanjutnya, saya menjalankan command berikut:
  ```
  git remote add pws http://pbp.cs.ui.ac.id/steven.setiawan/marquette
  git branch -M master
  git push pws master
  ```

  Apabila ke depannya saya perlu melakukan update pada deployment tersebut, saya dapat menjalankan command berikut:
  
  ```
  git branch -M main
  git push pws main:master
  ```

## Buatlah bagan yang berisi request client ke web aplikasi berbasis Django beserta responnya dan jelaskan pada bagan tersebut kaitan antara urls.py, views.py, models.py, dan berkas html.
![](answer_image/Tugas2-No2-Bagan.png)

Ketika user/client mengakses aplikasi, request akan dikirimkan ke webserver. Request ini akan diolah di `urls.py` dan kemudian diteruskan ke `views.py` yang sesuai/bersangkutan.

Setelah itu, `views.py` akan berinteraksi/berkomunikasi dengan `models.py` yang juga berkomunikasi dengan `database` untuk melakukan proses read/write data jika diperlukan.

Setelah data diproses dan diterima, data akan didisplay oleh `views.py` dengan menggunakan templates yang berisikan berkas html. Halaman html inilah yang akan ditampilkan dan diakses oleh user/client.

## Jelaskan fungsi git dalam pengembangan perangkat lunak!
Git merupakan sebuah _version control system_ yang memungkinkan kita selaku pengembang untuk melacak perubahan-perubahan kode dari waktu ke waktu. Git mempermudah kita melakukan kolaborasi dengan pengembang lainnya secara online dengan memanfaatkan fitur branch yang berbeda. Dengan demikian, setiap anggota tim dapat bekerja tanpa mengganggu kode satu sama lain. Selanjutnya, Git memudahkan pengembang dalam menyimpan proyek-proyek yang telah mereka buat tanpa perlu khawatir adanya kerusakan atau kehilangan pada kode mereka. Selain fungsi-fungsi yang telah disebutkan di atas, Git masih memiliki banyak sekali fungsi lainnya. Secara umum, Git meningkatkan efisiensi setiap pengembang dalam melakukan pengembangan perangkat lunak.

## Menurut Anda, dari semua framework yang ada, mengapa framework Django dijadikan permulaan pembelajaran pengembangan perangkat lunak?
Menurut saya, Django cocok menjadi titik awal pembelajaran pengembangan perangkat lunak karena framework ini berbasiskan bahasa pemrograman Python, yang terkenal ramah untuk dipelajari oleh pemula. Selanjutnya, Django menawarkan arsitektur yang terstruktur sekaligus mudah sekali untuk digunakan oleh para pemula, yakni arsitektur MVT atau Model-View-Template. Selain itu, Django juga memungkinkan pengembangan secara fullstack, yang mendukung hubungan antara front-end dan back-end secara sekaligus dalam satu framework.

## Mengapa model pada Django disebut sebagai ORM?
Model pada Django disebut sebagai ORM (Object-Relational Mapping) dikarenakan Django memetakan objek-objeknya dengan database relasional. ORM menjadi interpreter yang memungkinkan kita berinteraksi dengan database tanpa perlu menuliskan query-query SQL secara manual.