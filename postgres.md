**Show roles**

SELECT rolname, rolsuper FROM pg_roles;

**Make role a superuser**

ALTER USER root WITH SUPERUSER;

**Explain and analyze**

EXPLAIN ANALYZE SELECT company_id FROM job_listings WHERE company_id = 145;

**Log in to specific db**

psql myapp_development

**Quit psql**

\q

**Create database**

CREATE DATABASE myapp_development;

**Drop specific table**

DROP TABLE slot;

** List databases **

psql
\l

** List tables in database **

psql myapp_development
\dt

** Drop specific columns within table **

ALTER TABLE table DROP COLUMN col1, DROP COLUMN col2;

** List columns within table **

psql myapp_development
 \d+ myapp_development_table_name
 
 ** Import database **
 
 psql myapp_development < database.pgdump
