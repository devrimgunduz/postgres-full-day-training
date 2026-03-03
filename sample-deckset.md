autoscale: true

[.background-color: #8B008B]

# Big Bad World of Postgres Development Environments
<br>
<br>
## Elizabeth Garrett Christensen
### Snowflake
Djangocon US 2025


---

[.background-color: #000080]

# Django developers know Postgres

<br>
<br>
<br>


But some of you never touch production

---
[.background-color: #000080]

# Challenges of development environments

- We want to write tests
- We need to use a different database than production
- The scale and settings are different
- We want to protect my user data
- We don't want to waste time

---
[.background-color: #000080]

# You should have a development environment that you can see ... and touch

- knowing your schema and how the data works is good

---

[.background-color: #000080]

# You should have a development environment that you can see ... and touch

- knowing your schema and how the data works is good
- planning for major changes or rewrites

---

[.background-color: #000080]
# You should have a development environment that you can see ... and touch

- knowing your schema and how the data works is good
- planning for major changes or rewrites
- working on performance
- debugging SQL & inefficient queries


---
[.background-color: #008080]

# Performance table stakes in Postgres

- `pg_stat_statements` - look at production, run/debug SQL separately


---

[.background-color: #008080]
# Performance table stakes in Postgres

- `pg_stat_statements` - look at production, run/debug SQL separately
<br><br>
- `EXPLAIN` plans - have comparable data, in size and complexity


---
[.background-color: #CC5500]

# Methods for creating development environments

- Local
- Container
- Cloud

---
[.background-color: #CC5500]

# Local Postgres

- community editions @postgresql.org/download
- `brew install postgres@17`
- `git clone `
- Postgres.app


---

[.background-color: #CC5500]
# Postgres.app


![inline](postgres-app.mov)


---
[.background-color: #CC5500]

# Local Postgres upsides

- can be fast for simple systems
- lots of control
- the psql cli is connected directly

# Local Postgres downsides

- compiling extensions
- major and minor version management
- disconnected configs
	- you can copy a .conf file


---
[.background-color: #000080]


# Docker

![inline 80%](docker-postgres-latest.png)

```
docker pull postgres:latest
```


```
docker pull pgvector/pgvector:pg16
```

```
docker pull postgis/postgis
```

---
[.background-color: #000080]

# Postgres + all your other stuff with docker compose

```yaml
services:
  db:
    image: postgres:17-alpine 
    restart: always 
    ports:
      - "5432:5432" 
    environment:
      POSTGRES_USER: 
      POSTGRES_PASSWORD: 
      POSTGRES_DB: 
    volumes:
      - postgres_data:/var/lib/postgresql/data 

volumes:
  postgres_data:
```

^ mention Simon's blogs


---
[.background-color: #800020]

# But what about extensions? 



---
[.background-color: #800020]

# Contrib extensions

Packaged with Postgres

- `pg_stat_statements`
- `pg_buffercache`
- `postgres_fdw`
- `pgcrypto`

```sql
create extension pg_stat_statements;
```

```sql
ALTER SYSTEM SET shared_preload_libraries = 'pg_stat_statements,pgaudit'
```

---
[.background-color: #800020]

# Third party extensions

Installed and managed separately

- PostGIS
- pgvector
- Timescale
- HypoPG
- hyperloglog (all)
- pgpartman
- pg_cron


--- 
[.background-color: #800020]

# Docker files & extensions

```docker
FROM postgres:16-alpine

# Set environment variables for building pg_cron
ENV PG_VERSION=16
ENV PG_CONFIG=/usr/local/bin/pg_config

# Install dependencies needed to build pg_cron and clean up afterward
RUN apk add --no-cache \
    build-base \
    postgresql-dev \
    git \
    libpq \
    clang19 \
    llvm19 \
    && rm -rf /var/cache/apk/*

# Clone the pg_cron repository and checkout a specific, compatible version
RUN git clone https://github.com/citusdata/pg_cron.git \
    && cd pg_cron \
    && git checkout v1.6.0

# Build and install pg_cron
RUN cd pg_cron \
    && make \
    && make install

# Clean up build dependencies and source code to reduce image size
RUN apk del build-base postgresql-dev git clang llvm \
    && rm -rf /pg_cron

# Expose the standard PostgreSQL port
EXPOSE 5432

```


---
[.background-color: #556B2F]

# Build custom Docker files? 

## Maybe ... but there's another option. 

---
[.background-color: #008B8B]

# Cloud

build dev environments off of production
	- schema
	- configs
	- extensions
	- versions

---
[.background-color: #008B8B]

# Cloud

build dev environments off of production
	- schema
	- configs
	- extensions
	- versions

use tools like 
	- forking 	
	- replicas


---
[.background-color: #008B8B]


# Cloud

build dev environments off of production
	- schema
	- configs
	- extensions
	- versions

use tools like 
	- forking 	
	- replicas

downsides
	- cost
	- internet connection :palm_tree:


---
[.background-color: #008B8B]

# A note about the Postgres cli, psql

- comes with a Postgres download
- can download separately with 	`brew install libpq`
<br><br>
- psql version must be $$>=$$ version of your Postgres instance

---

[.background-color: #191970]
# Template Postgres databases

- Database per use
- a single Postgres instance can have hundreds of databases
- Copies can be made from a central template


```sql
CREATE DATABASE development1 
  WITH TEMPLATE template_db 
  OWNER development;
```
---
[.background-color: #191970]

# Template Postgres databases

- Database per use
- a single Postgres instance can have hundreds of databases
- Copies can be made from a central template


```sql
CREATE DATABASE development1 
  WITH TEMPLATE template_db 
  OWNER development;
```

---
[.background-color: #191970]

# How to create a template 

- dump your production schema and extensions

```bash
pg_dump -U my_user -h my_host -p 5432 -d my_database 
--exclude-table-data --no-owner --no-privileges > template.sql
```

- exclude table data will remove the table data but include the full schema and all extensions. 


---
[.background-color: #191970]

# Create a restore database with your schema

```sql
-- create the datbase
CREATE DATABASE my_template_db OWNER postgres;

-- restore from the the dump file
psql -U my_user -h my_host -p 5432 -d my_template_db -f template.sql
```

Add seed data that you want to be used at this point. Adding seed data here makes things really fast when you do a template copy, as opposed to doing a restore function.


---
[.background-color: #000080]

# We have a database but now we need data

<br><br><br>
### Fixtures are great for testing
####beyond that we need more data....

---
[.background-color: #000080]

# Data dump and restore from Postgres


```sql
pg_dump -U postgres -d my_database > my_database.sql
```
<br><br>
Single tables
```sql
pg_dump -U postgres -d my_database -t app_patient > app_patient.sql
```

---
[.background-color: #000080]

# `dumpdata` and `pg_dump`
[.column]

### Django dumpdata
- Human readable
- Database-agnostic 
- No schema, indexes, or db objects

[.column]

### pg_dump
- Postgres specific
- faster

--- 
[.background-color: #000080]

# For the uber :sunglasses:, you might like table statistics

Table statistics are how Postgres keeps internal details on tables and what it uses to make decisions about query planning. 


Coming in Postgres 18, you can `pg_dump --statistics-only`

Hacking the Postgres Statistics Tables for Faster Queries
https://www.crunchydata.com/blog/hacking-the-postgres-statistics-tables-for-faster-queries



---
[.background-color: #000080]

# PostgreSQL Anonymizer

### Extension from Dalibo

- Anonymous Dumps : Simply export the masked data into an SQL file

- Static Masking : Remove the PII according to the rules

- Dynamic Masking : Hide PII only for the masked users

- Masking Views : Build dedicated views for the masked users

- Masking Data Wrappers : Apply masking rules on external data


---
[.background-color: #000080]

## Anon w/ dynamic masking


```sql
SECURITY LABEL FOR anon 
  ON COLUMN app_patient.first_name 
  IS 'MASKED WITH FUNCTION anon.dummy_first_name()';

SECURITY LABEL FOR anon 
  ON COLUMN app_patient.last_name 
  IS 'MASKED WITH FUNCTION anon.dummy_last_name()';

SECURITY LABEL FOR anon 
  ON COLUMN people.phone
  IS 'MASKED WITH FUNCTION anon.partial(phone,2,$$******$$,2)';
```


---
[.background-color: #000080]

### Anon w/ dynamic masking

[.column]

Live database

```
 first_name | last_name |    phone     
------------+-----------+--------------
 Alice      | Smith     | 555-123-4567
 Bob        | Johnson   | 555-987-6543
 Carol      | Williams  | 555-555-5555
 David      | Brown     | 555-111-2222
 Eva        | Davis     | 555-333-4444
 Frank      | Miller    | 555-777-8888
 Grace      | Wilson    | 555-999-0000
 Henry      | Moore     | 555-222-3333
 Ivy        | Taylor    | 555-444-5555
 Jack       | Anderson  | 555-666-7777
 Kim        | Thomas    | 555-888-9999
 Leo        | Jackson   | 555-000-1111
 Mia        | White     | 555-121-3141
 Noah       | Harris    | 555-151-6171
 Olivia     | Martin    | 555-181-9202
 Paul       | Garcia    | 555-212-2223
 Quinn      | Rodriguez | 555-242-5262
 Rose       | Martinez  | 555-272-8292
 Sam        | Hernandez | 555-303-1323
 Tina       | Lopez     | 555-333-4353
(20 rows)
```

[.column]

Masked for developers

```
 first_name | last_name  |   phone    
------------+------------+------------
 Lambert    | McKenzie   | 55******67
 Euna       | Wiegand    | 55******43
 Wilford    | Collins    | 55******55
 Thaddeus   | Fadel      | 55******22
 Vicente    | Gislason   | 55******44
 Nova       | King       | 55******88
 Burley     | Rowe       | 55******00
 Aida       | Glover     | 55******33
 Ruben      | Erdman     | 55******55
 Quinton    | Kirlin     | 55******77
 Jammie     | Boehm      | 55******99
 Rosendo    | Farrell    | 55******11
 Magnus     | Denesik    | 55******41
 Agnes      | Macejkovic | 55******71
 Gregg      | Gusikowski | 55******02
 Julia      | Jast       | 55******23
 Vernie     | Senger     | 55******62
 Bonnie     | Schiller   | 55******92
 Lilla      | Mohr       | 55******23
 Alessandra | Turner     | 55******53
(20 rows)
```

^ mention Jay's blogs

----

![](anon.png)



----

[.background-color: #8B008B]
# Summary


## Platform
Local - difficult to compile
Docker - easy to use but stand alone
Cloud - allows for complex configs, utilize Postgres templates

## Data
Fixtures/tests - simple and good for testing
Seeds - easy to use but not great for query optimization
Anonymized data - more complex but facilitates performance development


