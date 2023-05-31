# DDL, или Data Definition Language

## Группа команд, которые используются для создания и изменения структуры объектов базы данных: таблиц, представлений, схем и индексов. Наиболее известные команды SQL DDL — CREATE, ALTER, DROP

### Команда ALTER DATABASE

Возможны только когда к БД нет ни одного активного подключения

### TABLESPACE

Необходима заранее созданная директория

```m
mkdir /home/name/namedb && chown -R postgres:postgres /home/name/namedb %% chmod 0700 /home/name/namedb
CREATE TABLESPACE namets LOCATION 'каталог';
```

Имя табличного простанстра не должно начинаться с pg_
Нельзя удалить пока в нем существует хотя бы один объект (выражение CASCADE не определено для команды DROP TABLESPACE т.к. в нем могут находиться объекты разных БД) вручную

```s
DROP TABLESPACE namets;
```

Если ругается is not emty ищем вручную tablespace:

```s
SELECT pg_relation_filepath('nametable');
ALTER TABLE nametable SET tablespace pg_default;
ALTER TABLE public.table_two SET tablespace pg_default;
-- SELECT pg_relation_filepath('nametable');
```

### TABLE

TEMPORY/TEMP - живут во время сессии
UNLOGGED - не записываются в WAL (нежурналированная таблица, работа с ней более ускоренная)
INHERITS - наследование (объявленм что у нас есть таблица родитель с которой идет копирование структуру)
PARTITION BY - секционированная

### SCHEMA

Тоже не должно начинаться с pg_
Схема принадлежит конкретной БД
Временные таблицы создаются в pg_temp_*
\dn - показывает имя схемы, владельца и т.д.

#### Настройка search_path

```s
SET search_path = ddl_pract, public;
SHOW search_path;
```

#### Создание копированной структуры TABLE

Создание таблицы из результатов запроса

```d
CREATE TABLE table_three
AS
SELECT T1.id_one, T1.some_text AS first_text, T2.id_two, T2.some_text AS second_text
FROM table_one T1
INNER JOIN table_two T2 ON T2.id_one = T1.id_one
```

Копирование СТРУКТУРЫ таблицы

```s
CREATE TABLE copy_of_table_two (LIKE table_two);
```

### VIEW

Каждый раз выполняет новый запрос (если запрос большой то будет долгий)

```v
CREATE VIEW ddl_pract.view_one
AS
SELECT T1.id_one, T1.some_text AS first_text, T2.id_two, T2.some_text AS second_text
FROM ddl_pract.table_one T1
INNER JOIN table_two T2 ON T2.id_one = T1.id_one;

SELECT * FROM ddl_pract.view_one;
```

### MATERIALIZED VIEW

Более быстрый способ, но данные при новом селекте не обнавляются, т.е. она сохраняет данные.
В случае длинных отчетов, можно заново переформировать ночью с помощью команды рефреша по расписанию.

```v
REFRESH MATERIALIZED VIEW ddl_pract.mat_view_one;
```

### ROLE/USER

### INDEX

Удаление не влияет на базу данных (только на время запросов)

### SEQUENCE (последовательность)

CREATE SEQUENCE seq001 START 10;

SELECT nextval('seq001'::regclass);
SELECT setval('seq001'::regclass, 999, false);
SELECT setval('seq001'::regclass, 999);