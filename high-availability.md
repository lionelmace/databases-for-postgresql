---

Copyright:
  years: 2018, 2019
lastupdated: "2019-02-07"

subcollection: databases-for-postgresql

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# High-Availability
{: #high-availability}

{{site.data.keyword.databases-for-postgresql_full}} is a managed cloud database service that is fully integrated into the {{site.data.keyword.cloud_notm}} ecosystem. The database, storage, and supporting infrastructure all run in {{site.data.keyword.cloud_notm}}.

{{site.data.keyword.databases-for-postgresql}} provides replication, fail-over, and high-availability features to protect your databases and data from infrastructure maintenance, upgrades, and failures. Deployments contain a cluster with two data members, a leader and a replica. Both members contain a copy of your data using asynchronous replication, with a distributed consensus mechanism to maintain cluster state and handle failovers. If one data member becomes unreachable, your cluster continues to operate normally.

By contrast, application resilience and connection error handling are the responsibility of the application developer.

## Application-level High-Availability

Applications that communicate over networks and cloud services are subject to transient connection failures. You want to design your applications to retry connections when errors are caused by a temporary loss in connectivity to your deployment or to {{site.data.keyword.cloud_notm}}.

Because {{site.data.keyword.databases-for-postgresql}} is a managed service, regular updates and database maintenance occurs as part of normal operations. This can occasionally cause short intervals where your database is unavailable. It can also cause the database to trigger a graceful fail-over, retry, and reconnect. It takes a short time for the database to determine which member is a replica and which is the leader, so you might also see a short connection interruption. Failovers generally take less than 30 seconds.

Your applications have to be designed to handle temporary interruptions to the database, implement error handling for failed database commands, and implement retry logic to recover from a temporary interruption.

Several minutes of database unavailability or connection interruption is not expected. Open a [support ticket](https://cloud.ibm.com/unifiedsupport/cases/add) with details if you have time periods longer than a minute with no connectivity so we can investigate.

## Resource Scaling

{{site.data.keyword.databases-for-postgresql}} deployments do not auto-scale. Deployment owners can [monitor](/docs/services/databases-for-postgresql?topic=databases-for-postgresql-monitoring) the state of the deployment, estimate typical resource usage, and scale the deployment accordingly.

If you are planning on running operations that might put a spike in the usual RAM usage, or any data operations that could overflow your allotted storage, you can manually scale your service's resources up first to avoid hitting any limits that can affect deployment operations.

## Connection Limits 

{{site.data.keyword.databases-for-postgresql}} sets the maximum number of connections to your PostgreSQL database to **115**. 15 connections are reserved for the superuser to maintain the state and integrity of your database, and 100 connections are available for you and your applications. If you see a string of connection failures and the error,
```
FATAL: remaining connection slots are reserved for
non-replication superuser connections
```
your application is exceeding this connection limit. The recommended solution is to make use of connection pooling. It is possible to raise the connection limit, but connections to the database consume resources. To raise the connection limit, you might have to [scale your deployment](/docs/services/databases-for-postgresql?topic=databases-for-postgresql-dashboard-settings#scaling-resources) and then [change the PostgreSQL configuration](/docs/services/databases-for-postgresql?topic=databases-for-postgresql-changing-configuration#changing-configuration).

### Connection Pooling

Exceeding the connection limit for your deployment can cause your database to be unreachable by your applications.

Application designers are responsible for monitoring and managing the number of connections that are being made to the database. Many PostgreSQL driver libraries have connection pooling classes and functions. You need to consult your driver's documentation to implement connection pooling that is optimal for your use case.

Alternatively, you can use a third-party tool such as [PgBouncer](https://pgbouncer.github.io/) to manage your application's connections.

## SLA

{{site.data.keyword.databases-for-postgresql}} deployments conform to the {{site.data.keyword.cloud_notm}} [SLA terms](/docs/overview?topic=overview-SLAs#SLAs).

