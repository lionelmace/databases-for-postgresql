---
copyright:
  years: 2017, 2022
lastupdated: "2022-10-17"

keywords: postgresql, databases, postgresql extensions, postgres extensions, ibm_extension

subcollection: databases-for-postgresql

---

{:external: .external target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}
{{site.data.keyword.attribute-definition-list}}


# Managing PostgreSQL Extensions
{: #extensions}

In PostgreSQL, extensions are modules that supply extra functions, operators, or types. Many extensions are available in {{site.data.keyword.databases-for-postgresql_full}}. To use them, [set the admin password](/docs/databases-for-postgresql?topic=databases-for-postgresql-admin-password) for your service and use it to [connect with `psql`](/docs/databases-for-postgresql?topic=databases-for-postgresql-connecting-psql).

## Listing Installed Extensions
{: #listing-installed-extensions}

Get a list of all the extensions installed on a database by using the `\dx` command.

For example, the output for `\dx` when run on the {{site.data.keyword.databases-for-postgresql}} default database shows the only installed extension.
```sh
ibmclouddb=> \dx
                 List of installed extensions
  Name   | Version |   Schema   |         Description
---------+---------+------------+------------------------------
 plpgsql | 1.0     | pg_catalog | PL/pgSQL procedural language
(1 row)
```

## Installing Extensions
{: #installing-extensions}

To install an extension on to a database use [`CREATE EXTENSION`](https://www.postgresql.org/docs/current/static/sql-createextension.html){: .external}. For example, to install `pg_stat_statements` on the `ibmclouddb` database, 

```sh
ibmclouddb=> CREATE EXTENSION pg_stat_statements;
CREATE EXTENSION
```
{: pre}

Extensions are installed into the `ibm_extension` schema. The schema is part of the `search_path` so extension objects do not need to be qualified with a schema. 
`ibm_extension` is a read-only schema.

If you run the `\dx` command after installing an extension, it appears in the table.
```sh
ibmclouddb=> \dx
                                     List of installed extensions
        Name        | Version |   Schema      |                        Description
--------------------+---------+---------------+-----------------------------------------------------------
 pg_stat_statements | 1.5     | ibm_extension | track execution statistics of all SQL statements executed
 plpgsql            | 1.0     | pg_catalog    | PL/pgSQL procedural language
(2 rows)
```

Database extensions in PostgreSQL are managed per database. If you have multiple databases that you need to install an extension on, run the `CREATE` command on each database.

## Upgrading Extensions
{: #upgrading-extensions}

If there is a newer version of an extension available than the one you currently have installed, use the `ALTER EXTENSION` to upgrade it.

## Extension-Specific Notes
{: #extensions-specific-notes}

### pg_repack
{: #pg_repack}

- [The `pg_repack` documentation](http://reorg.github.io/pg_repack/){: .external}
- When you run the `pg_repack` command, pass the -k flag in to bypass the check for superuser. For example,
   ```sh
   pg_repack -k [OPTION]... [DBNAME]
   ```
   {: pre}

- For `pg_repack` to run reliably, your deployment should be on PostgreSQL 9.6 and above.
- Any user can run `pg_repack`, but the command is only able to repack a table that they have permissions on.
- `pg_repack` needs to take an exclusive lock on objects it is reorganizing at the end of the reorganization. If it can't get this lock after a certain period, it cancels all conflicting queries. If it can't do so, the reorg fails. By default, only the admin user on PostgreSQL 9.6 and greater is able to cancel conflicting queries. To expose the ability to cancel queries to other database users, grant the `pg_signal_backend` role [from the admin user](/docs/databases-for-postgresql?topic=databases-for-postgresql-user-management#the-admin-user).

## Available Extensions
{: #available-extensions}

This list is what is returned from a {{site.data.keyword.databases-for-postgresql}} deployment running PostgreSQL version 12. For a list of available extensions on your deployment, use `SELECT name FROM pg_available_extensions;` in `psql`.

```sh
ibmclouddb=> SELECT name FROM pg_available_extensions order by 1;
```
{: pre}

```sh
             name             
------------------------------
 address_standardizer
 address_standardizer_data_us
 amcheck
 autoinc
 bloom
 btree_gin
 btree_gist
 citext
 cube
 dblink
 dict_int
 dict_xsyn
 earthdistance
 file_fdw
 fuzzystrmatch
 hstore
 insert_username
 intagg
 intarray
 isn
 lo
 ltree
 moddatetime
 pageinspect
 pg_buffercache
 pg_freespacemap
 pg_prewarm
 pg_repack
 pg_stat_statements
 pg_trgm
 pg_visibility
 pgaudit
 pgcrypto
 pgrouting
 pgrowlocks
 pgstattuple
 plpgsql
 postgis
 postgis_tiger_geocoder
 postgis_topology
 postgres_fdw
 refint
 seg
 sslinfo
 tablefunc
 tcn
 tsm_system_rows
 tsm_system_time
 unaccent
 uuid-ossp
 xml2
(51 rows)
ibmclouddb=> select version();
                                                 version                                                 
---------------------------------------------------------------------------------------------------------
 PostgreSQL 12.4 on x86_64-pc-linux-gnu, compiled by gcc (Debian 6.3.0-18+deb9u1) 6.3.0 20170516, 64-bit
(1 row)
 ```
