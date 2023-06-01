# Виды

## 2PL больще не поддерживается СУБД

## Undo (в postgreSQL не используется)

## ACID/MVCC

### UPDATE/DELETE самые дорогие операции

Дополнительные данные в таблице должны быть указаны явно

```s
SELECT i, xmin,xmax,cmin,cmax,ctid FROM boarding_passes;
```

Посмотреть мертвые таплы через статистику

SELECT relname, n_live_tup, n_dead_tup, trunc(100*n_dead_tup/(n_live_tup+1))::float "ratio%", last_autovacuum 
FROM pg_stat_user_tables WHERE relname = 'test1';

### ROLLBACK тоже внесется в таблицу мертвыми (побитовая маска будет стоять что они неактуальны)

