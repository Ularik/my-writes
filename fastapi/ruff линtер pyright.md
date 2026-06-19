
```
ruff check - проверит на ошибки
ruff format src - форматирует папку
```

пример как скрыть ошибку
```
from src.rules.models import DetectionRuleModel   # noqa: F403
```
или для всего файла
```
# ruff: noqa: E402
import ...
```
```
pyright - проверит на типизацию
```