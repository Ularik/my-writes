
```
pip install pytest
pytest-dotenv  # позволяет импортировать файл .env_test
pytest-asyncio
```

1. **Создаем папку** tests рядом с src
2. Создаем файлик pytest.ini рядом с src

```
[pytest]  
pythonpath = . src  # чтобы pytest видел директорию src и при выполнении тестов мог импортировать оттуда что-либо
env_files =  
    .env_test  # путь к тестовому файлу конфигураций
  
asyncio_mode = auto   # вкл асинхронный режим, чтобы не оборачивать в декоратоы функции пример "@pytest.mark.asyncio"

```
### Создаем тестовую БД вручную 
3. Создаем файлик .env_test с тестовыми подключениями
```
MODE=TEST  
  
DB_NAME=Booking-test  # тестовая база
DB_HOST=localhost  
# DB_HOST=host.docker.internal  
DB_USER=ular  
DB_PASSWORD=admin  
DB_PORT=5432  
  
REDIS_HOST=redis  
REDIS_PORT=6379  
  
SECRET_KEY=09d25e094faa6ca2556c818166b7a9563b93f7099f6f0f4caa6cf63b88e8d3e7  
ALGORITHM=HS256  
ACCESS_TOKEN_EXPIRE_MINUTES=30
```
3. создаем файлик conftest.py внутри папки tests

```
from src.database import Base, engine_null_pool  
from src.config import settings  
from src.models import *   # импортируем все таблицы для Base metadata
import pytest  
  
					  # session, “function”, “class”, “module”, “package”, 
					  
@pytest.fixture(scope="session", autouse=True)  
async def check_mode():  
    assert settings.MODE == "TEST"  
									      # autouse=True заупстить автоматически
@pytest.fixture(scope="session", autouse=True)  
async def setup_database(check_mode):  # очередность выполнения
    print("-------Fixtures start-----------")  
    async with engine_null_pool.begin() as conn:  
        await conn.run_sync(Base.metadata.drop_all)  
        await conn.run_sync(Base.metadata.create_all)
```


```
pytest -v -s
```