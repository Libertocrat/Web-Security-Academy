# Lab: SQL injection vulnerability in WHERE clause allowing retrieval of hidden data

**Objective**: display all products, released and unreleased.

## Briefing

This lab contains a SQL injection vulnerability in the product category filter. When the user selects a category, the application carries out a SQL query like the following:

`SELECT * FROM products WHERE category = 'Gifts' AND released = 1`

To solve the lab, perform a SQL injection attack that causes the application to display one or more unreleased products.

## Solution

Use the following payload as the `category` parameter:

``` SQL
' OR 1=1 --
```
