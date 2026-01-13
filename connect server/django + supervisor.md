
## create new project in server
sudo apt update
sudo apt list --upgradable
sudo apt upgrade
sudo apt install python3-venv python3-dev libpq-dev postgresql postgresql-contrib nginx curl redis-server redis-tools
 
### Creating the PostgreSQL Database and User :
 ```
 sudo -u postgres psql
 CREATE DATABASE truck;
 CREATE USER ular WITH PASSWORD 'admin';
 ALTER ROLE ular SET client_encoding TO 'utf8';
 ALTER ROLE ular SET default_transaction_isolation TO 'read committed';
 ALTER ROLE ular SET timezone TO 'UTC';
 ```

### Create another user
```
adduser ular
usermod -aG sudo ular
groups ular
su ular
```

### Creating a Python Virtual Environment for your Project
```
mkdir comix
mkdir core
cd comix
python3 -m venv venv
source venv/bin/activate
git clone 'mygiturl'
pip install -r requirements.txt
pip install gunicorn 
```


### Note:
==gunicorn must be installed in virtual environment 'venv'==

### Configuring a New Django Project
```
vim ~comix/core/core/settings.py
```

```
ALLOWED_HOSTS = ['your_server_domain_or_IP', 'second_domain_or_IP', . . ., 'localhost']
DEBUG = True
```
```
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.postgresql_psycopg2',
        'NAME': 'myproject',
        'USER': 'ular',
        'PASSWORD': 'admin',
        'HOST': 'localhost',
        'PORT': '',
    }
}
```

```
STATIC_URL = "static/"
STATIC_DIR = os.path.join(BASE_DIR, '')
STATICFILES_DIRS = [STATIC_DIR]
STATIC_ROOT = os.path.join(BASE_DIR, 'static')

```

```
~./manage.py makemigrations
~./manage.py migrate
~./manage.py createsuperuser
~./manage.py collectstatic
```

### if you have .mo files do this
```
~./manage.py compilemessages
```
```
sudo ufw allow 8000
```

```
./manage.py runserver 0.0.0.0:8000
Gunicorn:
gunicorn --bind 0.0.0.0:8000 core.wsgi
```

==myproject==.wsgi is the name of the core directory    # core/core/

## [[nginx]]

### create supervisor for gunicorn

sudo apt-get install supervisor


cd /etc/supervisor/conf.d/    # directory for service
vim comix.conf:
```
[program:cms]    # 'cms' is name of this config  
command=/home/ular/comix/venv/bin/gunicorn core.wsgi:application -c /home/ular/comix/core/conf/gunicorn.conf.py   # gunicorn.conf.py in project
directory=/home/ular/comix/core  
user=ular  
autorestart=true  
redirect_stderr=true  
stdout_logfile = /home/ular/comix/core/logs/debug.log
```

gunicorn.conf.py:
```
bind = '127.0.0.1:8000'  
workers = 5  
user = 'ular'  
timeout = 360
```

sudo update-rc.d supervisor enable
sudo service supervisor start
sudo supervisorctl reread
sudo supervisorctl update
sudo supervisorctl status

#### if you change comix.conf
sudo supervisorctl restart nameyourfile 