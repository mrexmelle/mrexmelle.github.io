#django-hello
Project ini akan memerlihatkan cara untuk membuat sebuah aplikasi web dasar dengan menggunakan **Django Framework**. Sebagai tambahan, project ini juga akan melliputi cara untuk menjalankan project ini di **Heroku**.

### Langkah Untuk Membuat

* Untuk menjalankan aplikasi Django, anda harus memiliki Python, Django, Virtual Environment dan Pip terinstal pada komputer anda.
	* Menginstall python dengan **Homebrew**
		`$ brew install python3`
	* Menginstall virtual environment dengan **Pip**
		`$ pip install virtualenv`<br>
		Buat sebuah direktori untuk project anda, dan pindah ke direktori tersebut<br>
		Aktifkan virtual environment pada direktori tersebut
		`$ virtualenv -p /usr/local/bin/python3 env`
		`$ source env/bin/activate`
	* Menginstall Django dengan **Pip**
		`(env) $ pip install django`
* Membuat project django
	`$ django-admin startproject django_hello`<br>
	helloapp merupakan nama project, anda dapat mengganti nama ini sesuai kemauan anda
* Pindah ke dalam direktori *django_hello*. Di dalam direktori ini akan ada *manage.py* dan sebuah direktori bernama *django_hello* juga. Di dalam direktori tersebut, terdapat pengaturan dari project ini dan rute dari project anda.
* Buat aplikasi di dalam project anda
	`$ python manage.py startapp hello`
* Ubah `settings.py` yang terdapat pada direktori *django_hello/django_hello*
	
	```py
	# django_hello/settings.py
	INSTALLED_APPS = [
    	'django.contrib.admin',
    	'django.contrib.auth',
    	'django.contrib.contenttypes',
    	'django.contrib.sessions',
    	'django.contrib.messages',
    	'django.contrib.staticfiles',
    	'hello'
	]
	```
* Ubah `urls.py` yang terdapat pada direktori *django_hello/django_hello*
	
	```py
	# django_hello/urls.py
	from django.conf.urls import url, include
	from django.contrib import admin
	urlpatterns = [
   		url(r'^admin/', admin.site.urls),
    	url(r'^', include('hello.urls')),
	]
	```
* Buat `urls.py` di direktori *django_hello/hello*
	
	```py
	# hello/urls.py
	from django.conf.urls import url
	from howdy import views
	urlpatterns = [
   		 url(r'^hello$', views.hello, name='hello')
	]
	```
* Modifikasi `views.py` anda menjadi
	
	```py
	# hello/views.py
	from django.shortcuts import render
	from django.views.generic import TemplateView
	from django.http import HttpResponse
	from datetime import datetime
	# Create your views here.
	def hello(request):

    name = request.GET.get('name')

    localHour = datetime.now().time().hour
    if localHour >= 4 and localHour <= 10:
        greeting = "Good morning, {0}!".format(name)
    elif localHour >= 11 and localHour <= 15:
        greeting = "Good day, {0}!".format(name)
    elif localHour >= 16 and localHour <= 18:
        greeting = "Good afternoon, {0}!".format(name)
    elif localHour >= 19 and localHour <= 22:
        greeting = "Good evening, {0}!".format(name)
    else:
        greeting = "Good night, {0}!".format(name)

    return HttpResponse (greeting)
	```
* Jalankan server

    ```bash
    $ python manage.py runserver
    ```
* Penggunaan

    Secara default, server akan melayani di:
    > http://localhost:8000/
    
    Untuk menjalankan aplikasi yang anda buat, anda dapat mengakses:
    > http://localhost:8000/hello?name=<your_name>
    
### Menjalankan di Heroku

* Setiap saat anda ingin menjalankan aplikasi di Heroku, anda harus memiliki file `Procfile`. Silahkan lihat contoh file ini di direktori ini.
* Anda juga perlu untuk menyediakan`requirements.txt`. Heroku akan mengunduh seluruh *dependancy* yang tertulis pada file ini. Cara mudah untuk membuat file ini adalah
	`$ pip freeze > requirements.txt`
* Heroku juga mengadakan method ```collectstatic``` dari `manage.py`. Sehingga anda perlu mengubah `settings.py` anda:

    ```python
PROJECT_ROOT = os.path.dirname(os.path.abspath(__file__))
STATIC_ROOT = os.path.join(PROJECT_ROOT, 'staticfiles')
STATIC_URL = '/static/'
STATICFILES_DIRS = (
    os.path.join(PROJECT_ROOT, 'static'),
)
```

    dan ```django.contrib.staticfiles``` ke dalam daftar INSTALLED_APP.
    
* Terakhir, anda perlu menyediakan direktori dengan nama ```static``` di dalam direktori yang sama dengan  ```settings.py```. Karena git mengabaikan direktori kosong, anda dapat menambahkan file `.gitignore` kosong ke direktori ini.

### Tautan ke git repository
[django-hello](https://github.com/mrexmelle/django-hello)