---

copyright:
  years: 2019
lastupdated: "2019-09-20"

subcollection: databases-for-postgresql

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Point-in-time Recovery
{: #pitr}

{{site.data.keyword.databases-for-postgresql_full}} offers Point-In-Time Recovery (PITR) for any time in the last 7 days. The deployment performs continuous incremental backups and can replay transactions to bring a new deployment restored from a backup to any point in that 7 day window you need.

The _Backups_ tab of your deployment's UI keeps all your PITR information under _Point-in-Time_.

![PITR section of the Backups tab](images/pitr-backups-tab.png)

Included information is the earliest time for a PITR. 

To discover the earliest recovery point through the API, use the [`/deployments/{id}/point_in_time_recovery_data`] endpoint to find the earliest PITR time. 
```
{
    "point_in_time_recovery_data": {
        "earliest_point_in_time_recovery_time": "2019-09-09T23:16:00Z"
    }
}
```

## Recovery

### In the UI
To initiate a PITR, enter the time you would like to restore back to in UTC. If you just want to restore to the most recent available time, select that option. Clicking **Restore** brings up the options for your recovery. Enter a name, select the version and region for the new deployment. Click **Recover** to start the process.

![Recovery Options Dialog](images/pitr-dialog.png)

If you use Key Protect and have a key, you have to ue the CLI to recover and a command is provided for your convenience.

Both backups and PITR are restored to a new deployment. The new deployment is auto-sized to the same disk and memory allocation as the source deployment at the time of the backup you are restoring from. After the new deployment finishes provisioning, your data is restored into the new deployment.
{: .tip}

### In the CLI

The Resource Controller supports provisioning of database deployments, and provisioning and restoring are the responsibility of the Resource Controller CLI. Use the [`resource service-instance-create`](/docs/cli?topic=cloud-cli-ibmcloud_commands_resource#ibmcloud_resource_service_instance_create) command.

For PITR, use the `point_in_time_recovery_time` and `point_in_time_recovery_deployment_id` parameters. The `point_in_time_recovery_deployment_id` is the source deployment's ID and `point_in_time_recovery_time` is the timestamp in UTC you want to restore to. If you want to restore to the latest available point-in-time use `"point_in_time_recovery_time":" "`.
```
ibmcloud resource service-instance-create <SERVICE_INSTANCE_NAME> <service-id> <region> -p '{"point_in_time_recovery_deployment_id":"DEPLOYMENT_ID", "point_in_time_recovery_time":"TIMESTAMP"}'
```

A pre-formatted command for a specific backup or PITR is available in detailed view of the backup.
{: .tip}

### In the API

The Resource Controller supports provisioning of database deployments, and provisioning and restoring are the responsibility of the Resource Controller API. You need to complete [the necessary steps to use the resource controller API](/docs/services/databases-for-postgresql?topic=cloud-databases-provisioning#provisioning-through-the-resource-controller-api) before you can use it to restore from a backup. 

Once you have all the information, the create request is a `POST` to the [`/resource_instances`](https://{DomainName}/apidocs/resource-controller#create-provision-a-new-resource-instance) endpoint.

```
curl -X POST \
  https://resource-controller.cloud.ibm.com/v2/resource_instances \
  -H 'Authorization: Bearer <>' \
  -H 'Content-Type: application/json' \
    -d '{
    "name": "<SERVICE_INSTANCE_NAME>",
    "target": "<region>",
    "resource_group": "<your-resource-group>",
    "resource_plan_id": "<service-id>"
    "point_in_time_recovery_time":"<TIMESTAMP>",
    "point_in_time_recovery_deployment_id":"<DEPLOYMENT_ID>"
  }'
```
The parameters `name`, `target`, `resource_group`, and `resource_plan_id` are all required. The `target` is the region where you want the new deployment to be located, which can be a different region from the source deployment. Cross-region restores are supported, with the exception of restoring a `eu-de` backup to another region.

For PITR, use the `point_in_time_recovery_time` and `point_in_time_recovery_deployment_id` parameters. The `point_in_time_recovery_deployment_id` is the source deployment's ID and `point_in_time_recovery_time` is the timestamp in UTC you want to restore to. If you want to restore to the latest available point-in-time use `"point_in_time_recovery_time":" "`.

## Verifying PITR

In order to verify the correct recovery time, you have to check the database logs. Checking the database logs requires the [Logging Integration](/docs/services/databases-for-postgresql?topic=cloud-databases-logging) to be set up on your deployment.

When you perform a recovery, your data is restored from the most recent incremental backup and any outstanding transactions from the WAL log are used to catch your database up to the time you recovered to. After the recovery is finished, and the transactions are run, the logs display a message. You can check that your logs have the message
```
LOG:  last completed transaction was at log time 2019-09-03 19:40:48.997696+00
```

There are two scenarios where recovery does not show up in the logs. 
1. Your deployment has a recent full backup and there is no activity after the backup was taken that needs to be replayed.
2. If you entered a time to recover to that is **after** the current time or is past latest available point-in-time recovery point.

In both cases the recovery is usually still successful, but there won't be an entry in the logs to check the exact time the database was restored to.