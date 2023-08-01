# Lab 3 - SQL injection UNION attack determining the number of columns returned by the query

**Status**: Solved the 1st August 2023.

**Objective**: Determine the number of columns returned by the original query

## Solving Process

- The `category` parameter is modified with an `UNION` based payload, to assess the number of columns that the original query outputs. Since the number of columns used in both sides of `UNION` must match.
  - Let's first try if the query has 1 column, by using this payload: `' UNION SELECT NULL --`
  - An error is received, indicating that the original query has more than 1 column
- Now, let's try with two, and so on, until the error disappears:
  - Two cols: `' UNION SELECT NULL,NULL --` generated an internal server error
  - Three cols: `' UNION SELECT NULL,NULL,NULL --` generated *no errors*
- The number of columns is 3
