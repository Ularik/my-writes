```
\du
```

![display user table postgres](https://phoenixnap.com/kb/wp-content/uploads/2022/02/display-user-table-postgres.png)

Locate the user for removal and use the name in the following step.

3. Run the following query to delete a user:

```
DROP USER <name>;
```

![drop user psql output](https://phoenixnap.com/kb/wp-content/uploads/2022/02/drop-user-psql-output.png)

Alternatively, to check if a user exists before dropping, enter:

```
DROP USER IF EXISTS <name>;
```

![drop user if exists psql output](https://phoenixnap.com/kb/wp-content/uploads/2022/02/drop-user-if-exists-psql-output.png)



SELECT table_name
FROM information_schema.tables
WHERE table_schema = 'public'
ORDER BY table_name;
         table_name         
 select all tables

DROP TABLE django_migrations; - delete table

DROP DATABASE  <database_name> - drop database

DELETE FROM django_migrations WHERE app LIKE <*app name>


## Перенос базы 
```
PGPASSWORD=eQ335 pg_dump -U user_cp cplkdb -f the_backup.sql
```

- `PGPASSWORD=eQ335` устанавливает пароль для пользователя `user_cp` как `eQ335`.
- `pg_dump` используется для создания резервной копии базы данных PostgreSQL.
- `-U user_cp` указывает, что операция будет выполняться от имени пользователя `user_cp`.
- `cplkdb` — это имя базы данных, из которой создается резервная копия.
- `-f the_backup.sql` указывает файл, в который будет сохранена резервная копия (в данном случае, файл `the_backup.sql`).

```
PGPASSWORD=eQ8H44 psql -U user_tan -d tanosdb -f the_backup.sql
```

- `PGPASSWORD=eQ8H44` устанавливает пароль для пользователя `user_tan` как `eQ8H44`.
- `psql` — это клиент командной строки для работы с базой данных PostgreSQL.
- `-U user_tan` указывает, что операция будет выполняться от имени пользователя `user_tan`.
- `-d tanosdb` указывает, что операция будет выполняться над базой данных `tanosdb`.
- `-f the_backup.sql` указывает файл резервной копии (`the_backup.sql`), из которого будет выполнено восстановление.

### Путь до Директории базы

> `sudo -u postgres psql -c "SHOW data_directory;"`