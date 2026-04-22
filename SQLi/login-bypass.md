# Lab: SQL Injection vulnerability allowing login bypass

## Vulnerability
This lab demonstrates a SQL Injection vulnerability in the login functionality, allowing authentication bypass.

## Explanation
The application constructs a SQL query using user-supplied input for username and password without proper sanitization.

Example query:
SELECT * FROM users 
WHERE username = 'INPUT' AND password = 'INPUT';

Since the input is directly embedded into the query, it is vulnerable to SQL Injection.

## Steps to Reproduce
1. Navigate to the login page.
2. Enter the following:
   - Username: administrator'--
   - Password: anything randomly (e.g., 1234567)

<img width="454" height="351" alt="image" src="https://github.com/user-attachments/assets/d88d0fa2-1882-4a46-9772-d8b00d35376f" />

3. Submit the form.

<img width="1249" height="641" alt="image" src="https://github.com/user-attachments/assets/3de40f65-bf1b-4d68-aeba-720567e13cce" />


4. Observe that login is successful without a valid password.

## How It Works
- The single quote (') closes the username string.
- The comment operator (--) ignores the rest of the query, including the password condition.

Final query becomes:
SELECT * FROM users 
WHERE username = 'administrator'--' AND password = '1234567';

Due to the comment, the password check is skipped, and the query effectively becomes:
SELECT * FROM users 
WHERE username = 'administrator';

## Impact
An attacker can bypass authentication and gain unauthorized access to user accounts, including administrative accounts.

## Learning
SQL Injection can be used to bypass authentication by manipulating query structure and disabling critical conditions like password 
verification.
