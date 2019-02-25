---

Copyright:
  years: 2018
lastupdated: "2018-12-05"
---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# About {{site.data.keyword.databases-for-postgresql_full_notm}}
{: #about}

{{site.data.keyword.databases-for-postgresql_full}} is a managed PostgreSQL service that is hosted in the {{site.data.keyword.cloud_notm}} and integrated with other {{site.data.keyword.cloud_notm}} services. PostgreSQL provides a powerful, open source object-relational database that is highly customizable. With PostgreSQL, development is fast and easily scalable. You can develop in a language that you're comfortable with, such as C/C++, Perl, Python, TCL/TK, Delphi/Kylix, VB, PHP, ASP, and Java. You get a feature-rich enterprise database with JSON support, giving you the best of both the SQL and NoSQL worlds. 
{:shortdesc}

## Provisioning {{site.data.keyword.databases-for-postgresql}}

{{site.data.keyword.databases-for-postgresql}} is an {{site.data.keyword.cloud_notm}} service. Provisioning and account management is handled through your {{site.data.keyword.cloud_notm}} account. If you already have an account, you can provision {{site.data.keyword.databases-for-postgresql}} from the [{{site.data.keyword.cloud_notm}} catalog](https://{DomainName}/catalog/services/databases-for-postgresql).

For detailed provisioning information, including {{site.data.keyword.cloud_notm}} CLI instructions, see the [Provisioning](/docs/services/databases-for-postgresql?topic=databases-for-postgresql-provisioning) page.

If you don't yet have an {{site.data.keyword.cloud_notm}} account, sign up on the [registration](https://{DomainName}/registration/) page.

### Managing Access to {{site.data.keyword.databases-for-postgresql}}

{{site.data.keyword.databases-for-postgresql}} is an Identity and Access Management (IAM) integrated service. Access to the service is governed by the roles and attributes that are consistent across IAM-integrated services in {{site.data.keyword.cloud_notm}}. Get started with managing your users on the [IAM Getting Started tutorial](/docs/iam?topic=iam-getstarted). For more information on IAM, see the [What is IAM?](/docs/iam?topic=iam-iamoverview) documentation.

More information on IAM roles and actions for the {{site.data.keyword.databases-for-postgresql}} service is available on the [Access Management](/docs/services/databases-for-postgresql?topic=databases-for-postgresql-iam) page.

## Using {{site.data.keyword.databases-for-postgresql}}

{{site.data.keyword.databases-for-postgresql}} provides a UI, accessible by selecting _Manage_ from the left sidebar of your service and opening the management panel. You get a quick [Overview](/docs/services/databases-for-postgresql?topic=databases-for-postgresql-dashboard-overview) of your service as well as configuration settings on the [Settings](/docs/services/databases-for-postgresql?topic=databases-for-postgresql-dashboard-settings) tab and access to your backups on the [Backups](/docs/services/databases-for-postgresql?topic=databases-for-postgresql-dashboard-backups) tab.

### Using the command-line interface

The [{{site.data.keyword.cloud_notm}} command-line interface](/docs/cli/reference/ibmcloud?topic=cloud-cli-install_use) provides in interactive terminal for your {{site.data.keyword.cloud_notm}} account and your {{site.data.keyword.cloud_notm}} services. The cloud databases plug-in extends this functionality to your {{site.data.keyword.databases-for-postgresql}} deployments. More information and installation instructions are on the [{{site.data.keyword.cloud_notm}} CLI Plug-in](/docs/databases-cli-plugin?topic=cloud-databases-cli-cdb-reference) page.


### Using the cloud databases API

{{site.data.keyword.databases-for-postgresql}} is compatible with the {{site.data.keyword.IBM_notm}} {{site.data.keyword.databases-for}} API, so you can access and manage your service programmatically. Each region has a unique endpoint, so you can find the API foundation endpoint for your deployment on the [_Overview_](/docs/services/databases-for-postgresql?topic=databases-for-postgresql-dashboard-overview) page. The {{site.data.keyword.IBM_notm}} API documentation contains the full [{{site.data.keyword.databases-for}} API reference](https://{DomainName}/apidocs/cloud-databases-api). Authentication is IAM-based and you use your {{site.data.keyword.cloud_notm}} account's platform API keys to access the API. More information on API keys is in the [IAM documentation](/docs/iam/apikeys?topic=iam-manapikey).

## PostgreSQL Database Administration

Although {{site.data.keyword.databases-for-postgresql}} is a managed service, you can still access your PostgreSQL databases directly with `psql`. Your deployment comes with a default admin user, so [set the admin password](/docs/services/databases-for-postgresql?topic=databases-for-postgresql-admin-password) and use it to [connect with `psql`](/docs/services/databases-for-postgresql?topic=databases-for-postgresql-connecting-psql). You can use `psql` to [install extensions](/docs/services/databases-for-postgresql?topic=databases-for-postgresql-extensions), set up [logical replication](/docs/services/databases-for-postgresql?topic=databases-for-postgresql-logical-replication), manage database roles and privileges, and many other administrative functions.

## Connecting to {{site.data.keyword.databases-for-postgresql}}

General information on getting connection strings for your applications, for `psql`, and for users you provision on your account can be found on the [Getting Connection Strings](/docs/services/databases-for-postgresql?topic=databases-for-postgresql-connection-strings) page.

Specific guidance on connecting with PostgreSQL drivers is on the [Connecting External Applications](/docs/services/databases-for-postgresql?topic=databases-for-postgresql-external-app) page. If you want to connect a Cloud Foundry application that is running in {{site.data.keyword.cloud_notm}}, see the [Connecting an {{site.data.keyword.cloud_notm}} Application](/docs/services/databases-for-postgresql?topic=databases-for-postgresql-ibmcloud-app) page. The [Getting Started tutorial](/docs/services/databases-for-postgresql?topic=databases-for-postgresql-getting-started) that provides a sample application that can run locally or on {{site.data.keyword.cloud_notm}} to test-drive your {{site.data.keyword.databases-for-postgresql}} deployment.

## Other {{site.data.keyword.cloud_notm}} Integrations

{{site.data.keyword.databases-for-postgresql}} deployments offer other cloud services integrations. 
- View events with [Activity Tracker](/docs/services/databases-for-postgresql?topic=databases-for-postgresql-activity-tracker)
- Monitor deployment resource use with the [Monitoring](/docs/services/databases-for-postgresql?topic=databases-for-postgresql-monitoring) service.
- BYOK encryption is available if you use [Key Protect](/docs/services/databases-for-postgresql?topic=databases-for-postgresql-key-protect)









