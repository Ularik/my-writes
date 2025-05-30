## **Установка и настройка Simple JWT**

Прежде чем начать, давайте установим библиотеку Simple JWT:

```bash
pip install djangorestframework-simplejwt
```

Затем добавим настройки JWT в файл **`settings.py`** нашего Django-проекта:

```python
# settings.py

# Настройка срока действия Access Token и Refresh Token
SIMPLE_JWT = {
    'ACCESS_TOKEN_LIFETIME': timedelta(minutes=60),  # Продолжительность жизни Access Token
    'SLIDING_TOKEN_REFRESH_LIFETIME': timedelta(days=1),  # Продолжительность жизни Refresh Token при его использовании
    'SLIDING_TOKEN_LIFETIME': timedelta(days=7),  # Продолжительность жизни Refresh Token
    'SLIDING_TOKEN_REFRESH_REUSE_ALLOWS_REFRESH': False,  # Разрешить повторное использование Refresh Token для обновления
    'SLIDING_TOKEN_REFRESH_REUSE_RESETS': True,  # Сбрасывать Refresh Token после успешного обновления
    'ROTATE_REFRESH_TOKENS': False,  # Вращать Refresh Token (если True, предыдущий Refresh Token становится недействительным после обновления)
    'ALGORITHM': 'HS256',  # Алгоритм подписи токенов
    'AUTH_HEADER_TYPES': ('Bearer',),  # Тип HTTP заголовка, содержащего токен (обычно 'Bearer')
    'AUTH_TOKEN_CLASSES': ('rest_framework_simplejwt.tokens.AccessToken',),
}

# Применение настроек в DRF
REST_FRAMEWORK = {
    'DEFAULT_AUTHENTICATION_CLASSES': [
        'rest_framework_simplejwt.authentication.JWTAuthentication',
    ],
}
```

Теперь у нас есть настройки, позволяющие контролировать срок действия токенов

## **Создание эндпоинтов для JWT**

Simple JWT предоставляет три стандартных эндпоинта для работы с JWT токенами:

- **`TokenObtainPairView`**: Используется для получения пары токенов (Access и Refresh) при успешной аутентификации.
- **`TokenRefreshView`**: Используется для обновления Access Token с помощью Refresh Token.
- **`TokenVerifyView`**: Используется для проверки действительности токена.

Чтобы создать маршруты для этих эндпоинтов, добавьте следующий код в файл **`urls.py`** вашего приложения:

```python
from django.urls import path
from rest_framework_simplejwt.views import (
    TokenObtainPairView,
    TokenRefreshView,
    TokenVerifyView,
)

urlpatterns = [
    path('token/', TokenObtainPairView.as_view(), name='token_obtain_pair'),
    path('token/refresh/', TokenRefreshView.as_view(), name='token_refresh'),
    path('token/verify/', TokenVerifyView.as_view(), name='token_verify'),
]
```

Теперь у вас есть URL-адреса для получения токенов, их обновления и проверки.

## **Запросы и ответы при использовании JWT**

### Получение Access и Refresh токенов

Для получения Access и Refresh токенов пользователь должен выполнить следующий запрос:

**Запрос на получение токенов (POST)**

```bash
POST /token/
Content-Type: application/json

{
    "username": "your_username",
    "password": "your_password"
}
```

- **`POST /token/`**: URL для получения токенов.
- **`Content-Type: application/json`**: Заголовок, указывающий, что тело запроса содержит JSON данные.
- **`"username"`** и **`"password"`**: Значения, предоставленные пользователем, обычно это имя пользователя и пароль.

### **Обновление Access Token с помощью Refresh Token**

Чтобы обновить Access Token с использованием Refresh Token, пользователь должен выполнить следующий запрос:

**Запрос на обновление Access Token (POST)**

```bash
POST /token/refresh/
Content-Type: application/json

{
    "refresh": "your_refresh_token"
}
```

- **`POST /token/refresh/`**: URL для обновления Access Token.
- **`Content-Type: application/json`**: Заголовок, указывающий, что тело запроса содержит JSON данные.
- **`"refresh"`**: Refresh Token, который будет использоваться для получения нового Access Token.