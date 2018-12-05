---

Copyright:
  years: 2018
lastupdated: "2018-12-03"
---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Architecture

{{site.data.keyword.databases-for-postgresql_full}} is a managed cloud database service that runs in containers that are orchestrated by Kubernetes. It is fully integrated into the {{site.data.keyword.cloud_notm}} ecosystem. The database, storage, and monitoring all run in {{site.data.keyword.cloud_notm}}.

## PostgreSQL Databases

A {{site.data.keyword.databases-for-postgresql}} service contains a cluster with two data members, a leader member and a follower member. Data is replicated across both data members, and their disk space is spread over the region's availability zones. High-availability is managed with [Patroni](https://github.com/zalando/patroni), where three etcd arbiters maintain cluster state, manage replication, and handle failover. If one data member becomes unreachable, your cluster continues to operate normally.

### Storage

Database storage for {{site.data.keyword.databases-for-postgresql}} is provided on {{site.data.keyword.cloud_notm}} Block Storage. Backup storage is on {{site.data.keyword.cloud_notm}} Object Storage.

### Full-disk Encryption

All {{site.data.keyword.databases-for-postgresql}} services all have encryption at rest. Both your data and your backups reside on servers that have volume-level encryption enabled. If your environment requires that you control the encryption keys, [Key Protect integration](./reference-key-protect.html) is available.

## Portals

{{site.data.keyword.databases-for-postgresql}} database connections are managed by 2 HAProxy portals which automatically connect to the leader member of the PostgreSQL cluster. The portals are then behind a Kubernetes endpoint, which provides the connection for your applications. Having two portals allows for applications to maintain connectivity if one of the portals becomes unreachable.

### Encryption in Transit

All {{site.data.keyword.databases-for-postgresql}} connections are TLS/SSL enabled and support TLS version 1.2. Self-signed certificates are provided to enable applications to validate the server upon connection.

## Access Management

{{site.data.keyword.databases-for-postgresql}} is an IAM-integrated service. Access to the service is governed by the roles and attributes that are consistent with IAM-integrated services across {{site.data.keyword.cloud_notm}}. For more information about IAM, see the [What is IAM?](https://{DomainName}/docs/iam/index.html#iamoverview) documentation. For more information on how IAM affects access to {{site.data.keyword.databases-for-postgresql}}, see [Managing Access](./access-management.html).

Database-level access can be managed through PostgreSQL's native [user management and database roles](https://www.postgresql.org/docs/current/static/database-roles.html).

