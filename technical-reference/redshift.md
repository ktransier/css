View sort/distkeys

```sql
select "column", type, encoding, distkey, sortkey, "notnull" 
from pg_table_def
where tablename = 'itemizations' 
and sortkey <> 0;
```
