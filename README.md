# HASURA

### database postgres
example db dvdrental in https://www.postgresqltutorial.com/postgresql-sample-database/ 

Postgres database with dvdrental db example, restore it.
1. extract dvdrental.zip (on mac it will be folder but on linux it will be tar file)
2. copy folder/tar file dvdrental to container. docker cp folder_host hasura_graphql-engine:/home
3. enter in hasura_graphql-engine container with command : docker exec -it hasura_graphql-engine sh
4. restore dvdrental with command. # pg_restore -U user -d dbname /home/dvdrental
5. your db will has tables dvdrental

- hasura postgres has requirement, read the doc at this page : 
https://hasura.io/docs/1.0/graphql/core/deployment/postgres-requirements.html#:~:text=The%20Hasura%20GraphQL%20engine%20needs,query%20for%20list%20of%20tables.

```
-- We will create a separate user and grant permissions on hasura-specific
-- schemas and information_schema and pg_catalog
-- These permissions/grants are required for Hasura to work properly.

-- create a separate user for hasura
CREATE USER hasurauser WITH PASSWORD 'hasurauser';

-- create pgcrypto extension, required for UUID
CREATE EXTENSION IF NOT EXISTS pgcrypto;

-- create the schemas required by the hasura system
-- NOTE: If you are starting from scratch: drop the below schemas first, if they exist.
CREATE SCHEMA IF NOT EXISTS hdb_catalog;
CREATE SCHEMA IF NOT EXISTS hdb_views;

-- make the user an owner of system schemas
ALTER SCHEMA hdb_catalog OWNER TO hasurauser;
ALTER SCHEMA hdb_views OWNER TO hasurauser;

-- grant select permissions on information_schema and pg_catalog. This is
-- required for hasura to query the list of available tables.
-- NOTE: these permissions are usually available by default to all users via PUBLIC grant
GRANT SELECT ON ALL TABLES IN SCHEMA information_schema TO hasurauser;
GRANT SELECT ON ALL TABLES IN SCHEMA pg_catalog TO hasurauser;

-- The below permissions are optional. This is dependent on what access to your
-- tables/schemas you want give to hasura. If you want expose the public
-- schema for GraphQL query then give permissions on public schema to the
-- hasura user.
-- Be careful to use these in your production db. Consult the postgres manual or
-- your DBA and give appropriate permissions.

-- grant all privileges on all tables in the public schema. This can be customised:
-- For example, if you only want to use GraphQL regular queries and not mutations,
-- then you can set: GRANT SELECT ON ALL TABLES...
GRANT USAGE ON SCHEMA public TO hasurauser;
GRANT ALL ON ALL TABLES IN SCHEMA public TO hasurauser;
GRANT ALL ON ALL SEQUENCES IN SCHEMA public TO hasurauser;
GRANT ALL ON ALL FUNCTIONS IN SCHEMA public TO hasurauser;

-- Similarly add these for other schemas as well, if you have any.
-- GRANT USAGE ON SCHEMA <schema-name> TO hasurauser;
-- GRANT ALL ON ALL TABLES IN SCHEMA <schema-name> TO hasurauser;
-- GRANT ALL ON ALL SEQUENCES IN SCHEMA <schema-name> TO hasurauser;
-- GRANT ALL ON ALL FUNCTIONS IN SCHEMA <schema-name> TO hasurauser;
```