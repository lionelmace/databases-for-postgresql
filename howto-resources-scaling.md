---

Copyright:
  years: 2019
lastupdated: "2019-04-08"

subcollection: databases-for-postgresql

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Managing Resources and Scaling
{: #resources-scaling}

A visual representation of your data members and their resource allocation is available on the _Settings_ tab of your deployment's _Manage_ page. 

![The Scale Resources Panel in _Settings_](images/settings-scaling.png)

{{site.data.keyword.databases-for-postgresql_full}} runs with two data members in a cluster, and resources are allocated to both members equally. For example, the minimum storage of a PostgreSQL deployment is 10240 MB, which equates to an initial size of 5120 MB per member. The minimum RAM for a PostgreSQL deployment is 2048 MB, which equates to an initial allocation of 1028 MB per member.

Billing is based on the _total_ amount of resources that are allocated to the service.
{: .tip}

**Disk Usage** -
Your disk allocation has to be enough to store all of your data. Your data is replicated to both data members so the total amount of disk you use is at least twice the size of your data set. 

Disk allocation also affects the performance of the disk, with larger disks having higher performance. Baseline Input-Output Operations per second (IOPS) performance for disk is 10 IOPS for each GB. Scale disk to increase the IOPS your deployment can handle.

You cannot scale down storage. If your data set size has decreased, you can recover space by backing up and restoring to a new deployment.
{: .tip} 

**RAM** - 
If you find that your deployment is suffering from performance issues due to a lack of memory, you can scale the amount of RAM allocated to it. The amount of memory you allocate to your deployment is split between both members. Adding memory to the total allocation adds memory to both members equally.

[`work_mem`](https://www.postgresql.org/docs/current/runtime-config-resource.html#GUC-WORK-MEM), [`maintenance_work_mem`](https://www.postgresql.org/docs/current/runtime-config-resource.html#GUC-MAINTENANCE-WORK-MEM), and [`effective_cache_size`](https://www.postgresql.org/docs/current/runtime-config-query.html#GUC-EFFECTIVE-CACHE-SIZE) are auto-tuned based on the deployment's total memory. They are also set when you scale memory on your deployment. When you scale, the values are adjusted without outage to the running deployment.

The amount of memory allocated to the database's shared buffer pool is **not** adjusted automatically when you scale your deployment. Its recommended to be set to 25% of the deployment's total memory. You can manually tune the shared buffer pool through the [`shared_buffer`](https://www.postgresql.org/docs/current/runtime-config-resource.html#GUC-SHARED-BUFFERS) setting in your [PostgreSQL's configuration](/docs/services/databases-for-postgresql?topic=databases-for-postgresql-changing-configuration). It is not auto-tuned because changing the `shared_buffer` requires a database restart.

## Scaling via the UI

Adjust the slider to increase or decrease the resources that are allocated to your service. The slider controls how much memory or disk is allocated per member. The UI shows the total allocated memory or disk for the position of the slider. Click **Scale** to trigger the scaling operations and return to the dashboard overview. 

The UI currently uses a coarser-grained resolution for scaling than the CLI or API commands. Use the API or CLI to scale if the stops on the slider do not meet your size requirements.
{: .tip}

## Resources and Scaling in the CLI 

[{{site.data.keyword.cloud_notm}} CLI cloud databases plug-in](/docs/cli/reference/ibmcloud?topic=cloud-cli-ibmcloud-cli) supports viewing and scaling the resources on your deployment. To scale any of the available resource groups, use `cdb deployment-groups-set` command. 

For example, the command to view the resource groups for a deployment named "example-deployment":  
`ibmcloud cdb deployment-groups example-deployment`

This produces the output:

```
Group   member
Count   2
|
+   Memory
|   Allocation              2048mb
|   Allocation per member   1024mb
|   Minimum                 2048mb
|   Step Size               256mb
|   Adjustable              true
|
+   Disk
|   Allocation              10240mb
|   Allocation per member   5120mb
|   Minimum                 10240mb
|   Step Size               1024mb
|   Adjustable              true
```

The deployment has two members, with 2048 MB of RAM and 10240 MB of disk allocated in total. The "per member" allocation is 1024 MB of RAM and 5120 MB of disk. The minimum value is the lowest the total allocation can be set. The step size is the smallest amount by which the total allocation can be adjusted.

The `cdb deployment-groups-set` command allows either the total RAM or total disk allocation to be set, in MB. For example, to scale the memory of the "example-deployment" to 2048 MB of RAM for each memory member (for a total memory of 4096 MB), you use the command:  
`ibmcloud cdb deployment-groups-set example-deployment member --memory 4096`

## Resources and Scaling in the API

The _Foundation Endpoint_ that is shown on the _Overview_ panel of your service provides the base URL to access this deployment through the API. Use it with the `/groups` endpoint if you need to manage or automate scaling programmatically. 

To view the current and scalable resources on a deployment, use
```
curl -X GET -H "Authorization: Bearer $APIKEY" 'https://api.{region}.databases.cloud.ibm.com/v4/ibm/deployments/{id}/groups'
```

To scale the memory of a deployment to 2048 MB of RAM for each memory member (for a total memory of 4096 MB).
```
curl -X PATCH 'https://api.{region}.databases.cloud.ibm.com/v4/ibm/deployments/{id}/groups/member' \
-H "Authorization: Bearer $APIKEY" \
-H "Content-Type: application/json" \
-d '{"memory": {
        "allocation_mb": 4096
      }
    }'
```

More information is in the [API Reference](https://{DomainName}/apidocs/cloud-databases-api#get-currently-available-scaling-groups-from-a-depl).