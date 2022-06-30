---

copyright:
  years: 2021, 2022
lastupdated: "2022-06-30"

keywords: IBM Cloud, databases, ICD, IAM authorization

subcollection: databases-for-postgresql

---

{:external: .external target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Configuring IAM Authorizations
{: #pgsql-iam}

To create a storage configuration for a newly created IBM Cloud Databases for PostgreSQL enabled by IBM Cloud Satellite service location, you need to configure IAM authorizations.
{: shortdesc}

## Configure IAM Authorizations
{: #cd-postgresql-iamauth}

- Configure your IAM Authorizations under the **Manage** tab.

- Choose the **Authorizations** tab from the left menu.
- Click the **Create** button to create an authorization to allow a service instance access to another service instance.

## Grant a service authorization
{: #cd-postgresql-serviceauth}

The source service is the service that is granted access to the target service. The roles that you select define the level of access for this service. The target service is the service that you are granting permission to be accessed by the source service based on the assigned roles.

- In the **Source Service** field, select **Databases for PostgreSQL development**.
- In the **Target Service** field, select **Satellite**.
- Select all options:
    - **Satellite Cluster Creator** 
    - **Satellite Link Administrator** 
    - **Satellite Link Source Access Controller** 
    - Then, **Authorize**.
