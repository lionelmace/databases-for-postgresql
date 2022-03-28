---
copyright:
  years: 2020, 2022
lastupdated: "2022-03-28"

keyowrds: postgresql, databases, upgrading, major versions, changing versions

subcollection: databases-for-postgresql

---

{:external: .external target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Upgrading to a new Major Version
{: #upgrading}

Once a major version of a database is at its End Of Life (EOL), it is a good idea to upgrade to a current major version. 

You can find the available versions of PostgreSQL on the [{{site.data.keyword.databases-for-postgresql_full}} the catalog](https://cloud.ibm.com/catalog/databases-for-postgresql) page, from the cloud databases cli plug-in command [`ibmcloud cdb deployables-show`](/docs/databases-cli-plugin?topic=databases-cli-plugin-cdb-reference#deployables-show), or from the cloud databases API [`/deployables`](https://cloud.ibm.com/apidocs/cloud-databases-api#get-all-deployable-databases) endpoint.

## Requirements for upgrading to PostgreSQL (v 13, 14) from PostgreSQL (v10, 11, 12)
{: #upgrading-reqs}

- If you have `pg_repack` installed, you need to remove it before performing the upgrade. This can be done with 

```shell
DROP EXTENSION pg_repack; 
```

After upgrading, you can re-install `pg_repack`.

- If you are using PostGIS, you must upgrade to PostGIS 3.1 prior to upgrading. This can by done by running the following against a database with PostGIS installed 

```shell
SELECT * FROM update_to_postgis_31();
```

## Upgrading from a Read-only Replica
{: #upgrading-replica}

You can also upgrade by setting up a read-only replica. A quick summary: you provision a read-only replica with the same database version as your deployment, and wait while it replicates all of your data. Once your deployment and its replica are synced, you can promote and upgrade the read-only replica to a full, stand-alone deployment running the new version of the database.

The [Configuring Read-only Replicas](/docs/databases-for-postgresql?topic=databases-for-postgresql-read-only-replicas) doc covers the provisioning, syncing, and other details for read-only replicas. To perform the upgrade and promotion step, use a POST to the [`/deployments/{id}/remotes/promotion`](https://cloud.ibm.com/apidocs/cloud-databases-api#promote-read-only-replica-to-a-full-deployment) endpoint with the version that you want to upgrade to in the body of the request.
```curl
curl -X POST \
  https://api.{region}.databases.cloud.ibm.com/v4/ibm/deployments/{id}/remotes/promotion \
  -H 'Authorization: Bearer <>'  \
 -H 'Content-Type: application/json' \
 -d '{
    "promotion": {
        "version": "11",
        "skip_initial_backup": false
    }
}' \
```
{: pre}

`skip_initial_backup` is optional. If set to `true`, the new deployment does not take an initial backup when the promotion completes. Your new deployment is available in a shorter amount of time, at the expense of not being backed up until the next automatic backup is run, or you take an on-demand backup.

## Backup and Restore Upgrade
{: #backup-restore}

One way to upgrade your database version is to [restore a backup](/docs/databases-for-postgresql?topic=cloud-databases-dashboard-backups#restoring-a-backup) of your data into a new deployment that is running the new database version.

### Upgrading in the UI
{: #upgrading-ui}

You can upgrade to a new version when [restoring a backup](/docs/databases-for-postgresql?topic=cloud-databases-dashboard-backups#restoring-a-backup) from the _Backups_ menu of your _Deployment dashboard_. Clicking **Restore** on a backup brings up a dialog box where you can change some options for the new deployment. One of them is the database version, which is auto-populated with the versions available for you to upgrade to. Select a version and click **Restore** to start the provision and restore process.

### Upgrading through the CLI
{: #upgrading-cli}

When you upgrade and restore from backup through the {{site.data.keyword.cloud_notm}} CLI, use the provisioning command from the resource controller.
```shell
ibmcloud resource service-instance-create <service-name> <service-id> <service-plan-id> <region>
```
{: pre}

The parameters `service-name`, `service-id`, `service-plan-id`, and `region` are all required. You also supply the `-p` with the version and backup ID parameters in a JSON object. The new deployment is automatically sized with the same disk and memory as the source deployment at the time of the backup.

```shell
ibmcloud resource service-instance-create example-upgrade databases-for-postgresql standard us-south \
-p \ '{
  "backup_id": "crn:v1:bluemix:public:databases-for-postgresql:us-south:a/54e8ffe85dcedf470db5b5ee6ac4a8d8:1b8f53db-fc2d-4e24-8470-f82b15c71717:backup:06392e97-df90-46d8-98e8-cb67e9e0a8e6",
  "version":11
}'
```
{: pre}

### Upgrading through the API
{: #upgrading-api}

Similar to provisioning through the API, you need to complete [the necessary steps to use the resource controller API](/docs/databases-for-postgresql?topic=cloud-databases-provisioning#provisioning-through-the-resource-controller-api) before you can use it to upgrade from a backup. Then, send the API a POST request. The parameters `name`, `target`, `resource_group`, and `resource_plan_id` are all required. You also supply the version and backup ID. The new deployment has the same memory and disk allocation as the source deployment at the time of the backup.
```curl
curl -X POST \
  https://resource-controller.cloud.ibm.com/v2/resource_instances \
  -H 'Authorization: Bearer <>' \
  -H 'Content-Type: application/json' \
    -d '{
    "name": "my-instance",
    "target": "bluemix-us-south",
    "resource_group": "5g9f447903254bb58972a2f3f5a4c711",
    "resource_plan_id": "databases-for-postgresql-standard",
    "backup_id": "crn:v1:bluemix:public:databases-for-postgresql:us-south:a/54e8ffe85dcedf470db5b5ee6ac4a8d8:1b8f53db-fc2d-4e24-8470-f82b15c71717:backup:06392e97-df90-46d8-98e8-cb67e9e0a8e6",
    "version":11
  }'
```
{: pre}

### Dry running the promotion and upgrade
{: #promotion-dry-run}

In order to evaluate the effects of major version upgrades, you can trigger a dry run. A dry run does not perform the promotion and upgrade, but it does simulate it, with the results printed to the database logs. You can access and view your database logs through the [Log Analysis Integration](/docs/databases-for-postgresql?topic=cloud-databases-logging). This ensures the version you are currently running with its extensions can be successfully upgraded to your intended version.

The dry run must be run with `skip_initial_backup` set to `false`, and `version` defined.
```curl
curl -X POST \
  https://api.{region}.databases.cloud.ibm.com/v4/ibm/deployments/{id}/remotes/promotion \
  -H 'Authorization: Bearer <>'  \
 -H 'Content-Type: application/json' \
 -d '{
    "promotion": {
        "version": "11",
        "skip_initial_backup": false,
        "dry_run": true
    }
}' \
```
{: pre}

## Changelog for Major PostgreSQL Versions
{: #changelog-postgres}

- [PostgreSQL 10](https://www.postgresql.org/docs/10/release-10.html)
- [PostgreSQL 11](https://www.postgresql.org/docs/11/release-11.html)
- [PostgreSQL 12](https://www.postgresql.org/docs/current/release-12.html)
- [PostgreSQL 13](https://www.postgresql.org/docs/13/release-13.html)
- [PostgreSQL 14](https://www.postgresql.org/docs/14/release-14.html)
