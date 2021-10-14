# RPi Django
Raspberry Django

## Basic info
- Owner: Martin Simecek
- Contact: hello@martinsimecek.cz

## About
This is only setup guide for Django.

## Setup

### Clone repository
    cd /home/pi/Documents
    git clone https://github.com/martinsimecek/rpi-django

### Install Django and start project
    cd /home/pi/Documents/rpi-django
    python3 -m venv venv
    source venv/bin/activate
    python -m pip install Django
    django-admin startproject rpi .
    pip install mysqlclient

### MariaDB database
Log in to MariaDB as **root** user and run following SQL commands. Note that you have to change **password** of the user.

    CREATE DATABASE django CHARACTER SET utf8;
    CREATE USER 'django'@'localhost' IDENTIFIED BY '<password>';
    GRANT ALL ON django.* TO 'django'@'localhost';
    FLUSH PRIVILEGES;
    EXIT

### Change settings
Import `os` module to **rpi/settings.py**, edit `ALLOWED_HOSTS`, `DATABASES` (don't forget to change password) and `TIME_ZONE` settings and insert `STATIC_ROOT` parameter (to the end).

    import os

    ALLOWED_HOSTS = ['*']

    DATABASES = {
        'default': {
            'ENGINE': 'django.db.backends.mysql',
            'NAME': 'django',
            'USER': 'django',
            'PASSWORD': '<password>',
            'HOST': 'localhost',
            'PORT': '',
        }
    }

    TIME_ZONE = 'Europe/Prague'

    STATIC_ROOT = os.path.join(BASE_DIR, 'static')

### Start server
    cd /home/pi/Documents/rpi-django
    python manage.py migrate
    python3 manage.py createsuperuser
    python manage.py runserver 0:8000

## Useful commands

### General
    python -m django --version              -> Get actual version of Django
    python manage.py createsuperuser        -> Create super user
    python manage.py runserver 0:8000       -> Start server on port 8000
    python manage.py shell                  -> Start Python shell of Django
    django-admin startproject <name>        -> Start project named <name>
    django-admin startapp <name>            -> Start app named <name>    

### Migrations
    python manage.py showmigrations         -> Show migrations and their status
    python manage.py makemigrations <name>  -> Create migration based on models of app <name>
    python manage.py sqlmigrate <name> 0001 -> Show SQL command of migration 0001 of app <name>
    python manage.py migrate                -> Apply migrations
