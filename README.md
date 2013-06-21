# allvbg

���� ������ - ����������������� ������ �� ������ Django ��� ��������� �������, � ������� ��� ��� ��� �� ����������� 2Gis.

������ ���������� �������� ����������� � ��������� � ������ ������� � ���������� ���������� �����������. �������� ������, ��������� ������� � views, ����������� ���������� �����, ��������� �������� ��� ������������� ������� �������, �������� ����������� �������� �������������� ���������� � ������ ��������������, ������� �������� � ���������. � �������� ������������� ����������� ����������� ��������-�������� ����� ECWID. ������� ��������� SEO-����������: META-����, ���, sitemap.

### ������������ ������:
* Django
* FeinCMS
* django-admin-tools
* django-debug-toolbar
* django-filebrowser
* django-grappelli
* django-modeltranslation
* django-mptt
* django-ratings
* django-tinymce
* easy-thumbnails
* feedparser

# ���������

��� ������� ������� ����������� ������� � ���������� pytnon � Django.

## ��������� ����������

1 ������������ sql-���� � ����-������� � ���� ������ MySQL, ��������� ��� ������.
2 �������� ���� settings.py � �������� ������� � �������� ��� ������ � �������������� �������� ������.
������, ��������� ���������:
```python
PROJECT_PATH = '/var/www/pman/data/www/allvbgru'

ADMINS = (
    ('PMaN', 'pman89@yandex.ru'),
)

DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.mysql',
        'NAME': 'django',
        'USER': 'user',
        'PASSWORD': 'password',
    }
}

STATIC_URL = 'http://allvbg.ru/static/'

ST_URL = 'http://allvbg.ru/static/allvbg/'
ST_ROOT = '/var/www/pman/data/www/allvbgru/static/allvbg/'

MEDIA_ROOT = '/var/www/pman/data/www/allvbgru/static/allvbg/'

MEDIA_URL = 'http://allvbg.ru/static/allvbg/'

TINYMCE_JS_URL = 'http://allvbg.ru/static/tiny_mce/tiny_mce_src.js'

CACHES = {
    'default': {
        'BACKEND': 'django.core.cache.backends.filebased.FileBasedCache',
        'LOCATION': '/var/www/pman/data/Django_cache',
        'TIMEOUT': 60,
        'OPTIONS': {
            'MAX_ENTRIES': 1000
        }
    }
}
```

## ��������� ������� 

### ���� �� ����������� shared-���������

���������� � ������ ��������� �������� � �������� ��������� ������� ������� python-���������� � ��������� �������������� �������.

### ��� ������������� ������������ ������

#### ��������� �������

������� � ���������� ������� � ��������� �������

`pip install -r requirements.txt`

����� ��������� ���������� ������� ��� ����������� ��� ������ ������� ������ ����� �����������.

� ������ ������ ��� �������� �������, � ������ ����� ���������� ��� ��������� PIL � MySQL-python, ���������� ��� ������ �� ����������� ������ ������������, ��������, ��� Debian:
```
apt-get install python-imaging
apt-get install python-mysqldb
 ```
 
#### ��������� web-�������

##### WSGI

�������� ���� django-fcgi � ������� � �� ���������� ���� �� �����, ���� ��� ���������� ��� ������.

����� ����� ��������� �������:
```
cp ./django-fcgi /etc/init.d/django-fcgi 
chmod +x /etc/init.d/django-fcgi
update-rc.d /etc/init.d/django-fcgi defaults
```

������ �� ������ ��������� ���������� ��������:

`/etc/init.d/django-fcgi start`

��� �� ����������� ������� stop � restart

���������� ����� �������� �� localhost:8881

##### Ngnix

�������� � ���� �������� nginx (������ /etc/nginx/nginix.conf) ��� ���� ������.

    server {
      listen #your ip#;
      server_name #your site adress#;
      location /static {
        root #path to project folder#;
      }
      location / {
        root   #path to project folder#;
        index  index.html index.htm;
        fastcgi_pass 127.0.0.1:8881;

        fastcgi_param PATH_INFO $fastcgi_script_name;
        fastcgi_param REQUEST_METHOD $request_method;
        fastcgi_param QUERY_STRING $query_string;
        fastcgi_param CONTENT_TYPE $content_type;
        fastcgi_param CONTENT_LENGTH $content_length;
        fastcgi_param REMOTE_PORT $remote_port;
        fastcgi_param SERVER_PROTOCOL $server_protocol;
        fastcgi_param SERVER_PORT $server_port;
        fastcgi_param SERVER_NAME $server_name;
        fastcgi_pass_header Authorization;
        fastcgi_intercept_errors off;
        fastcgi_param REMOTE_ADDR $remote_addr;
      }
    }