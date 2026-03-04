## **Установка DRF-YASG**

- Установка DRF-YASG происходит через pip:

```bash
pip install drf-yasg
```

## **Настройка DRF-YASG в файле [settings.py](http://settings.py)**

- Для начала нам нужно настроить DRF-YASG в файле **`settings.py`** вашего Django-проекта.

```python
INSTALLED_APPS = [
   'django.contrib.staticfiles',  # required for serving swagger ui's css/js files
   'drf_yasg',
]
```

## **Подключение DRF-YASG к проекту**

- Импортируем необходимые модули в корневом файле **`urls.py`** :

```python
from rest_framework import permissions
from drf_yasg.views import get_schema_view
from drf_yasg import openapi
```

### **`from rest_framework import permissions`**:
- Этот импорт позволяет нам использовать различные разрешения (permissions) в Django REST Framework (DRF). Разрешения определяют, кто и как может получить доступ к данным и функциям в нашем API. Например, мы можем использовать разрешения, чтобы разрешить доступ только авторизованным пользователям или ограничить доступ к определенным действиям.

### **`from drf_yasg.views import get_schema_view`**:

- Этот импорт позволяет нам использовать функцию **`get_schema_view`** из библиотеки DRF-YASG. Эта функция используется для создания Swagger-документации для нашего API. Она помогает собрать информацию о нашем API и представить ее в удобном формате для документации.

### **`from drf_yasg import openapi`**:

- Этот импорт позволяет нам использовать модуль **`openapi`** из библиотеки DRF-YASG. Модуль **`openapi`** содержит различные инструменты и классы, которые помогают нам создавать описания и метаданные для нашей Swagger-документации. Например, мы можем использовать класс **`Info`** из этого модуля, чтобы задать информацию о нашем API, такую как название и описание.

### Далее создаем объект **`schema_view`** с настройками:

```python
schema_view = get_schema_view(
   openapi.Info(
      title="Snippets API",
      default_version='v1',
      description="Test description",
      terms_of_service="<https://www.google.com/policies/terms/>",
      contact=openapi.Contact(email="contact@snippets.local"),
      license=openapi.License(name="BSD License"),
   ),
   public=True,
   permission_classes=(permissions.AllowAny,),
)
```

1. **`schema_view = get_schema_view(...)`** - Здесь вы создаете переменную **`schema_view`**, которая будет представлять настройки для создания Swagger-документации.
2. **`openapi.Info(...)`** - Вы определяете информацию о вашем API, такую как название, версия, описание, ссылка на условия использования, контактную информацию и лицензию. Все эти детали будут отображаться в документации.
3. **`public=True`** - Этот параметр указывает, что ваша документация будет доступна публично.
4. **`permission_classes=(permissions.AllowAny,)`** - Этот параметр указывает, что документация будет доступна для всех, даже неавторизованных пользователей. **`permissions.AllowAny`** означает, что разрешено любому пользователю просматривать документацию.


## **Подключение URL-маршрутов**

- Добавляем URL-маршруты для Swagger-документации в вашем корневом файле **`urls.py`**:

```python
urlpatterns = [
    path('swagger/', schema_view.with_ui('swagger', cache_timeout=0), name='schema-swagger-ui'),
    # Другие URL-маршруты вашего API
]
```

## **Запуск проекта**

- Запустите ваш проект Django.
- Теперь документация Swagger доступна по URL **`/swagger/`**.