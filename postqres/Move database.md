### Move data from local database to serverdb

```
PGPASSWORD=admin pg_dump -U ular -h 127.0.0.1 apicar-base | PGPASSWORD=oS2rI0jN5s psql -U car-dev-usr -h 91.242.37.59 -d car-dev
```

my source_host: 127.0.0.1
target_password: oS2rI0jN5s
target_user: car-dev-usr
target-host: 91.242.37.59
target_db_name: car-dev


### Copy database

1. Create dump 
```
pg_dump -U postgres -h localhost -d mydatabase -F c -b -v -f /backups/mydatabase.dump
```

- `-U` — имя пользователя PostgreSQL.
- `-h` — хост базы данных (например, `localhost`).
- `-d` — имя базы данных, которую нужно скопировать.
- `-F c` — формат архивного файла (custom).
- `-b` — включает в дамп большие объекты.
- `-v` — выводит подробные сообщения.
- `<путь_к_дампу>` — путь к файлу, куда будет сохранён дамп (например, `/path/to/backup.dump`).

2. Create new database
```
createdb -U <пользователь> -h <хост> <имя_новой_базы>
```
3. Restore dump in new database
```
pg_restore -U <пользователь> -h <хост> -d <имя_новой_базы> -v <путь_к_дампу>
```

```
createdb -U postgres -h localhost newdatabase
pg_restore -U postgres -h localhost -d newdatabase -v /backups/mydatabase.dump
```