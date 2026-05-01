# Lab: User ID Controlled by Request Parameter

## Vulnerability
This lab demonstrates an **Insecure Direct Object Reference (IDOR)** vulnerability where user data is accessed based on a user-controlled 
parameter without proper authorization checks.

---

## Explanation
The application retrieves user account information using a query parameter (`id`). Since this parameter is not validated against the 
authenticated user, it can be modified to access other users' data.

---


## Steps to Reproduce

### 1. Login as Normal User
- Username: wiener  
- Password: peter  

<img width="370" height="330" alt="image" src="https://github.com/user-attachments/assets/cfc06bba-2d1a-40f1-b015-37ce756b1a5b" />


---

### 2. Access Account Page
Navigate to:
/my-account?id=wiener

<img width="691" height="618" alt="image" src="https://github.com/user-attachments/assets/18a38f59-2223-41c3-bd4b-fcbd9f2dc08e" />


- Observe that the page displays account details, including an API key

---

### 3. Modify the Parameter
Change the URL to:
/my-account?id=carlos



---

### 4. Access Unauthorized Data
- The application returns Carlos’s account details  
- An API key for Carlos is visible

<img width="731" height="604" alt="image" src="https://github.com/user-attachments/assets/7e694cb5-5828-4337-a014-628716380641" />


---

### 5. Submit the API Key
- Copy the API key  
- Submit it as required to solve the lab  

<img width="455" height="230" alt="image" src="https://github.com/user-attachments/assets/a30d511e-47c8-46b8-8882-5a0919d0e0a0" />


---

## How It Works
- The application uses a user-controlled parameter (`id`) to fetch user data  
- The server does not verify whether the requested ID belongs to the authenticated user  
- This allows access to other users’ sensitive information  

---

## Root Cause
- Missing authorization checks  
- Trusting user-controlled input  
- Direct object reference without access control  

---

## Impact
- Unauthorized access to sensitive user data  
- Exposure of API keys and personal information  
- Potential account takeover or further exploitation   

---

## Learning
Applications must enforce authorization checks for every request. Simply relying on user-supplied identifiers without validation leads to 
IDOR vulnerabilities and data exposure.
