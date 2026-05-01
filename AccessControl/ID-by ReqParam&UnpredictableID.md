# Lab: User ID Controlled by Request Parameter with Unpredictable User IDs

## Vulnerability
This lab demonstrates an **Insecure Direct Object Reference (IDOR)** vulnerability where user data is accessed using a user-controlled 
parameter, even when user IDs are unpredictable.

---

## Explanation
The application uses randomly generated user IDs instead of predictable usernames. However, it fails to enforce proper authorization 
checks, allowing attackers to access other users’ data if they can obtain their user ID.

---

## Steps to Reproduce

### 1. Login as Normal User
- Username: wiener  
- Password: peter  

<img width="433" height="322" alt="image" src="https://github.com/user-attachments/assets/ed27eae8-0681-4bcb-bf93-fc2c39e2e9bf" />


---

### 2. Observe User ID
- Navigate to account page  
- URL contains a random user ID:
/my-account?id=abc123-random-id

<img width="835" height="44" alt="image" src="https://github.com/user-attachments/assets/a90ae555-106f-4d8a-a2ce-fd8de319531f" />


---

### 3. Discover Another User’s ID
- Browse the application  
- Open a blog post by **carlos**  
- Click on the username

<img width="824" height="478" alt="image" src="https://github.com/user-attachments/assets/dc73a867-bc22-4d25-8dc6-0617e53baa64" />


- URL contains Carlos’s user ID:
/user?id=xyz789-random-id

<img width="828" height="55" alt="image" src="https://github.com/user-attachments/assets/5d4b237e-6b6c-4b51-9c75-a6f8322a39b6" />


---

### 4. Modify the Request
- Replace your user ID with Carlos’s ID:
/my-account?id=xyz789-random-id

<img width="895" height="631" alt="image" src="https://github.com/user-attachments/assets/28fe50e5-375a-4ca1-b713-3c18942a4e0d" />


---

### 5. Access Unauthorized Data
- Application returns Carlos’s account details  
- API key for Carlos is visible  

---

### 6. Submit the API Key
- Copy the API key  
- Submit it to complete the lab  

---

## How It Works
- Application uses user-controlled parameter (`id`) to fetch account data  
- User IDs are unpredictable but exposed through public content  
- Server does not verify if the requested ID belongs to the authenticated user  

---

## Root Cause
- Missing authorization checks  
- Exposure of user identifiers in public areas  
- Trusting user-controlled input  

---

## Impact
- Unauthorized access to sensitive user data  
- Exposure of API keys and private information  
- Potential account compromise

---

## Learning
Using unpredictable identifiers does not prevent IDOR vulnerabilities. Proper authorization checks must always be enforced to ensure users 
can only access their own data.
