
## 🔹 SQL (чистые запросы)

### ⭐ 1. **LeetCode (Database)**

📍 [https://leetcode.com/problemset/database/](https://leetcode.com/problemset/database/)

**Почему хорош:**

- задачи от простых до адских
    
- реальные кейсы (JOIN, GROUP BY, HAVING, window functions)
    
- сразу проверка
    

💡 Совет: не смотри решения ORM — сначала **напиши SQL**

---

### ⭐ 2. **SQLBolt**

📍 [https://sqlbolt.com/](https://sqlbolt.com/)

**Для старта и закрепления:**

- интерактивно
    
- объясняют ошибки
    
- короткие уроки + практика
    

---

### ⭐ 3. **Mode SQL Tutorial**

📍 [https://mode.com/sql-tutorial/](https://mode.com/sql-tutorial/)

**Очень близко к аналитике и продакшену**

- агрегации
    
- подзапросы
    
- реальные данные
    

---

### ⭐ 4. **HackerRank – SQL**

📍 [https://www.hackerrank.com/domains/sql](https://www.hackerrank.com/domains/sql)

**Хорошо прокачивает мышление**

- tricky кейсы
    
- edge cases
    

---

### ⭐ 5. **PostgreSQL Exercises** (🔥🔥)

📍 [https://pgexercises.com/](https://pgexercises.com/)

**ТОП для backend'а**

- чистый PostgreSQL
    
- реальные таблицы
    
- отличные задачи на JOIN / агрегаты
    

---

## 🔹 Django ORM (то, что реально спрашивают)

### ⭐ 6. **Django ORM Playground**

📍 https://django-orm-playground.com/

- пишешь ORM
    
- видишь сгенерированный SQL
    
- идеально для `annotate`, `select_related`
    

---

### ⭐ 7. **Codewars (SQL & ORM)**

📍 [https://www.codewars.com/](https://www.codewars.com/)

Ищи kata:

- `SQL`
    
- `PostgreSQL`
    
- `Django ORM`
    

---

## 🔹 Самая эффективная практика 💪 (реально работает)

### 🛠 Локально + реальные задачи

1️⃣ Подними PostgreSQL  
2️⃣ Создай 4–5 связанных моделей  
3️⃣ Реализуй API:

Примеры задач:

- список категорий + кол-во товаров
    
- топ-5 продуктов по продажам
    
- суммарная выручка по дням
    
- пользователи без заказов
    
- последний заказ каждого пользователя
    

👉 Решай:

- сначала **SQL**
    
- потом **Django ORM**
    
- потом сравни SQL через `queryset.query`
    

---

## 🔹 Чеклист навыков (middle+)

Если можешь это написать — ты 🔥

- `annotate + Count / Sum / Avg`
    
- `filter внутри annotate`
    
- `Subquery + OuterRef`
    
- `Exists`
    
- `Coalesce`
    
- `Window` (`RowNumber`, `Rank`)
    
- `select_related vs prefetch_related`