# Lab: User Role Can Be Modified in User Profile

## Vulnerability
This lab demonstrates a **broken access control vulnerability** where a user can modify their role by tampering with request parameters 
in a profile update request.

---

## Explanation
The application allows users to update their profile information via a JSON request. However, it does not properly validate which 
parameters can be modified. By adding a `roleid` parameter to the request, an attacker can escalate their privileges to an admin.

---

## Steps to Reproduce

### 1. Login as Normal User
- Username: wiener
- Password: peter

<img width="363" height="272" alt="image" src="https://github.com/user-attachments/assets/f62e6708-cee4-49db-9397-0cc70d45076c" />

---

### 2. Update Profile
- Change email address
- Intercept the request using Burp Suite

<img width="367" height="304" alt="image" src="https://github.com/user-attachments/assets/7c20df48-5898-4fa9-ba68-c48f2a4061c6" />

<img width="326" height="271" alt="image" src="https://github.com/user-attachments/assets/3ed5d3c5-8ab5-4b6b-9ebc-3199079a4386" />


<img width="380" height="309" alt="image" src="https://github.com/user-attachments/assets/08e4b2f7-53b4-4bad-999a-5b3c01b302af" />

---

### 3. Analyze Request
Original request:
```json
{ "email": "wiener@normal-user.net" }
```

<img width="666" height="481" alt="image" src="https://github.com/user-attachments/assets/cfa9d4a5-0a69-45e7-a94b-cfd0336ac03f" />

---

### 4. Modify Request in Repeater
Send request to Repeater
Add roleid parameter:
```json
{ "email": "wiener@normal-user.net", 
"roleid": 2 }
```

<img width="1093" height="448" alt="image" src="https://github.com/user-attachments/assets/ac11c45a-84e4-49af-852c-295a34482bc4" />


### 5. Send Modified Request
Server accepts the request
Response confirms role update

---

### 6. Access Admin Panel
Refresh browser
Admin panel becomes visible
Delete user carlos

<img width="1206" height="266" alt="image" src="https://github.com/user-attachments/assets/9d4ab3e5-fb63-4810-95c6-dfaf6b35f2ae" />

<img width="361" height="247" alt="image" src="https://github.com/user-attachments/assets/95fb18e1-7798-44f7-ba83-e70bb5d04517" />


---

### How It Works
Application allows updating user profile via JSON request
Server does not restrict which fields can be modified
Adding roleid=2 changes user role to admin

---

### Root Cause
Lack of input validation on server side
Allowing modification of sensitive fields (roleid)
Trusting client-controlled data

---

### Impact
Privilege escalation (user → admin)
Unauthorized access to admin functionality
Full compromise of application integrity

---

### Learning
Applications must validate and restrict which parameters users can modify. Allowing sensitive fields like roles to be updated through 
user input leads to privilege escalation vulnerabilities.
