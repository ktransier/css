**Show roles**

```sql
SELECT rolname, rolsuper FROM pg_roles;
```

**Make role a superuser**

```sql
ALTER USER root WITH SUPERUSER;
```

**Explain and analyze**

```sql
EXPLAIN ANALYZE SELECT company_id FROM job_listings WHERE company_id = 145;
```

**Log in to specific db**

```sql
psql myapp_development
```

**Quit psql**

```sql
\q
```

**Create database**

```sql
CREATE DATABASE myapp_development;
```

**Drop specific table**

```sql
DROP TABLE slot;
```

** List databases **

```sql
psql
\l
```

** List tables in database **

```sql
psql myapp_development
\dt
```

** Drop specific columns within table **

```sql
ALTER TABLE table DROP COLUMN col1, DROP COLUMN col2;
```

** List columns within table **

```sql
psql myapp_development
 \d+ myapp_development_table_name
 ```
 
 ** Import database **
 
 ```sql
 psql myapp_development < database.pgdump
 ```
