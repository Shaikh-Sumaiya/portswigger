# Lab: SQL injection vulnerability in WHERE clause allowing retrieval of hidden data

## Vulnerability
This lab demonstrates a SQL Injection vulnerability in the WHERE clause, allowing an attacker to retrieve hidden data from the database.

## Explanation
The application filters products by category and only displays released items. The SQL query likely includes a condition to restrict results.

Example:
SELECT * FROM products WHERE category = 'Gifts' AND released = 1;

Since user input is directly included in the query without proper validation, it is vulnerable to SQL Injection.

## Steps to Reproduce
1. Navigate to a product category (e.g., Accessories).

<img width="1160" height="603" alt="image" src="https://github.com/user-attachments/assets/29632289-3243-4fd7-a1e4-394a64d3da59" />

We can see the category in the URl, so let's try modifying it

2. Modify the URL parameter by injecting:
   ' OR 1=1--

<img width="1074" height="658" alt="image" src="https://github.com/user-attachments/assets/22efb81f-042f-469c-8c4c-97a4b51d3309" />

Observe that all products are displayed, including hidden/unreleased ones.

## How It Works
- The single quote (') breaks out of the original string.
- OR 1=1 adds a condition that is always TRUE.
- The comment operator (--) ignores the rest of the query, including conditions like AND released = 1.

Final query becomes:
SELECT * FROM products WHERE category = 'Gifts' OR 1=1-- AND released = 1;

This causes the database to return all records.

## Impact
An attacker can bypass application restrictions and access sensitive or hidden data.

## Learning
SQL Injection allows attackers to manipulate query logic. By injecting conditions that always evaluate to true, attackers can bypass 
filters and retrieve unintended data.
