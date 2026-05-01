# Lab: Unprotected Admin Functionality with Unpredictable URL

## Vulnerability
This lab demonstrates a **broken access control vulnerability** where administrative functionality is hidden using an unpredictable URL 
but is not protected by proper authorization checks.

---

## Explanation
The application attempts to hide the admin panel by using a non-obvious URL instead of exposing it directly. However, the endpoint is 
still accessible and is revealed through client-side JavaScript. Since no server-side access control is enforced, any user can access it.

---

## Steps to Reproduce

### 1. Inspect the Page Source
- Open the webpage
- View source (`Ctrl + U`) or inspect elements

---

### 2. Analyze JavaScript Code
- Found JavaScript referencing an admin endpoint, such as:
/admin-lxhs92


<img width="507" height="192" alt="image" src="https://github.com/user-attachments/assets/150934d3-de47-429c-8a99-f6e0d7cd9f08" />


✔️ This reveals the hidden admin URL

---

### 3. Access Admin Panel
- Navigate directly to:
/admin-lxhs92

<img width="615" height="413" alt="image" src="https://github.com/user-attachments/assets/f9fac6e8-c7e6-4822-89b8-2c29f7004bd5" />


✔️ Admin panel is accessible without authentication or authorization

---

### 4. Perform Admin Action
- Use the admin interface to delete the user **carlos**

<img width="387" height="191" alt="image" src="https://github.com/user-attachments/assets/4dec26c9-4f9b-4dfe-85c3-f4c8b8734ad8" />

---

## How It Works
- The application hides admin functionality using an unpredictable URL
- The URL is exposed in client-side JavaScript
- No server-side authorization checks are implemented
- Any user who discovers the URL can access admin functionality

---

## Root Cause
- Missing server-side access control
- Exposure of sensitive endpoints in client-side code
- Reliance on obscurity instead of proper security

---

## Learning
Hiding sensitive functionality using obscure URLs is not a security measure. Attackers can easily discover such endpoints through 
client-side code, and without proper authorization checks, this leads to complete access control failure.
