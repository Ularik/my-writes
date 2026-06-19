
```
pip install alembic
alembic init src/migrations
```

Открываем alembic.ini и меняем строку:
```
prepend_sys_path = . src

# раскомментируй 14 строка
file_template = %%(year)d_%%(month).2d_%%(day).2d_%%(hour).2d%%(minute).2d-%%(rev)s_%%(slug)s
```

Открываем файл env.py и добавляем:
```
import sys  
from os.path import dirname, abspath  
  
# Добавляем корень проекта в sys.path, чтобы Python видел папку src  
root_dir = dirname(dirname(dirname(abspath(__file__))))  
sys.path.insert(0, root_dir)

from src.database import URL_DB  # импорт URL
from src.vehicles.models import Vehicles    # импорт всех таблиц
from src.database import Base   
 
 
 
# переопределяем путь к базе
config.set_main_option("sqlalchemy.url", URL_DB + "?async_fallback=True")
target_metadata = Base.metadata
```

Далее запуск создания миграций:
```
alembic revision --autogenerete
alembic revision --autogenerate -m "Initial migration"

```

Далее мигрируем миграции в БД:
```
alembic upgrade head
```

```
alembic downgrade -1
```

### PEP 8 ADD

