# Серверные процессы и память

## POSTMASTER (backend processes)

- первый процесс postgresql
- запускается при старте сервиса
- порождает все остальные процессы
- создает shared memory
- слушает TCP и UNIX socket

Shared memory нарезается между всеми процессами\форками(подключениями)

Выделенные роли:

- logger
- checkpoint
- writer
- wal writer
- autovacuum
- arcviver
- statscollector

## Память сессии (выделяется сразу процессу подключения)

- work_mem (4MB) для выполнения запросов сортировок
- maintence_work_mem (64MB) исползуется сдужебными операциями (вакуум.реиндекс)
- temp_buffers

## Процессы внутри сессии (не можем управлять)

### Просмотр процессов

```s
ps -xf

SELECT pid, backend_type, backend_start, state FROM pg_stat_activity;
```

### Что находится в схеме (таблица\табличные простр-а)

```n
SELECT tablename, tablespace FROM pg_tables WHERE schemaname = 'public';
```

