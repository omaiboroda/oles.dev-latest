---
title: 'Creating vector index in Postgres: applying to production'
description: 'How to create index in production environment'
publishDate: '2024-12-18'
---


After verifying that index works as expected in local environment, we can apply it to production.

There are some recommendations for index creation which speed it up and avoid blocking.

## statement_timeout

Disable statement timeout if you have any, as index creation can take a longer time.

```sql
SET statement_timeout = 0;
```

Default: depends on provider, AWS RDS is 0, Supabase is 120s.

## maintenance_work_mem

```sql
SET maintenance_work_mem = '10 GB'
```

`maintenance_work_mem` is a PostgreSQL configuration parameter that sets the maximum amount of memory to be used for maintenance operations.
The recommended setting is your working set size (the size of your tuples for vector index creation). However, your maintenance_work_mem setting should not exceed 50 to 60 percent of your compute's available RAM.

Default: 64 MB

```sql
SET max_parallel_maintenance_workers = 7
```

The `max_parallel_maintenance_workers` sets the maximum number of parallel workers that can be started by a single utility command such as `CREATE INDEX`.
You can set it to the number of cores in your CPU.

Default: 2.

```sql
CREATE INDEX CONCURRENTLY ON desserts USING hnsw (vector vector_cosine_ops);
```

CREATE INDEX CONCURRENTLY is a PostgreSQL command that allows you to create an index on a table without blocking concurrent reads and writes to that table. This will create index longer, but without blocking other operations.

Default: CREATE INDEX acquires ShareLock, which allows only read operations to the table.

Check index creation progress

```sql
SELECT phase, round(100.0 * blocks_done / nullif(blocks_total, 0), 1) AS "%" FROM pg_stat_progress_create_index;
```

The phases for HNSW are: `initializing`, `loading tuples`.

After index creation is finished, you can run `VACUUM ANALYZE` to update statistics.

```sql
VACUUM ANALYZE desserts;
```

This is basically it, you can now use your index for queries.

To check if index is used, you can use `EXPLAIN ANALYZE` as usual.