---
copyright:
  years: 2017, 2022
lastupdated: "2022-11-10"

keywords: postgresql, databases, postgres connections string, postgresql connection string

subcollection: databases-for-postgresql

---

{:external: .external target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}
{{site.data.keyword.attribute-definition-list}}


# Getting Connection Strings
{: #connection-strings}

## Getting Connection Strings in the UI
{: #connection-strings-ui}
{: ui}

To connect to {{site.data.keyword.databases-for-postgresql_full}}, you need some users and connection strings. Connection Strings for your deployment are displayed on the _Dashboard Overview_, in the _Endpoints_ panel. 

![Endpoints panel on the Dashboard Overview](images/getting-started-endpoints-panel.png){: caption="Figure 1. Endpoints panel on the Dashboard Overview" caption-side="bottom"}

A {{site.data.keyword.databases-for-postgresql}} deployment is provisioned with an admin user, and after [setting the admin password](/docs/databases-for-postgresql?topic=databases-for-postgresql-admin-password), you can use its connection strings to connect to your deployment.
{: .tip}

## Getting Connection Strings in the CLI
{: #connection-strings-cli}
{: cli}

Grab connection strings from the [CLI](/docs/databases-cli-plugin?topic=databases-cli-plugin-cdb-reference#deployment-connections).
```sh
ibmcloud cdb deployment-connections example-deployment -u <newusername> [--endpoint-type <endpoint type>]
```
{: pre}

Full connection information is returned by the `ibmcloud cdb deployment-connections` command with the `--all` flag. To retrieve all the connection information for a deployment named "example-deployment", use the following command.
```sh
ibmcloud cdb deployment-connections example-deployment -u <newusername> --all [--endpoint-type <endpoint type>]
```
{: pre}

If you don't specify a user, the `deployment-connections` commands return information for the admin user by default. If you don't specify an endpoint type, the connection string returns the public endpoint by default. If your deployment has only a private endpoint, you must specify `--endpoint-type private` or the commands return an error. The user and endpoint type is not enforced. You can use any user on your deployment with either endpoint (if both exist on your deployment).

To use the `ibmcloud cdb` CLI commands, you must [install the {{site.data.keyword.databases-for}} plug-in](/docs/databases-for-mongodb?topic=databases-cli-plugin-cdb-reference#installing-the-cloud-databases-cli-plug-in).
{: .tip}

## Getting Connection Strings in the API
{: #connection-strings-api}
{: api}

To retrieve user's connection strings from the API, use the [`/users/{userid}/connections`](https://{DomainName}/apidocs/cloud-databases-api#discover-connection-information-for-a-deployment-f-e81026) endpoint. You must specify in the path which user and which type of endpoint (public or private) is to be used in the returned connection strings. The user and endpoint type is not enforced. You can use any user on your deployment with either endpoint (if both exist on your deployment).
```sh
curl -X GET -H "Authorization: Bearer $APIKEY" 'https://api.{region}.databases.cloud.ibm.com/v4/ibm/deployments/{id}/users/{userid}/connections/{endpoint_type}'
```
{: pre}

## Connection String Breakdown
{: #connection-string-breakdown}

### The PostgreSQL Section
{: #postgres-section}

The "postgres" section contains information that is suited to applications that make connections to PostgreSQL.

| Field Name | Index | Description |
| ---------- | ----- | ----------- |
| `Type` | | Type of connection - for PostgreSQL, it is "URI" |
| `Scheme` | | Scheme for a URI - for PostgreSQL, it is "postgresql" |
| `Path` | | Path for a URI - for PostgreSQL, it is the database name. The default is `ibmclouddb`. |
| `Authentication` | `Username` | The username that you use to connect. |
| `Authentication` | `Password` | A password for the user - might be shown as `$PASSWORD` |
| `Authentication` | `Method`|How authentication takes place; "direct" authentication is handled by the driver. |
| `Hosts` | `0...` | A hostname and port to connect to |
| `Composed` | `0...` | A URI combining Scheme, Authentication, Host, and Path |
| `Certificate` | `Name` | The allocated name for the self-signed certificate for database deployment |
| `Certificate` | Base64 | A base64 encoded version of the certificate. |
{: caption="Table 1. postgresql/URI connection information" caption-side="top"}

* `0...` indicates one or more of these entries in an array.

### The CLI Section
{: #cli-section}

The "CLI" section contains information that is suited for connecting with `psql`.

| Field Name | Index | Description |
| ---------- | ----- | ----------- |
| `Bin` | | The binary to create a connection; in this case it is `psql`. |
| `Composed` | | A formatted command to establish a connection to your deployment. The command combines the `Bin` executable, `Environment` variable settings, and uses `Arguments` as command line parameters. |
| `Environment` | | A list of key/values you set as environment variables. |
| `Arguments` | 0... | The information that is passed as arguments to the command shown in the Bin field. |
| `Certificate` | Base64 | A self-signed certificate that is used to confirm that an application is connecting to the appropriate server. It is base64 encoded. |
| `Certificate` | Name | The allocated name for the self-signed certificate. |
| `Type` | | The type of package that uses this connection information; in this case `cli`.  |
{: caption="Table 2. psql/cli connection information" caption-side="top"}

* `0...` indicates that there might be one or more of these entries in an array.

