---

copyright:
  years: 2019, 2022
lastupdated: "2022-06-30"

keywords: postgresql, scaling, memory, disk IOPS, CPU, postgresql dedicated cores, scaling postgresql

subcollection: databases-for-postgresql

---

{:external: .external target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Scaling Disk, RAM, and CPU
{: #resources-scaling}

You can manually adjust the amount of resources available to your {{site.data.keyword.databases-for-postgresql_full}} deployment to suit your workload and the size of your data.

## Resource Breakdown
{: #resource-breakdown}

{{site.data.keyword.databases-for-postgresql}} deployments have two data members in a cluster, and resources are allocated to both members equally. For example, the minimum storage of a PostgreSQL deployment is 10240 MB, which equates to an initial size of 5120 MB per member. The minimum RAM for a PostgreSQL deployment is 2048 MB, which equates to an initial allocation of 1028 MB per member.

Billing is based on the _total_ amount of resources that are allocated to the service.
{: .tip}

### Disk
{: #disk-allocation}

Your disk allocation must be enough to store all of your data. Your data is replicated to both data members so the total amount of disk that you use is at least twice the size of your data set. 

Disk allocation also affects the performance of the disk, with larger disks having higher performance. Baseline input/output operations per second (IOPS) performance for disk is 10 IOPS for each GB. Scale disk to increase the IOPS that your deployment can handle.

You cannot scale down storage.
{: .tip} 

### RAM
{: #ram-allocation}

If you find that your deployment is suffering from performance issues due to a lack of memory, you can scale the amount of RAM allocated to it. The amount of memory you allocate to your deployment is split between both members. Adding memory to the total allocation adds memory to both members equally.

[`work_mem`](https://www.postgresql.org/docs/current/runtime-config-resource.html#GUC-WORK-MEM), [`maintenance_work_mem`](https://www.postgresql.org/docs/current/runtime-config-resource.html#GUC-MAINTENANCE-WORK-MEM), and [`effective_cache_size`](https://www.postgresql.org/docs/current/runtime-config-query.html#GUC-EFFECTIVE-CACHE-SIZE) are auto-tuned based on the deployment's total memory. They are also set when you scale memory on your deployment. When you scale, the values are adjusted without outage to the running deployment.

The amount of memory allocated to the database's shared buffer pool is **not** adjusted automatically when you scale your deployment. Setting it to 25% of the deployment's total memory is recommended. You can manually tune the shared buffer pool through the [`shared_buffer`](https://www.postgresql.org/docs/current/runtime-config-resource.html#GUC-SHARED-BUFFERS) setting in your [PostgreSQL's configuration](/docs/databases-for-postgresql?topic=databases-for-postgresql-changing-configuration). It is not auto-tuned because changing the `shared_buffer` requires a database restart.

### Dedicated Cores
{: #resources-dedicated-cores}

You can enable or increase the CPU allocation to the deployment. With dedicated cores, your resource group is given a single-tenant host with a reserve of CPU shares. Your deployment is then guaranteed the minimum number of CPUs you specify. The default of 0 dedicated cores uses compute resources on shared hosts. Going from a 0 to a >0 CPU count provisions and moves your deployment to new hosts, and your databases are restarted as part of that move. Going from >0 to a 0 CPU count, moves your deployment to a shared host and also restarts your databases as part of the move.

## Scaling Considerations
{: #resources-scaling-consider}

- Scaling your deployment up might cause your databases to restart. If your scaled deployment needs to be moved to a host with more capacity, then the databases are restarted as part of the move.

- Scaling down RAM or CPU will only trigger database restarts if CPU reaches zero, in which case pods will move back to the shared host. 

- Disk cannot be scaled down.

- A few scaling operations can be more long running than others. Enabling dedicated cores moves your deployment to its own host and can take longer than just adding more cores. Similarly, drastically increasing CPU, RAM, or Disk can take longer than smaller increases to account for provisioning more underlying hardware resources.

- Scaling operations are logged in [{{site.data.keyword.at_full}}](/docs/databases-for-postgresql?topic=cloud-databases-activity-tracker).

- If you find consistent trends in resource usage or would like to set up scaling when certain resource thresholds are reached, see [enabling autoscaling](/docs/databases-for-postgresql?topic=databases-for-postgresql-autoscaling).

## Scaling in the UI
{: #resources-scaling-ui}

A visual representation of your data members and their resource allocation is available on the _Resources_ tab of your deployment's _Manage_ page. 

![The Scale Resources Panel in Resources](images/scaling-update.png){: caption="Figure 1. The Scale Resources Panel in _Resources_" caption-side="bottom"}

Adjust the slider to increase or decrease the resources that are allocated to your service. The slider controls how much memory or disk is allocated per member. The UI shows the total allocated memory or disk for the position of the slider. Click **Scale** to trigger the scaling operations and return to the dashboard overview. 

The UI currently uses a coarser-grained resolution for scaling than the CLI or API commands. Use the API or CLI to scale if the stops on the slider do not meet your size requirements.
{: .tip}

## Scaling in the CLI 
{: #resources-scaling-cli}

[{{site.data.keyword.cloud_notm}} CLI cloud databases plug-in](/docs/databases-cli-plugin?topic=databases-cli-plugin-cdb-reference) supports viewing and scaling the resources on your deployment. To scale any of the available resource groups, use `cdb deployment-groups-set` command. 

For example, the command to view the resource groups for a deployment named "example-deployment" is, 
```shell
ibmcloud cdb deployment-groups example-deployment
```
{: pre}

This produces the output,
```shell
Group   member
Count   2
|
+   Memory
|   Allocation              4096mb
|   Allocation per member   2048mb
|   Minimum                 2048mb
|   Step Size               256mb
|   Adjustable              true
|
+   CPU
|   Allocation              6
|   Allocation per member   3
|   Minimum                 6
|   Step Size               2
|   Adjustable              true
|
+   Disk
|   Allocation              10240mb
|   Allocation per member   5120mb
|   Minimum                 10240mb
|   Step Size               1024mb
|   Adjustable              true
```

The deployment has two members, with 4096 MB of RAM and 10240 MB of disk allocated in total. The "per member" allocation is 2048 MB of RAM and 5120 MB of disk. The minimum value is the lowest the total allocation can be set. The step size is the smallest amount by which the total allocation can be adjusted.

The `cdb deployment-groups-set` command allows either the total RAM or total disk allocation to be set, in MB. For example, to scale the memory of the "example-deployment" to 2048 MB of RAM for each memory member (for a total memory of 4096 MB), you use the command 
```shell
ibmcloud cdb deployment-groups-set example-deployment member --memory 4096
```
{: pre}

## Scaling in the API
{: #resources-scaling-api}

The _Foundation Endpoint_ that is shown on the _Overview_ panel _Deployment details_ of your service provides the base URL to access this deployment through the API. Use it with the `/groups` endpoint if you need to manage or automate scaling programmatically. 

To view the current and scalable resources on a deployment, use
```curl
curl -X GET -H "Authorization: Bearer $APIKEY" 'https://api.{region}.databases.cloud.ibm.com/v4/ibm/deployments/{id}/groups'
```
{: pre}

To scale the memory of a deployment to 2048 MB of RAM for each memory member (for a total memory of 4096 MB).
```curl
curl -X PATCH 'https://api.{region}.databases.cloud.ibm.com/v4/ibm/deployments/{id}/groups/member' \
-H "Authorization: Bearer $APIKEY" \
-H "Content-Type: application/json" \
-d '{"memory": {
        "allocation_mb": 4096
      }
    }'
```

More information is in the [API Reference](https://{DomainName}/apidocs/cloud-databases-api#get-currently-available-scaling-groups-from-a-depl).
