
добавляем эту функцию в файл conftest.py для доступа к ручкам api:
```
@pytest.fixture(scope="session")  
async def ac() -> AsyncClient:  
    async with AsyncClient(transport=ASGITransport(app=app), base_url="http://test") as ac:  
        yield ac
```

юзаем в других тестах:
```
async def test_get_hotels(ac) -> list[HotelOutSchema]:  
    response = await ac.get(  
        "/hotels/empty_hotels",  
        params={  
            'from_date': '2026-04-18',  
            'to_date': '2026-04-23'  
        }  
    )  
    hotels = response.json()  
    print(f"Empty hotels: ")  
    for hotel in hotels:  
        print(hotel['title'])  
    return hotels
```

#### Но, чтобы не ругался sqlAlchemy на "attached to a different loop"
при использовании ручек api, так как ручки юзают основной движок 
async def get_empty_hotels(  
        db: DBDep,   # зависимость

поменяем его так в conftest.py
```
app.dependency_overrides[get_db] = get_db_null_pool
```


### Либо можно по другому пути поменять:
в файле database.py
```
db_params = {}  
if settings.MODE == 'TEST':  
    db_params = {'poolclass': NullPool}  
  
engine = create_async_engine(DB_URL, echo=False, **db_params)  # теперь движок использует nul pool соединение во всех ручках
```