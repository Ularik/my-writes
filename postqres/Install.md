sudo apt-get update 
sudo apt-get install postgresql postgresql-contrib

### Run
```
sudo -u postgres psql
psql -U <my_ular> -d <my_database_manga> - exec to database as user
```

Создайте базу данных с помощью следующей команды SQL:

```sql
CREATE DATABASE cert_gov;
```

Здесь **`mydatabase`** - это имя вашей базы данных, вы можете выбрать любое имя.

Теперь создайте пользователя с помощью следующей команды SQL:

```sql
CREATE USER ular WITH ENCRYPTED PASSWORD 'admin';
```

**`myuser`** - это имя пользователя, замените его на свой выбор.

Дайте пользователю все необходимые права на базу данных с помощью следующей команды SQL:

```sql
GRANT ALL PRIVILEGES ON DATABASE cert_gov TO ular;
```

```sql
ALTER DATABASE cert_gov OWNER TO ular;
```

Это предоставит пользователю **`myuser`** полные права доступа к **`mydatabase`**.

Вывести имена всех баз данных:

- SELECT datname FROM pg_database WHERE datistemplate = false;
- \l