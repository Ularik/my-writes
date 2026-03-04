
```
fastapi dev main.py --port 8001
or
uvicorn main:app --reload --port 8001

```

```
import uvicorn

if __name__ == '__main__':  
    uvicorn.run('main:app', port=8001, reload=True)
```