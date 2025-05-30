### Создание собственного менеджера пользователей

```python
from django.contrib.auth.models import BaseUserManager

class CustomUserManager(BaseUserManager):
    def create_user(self, email, password=None, **extra_fields):
        if not email:
            raise ValueError('Email обязателен')
        email = self.normalize_email(email)
        user = self.model(email=email, **extra_fields)
        user.set_password(password)
        user.save(using=self._db)
        return user

    def create_superuser(self, email, password=None, **extra_fields):
        extra_fields.setdefault('is_staff', True)
        extra_fields.setdefault('is_superuser', True)

        return self.create_user(email, password, **extra_fields)
```

В этом примере **`CustomUserManager`** создает пользователей с использованием электронной почты и пароля. Метод **`create_superuser`** создает суперпользователя с дополнительными правами.

## **AbstractBaseUser**

`AbstractBaseUser` является абстрактным базовым классом для создания пользовательской модели. Он предоставляет основу для определения полей и методов, необходимых для осуществления аутентификации и авторизации пользователей. Путем наследования от `AbstractBaseUser` вы можете создать собственную модель пользователя с полностью настраиваемыми полями и методами.

### **Создание собственной модели пользователя**

```python
from django.contrib.auth.models import AbstractBaseUser, BaseUserManager, PermissionsMixin
from django.db import models

class CustomUserManager(BaseUserManager):
    def create_user(self, email, password=None, **extra_fields):
        if not email:
            raise ValueError('Email обязателен')
        email = self.normalize_email(email)
        user = self.model(email=email, **extra_fields)
        user.set_password(password)
        user.save(using=self._db)
        return user

    def create_superuser(self, email, password=None, **extra_fields):
        extra_fields.setdefault('is_staff', True)
        extra_fields.setdefault('is_superuser', True)

        return self.create_user(email, password, **extra_fields)

class CustomUser(AbstractBaseUser, PermissionsMixin):
	username = models.CharField(max_length=100, unique=False, blank=True, null=True)
    email = models.EmailField(unique=True)
    first_name = models.CharField(max_length=30)
    last_name = models.CharField(max_length=30)
    is_admin = models.BooleanField(default=False)

    # Дополнительные методы и свойства
objects = CustomUserManager()  
  
USERNAME_FIELD = 'email'  
REQUIRED_FIELDS = ('name',)  
  
def has_perm(self, perm, obj=None):  
    """Does the myuser have a specific permission?"""  
    # Simplest possible answer: Yes, always  
    return True  
  
def has_module_perms(self, app_label):  
    """Does the myuser have permissions to view the app `app_label`?"""  
    # Simplest possible answer: Yes, always  
    return True  
  
  
@property  
def is_staff(self):  
    """Is the myuser a member of staff?"""  
    # Simplest possible answer: All admins are staff  
    return self.is_admin

```

В этом примере **`CustomUser`** наследуется от **`AbstractBaseUser`** и **`PermissionsMixin`**, и у него есть собственные поля, такие как **`email`**, **`first_name`**, **`last_name`**, **`is_active`**, и **`is_staff`**. **`CustomUserManager`** используется для создания пользователей и



## **Использование собственной модели пользователя**

Чтобы использовать собственную модель пользователя, вам нужно настроить ее в файле **`settings.py`** вашего проекта:

```python
AUTH_USER_MODEL = 'myapp.CustomUser'
```

Где **`'myapp'`** - это имя вашего приложения, в котором определена модель **`CustomUser`**

Теперь вы можете использовать собственную модель пользователя для аутентификации и управления пользователями в Django.



## Serializers


class UserCreateSerializer(serializers.ModelSerializer):  
    class Meta:  
        model = MyUser  
        fields = ('email', 'country', 'city', 'language', 'password')    
  
    def create(self, validated_data):  
        user = MyUser(**validated_data)  
        user.set_password(validated_data['password'])  
        user.save()  
        return user  
  
    def update(self, instance, validated_data):  
        for field, value in validated_data.items():  
            if field == 'password':  
                instance.set_password(value)  
            else:  
                setattr(instance, field, value)  
        instance.save()  
        return instance