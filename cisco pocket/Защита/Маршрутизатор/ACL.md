
### Чтобы к нашей внутренней сети никто не прописал маршруты 
Ставим разрешение только на запросы к белому ip на внешнем порту маршрутизатора
```
ip access-list extended FROM-OUTSIDE
	permit ip any host 212.212.60.80

int gig0/0/0
	ip access-group FROM-OUTSIDE in
```