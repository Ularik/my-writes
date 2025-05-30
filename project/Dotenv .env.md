pip install python-dotenv

```
import os 
import dotenv 
dotenv.load_dotenv() 
print(os.getenv('BOT_TOKEN')) 
print(os.getenv('ADMIN_ID'))
```

pip install environs
```
from environs import Env 
env = Env() # Создаем экземпляр класса Env 
env.read_env() # Методом read_env() читаем файл .env и загружаем из него переменные в окружение
```