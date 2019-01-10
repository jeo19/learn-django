# learn-django

It's the training Django

> ### Setup Django

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
