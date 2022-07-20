---
copyright:
  years: 2017, 2022
lastupdated: "2022-07-20"

keywords: postgresql, databases, psql, postgresql command line

subcollection: databases-for-postgresql

---

{:external: .external target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}


# Connecting with `psql`
{: #connecting-psql}

You can access your PostgreSQL database directly from its command-line client, `psql`. You can use `psql` for direct interaction and monitoring of the data structures that are created within the database. It is also useful for testing and monitoring the queries and performance, installing and modifying scripts, and other management activities.

The admin user comes with the PostgreSQL default role [`pg_monitor`](https://www.postgresql.org/docs/10/static/default-roles.html){: .external}, allowing access to PostgreSQL monitoring views and functions. By default, the admin user does not have permissions on objects that are created by other users.

You must set the admin password before you use it to connect to the database. For more information, see the [Setting the Admin Password](/docs/databases-for-postgresql?topic=databases-for-postgresql-admin-password) page.
{: .tip}

## Installing `psql`
{: #installing-psql}

Install the command-line client for PostgreSQL, `psql`. To use `psql`, the PostgreSQL client tools need to be installed on the local system. They can be installed with the full PostgreSQL package that is provided from [postgresql.org](https://www.postgresql.org/download/){: .external}, or as a [package from your operating system's package manager](https://www.compose.com/articles/postgresql-tips-installing-the-postgresql-client/){: .external}. 

For more information about `psql`, see the [PostgreSQL documentation](https://www.postgresql.org/docs/current/static/app-psql.html){: .external}.

## `psql` Connection Strings
{: #psql-connection-strings}

Connection strings are displayed in the _Endpoints_ panel of your deployment's _Overview_, and can also be retrieved from the [cloud databases CLI plug-in](/docs/databases-cli-plugin?topic=databases-cli-plugin-cdb-reference#deployment-connections), and the [API](https://{DomainName}/apidocs/cloud-databases-api#discover-connection-information-for-a-deployment-f-e81026).

The information that you need to make a connection with `psql` is in the "cli" section of your connection strings. The table contains a breakdown for reference.

| Field Name|Index|Description|
| ---------- | ----- | ----------- |
| `Bin` | | The recommended binary to create a connection; in this case it is `psql`. |
| `Composed` | | A formatted command to establish a connection to your deployment. The command combines the `Bin` executable, `Environment` variable settings, and uses `Arguments` as command-line parameters. |
| `Environment` | | A list of key/values you set as environment variables. |
| `Arguments` | 0... | The information that is passed as arguments to the command shown in the Bin field. |
| `Certificate` | Base64 | A self-signed certificate that is used to confirm that an application is connecting to the appropriate server. It is base64 encoded. |
| `Certificate` | Name | The allocated name for the self-signed certificate. |
| `Type` | | The type of package that uses this connection information; in this case `cli`. |
{: caption="Table 1. psql/cli connection information" caption-side="top"}

* `0...` indicates that there might be one or more of these entries in an array.

## Creating a command-line client connection
{: #create-cli-connection}

Before creating a command-line client connection, ensure that you have [set the Admin password](/docs/databases-for-postgresql?topic=databases-for-postgresql-admin-password) for your deployment.

The `ibmcloud cdb deployment-connections` command handles everything that is involved in creating a command-line client connection. For example, to connect to a deployment named "example-postgres", use the following command:

```sh
ibmcloud cdb deployment-connections example-postgres --start
```
{: pre}

or
```sh
ibmcloud cdb cxn example-postgres -s
```
{: pre}

The command prompts for the admin password and then runs the `psql` command-line client to connect to the database.

If you have not installed the cloud databases plug-in, connect to your PostgreSQL databases using `psql` by giving it the "composed" connection string. It provides environment variables `PGPASSWORD` and `PGSSLROOTCERT`. Set `PGPASSWORD` to the admin's password and `PGSSLROOTCERT` to the path or file name for the self-signed certificate. 

```sh
PGPASSWORD=$PASSWORD PGSSLROOTCERT=0b22f14b-7ba2-11e8-b8e9-568642342d40 psql 'host=4a8148fa-3806-4f9c-b3fc-6467f11b13bd.8f7bfd7f3faa4218aec56e069eb46187.databases.appdomain.cloud port=32325 dbname=ibmclouddb user=admin sslmode=verify-full'
```
{: .codeblock}

## Using the self-signed certificate
{: #using-certificate}

1. Copy the certificate information from the _Endpoints_ panel or the Base64 field of the connection information. 
2. If needed, decode the Base64 string into text. 
3. Save the certificate to a file. (You can use the Name that is provided or your own file name).
4. Provide the path to the certificate to the `ROOTCERT` environment variable.

You can display the decoded certificate for your deployment with the CLI plug-in with the command:
```sh
ibmcloud cdb deployment-cacert "your-service-name"
```
{: pre}

The command decodes the base64 into text. Copy and save the command's output to a file and provide the file's path to the `ROOTCERT` environment variable.

Another option is to add `&sslrootcert=/path/to/cert` to your connection string, for example:

```sh
postgres://$USERNAME:$PASSWORD@6eb96148-90bc-49a0-a5a4-dc2b53334653.btdl8mld0r95fevivv30.databases.appdomain.cloud:32109/ibmclouddb?sslmode=verify-full&sslrootcert=/path/to/cert
```