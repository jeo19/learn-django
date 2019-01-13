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

> ### ☰ The Django Templates

#### - Add python code in the 'blog/templates/blog/post_list.html'

```python
<div>
    <h1><a href="/">Django Girls Blog</a></h1>
</div>

{% for post in posts %}
    <div>
        <p>published: {{ post.published_date }}</p>
        <h1><a href="">{{ post.title }}</a></h1>
        <p>{{ post.text|linebreaksbr }}</p>
    </div>
{% endfor %}
```

> ### ☰ Apply a css on the templates

#### - The Static file

<code>
The static file is includes a css and image file etc <br> 
> Create a static directory<br>
</code>
<pre>
  djangogirls
    ├── blog
    │   ├── migrations
    │   ├── static
    │   └── templates
    └── conf
</pre>
<code>
> First the css file
</code>
<pre>
    learn-django
    └─── blog
         └─── static
              └─── css
                   └─── blog.css
</pre>
<code>
> Add the code in post_list.html below
</code>

```python
{% load static %}
<html>
    <head>
        <title>Django Girls blog</title>
        <link rel="stylesheet" href="//maxcdn.bootstrapcdn.com/bootstrap/3.2.0/css/bootstrap.min.css">
        <link rel="stylesheet" href="//maxcdn.bootstrapcdn.com/bootstrap/3.2.0/css/bootstrap-theme.min.css">
        <link rel="stylesheet" href="{% static 'css/blog.css' %}">
    </head>
    <body>
        <div>
            <h1><a href="/">Django Girls Blog</a></h1>
        </div>

        {% for post in posts %}
            <div>
                <p>published: {{ post.published_date }}</p>
                <h1><a href="">{{ post.title }}</a></h1>
                <p>{{ post.text|linebreaksbr }}</p>
            </div>
        {% endfor %}
    </body>
</html>
```

> ### ☰ Extends of the Templates on Django

<code>- Create a basic template (base.html)</code>

<pre>
blog
└───templates
    └───blog
            base.html
            post_list.html
</pre>

<code>- blog/templates/blog/base.html</code>

```python
<body>
    <div class="page-header">
        <h1><a href="/">Django Girls Blog</a></h1>
    </div>
    <div class="content container">
        <div class="row">
            <div class="col-md-8">
            {% block content %}
            {% endblock %}
            </div>
        </div>
    </div>
</body>
```

<code>- Change a code from the 'blog/templates/blog/post_list.html'</code>

```python
{% extends 'blog/base.html' %}

{% block content %}
    {% for post in posts %}
        <div class="post">
            <div class="date">
                {{ post.published_date }}
            </div>
            <h1><a href="">{{ post.title }}</a></h1>
            <p>{{ post.text|linebreaksbr }}</p>
        </div>
    {% endfor %}
{% endblock %}
```

> ### ☰ Extends of the App on Django

<code>- Create a link of template on Post model (blog/templates/blog/post_list.html)</code>

```python
{% extends 'blog/base.html' %}

{% block content %}
    {% for post in posts %}
        <div class="post">
            <div class="date">
                {{ post.published_date }}
            </div>
            <h1><a href="{% url 'post_detail' pk=post.pk %}">{{ post.title }}</a></h1>
            <p>{{ post.text|linebreaksbr }}</p>
        </div>
    {% endfor %}
{% endblock %}
```

<code>- Create the detail url on Post model</code>

<code>blog/urls.py</code>

```python
from django.urls import path
from . import views
urlpatterns = [
    path('', views.post_list, name="post_list"),
    path('post/<int:pk>/', views.post_detail, name='post_detail'),
]
```

<code>- Add a 404 pages (blog/views.py)</code>

```python
from django.shortcuts import render, get_object_or_404

```

<code>- Add the view as post_detail</code>

```python
def post_detail(request, pk):
    post = get_object_or_404(Post, pk=pk)
    return render(request, 'blog/post_detail.html', {'post': post})
```

<code>- Create a template for the post details </br> We will create a file in blog/templates/blog called post_detail.html, and open it in the code editor.</code>

<code>blog/templates/blog/post_detail.html</code>

```python
{% extends 'blog/base.html' %} {% block content %}
<div class="post">
  {% if post.published_date %}
  <div class="date">{{ post.published_date }}</div>
  {% endif %}
  <h2>{{ post.title }}</h2>
  <p>{{ post.text | linebreaksbr }}</p>
</div>
{% endblock %}
```

> ### ☰ Django Forms

<code> We need to create a file with this name in the blog directory.</code>

<pre>
blog
   └── forms.py
</pre>

<code>Ok! Let's open it in the code editor and type the following code</code>

<code>blog/forms.py</code>

```python
from django import forms

from .models import Post

class PostForm(forms.ModelForm):

    class Meta:
        model = Post
        fields = ('title', 'text',)
```

<code>It's time to open blog/templates/blog/base.html in the code editor,We will add a link in div named page-header</code>

```python
<a href="{% url 'post_new' %}" class="top-menu"><span class="glyphicon glyphicon-plus"></span></a>
```

<code>We open blog/urls.py in the code editor, add a line</code>

<code>blog/urls.py</code>

```python
path('post/new/',views.post_new, name="post_new"),
```

<code>Add new view named post_new</code>

<code>blog/views.py</code>

```python
from .forms import PostForm
```

<code>and then add post_new</code>

```python
def post_new(request):
    form = PostForm()
    return render(request, 'blog/post_edit.html', {'form': form})
```

<code>Create the new template</code>

<code>blog/templates/blog/blog_edit.html</code>

<code>Let's open the blog_edit.html, then add the following code</code>

```python
{% extends 'blog/base.html' %}

{% block content %}
    <h2>New post</h2>
    <form method="POST" class="post-form">{% csrf_token %}
        {{ form.as_p }}
        <button type="submit" class="save btn btn-default">Save</button>
    </form>
{% endblock %}
```

> ### ☰ Saving the form

<code>- Add the following code in the blog/views.py </code>

```python
from django.shortcuts import redirect
```

<code>and then We need to change blog/views.py like this</code>

```python
def post_new(request):
    if request.method == "POST":
        form = PostForm(request.POST)
        if form.is_valid():
            post = form.save(commit=False)
            post.author = request.user
            post.published_date = timezone.now()
            post.save()
            return redirect('post_detail', pk=post.pk)
    else:
        form = PostForm()
    return render(request, 'blog/post_edit.html', {'form': form})
```

> ### ☰ Edit form

<code>Open blog/templates/blog/post_detail.html in the code editor and add the line</code>

```python
<a class="btn btn-default" href="{% url 'post_edit' pk=post.pk %}"><span class="glyphicon glyphicon-pencil"></span></a>
```

<code>So that the template will look like this</code>

```python
{% extends 'blog/base.html' %}

{% block content %}
    <div class="post">
        {% if post.published_date %}
            <div class="date">
                {{ post.published_date }}
            </div>
        {% endif %}
        <a class="btn btn-default" href="{% url 'post_edit' pk=post.pk %}"><span class="glyphicon glyphicon-pencil"></span></a>
        <h2>{{ post.title }}</h2>
        <p>{{ post.text|linebreaksbr }}</p>
    </div>
{% endblock %}
```

<code>Open blog/urls.py in the code editor, and add this line:</code>

```python
path('post/<int:pk>/edit/', views.post_edit, name='post_edit'),
```

<code>and then, We need to add the following code in blog/views.py</code>

```python
def post_edit(request, pk):
    post = get_object_or_404(Post, pk=pk)
    if request.method == "POST":
        form = PostForm(request.POST, instance=post)
        if form.is_valid():
            post = form.save(commit=False)
            post.author = request.user
            post.published_date = timezone.now()
            post.save()
            return redirect('post_detail', pk=post.pk)
    else:
        form = PostForm(instance=post)
    return render(request, 'blog/post_edit.html', {'form': form})
```

> 📌 Security for the input or change authentication of post's

<code>Open blog/templates/blog/base.html in the code editor and change like this:</code>

```python
{% if user.is_authenticated %}
    <a href="{% url 'post_new' %}" class="top-menu"><span class="glyphicon glyphicon-plus"></span></a>
{% endif %}
```

<code>and open blog/templates/blog/blog_detail.html and change like the following code</code>

```python
{% if user.is_authenticated %}
     <a class="btn btn-default" href="{% url 'post_edit' pk=post.pk %}"><span class="glyphicon glyphicon-pencil"></span></a>
{% endif %}
```
