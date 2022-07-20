---
copyright:
  years: 2021, 2022
lastupdated: "2022-07-20"

keywords: postgresql, databases, instance

subcollection: databases-for-postgresql

---

{:external: .external target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}
{:important: .important}

# Assigning storage to service cluster
{: #instance-sat}

**Create** an instance in the Satellite location and verify that a cluster has been created and the instance is provisioning. 
When the service cluster is created, if the corresponding hosts are available then the hosts are typically **auto-assigned** to the cluster.

## Cluster Configuration
{: #cluster-config}

To assign an existing Satellite storage configuration to IBM Cloud™ Databases for PostgreSQL enabled by IBM Cloud Satellite service, the Satellite location administrator must perform the following manual step to fetch the `ROKS` cluster ID and Service Cluster ID from the **application developer**.
{: important}

- This should be done using the Satellite CLI, as shown below:

```sh
 ibmcloud sat storage assignment create --config CONFIG [--name NAME] [-q] (--cluster CLUSTER | --group GROUP | --service-cluster-id CLUSTER)
PARAMETERS:
    --name value                The name of a Satellite storage assignment.
    --group value, -g value     The cluster groups that you want to assign the storage configuration to.
    --service-cluster-id value  The ID of the service cluster that you want to assign the storage configuration to. To get the service cluster ID, run 'ibmcloud sat service ls --location <location>'.
    --cluster value, -c value   The ID of the Satellite cluster that you want to assign the storage configuration to. To get the cluster ID, run 'ibmcloud oc cluster ls --provider satellite'.
    --config value              The Satellite storage configuration to use for the assignment.
    -q                          Do not show the message of the day or update reminders.
```
{: pre}

- Once this step is complete, the next step is to deploy the requested database instance.
