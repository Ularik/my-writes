
### Добавляем backend
```
from celery import Celery  
from src.config import settings  
  
broker_url = settings.REDIS_URL  
backend_url = broker_url  
  
celery_instance = Celery(  
    "tasks",  
    broker=broker_url,  
    backend=backend_url + "/0",  ### backend обязательно номер Бд /0
    include={  
        "src.tasks.tasks"  
    }  
)
```
### Создаем таски
```
@celery_instance.task  
def get_todays_bookings():  
    result = [{"id": 1, "name": "Pumba"}]  
    return result
    

@celery_instance.task  
def generate_pdf(bookings: list[dict]):  
    ... generate file...
    filename = "bookings.pdf" 
    return filename
```

### API
```
from celery import chain  
from celery.result import AsyncResult  



@router.post("/reports/generate/")  
async def start_report_generation():  
    report_workflow = chain(  
        get_todays_bookings.s() | task_generate_pdf.s()   # 1
    )  
  
    result_group = report_workflow.delay()  
    task_id = result_group.id  # 2
  
    return {"task_id": task_id, "status": "processing"}
```

1. Запуск цепочки задач, результат первой передается во второй
2. Идентификатор задачи

### Получаем результат по task_id
```
from src.tasks.celery_app import celery_instance


@router.get("/reports/")  
async def get_report(
	task_id: str,    # task_id
	request: Request
	):  
	
    result = AsyncResult(task_id, app=celery_instance)  
  
    if result.state == "SUCCESS":  
        relative_path = result.result  
  
        file_name = os.path.basename(relative_path)  
  
        full_url = request.url_for("media", path=f"reports/{file_name}")  
        return {  
            "status": "COMPLETED",  
            "result": str(full_url)  
        }  
  
    elif result.state == "PENDING":  
        return {  
            "status": "PENDING",  
            "message": "Задача всё еще выполняется или находится в очереди"  
        }  
  
    elif result.state == "FAILURE":  
        return {  
            "status": "FAILED",  
            "error": str(result.info)  
        }  
  
    return {"status": result.state}
```

## Важные методы `AsyncResult`, которые тебе пригодятся

Когда ты создаешь `result = AsyncResult(task_id)`, у этого объекта есть несколько ключевых свойств:

- **`result.state`** (или `result.status`) — возвращает строку с текущим состоянием задачи (`PENDING`, `STARTED`, `SUCCESS`, `FAILURE`, `RETRY`).
    
- **`result.ready()`** — возвращает `True`, если задача завершилась (неважно, успешно или с ошибкой), и `False`, если она еще в процессе.
    
- **`result.result`** (или `result.info`) — **самое главное поле**.
    
    - Если статус `SUCCESS` — тут лежит то, что вернула задача через `return`.
        
    - Если статус `FAILURE` — тут лежит объект ошибки (`Exception`).
        
- **`result.get(timeout=X)`** — 🛑 **НЕ используй его внутри `async def` эндпоинтов** без крайней необходимости. Этот метод блокирует поток и ждет, пока задача выполнится. Для фронтенда ( polling ) нужен именно пример выше, где мы просто проверяем статус и сразу отдаем ответ, не блокируя сервер.
    

### ⚠️ О чем нужно помнить:

Чтобы этот код работал, в конфигурации твоего `celery_instance` **обязательно должен быть настроен Result Backend** (например, Redis или PostgreSQL). Если у тебя в настройках Celery прописан только `broker_url`, но нет `result_backend`, то Celery просто не записывает результаты, и на любой `task_id` ты всегда будешь получать статус `PENDING`.