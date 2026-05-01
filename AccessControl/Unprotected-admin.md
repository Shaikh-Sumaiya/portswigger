# Lab: Unprotected Admin Functionality

## Vulnerability
This lab demonstrates a **broken access control vulnerability** where administrative functionality is exposed without proper authorization 
checks.

---

## Explanation
The application attempts to hide sensitive endpoints (like the admin panel) using a `robots.txt` file. However, it does not enforce 
access control on the server side, allowing any user to directly access the admin functionality.

---

## Steps to Reproduce

### 1. Discover Hidden Endpoint
Navigate to:
/robots.txt

<img width="642" height="141" alt="image" src="https://github.com/user-attachments/assets/51251d20-4739-4d09-8b87-0b2cf13a887c" />


---

### 2. Analyze robots.txt
Observed entries such as:
Disallow: /administrator-panel

This reveals the existence of a hidden admin panel.

---

### 3. Access Admin Panel
Navigate directly to:
/administrator-panel


Access is granted without authentication or authorization checks.

<img width="687" height="414" alt="image" src="https://github.com/user-attachments/assets/cc980788-99d6-4d30-be96-464633321ddd" />


---

### 4. Perform Admin Action
Use the admin interface to delete the user **carlos**.

<img width="362" height="280" alt="image" src="https://github.com/user-attachments/assets/a9a00e5e-c661-4995-b5e2-13db4a7e5ca4" />


---

## How It Works
- The application relies on `robots.txt` to hide sensitive endpoints.
- No server-side authorization checks are implemented.
- Any user can directly access administrative functionality by knowing the URL.

---

## Root Cause
- Missing access control on sensitive endpoints.
- Reliance on client-side or publicly visible mechanisms (`robots.txt`) for security.

---

## Impact
- Unauthorized access to admin functionality.
- Ability to perform critical actions (e.g., deleting users).
- Complete compromise of application integrity.

---

## Learning
Sensitive functionality should always be protected with proper authorization checks. Publicly accessible files like `robots.txt` can 
unintentionally expose hidden endpoints and should never be relied upon for security.
