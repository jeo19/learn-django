# learn-django

It's the training Django

> ### ☰ Setup Django

#### 1.Configuration virtual environment

##### 1) Case to uses the virtualenv

- [step 1]

<pre> - pip install virtualenv</pre>

- [step 2]

<pre> - virtualenv [venv name]</pre>

- [step 3]

<pre> - [venv name Directory] > Scripts > activate</pre>

##### 2) Case to uses the venv

<pre>- python3 -m venv [venv name]</pre>

#### 2. Setup the Django

##### 1) Case to install with manually the Django

<pre>- pip install Django</pre>

##### 2) Case to install by requirement.txt

<pre>- pip install -r requirement.txt</pre>

#### 3. Check if Django is installed

```sh
- python
- import django
- print(django.VERSION)
- print(django.get_version())
```

#### 4. Start the project of Django

```sh
(myvenv) [The directory that venv installed /Scripts]> django-admin.py startproject [mysite] .
(myvenv) [The directory that venv installed]> python manage.py runserver
```

#### 5. How to use the postgreSQL on Django

> postgreSQL installer execute

```sh

```

> Install the psycopg2 package

```sh
pip install psycopg2
```

> Add the DATABASE and USER, PASSWORD

```sh
CREATE DATABASE django_test;
CREATE USER django_user WITH PASSWORD 'django_pass';
ALTER ROLE django_user SET client_encoding TO 'utf8';
ALTER ROLE django_user SET default_transaction_isolation TO 'read committed';
ALTER ROLE django_user SET timezone TO 'UTC';
GRANT ALL PRIVILEGES ON DATABASE django_test TO django_user;
```

> ### ☰ Add the model from django

1. Add the model

   ```python
   from django.db import models
   from django.utils import timezone
   class Post(models.Model):
   author = models.ForeignKey('auth.User', on_delete=models.CASCADE)
   title = models.CharField(max_length=200)
   text = models.TextField()
   created_date = models.DateTimeField(
   default=timezone.now)
   published_date = models.DateTimeField(
   blank=True, null=True)

    def publish(self):
        self.published_date = timezone.now()
        self.save()

    def __str__(self):
        return self.title
   ```

2. Create a table in database

```sh
(myvenv) ~/djangogirls$ python manage.py makemigrations blog
Migrations for 'blog':
  blog/migrations/0001_initial.py:
  - Create model Post
```

3. Apply the model add in database

```sh
(myvenv) ~/djangogirls$ python manage.py migrate blog
Operations to perform:
  Apply all migrations: blog
Running migrations:
  Rendering model states... DONE
  Applying blog.0001_initial... OK
```

> ### ☰ The Django admin manager

1. Register a model from admin.py
   ```python
   from django.contrib import admin
   from .models import Post
   admin.site.register(Post)
   ```
   then excute 'python manage.py runserver' on commandline  
   http://127.0.0.1:8000/admin/ connect in the browser.
2. Should create superuser.

   ```sh
   (myvenv) ~/djangogirls$ python manage.py createsuperuser
   Username: admin
   Email address: admin@admin.com
   Password:root1234
   Password (again):
   Superuser created successfully.
   ```

> ### ☰ The Django ORM and QuerySets

#### - Django shell

```sh
(myvenv) ~/learn-django>$ python manage.py shell
```

#### - Inquire all objects

```python
from blog.models import Post
Post.objects.all()
```

#### - Creates an Object

```python
Post.objects.create(author=username, title='Sample title', text='Test')
```

#### - Inquire User Object and username sets the value

```python
from django.contrib.auth.models import User
User.objects.all()
username=User.objects.get(username='[username]')
```

#### - Filtering the list of Post

> ##### ☑︎ Case the author is inquire

```python
Post.objects.filter(author=me)
```

> ##### ☑︎ If you only inquires posts whose titles are "sample" in the post list

```python
Post.objects.filter(title__contains='sample')
```

> ##### ☑︎ Ordering QuerySet

```python
Post.objects.order_by('created_date')
Post.objects.order_by('-created_date') ### orderby desc
```

> ##### ☑︎ Joining QuerySet

```python
Post.objects.filter(title__contains='sample').order_by('published_date')
```
