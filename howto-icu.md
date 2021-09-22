---
copyright:
  years: 2019, 2021
lastupdated: "2021-08-30"

keywords: postgresql, databases, icu

subcollection: databases-for-postgresql

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# PostgreSQL ICU Support
{: #postgres-icu}

[ICU](http://site.icu-project.org/) (International Components for Unicode) uses C/C++ and Java libraries to provide Unicode and Globalization internationalization and localization facilities, including collation. ICU provides stability to your database by preventing index corruption caused by operating system collation changes. 

{{site.data.keyword.databases-for-postgresql_full}} supports ICU. To use ICU, your tables need to be created with ICU support, as outlined in the [PostgreSQL Collation Support documentation](http://www.postgresql.org/docs/10/static/collation.html). 

ICU-based collations are offered alongside the `libc` collations. `libc` uses the locales provided by the operating system C library and are the locales that most tools provided by the operating system use. Building with ICU support does not remove `libc` collation support. For more information regarding PostgreSQL collation, see [PostgreSQL Collation Support](https://www.postgresql.org/docs/12/collation.html). 

For example, if you previously specified 

`CREATE TABLE ... (... x text COLLATE "en_US" ...)`

you might now 

`CREATE TABLE ... (... x text COLLATE "en-x-icu" ...)`
