---
copyright:
  years: 2017,2018
lastupdated: "2018-09-27"
---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}


# Connecting with `psql`

You can access your PostgreSQL database directly from its command line client, `psql`. You can use `psql` for direct interaction and monitoring of the data structures that are created within the database. It is also useful for testing and monitoring the queries and performance, installing and modifying scripts, and other management activities.

The admin user comes with the PostgreSQL default role [`pg_monitor`](https://www.postgresql.org/docs/10/static/default-roles.html), allowing access to PostgreSQL monitoring views and functions. By default, the admin user does not have permissions on objects that are created by other users.

You have to set the admin password before you use it to connect to the database. For more information, see the [Setting the Admin Password](./admin-password.html) page.
{: .tip}

## Installing `psql`

Install the command line client for PostgreSQL, `psql`. To use `psql`, the PostgreSQL client tools need to be installed on the local system. They can be installed with the full PostgreSQL package that is provided from postgresql.org, or from your operating systems packages. For more information about `psql`, see the [PostgreSQL documentation](https://www.postgresql.org/docs/current/static/app-psql.html).

## `psql` Connection Strings

{{site.data.keyword.databases-for-postresql_full}} provides connection strings specifically for CLI clients. They contain all the relevant pieces of connection information. You can get the admin connection strings by following the steps in the [Getting your Connection Strings](./working-connection-strings) page. 

A [table](./working-connection-strings#the-cli-section) with a breakdown of all the CLI connection information is also included for reference.

## Connecting with `psql`

The `ibmcloud cdb deployment-connections` command handles everything that is involved in creating a command line client connection. For example, to connect to a deployment named  "example-postgres", use the following command.

```
ibmcloud cdb deployment-connections example-postgres -start
```
Or
```
ibmcloud cdb cxn example-postgres -s
```

The command prompts for the admin password and then runs the `psql` command line client to connect to the database.

If you have not installed the cloud databases plug-in, connect to your PostgreSQL databases using `psql` by giving it the "composed" connection string. It provides environment variables `PGPASSWORD` and `PGSSLROOTCERT`. Set `PGPASSWORD` to the admin's password and `PGSSLROOTCERT` to the path or file name for the self-signed certificate.  

```
PGPASSWORD=$PASSWORD PGSSLROOTCERT=0b22f14b-7ba2-11e8-b8e9-568642342d40 psql 'host=4a8148fa-3806-4f9c-b3fc-6467f11b13bd.8f7bfd7f3faa4218aec56e069eb46187.databases.appdomain.cloud port=32325 dbname=ibmclouddb user=admin sslmode=verify-full'
```

