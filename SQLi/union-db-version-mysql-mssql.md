# Lab: SQL injection attack, querying the database type and version on MySQL and Microsoft

## Vulnerability
This lab demonstrates an SQL Injection vulnerability that allows an attacker to extract database version information using a UNION-based 
attack.

## Explanation
The application is vulnerable because user input is directly embedded into a SQL query without proper sanitization. This allows attackers 
to manipulate the query and extract additional data.
In this lab, the goal is to determine the database type and version.

## Steps to Reproduce

### Method 1: Browser
1. Navigate to a product category page.
2. Modify the URL parameter by injecting:
   '+UNION+SELECT+@@version,+NULL--
3. Observe that no version information is clearly visible on the webpage and it shows internal server error. So, let's try another way.

---

### Method 2: Using Burp Suite
1. Open the application and enable interception in Burp Suite.
2. Capture the request when selecting a category.

<img width="1314" height="708" alt="image" src="https://github.com/user-attachments/assets/ae789114-64cc-450f-9556-671d4c20bded" />


3. Send the request to Repeater.
4. Modify the parameter catrgory by injecting:
   ' UNION SELECT 'abc','def'#

<img width="583" height="316" alt="image" src="https://github.com/user-attachments/assets/0dbfb2fa-a3b5-4e4a-a4ec-eeab00586bcb" />

<img width="77" height="67" alt="image" src="https://github.com/user-attachments/assets/b0384166-e144-4965-932c-1b716d5c9a39" />

   
6. Modify the parameter by injecting:
   ' UNION SELECT @@version, NULL#

<img width="589" height="141" alt="image" src="https://github.com/user-attachments/assets/3d935b2e-0344-4118-b829-74548648b362" />

   
8. Send the request and analyze the response, we can also see the version in browser at the end of the web page after opening the URL in
   browser

<img width="275" height="64" alt="image" src="https://github.com/user-attachments/assets/09b1ae42-5449-4901-89e9-c5b0167ca562" />


## Observation
- The database version was not clearly visible in the browser response.
- However, in Burp Suite Repeater, the raw HTTP response revealed the database version.

## How It Works
- The single quote (') closes the original query string.
- UNION combines the original query with the injected query.
- @@version retrieves database version and system information.
- NULL is used to match the number of columns.
- The comment operator (#) ignores the rest of the original query.

Final query becomes:
SELECT column1, column2 FROM products 
WHERE category = 'Accessories'
UNION
SELECT @@version, NULL#;

## Impact
An attacker can extract sensitive database and system information, which can be used for further targeted attacks.

## Key Learning
- Not all injected data is visible in the browser.
- Burp Suite allows inspection of raw HTTP responses, revealing hidden data.
- Different databases use different methods to retrieve version information (e.g., @@version for MySQL/MSSQL).
