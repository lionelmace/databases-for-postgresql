---
copyright:
  years: 2019
lastupdated: "2019-03-06"

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

{{site.data.keyword.databases-for-postgresql_full}} allows you to change some of the PosgreSQL configuration settings so you can tune your PostgreSQL databases to your use-case. 
The configuration is defined in a schema. When you change the the configuration via the API or the CLI, you make changes to the configuration's JSON schema. To make a change, you make a JSON object with the settings and their new values. For example, to set the `max_connections` setting to 150, you would supply 
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

To change the configuration, send the settings you would like to change as a JSON object in the request body of a `PATCH` request to `/deployments/{id}/configuration`.

For more information, see the [API Reference](https://cloud.ibm.com/apidocs/cloud-databases-api#change-your-database-configuration)


## Available Configuration settings

Setting|Default|Notes
----------|-----|-----------
[`max_connections`](https://www.postgresql.org/docs/current/runtime-config-connection.html#max_connections)|115|[You may need to scale before increasing max connections.](/docs/databases-for-postgresql?topic=databases-for-postgresql-high-availability#connection-limits)
[`max_prepared_transactions`](https://www.postgresql.org/docs/current/runtime-config-resource.html#max_prepared_transactions)|0|The default value of `0` disables use of [prepared transactions](https://www.postgresql.org/docs/current/sql-prepare-transaction.html) and is strongly recommended unless you need to use them.
{: caption="Table 1. Configuration Settings for PostgreSQL" caption-side="top"}

