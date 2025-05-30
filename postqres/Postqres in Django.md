Теперь, когда PostgreSQL установлен и настроен, настройте Django для его использования:

1. Установите **`psycopg2`** - адаптер PostgreSQL для Python:
    
    ```bash
    pip install psycopg2-binary
    ```
    
    Этот пакет позволит Django взаимодействовать с PostgreSQL.
    
2. В файле **`settings.py`** вашего проекта укажите следующие настройки:
    
    ```python
    DATABASES = {
        'default': {
            'ENGINE': 'django.db.backends.postgresql',
            'NAME': 'mydatabase',  # Имя вашей базы данных
            'USER': 'myuser',      # Имя вашего пользователя
            'PASSWORD': 'mypassword',  # Ваш пароль
            'HOST': 'localhost',   # Хост, на котором работает PostgreSQL
            'PORT': '5432',            # Порт (по умолчанию 5432)
        }
    }
    ```
    
    Здесь **`mydatabase`**, **`myuser`** и **`mypassword`** - это значения, которые вы выбрали при создании базы данных и пользователя.
    
    3. Выполните миграции для вашего проекта:
    
    ```bash
    python manage.py migrate
    ```
    
    Django создаст необходимые таблицы в PostgreSQL.
    

Теперь ваш проект Django настроен на использование PostgreSQL вместо SQLite.



DATABASES = {  
    'default': {  
        'ENGINE': 'django.db.backends.postgresql',  
        'OPTIONS': {  
            'service': 'my_service',                         # protect your info
        },  
    }  
}

1. touch the file '.env':
	1. [my_service]  
		host=localhost  
		user=ular  
		password=admin  
		dbname=checks  
		port=5432
2. env PGSERVICEFILE=/home/ular/PycharmProjects/<your_project>/core/.env

