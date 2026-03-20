
```
pip install celery
```

### Создаем папку src.tasks
```
src
	- tasks
	  - celery_app.py  # сюда пишем следующее
```

указываем брокер сообщений, тут это redis; 
```
from celery import Celery

celery_instance = Celery(  
    "tasks",  
    broker=settings.REDIS_URL,  
    include={  
        "src.tasks.tasks"  # import tasks file 
    }  
)

```

### Создаем непосредственно сам файл tasks.py
```
- celery_app.py
- tasks.py
  
	from src.tasks.celery_app import celery_instance  
	from time import sleep  
	  
	@celery_instance.task  
	def task_test():  
	    sleep(5)  
	    print("I am groot")
```


### В любой ручке вставляем эту таску
```
from src.tasts.tasks import task_test

@router.post('/')  
async def post_facilities(  
        db: DBDep,  
        data: FacilitiesAddSchema  
):  
  
    facilities = await db.facilitiesModel.add_obj(data)  
  
    task_test.delay()  # here this
  
    await db.save()  
    return facilities
```
