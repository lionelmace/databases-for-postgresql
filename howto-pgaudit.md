---

copyright:
  years: 2020
lastupdated: "2021-11-11"

keywords: postgresql, databases, pgaudit, logging, session, object, pg role

subcollection: databases-for-postgresql

---

{:external: .external target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Logging with pgAudit
{: #pgaudit}

The PostgreSQL Audit Extension (pgAudit) provides enablement of session logging for your {{site.data.keyword.databases-for-postgresql_full}} deployments. 

## Session Logging
{: #session-logging}

Session logging is off by default. You can enable Session logging parameters that will log all activity for sets of audit event types. Session logging is enabled for the whole DB cluster and is either `on` or `off` for a specific event type.

## Event Types
{: #event-types}

Session logging is configured per event type. The supported event types across all versions are:  
* FUNCTION 
* ROLE
* DDL
* MISC
* MISC_SET (this additional type is only supported in PostgreSQL 12 or greater)

Further details of these event types and what they log are [documented here](https://github.com/pgaudit/pgaudit/blob/master/README.md#pgauditlog).


## Enabling pgAudit session logging
{: #enable-pgaudit-session-logging}

To enable pgAudit session logging, connect as the admin user and call the `set_pgaudit_session_logging` function with the appropriate event parameters specified. Session logging is enabled directly in the database and no API or CLI access is provided. 

For example, to enable DDL and ROLE you would call:
```shell
SELECT public.set_pgaudit_session_logging('{ddl, role}');
```
{: .codeblock}

Any subsequent calls replace the existing configuration; they are not additive. For example, a subsequent call to `SELECT public.set_pgaudit_session_logging('{misc}');` would log only `misc` but disable `ddl` and `role`.
{: .note}

## Disabling pgAudit
{: #disable-pgaudit}

To disable audit logging: call the same function with `none` specified. For example:
```shell
SELECT public.set_pgaudit_session_logging('{none}');
```
{: .codeblock}

Changing audit levels happens immediately when calling the function without interrupting the database activity.
{: .note}

## Audit Logs
{: #pgaudit-logs}

Audit events appear in {{site.data.keyword.la_full}} with the following format:
```shell
LOG: AUDIT: SESSION,1,1,DDL,CREATE TABLE,,,create table f2 (id int);,<not logged>
```
The format is documented [here](https://github.com/pgaudit/pgaudit/blob/master/README.md#format). 

If you want to see the current log level, you can run the command:  
```shell
show pgaudit.log;
```
{: .codeblock}

