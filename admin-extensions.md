---
copyright:
  years: 2017,2018
lastupdated: "2018-12-03"
---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Managing PostgreSQL Extensions
{: #extensions}

In PostgreSQL, extensions are modules that supply extra functions, operators, or types. Many extensions are available in {{site.data.keyword.databases-for-postgresql_full}}. In order to use them, you need to [set the admin password](/docs/services/databases-for-postgresql?topic=databases-for-postgresql-admin-password) for your service and use it to [connect with `psql` ](/docs/services/databases-for-postgresql?topic=databases-for-postgresql-connecting-psql).

## Listing Installed Extensions

Get a list of all the extensions installed on a database by using the `\dx` command.

For example, the output for `\dx` when run on the {{site.data.keyword.databases-for-postgresql}} default database shows the only installed extension.
```
ibmclouddb=> \dx
                 List of installed extensions
  Name   | Version |   Schema   |         Description
---------+---------+------------+------------------------------
 plpgsql | 1.0     | pg_catalog | PL/pgSQL procedural language
(1 row)
```

## Installing Extensions

To install an extension on to a database use [`CREATE EXTENSION`](https://www.postgresql.org/docs/current/static/sql-createextension.html). For example, to install `pg_stat_statements` on the `ibmclouddb` database, 

```
ibmclouddb=> CREATE EXTENSION pg_stat_statements;
CREATE EXTENSION
```

If you run the `\dx` command after installing an extension, it appears in the table.
```
ibmclouddb=> \dx
                                     List of installed extensions
        Name        | Version |   Schema   |                        Description
--------------------+---------+------------+-----------------------------------------------------------
 pg_stat_statements | 1.5     | public     | track execution statistics of all SQL statements executed
 plpgsql            | 1.0     | pg_catalog | PL/pgSQL procedural language
(2 rows)
```

Database extensions in PostgreSQL are managed per database. If you have multiple databases that you need to install an extension on, you run the `CREATE` command on each database.

## Upgrading Extensions

If there is a newer version of an extension available than the one you currently have installed, use the `ALTER EXTENSION` to upgrade it.

## Available Extensions
```
ibmclouddb=> SELECT name FROM pg_available_extensions;
             name
------------------------------
 pg_buffercache
 plperl
 tsm_system_time
 plpgsql
 file_fdw
 ltree
 pgrowlocks
 hstore_plperlu
 postgres_fdw
 dict_xsyn
 intarray
 hstore
 insert_username
 lo
 bloom
 refint
 dblink
 tablefunc
 timetravel
 hstore_plperl
 dict_int
 btree_gist
 pg_freespacemap
 earthdistance
 pg_visibility
 uuid-ossp
 intagg
 pg_prewarm
 btree_gin
 isn
 pg_trgm
 tcn
 tsm_system_rows
 seg
 cube
 pgstattuple
 moddatetime
 citext
 amcheck
 sslinfo
 pgcrypto
 pg_stat_statements
 chkpass
 unaccent
 autoinc
 pageinspect
 fuzzystrmatch
 postgis_tiger_geocoder
 postgis
 address_standardizer
 address_standardizer_data_us
 postgis_topology
 plv8
 plls
 plcoffee
 pgrouting
 ```