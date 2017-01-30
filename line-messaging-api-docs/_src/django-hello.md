# Heroku + Django
Proyek ini akan menunjukkan cara untuk membuat sebuah aplikasi web dasar dengan menggunakan **Django Framework**. Sebagai tambahan, proyek ini juga akan melliputi cara untuk menjalankan proyek ini di **Heroku**.

![Image of Django](https://simpleisbetterthancomplex.com/media/2016-08-09-how-to-deploy-django-applications-on-heroku/featured.jpg)

## Langkah Untuk Membuat

* Untuk menjalankan aplikasi Django, anda harus memiliki `brew` dan `pip` untuk memasang `python` dan `Django` pada komputer anda.
	
	* Silakhan ikuti petunjuk di URL ini untuk memasang `brew` pada komputer anda:

		```url
		http://brew.sh/
		```
	* Silakhan ikuti petunjuk di URL Ini untuk memasang `pip` pada komputer anda:
		
		```url
		https://pip.pypa.io/en/latest/installing/#installing-with-get-pip-py
		```

	* Memasang `python` menggunakan `brew`
		
		```bash
		$ brew install python
		```
		
	* Memasang Django menggunakan `pip`

		```bash
		$ pip install django
		```	
		
* Membuat proyek Django

	Ketikkan perintah di bawah ini untuk membuat proyek bernama `django_hello`:
	
	```bash
	$ django-admin startproject django_hello
	```
	
	`django_hello` merupakan nama proyek, anda dapat mengganti nama ini sesuai kemauan anda.
	
* Masuk ke dalam direktori `django_hello`.

	```bash
	cd django_hello
	```

* Di dalam direktori ini akan ada file `manage.py` dan sebuah direktori bernama `django_hello` juga. Di dalam direktori tersebut, terdapat pengaturan dari projek ini.

* Buat aplikasi `hello` di dalam proyek anda

	```bash
	$ python manage.py startapp hello
	```
	
* Tambahkan aplikasi `hello` pada `settings.py` di bagian `INSTALLED_APPS` yang terdapat pada direktori `django_hello/django_hello`
	
	```python
	# django_hello/settings.py
	INSTALLED_APPS = [
    	'django.contrib.sessions',
    	'django.contrib.messages',
    	'django.contrib.staticfiles',
    	'hello'
	]
	```
* Tambahkan rute ke aplikasi `hello` dengan mengubah setting `urlpatterns` di `urls.py` yang terdapat pada direktori `django_hello/django_hello`
	
	```python
	# django_hello/urls.py
	from django.conf.urls import url, include

	urlpatterns = [
    	url(r'^hello', include('hello.urls')),
	]
	```
* Buat `urls.py` di direktori *django_hello/hello*
	
	```python
	# hello/urls.py
	from django.conf.urls import url
	from howdy import views
	urlpatterns = [
   		 url(r'', views.hello, name='hello')
	]
	```
* Modifikasi `views.py` anda menjadi
	
	```python
    # hello/views.py
    from django.http import HttpResponse
    from datetime import datetime
    # Create your views here.
    def hello(request):
        # get name param
        name = request.GET.get('name')
        # compose head
        head='<head><title>Spring Hello</title></head>';
        # get time
        hour = datetime.now().time().hour
        greeting=''
        if hour >= 4 and hour <= 10:
            greeting = 'Good morning, {0}!'
        elif hour >= 11 and hour <= 15:
            greeting = 'Good day, {0}!'
        elif hour >= 16 and hour <= 18:
            greeting = 'Good afternoon, {0}!'
        elif hour >= 19 and hour <= 22:
            greeting = 'Good evening, {0}!'
        else:
            greeting = 'Good night, {0}!'
        print hour
        # compose body
        body=('<body>'+greeting+'</body>').format(name if name != None else 'hoo-man')
        # compose html
        html='<html>'+head+body+'</html>'
        return HttpResponse(html)

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
    
## Menjalankan di Heroku

* Setiap saat anda ingin menjalankan aplikasi di Heroku, anda harus memiliki file `Procfile`. Silahkan lihat contoh file ini di direktori ini.
* Anda juga perlu untuk menyediakan`requirements.txt`. Heroku akan mengunduh seluruh *dependency* yang tertulis pada file ini. Cara mudah untuk membuat file ini adalah
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

## Tautan ke git repository
[django-hello](https://github.com/mrexmelle/django-hello)
