

### create table
```
CREATE TABLE user (
    user_id SERIAL PRIMARY KEY,
    full_name TEXT NOT NULL
);
```


### добавляем новое поле в существующую таблицу car
```
ALTER TABLE car
ADD COLUMN user_id INT;
```


### привязываем поле user_id таблицы car к полю user_id таблицы user
```
ALTER TABLE car
ADD CONSTRAINT car_user_id_fkey
FOREIGN KEY (user_id) REFERENCES user(user_id);
```

### Обновить таблицу, добавить новое значение в поле
```
UPDATE car SET user_id = 1 WHERE car_id IN (1,2,3);  -- все эти машины у Ивана
UPDATE car SET user_id = 2 WHERE car_id IN (4,5);    -- эти у Петра

```