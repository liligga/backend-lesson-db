---
theme: default
class: text-center
highlighter: shiki
transition: slide-left
layout: center
---

# Сложные запросы в БД

---
layout: center
---

## Фильтрация данных

Таблица товаров:
| ID | name | price | category_name | category_descr |
|---:|:---:|:---:|:---:|:---:|
| 1 | Кружка | 200 | Кружки | Кружки хороши |
| 2 | Нож | 300 | Ножи | Ножи хороши |
| 3 | Кувшин | 400 | Кувшины | Кувшины хороши |
| 4 | Кружка | 500 | Кружки | Кружки хороши |
| 5 | Нож | 600 | Ножи | Ножи хороши |

---
layout: center
---

Как получить только второй товар:
```sql
SELECT * FROM products WHERE id = 2;
```

Как получить только кружки:
```sql
SELECT * FROM products WHERE category_name = 'Кружки';
```

---
layout: center
---

## Связи таблиц в реляционных базах данных

Иногда данных бывает очень много, и часто они могут быть смешаны и повторяться в таблицах
В таких случаях данные разделяют на несколько таблиц

---
layout: center
---

| ID | name | price | category_name | category_descr |
|---:|:---:|:---:|:---:|:---:|
| 1 | Кружка | 200 | Кружки | Кружки хороши |
| 2 | Нож | 300 | Ножи | Ножи хороши |
| 3 | Кувшин | 400 | Кувшины | Кувшины хороши |
| 4 | Кружка | 500 | Кружки | Кружки хороши |
| 5 | Нож | 600 | Ножи | Ножи хороши |

в таблице выше описание категории повторяется для всех товаров

---
layout: two-cols
---

#### Таблица товаров
| ID | name | price |
|---:|:---:|:---:|
| 1 | Кружка | 200 |
| 2 | Нож | 300 |
| 3 | Кувшин | 400 |
| 4 | Кружка | 500 |
| 5 | Нож | 600 |


*эта таблица не полная*

::right::
 
#### Таблица категорий
| ID | category_name | category_descr |
|---:|:---:|:---:|
| 1 | Кружки | Кружки хороши |
| 2 | Ножи | Ножи хороши |
| 3 | Кувшины | Кувшины хороши |

---
layout: two-cols
---

#### Таблица товаров
| ID | name | price | category_id |
|---:|:---:|:---:|:---:|
| 1 | Кружка | 200 | 1 |
| 2 | Нож | 300 | 2 |
| 3 | Кувшин | 400 | 3 |
| 4 | Кружка | 500 | 1 |
| 5 | Нож | 600 | 2 |
| 6 | Нож | 700 | 2 |
| 7 | Кружка | 800 | 1 |

::right::
 
#### Таблица категорий
*ID категорий записаны в таблице товаров*

| ID | category_name | category_descr |
|---:|:---:|:---:|
| 1 | Кружки | Кружки хороши |
| 2 | Ножи | Ножи хороши |
| 3 | Кувшины | Кувшины хороши |

---
layout: center
---

Как записать связь между таблицами:
```sql
CREATE TABLE products (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    name TEXT,
    price INTEGER,
    category_id INTEGER,
    FOREIGN KEY(category_id) REFERENCES categories(id)
)
```

Как вносить данные в таблицу товаров:
```sql
INSERT INTO products (name, price, category_id) VALUES
    ('Кружка', 200, 1),
    ('Нож', 300, 2),
    ('Кувшин', 400, 3),
    ('Кружка', 500, 1),
    ('Нож', 600, 2),
    ('Нож', 700, 2),
    ('Кружка', 800, 1);
```

---
layout: center
---

Как получать данные из таблицы товаров по ID категории:
```sql
SELECT * FROM products WHERE category_id = 1;
```

---
layout: center
---

Как получать данные из таблицы товаров по названию категории:
```sql
SELECT * FROM products WHERE category_name = 'Кружки';
```

