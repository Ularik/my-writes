
```
pip install "fastapi[standard]"
pip install alembic
pip install asyncpg
```

#### src.main.py
```
from fastapi import FastAPI 

from src.api.signature import router as signature_router  
  
app = FastAPI()  
  
app.include_router(signature_router)
```


#### src.api.signature.py
```
from fastapi import APIRouter  
  
router = APIRouter(prefix="/signature", tags=["Сигнатуры"])  
  
@router.get("/test")  
async def get_test():  
    return "test"
```

```
fastapi dev main.py --port 8001
or
uvicorn src.main:app --reload --port 8001

```

```
import uvicorn

if __name__ == '__main__':  
    uvicorn.run('main:app', port=8001, reload=True)
```

