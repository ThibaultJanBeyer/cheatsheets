[back to overwiev](/../..)

# Django Cheatsheet

## Table of Contents

[Install](#install)
[Basics](#basics)

## Install

Python: https://www.python.org/  
Django: `pip install Django`  

### VSCode

for plint to work correctly, add the following to your settings.json:  
```
"python.linting.pylintArgs": [
    "--load-plugins",
    "pylint_django"
]
```

## Basics


## Setup

### Scaffold a project

```
django-admin startproject <name>
```

### The development server

```
python manage.py runserver <ip>:<port>
```

Where IP and PORT are optional

### Create a Project APP

```
python manage.py startapp <name>
```

### Database Setup

#### Add Database

Django comes with sql light by default. If you wish to use another database, install the appropriate database bindings and change the following keys in the DATABASES 'default' item to match your database connection settings:  

* ENGINE – Either 'django.db.backends.sqlite3', 'django.db.backends.postgresql', 'django.db.backends.mysql', or 'django.db.backends.oracle'. Other backends are also available.
* NAME – The name of your database. If you’re using SQLite, the database will be a file on your computer; in that case, NAME should be the full absolute path, including filename, of that file. The default value, os.path.join(BASE_DIR, 'db.sqlite3'), will store the file in your project directory.  

If you are not using SQLite as your database, additional settings such as USER, PASSWORD, and HOST must be added. For more details, see the reference documentation for DATABASES.

#### Add Database Tables 

Inside `mysite/settings.py` you’ll find a list of `INSTALLED_APPS`. Most of them need a database table to work. To create those tables, run:

```
python manage.py migrate
```

#### Create Models

A model is your database layout


