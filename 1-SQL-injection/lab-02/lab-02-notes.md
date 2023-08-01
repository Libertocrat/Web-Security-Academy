# Lab 2: SQL injection vulnerability allowing login bypass

**Status**: Solved the 1st August 2023.

**Objective**: Login to the application as the administrador user.

## Solving Process

- In the login page, let's verify if the user field is vulnerable, by adding `administrator' --` as payload.
- It's necessary to enter a password, any text will be useful just to let the form send the data
- The payload did work, since by adding comments after the `administrator'` username, the password condition was ignored and the query evaluated to true thus letting us know login.
