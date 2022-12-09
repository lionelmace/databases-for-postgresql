---
copyright:
  years: 2017, 2022
lastupdated: "2022-12-08"

keywords: postgresql, databases, postgresql admin password, postgres admin password

subcollection: databases-for-postgresql

---

{{site.data.keyword.attribute-definition-list}}


# Setting the Admin Password
{: #admin-password}

## Setting the Admin Password in the UI
{: #admin-password-ui}
{: ui}

The {{site.data.keyword.databases-for-postgresql_full}} service is provisioned with an admin user, so you can manage PostgreSQL by using its command line tool, `psql`.

Set the admin password before using it to connect. To set the password through the {{site.data.keyword.cloud_notm}} dashboard, select _Manage_ from the service dashboard to open the management panel for your service. Open the _Settings_ tab, and use the _Change Database Admin Password_ pane to set a new admin password.

![The Change Database Admin Password pane in Settings](images/settings-admin-password.png){: caption="Figure 1. The Change Database Admin Password pane in _Settings_" caption-side="bottom"}

## Setting the admin password through the command line
{: #setting-admin-password-cli}
{: cli}

Use the `cdb user-password` command from the {{site.data.keyword.cloud_notm}} CLI cloud databases plug-in to set the admin password with the command line.

For example, to set the admin password for a deployment named "example-deployment", use the following command.
```sh
ibmcloud cdb user-password example-deployment admin <newpassword>
```
{: pre}

## Setting the admin password through the API
{: #setting-admin-password-api}
{: api}

The _Foundation Endpoint_ that is shown on the _Overview_ panel _Deployment Details_ section of your service provides the base URL to access this deployment through the API. Use it with the `/deployments/{id}/users/{username}` endpoint to set the admin password.
```sh
curl -X PATCH `https://api.{region}.databases.cloud.ibm.com/v4/ibm/deployments/{id}/users/admin' \
-H "Authorization: Bearer $APIKEY" \
-H "Content-Type: application/json" \
-d '{"password":"newrootpasswordsupersecure21"}'
```
{: pre}

For more information, see [API Reference](https://{DomainName}/apidocs/cloud-databases-api#set-database-level-user-s-password).
