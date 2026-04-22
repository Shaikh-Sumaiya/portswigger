# Lab: SQL injection attack, querying the database type and version on Oracle

## Vulnerability
This lab demonstrates an SQL Injection vulnerability that allows an attacker to extract database version information using a UNION-based 
attack.

## Explanation
The application is vulnerable to SQL Injection because user input is directly included in the SQL query without proper sanitization.
By using a UNION query, it is possible to combine the original query results with a custom query to extract data from the database.

## Steps to Reproduce
1. Navigate to a product category page.
2. Modify the URL parameter by injecting:
  '+UNION+SELECT+BANNER,+NULL+FROM+v$version--

<img width="1103" height="661" alt="image" src="https://github.com/user-attachments/assets/5ff19518-db0e-4c9c-9d11-05c00e0b2da9" />
   
3. Observe that the response now includes database version information.

## How It Works
- The single quote (') closes the original query string.
- UNION allows combining the original query with a new query.
- SELECT BANNER, NULL retrieves version information from the Oracle system table.
- v$version is a system table that stores database version details.
- NULL is used to match the number of columns in the original query.
- The comment operator (--) ignores the rest of the original query.

Final query becomes:
SELECT column1, column2 FROM products 
WHERE category = 'Gifts'
UNION
SELECT BANNER, NULL FROM v$version--;

This causes the database to return both application data and database version details.

## Impact
An attacker can extract sensitive information about the database, which can be used to perform further targeted attacks.

## Learning
UNION-based SQL Injection allows attackers to extract data from arbitrary tables by combining their query with the original query. 
Matching the number of columns is essential for a successful UNION attack.
