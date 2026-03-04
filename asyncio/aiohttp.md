
# Делаем асинхронные запросы
```
async with aiohttp.ClientSession() as session:

    tasks = [
       fetch_data(session, external_api_url, params, "message"),
       fetch_data(session, external_api_url2, params2, "message"),
       fetch_data(session, external_api_url3, params3, "message")
     ]

   # Запуск всех запросов параллельно
   responses = await asyncio.gather(*tasks)

   data = await responses[0].read()
   response = json.loads(data.decode('utf-8'))
   data = await responses[1].read()
   response2 = json.loads(data.decode('utf-8'))
   data2 = await responses[2].read()
   response3 = json.loads(data2.decode('utf-8'))
```


#  Функция для запроса
```
async def fetch_data(client, url, params, error_key):
    try:
        response = await client.get(url, params=params)
        return response
    except Exception as e:
        return {error_key: str(e)}
        
```
