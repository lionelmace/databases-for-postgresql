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

# Connecting an external application
{: #connecting-external-app}

Your applications and drivers use connection strings to make a connection to {{site.data.keyword.databases-for-postgresql_full}}. The service provides connection strings specifically for drivers and applications. 

## Getting Connection Strings

The easiest way to get connection strings for an application is to create a set of _Service Credentials_ specifically for your application to connect with. Doing so also returns all the connection information a JSON object in a click-to-copy field.

Alternatively, the {{site.data.keyword.cloud_notm}} CLI [cloud databases plug-in](./howto-getting-connection-strings.html#generating-connection-strings-from-the-command-line) supports generating users and connection strings, as does the [{{site.data.keyword.IBM_notm}} {{site.data.keyword.databases-for}} API](https://{DomainName}/apidocs/cloud-databases-api#creates-a-database-level-user).

Full documentation on generating and retrieving connection strings is on the [Getting Connection Strings](./howto-getting-connection-strings.html) page.

## Connecting with a language's driver

All the information a driver needs to make a connection to your deployment is in the "postgres" section of your connection strings. The table contains a breakdown for reference.

Field Name|Index|Description
----------|-----|-----------
`Type`||Type of connection - for PostgreSQL, it is "URI"
`Scheme`||Scheme for a URI - for PostgreSQL, it is "postgresql"
`Path`||Path for a URI - for PostgreSQL, it is the database name. The default is `ibmclouddb`.
`Authentication`|`Username`|The username that you use to connect.
`Authentication`|`Password`|A password for the user - might be shown as `$PASSWORD`
`Authentication`|`Method`|How authentication takes place; "direct" authentication is handled by the driver.
`Hosts`|`0...`|A hostname and port to connect to
`Composed`|`0...`|A URI combining Scheme, Authentication, Host, and Path
`Certificate`|`Name`|The allocated name for the self-signed certificate for database deployment
`Certificate`|Base64|A base64 encoded version of the certificate.
{: caption="Table 1. `postgres`/`URI` connection information" caption-side="top"}

* `0...` indicates that there might be one or more of these entries in an array.

Many PostgreSQL drivers are able to make a connection to your deployment when given the URI-formatted connection string found in the "composed" field of the connection information. For example,
```
postgres://ibm_cloud_30399dec_4835_4967_a23d_30587a08d9a8:$PASSWORD@981ac415-5a35-4ac7-b6bb-fb609326dc42.8f7bfd8f3faa4218aec56e069eb46187.databases.appdomain.cloud:32704/ibmclouddb?sslmode=verify-full
```

PostgreSQL has a vast array of language drivers. The table covers a few of the most common.

Language|Driver|Examples
----------|-----------
PHP|`pgsql`|[Link](http://php.net/manual/en/pgsql.examples-basic.php)
Ruby|`ruby-pg`|[Link](https://bitbucket.org/ged/ruby-pg/wiki/Home)
Ruby on Rails|Rails|[Rails Guide](http://edgeguides.rubyonrails.org/configuring.html#configuring-a-postgresql-database)
Python|`Psycopg2`|[Link](https://wiki.postgresql.org/wiki/Psycopg2_Tutorial)
C#|`ODBC`|[Link](https://wiki.postgresql.org/wiki/Using_Microsoft_.NET_with_the_PostgreSQL_Database_Server_via_ODBC)
Go|`pq`|[Link](https://godoc.org/github.com/lib/pq)
Node|`node-postgres`|[Link](https://github.com/brianc/node-postgres/wiki/Example)
Java|`JDBC driver`|[Link](https://jdbc.postgresql.org/documentation/head/index.html)
{: caption="Table 2. PostgreSQL drivers" caption-side="top"}

## Driver TLS and self-signed certificate support

All connections to {{site.data.keyword.databases-for-postgresql}} are TLS 1.2 enabled, so the driver you use to connect needs to be able to support encryption. Your deployment also comes with a self-signed certificate so the driver can verify the server upon connection. 

### Using the self-signed certificate

1. Copy the certificate information from the Base64 field of the connection information. 
2. Decode the Base64 string into text and save it to a file. (You can use the Name that is provided or your own file name).
3. Provide the path to the driver.

### CLI plug-in support for the self-signed certificate

You can display the decoded certificate for your deployment with the CLI plug-in with the command `ibmcloud cdb deployment-cacert "your-service-name"`. It decodes the base64 into text. Copy and save the command's output to a file and provide the file's path to the driver.







 
