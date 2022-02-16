---
layout: post
title: "How to change all columns' and tables' collation to 'utf8_bin' in MySQL"
subtitle:   "jira mysql collation utf8_bin"
date:       2018-08-15 0:20:00 +0800
author:     "Dongyupu"
header-img: "img/post-bg-01.jpg"
---

https://confluence.atlassian.com/jirakb/how-to-change-all-columns-and-tables-collation-to-utf8_bin-in-mysql-601456761.html


# How to change all columns' and tables' collation to 'utf8_bin' in MySQL
Related content
Portfolio Plan missing after updating SQL Server Collation
How to fix the collation of a Microsoft SQL Server JIRA database
Upgrade to 6.3 Fails Due to Unable to create the Quartz trigger
Installing JIRA Agile gives Error message "JIRA Agile is currently unavailable. This might be because an upgrade task has failed to run or has not not yet completed"
Issue Types missing, blank or duplicated in Issue Type Schemes
Still need help?

# Diagnosis
Find database, database tables, and database columns with collations different than the recommended utf8_bin using the below queries. Then, take note of the ones returned for next steps.

~~~sql
SELECT DEFAULT_COLLATION_NAME FROM information_schema.SCHEMATA S WHERE schema_name = '<database-name>' AND DEFAULT_COLLATION_NAME != 'utf8_bin';
SELECT * FROM information_schema.COLUMNS WHERE table_schema = '<database-name>' AND collation_name != 'utf8_bin';
SELECT * FROM information_schema.TABLES WHERE table_schema = '<database-name>' AND table_collation != 'utf8_bin';
~~~

(info) Replace <database-name> with the name of the database used by your JIRA application instance.

# Step-by-step guide
Always back up your data before performing any modification to the database. If possible, try your modifications on a test server.

Stop your JIRA application;
Run the below queries in the application database to alter the database default collation;

~~~sql
ALTER DATABASE <database-name> CHARACTER SET utf8 COLLATE utf8_bin;
~~~

(info) Replace <database-name> with the name of the database used by your JIRA application instance.

Disable MySQL's constraint checks, otherwise it won't allow modifications on the tables;

~~~sql
SET foreign_key_checks = 0;
~~~

Alter the collation of a table with the below query;

~~~sql
ALTER TABLE <table-name> CONVERT TO CHARACTER SET utf8 COLLATE utf8_bin;
~~~

(info) Replace <table-name> with the name of a table returned by the second query in the Diagnosis section.

If the above fails, use the below instead;

~~~sql
 ALTER TABLE <table-name> CHARACTER SET utf8 COLLATE utf8_bin;
~~~

If the query for tables set with wrong collation ran on the Diagnostics step returns more than one result, you can run the below steps to generate a set of queries for all affected tables in a file.

(info) The following is an example provided for UNIX platform only.

1. Generate a file containing the ALTER TABLE queries.

~~~sql
SELECT
    CONCAT('ALTER TABLE ',  table_name, ' CONVERT TO CHARACTER SET utf8 COLLATE utf8_bin;') INTO OUTFILE '/tmp/alterstatements.sql'
FROM
    information_schema.tables
WHERE
    table_schema='<database-name>'
    AND table_collation != 'utf8_bin' 
GROUP BY table_name;
~~~

2. Check if the output file is correct.

~~~bash
cat /tmp/alterstatements.sql
~~~

3. Run the SQL file against the current database.

~~~bash
mysql my_jira_db  < /tmp/alterstatements.sql
After running the query for all affected tables , verify that the queries below now return no results;
~~~

~~~sql
SELECT * FROM information_schema.COLUMNS WHERE table_schema = '<database-name>' AND collation_name != 'utf8_bin';
SELECT * FROM information_schema.TABLES WHERE table_schema = '<database-name>' AND table_collation != 'utf8_bin';
~~~

If the above queries return any results, take note of the tables it returns and execute the below query, which will define the default collation for the table;

~~~sql
ALTER TABLE <table-name> DEFAULT CHARACTER SET utf8 COLLATE utf8_bin;
~~~

Revert the MySQL constraint checks for foreign keys with the below query;

~~~sql
SET foreign_key_checks = 1;
~~~
