# Lab 8 - SQL injection attack, querying the database type and version on MySQL and Microsoft

**Status**: Solved on August 2, 2023.

**Objective**: Display the database version string.

## Solving Process

- This lab is very similar to the one where an Oracle db version is obtained.
- Let's suppose that the query has the same shape as the Oracle one, it means that it has 2 columns and both accept strings.
- With that info, a payload can be prepared for different database versions. With the help of the [SQLi Cheatsheet](https://portswigger.net/web-security/sql-injection/cheat-sheet), several payloads are tested, and the following one returned the data successfully:
  - `' UNION SELECT @@version,NULL#`, in plain text
  - `'+union+select+%40%40version,null%23`, URL Encoded (the actual payload used)
- The version returned, within the response's HTML, was `8.0.33-0ubuntu0.20.04.4`
- From the `@@version` and the `#` for comments, it seems we're dealing with a MySQL db.

### Important notes

- The database uses a different version that requires `#` for comments. I also noted that the payload needs to be url encoded in order for it to work.
- If the payload isn't url encoded, the server returns an internal error.
