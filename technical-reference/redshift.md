View sort/distkeys

```sql
select "column", type, encoding, distkey, sortkey, "notnull" 
from pg_table_def
where tablename = 'itemizations' 
```

View column encoding

```sql
SELECT "table" tablename,
    CASE
        WHEN encoded = 'Y' THEN 1
        ELSE 0
    END has_col_encoding
FROM svv_table_info ti
JOIN
  (SELECT tbl,
        MIN(c) min_blocks_per_slice,
        MAX(c) max_blocks_per_slice,
        COUNT(DISTINCT slice) dist_slice
   FROM
    (SELECT b.tbl,
            b.slice,
            COUNT(*) AS c
    FROM STV_BLOCKLIST b
    GROUP BY b.tbl,
            b.slice)
   WHERE tbl IN
    (SELECT table_id
        FROM svv_table_info)
   GROUP BY tbl) iq ON iq.tbl = ti.table_id
ORDER BY SCHEMA,
        "Table";
```

View skew ratio

```sql
SELECT "table" tablename,
    100* (ROUND(100*CAST(max_blocks_per_slice - min_blocks_per_slice AS FLOAT) / GREATEST(NVL (min_blocks_per_slice,0)::int,1),2)) ratio_skew_across_slices
FROM svv_table_info ti
JOIN
  (SELECT tbl,
        MIN(c) min_blocks_per_slice,
        MAX(c) max_blocks_per_slice,
        COUNT(DISTINCT slice) dist_slice
   FROM
    (SELECT b.tbl,
            b.slice,
            COUNT(*) AS c
    FROM STV_BLOCKLIST b
    GROUP BY b.tbl,
            b.slice)
   WHERE tbl IN
    (SELECT table_id
        FROM svv_table_info)
   GROUP BY tbl) iq ON iq.tbl = ti.table_id
ORDER BY SCHEMA,
        "Table";
```

```sql
View distribution
SELECT locati on_id, COUNT(*) 
FROM itemizations
GROUP BY 1 
ORDER BY 2 DESC 
LIMIT 100;
```
