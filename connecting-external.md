---
copyright:
  years: 2017,2019
lastupdated: "2019-04-10"

subcollection: databases-for-postgresql

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Connecting an external application
{: #external-app}

Your applications and drivers use connection strings to make a connection to {{site.data.keyword.databases-for-postgresql_full}}. The service provides connection strings specifically for drivers and applications. Connection strings are displayed in the _Connections_ panel of your deployment's _Overview_, and can also be retrieved from the [cloud databases CLI plugin](/docs/databases-cli-plugin?topic=cloud-databases-cli-cdb-reference#deployment-connections), and the [API](https://{DomainName}/apidocs/cloud-databases-api#discover-connection-information-for-a-deployment-f-e81026).

The connection strings can be used by any of the credentials you have created on your deployment. While you can use the admin user for all of your connections and applications, it might be better to create users specifically for your applications to connect with. Documentation on generating credentials is on the [Creating Users and Getting Connection Strings](/docs/services/databases-for-postgresql?topic=databases-for-postgresql-connection-strings) page.

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
Node|`node-postgres`|[Link](https://node-postgres.com/)
Java|`JDBC driver`|[Link](https://jdbc.postgresql.org/documentation/head/index.html)
{: caption="Table 2. PostgreSQL drivers" caption-side="top"}

## Driver TLS and self-signed certificate support

All connections to {{site.data.keyword.databases-for-postgresql}} are TLS 1.2 enabled, so the driver you use to connect needs to be able to support encryption. Your deployment also comes with a self-signed certificate so the driver can verify the server upon connection. 

### Using the self-signed certificate

1. Copy the certificate information from the _Connections_ panel or the Base64 field of the connection information. 
2. If needed, decode the Base64 string into text. 
3. Save the certificate  to a file. (You can use the Name that is provided or your own file name).
4. Provide the path to the certificate to the driver or client.

### CLI plug-in support for the self-signed certificate

You can display the decoded certificate for your deployment with the CLI plug-in with the command `ibmcloud cdb deployment-cacert "your-service-name"`. It decodes the base64 into text. Copy and save the command's output to a file and provide the file's path to the driver.







 
