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
