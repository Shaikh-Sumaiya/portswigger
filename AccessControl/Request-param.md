# Lab: User Role Controlled by Request Parameter

## Vulnerability
This lab demonstrates a **broken access control vulnerability** where user roles are determined by a client-controlled parameter, allowing 
privilege escalation.

---

## Explanation
The application stores the user's role in a cookie (e.g., `admin=false`). Since this value is controlled by the client, it can be modified 
to `admin=true`, granting unauthorized administrative access.

---

## Tools Used
- Burp Suite (Proxy & Repeater)
- Browser (Developer Tools)

---

## Steps to Reproduce

### 1. Attempt Direct Access
- Tried accessing `/admin` directly
- Access denied as user is not an admin

<img width="658" height="349" alt="image" src="https://github.com/user-attachments/assets/e268b1ec-1a58-4426-b647-d872435d03ec" />


---

### 2. Intercept Login Request
- Logged in using valid credentials:
  - Username: wiener
  - Password: peter
- Intercepted request using Burp Suite

<img width="406" height="331" alt="image" src="https://github.com/user-attachments/assets/e9396d58-4c7f-4865-ae4f-ed0daba8a73d" />


---

### 3. Analyze Request
- Observed cookie:
admin=false

<img width="419" height="313" alt="image" src="https://github.com/user-attachments/assets/c8514548-e263-4a25-8b96-6cd2d31a809b" />


---

### 4. Modify Parameter in Repeater
- Sent request to Repeater
- Changed:
admin=false → admin=true

<img width="222" height="32" alt="image" src="https://github.com/user-attachments/assets/b52040d6-b8dd-4cb6-a9bd-8330e8014bf8" />


- Sent request and observed response

✔️ Response now showed admin functionality

---

### 5. Modify Cookie in Browser
- Opened Developer Tools → Application → Cookies
- Changed:
admin=false → admin=true

<img width="1348" height="581" alt="image" src="https://github.com/user-attachments/assets/2b687935-2ed3-4a8c-bf43-7e5cb0eeeeac" />


<img width="377" height="62" alt="image" src="https://github.com/user-attachments/assets/f96e3c4d-c417-46dd-becd-73ceefba607d" />


---

### 6. Access Admin Panel
- Refreshed page
- Admin panel became accessible
- Deleted user **carlos**

<img width="810" height="349" alt="image" src="https://github.com/user-attachments/assets/971a4b75-8623-4237-8454-762707b15d8a" />

<img width="795" height="194" alt="image" src="https://github.com/user-attachments/assets/be6bd4d7-245e-4a7c-b405-7b18aa3bd25b" />


---

## How It Works
- Application stores user role in a client-side cookie
- Server trusts this value without validation
- Modifying the cookie grants elevated privileges

---

## Root Cause
- Authorization based on client-controlled input
- Lack of server-side role validation

---

## Impact
- Privilege escalation (user → admin)
- Unauthorized access to sensitive functionality
- Full compromise of application security

---

## Learning
Client-side data should never be trusted for authorization decisions. Any role or privilege information must be validated on the server 
side to prevent privilege escalation attacks.
