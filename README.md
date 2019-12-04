# Jasmin Web Panel

[![Build Status](https://travis-ci.org/101t/jasmin-web-panel.svg?branch=master)](https://travis-ci.org/101t/jasmin-web-panel)

Jasmin SMS Web Interface for Jasmin SMS Gateway

## Installation
Download and Extract folder
We recommend installing in a virtualenv

Install dependencies:

```shell
$ pip install -r requirements.pip
```
cd to **JasminWebPanel** and run:
```shell
$ ./manage.py migrate 
$ ./manage.py createsuperuser 
$ ./manage.py collectstatic
```
The last is only needed if you are running the production server (see below) rather than the Django dev server. It should be run again on any upgrade that changes static files. If in doubt, run it.
You can also override the default settings for the telnet connection in local_settings.py. These settings with their defaults are:
```python
TELNET_HOST = '127.0.0.1'
TELNET_PORT = 8990
TELNET_USERNAME = 'jcliadmin'
TELNET_PW = 'jclipwd'
```
## Running

To run for testing and development: 
```shell
./manage.py runserver
```
This is slower, requires `DEBUG=True`, and is much less secure

To run on production:
```shell
./run_cherrypy.py
```
This requires that you run the collectstatic command (see above) and you should have `DEBUG=False`.


## For Docker if you want to automate the Django create super user
...
ENV DJANGO_DB_NAME=default
ENV DJANGO_SU_NAME=admin
ENV DJANGO_SU_EMAIL=admin@my.company
ENV DJANGO_SU_PASSWORD=mypass

RUN python -c "import django; django.setup(); \
   from django.contrib.auth.management.commands.createsuperuser import get_user_model; \
   get_user_model()._default_manager.db_manager('$DJANGO_DB_NAME').create_superuser( \
   username='$DJANGO_SU_NAME', \
   email='$DJANGO_SU_EMAIL', \
   password='$DJANGO_SU_PASSWORD')"
...
