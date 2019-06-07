---
copyright:
  years: 2019
lastupdated: "2019-05-23"

subcollection: databases-for-postgresql

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Changing the PostgreSQL Configuration
{: #changing-configuration}

{{site.data.keyword.databases-for-postgresql_full}} allows you to change some of the PosgreSQL configuration settings so you can tune your PostgreSQL databases to your use-case. To make permanent changes to the database configuration, use the {{site.data.keyword.databases-for}} [cli-plugin](/docs/databases-cli-plugin?topic=cloud-databases-cli-cdb-reference#deployment-configuration) or [API](https://{DomainName}/apidocs/cloud-databases-api#change-your-database-configuration) to write the changes to the configuration file for your deployment.

The configuration is defined in a schema. To make a change, you send a JSON object with the settings and their new values to the API or the CLI.  For example, to set the `max_connections` setting to 150, you would supply 
```
{"configuration":{"max_connections":150}}
```
to either the CLI or to the API.

## Using the CLI

To change your configuration through the {{site.data.keyword.databases-for}} cli-plugin, use `deployment-configuration` command. 
```
ibmcloud cdb deployment-configuration <deployment name or CRN> [@JSON_FILE | JSON_STRING]
```

The command reads the changes you would like to make from the JSON object or a file. For more information, see the [reference page](/docs/databases-cli-plugin?topic=cloud-databases-cli-cdb-reference#deployment-configuration).

## Using the API

There are two deployment-configuration endpoints, one for viewing the configuration schema and one for changing the configuration. To view the configuration schema, send a `GET` request to `/deployments/{id}/configuration/schema`.

To change the configuration, send the settings that you would like to change as a JSON object in the request body of a `PATCH` request to `/deployments/{id}/configuration`.

For more information, see the [API Reference](https://cloud.ibm.com/apidocs/cloud-databases-api#change-your-database-configuration)


## Available Configuration settings

### Memory Settings

[`shared_buffers`](https://www.postgresql.org/docs/current/runtime-config-resource.html#GUC-SHARED-BUFFERS)
  - Default - `32000` (number of 8 KiB buffers, or about 262 MB) 
  - Restarts Database? - **Yes**
  - Options - The maximum number of buffers is 1048576. 
  - Notes - The setting specifies the number of 8 KiB shared memory buffers. For example, 1 GB of `shared_buffers` space is 1048576 KiB, and (1048576 KiB / 8 KiB) is 131072 buffers. The recommended memory allocation for `shared_buffers` is 1/4 of the deployment's RAM. Setting `shared_buffers` any higher can result in memory issues leading to a database crash. Setting `shared_buffers` equal, close to equal, or higher than the amount of allocated memory will prevent the database from starting. The maximum amount of total space for `shared_buffers` is 8 GB or 1048576 buffers based on recommendations from the PostgreSQL community. Your deployment can make use of additional RAM for caching and performance, even without allocating it to `shared_buffers` . You do not have to configure the database to use all of the allocated RAM in order for your deployment to use it.

### General Settings

[`max_connections`](https://www.postgresql.org/docs/current/runtime-config-connection.html#GUC-MAX-CONNECTIONS)
  - Default - 115
  - Restarts Database? - **YES**
  - Notes - [You might need to scale before you increase max connections.](/docs/databases-for-postgresql?topic=databases-for-postgresql-high-availability#connection-limits)

[`max_prepared_transactions`](https://www.postgresql.org/docs/current/runtime-config-resource.html#GUC-MAX-PREPARED-TRANSACTIONS)
  - Default - 0
  - Restarts Database? - **YES**
  - Notes - The default value of `0` disables use of [prepared transactions](https://www.postgresql.org/docs/current/sql-prepare-transaction.html) and is strongly recommended unless you need to use them.

[`synchronous_commit`](https://www.postgresql.org/docs/current/wal-async-commit.html)
  - Default - `local`
  - Restarts Database? - No
  - Options - `local` or `off`
  - Notes - Setting `synchronous_commit` to `off` increases transaction commit rate at the expense of a loss of committed transactions in the event of an unclean shutdown.

[`effective_io_concurrency`](https://www.postgresql.org/docs/current/runtime-config-resource.html#GUC-EFFECTIVE-IO-CONCURRENCY)
  - Default - `12`
  - Restarts Database - No
  - Notes - It is recommended to leave this setting at the default. Only increase it you have profiled SQL queries and have observed inefficient bitmap heap scans. As [IOPS are tied to disk size](/docs/services/databases-for-postgresql?topic=databases-for-postgresql-high-availability#disk-iops), increasing this setting on default or smaller sized disks is also not recommended.

[`deadlock_timeout`](https://www.postgresql.org/docs/current/runtime-config-locks.html)
  - Default - 10000
  - Restarts Database - No
  - Options - Minimum value of 100
  - Notes - The number of milliseconds to wait before checking for deadlock and the duration where lock waits are logged. Logs available through the [logging integration](/docs/services/databases-for-postgresql?topic=cloud-databases-logging). Setting this too low negatively impacts performance.

