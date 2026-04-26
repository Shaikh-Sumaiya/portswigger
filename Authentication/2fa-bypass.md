# Lab: 2FA Simple Bypass

## Vulnerability

This lab demonstrates a **broken two-factor authentication (2FA)** vulnerability where the application fails to properly enforce the 
second authentication step, allowing attackers to bypass it.

---

## Explanation

Although the application implements a 2FA mechanism, it does not verify whether the user has successfully completed the 2FA step before 
granting access to protected resources. As a result, an attacker can skip the verification step and directly access authenticated pages.

---

## Steps to Reproduce

Eg.: Use your valid credentials

<img width="421" height="339" alt="image" src="https://github.com/user-attachments/assets/b6122dfa-39a4-4239-97b3-94046890f35f" />

Find the code in Email client and paste it in the box
<img width="1253" height="413" alt="image" src="https://github.com/user-attachments/assets/ebfcd4c4-505e-4633-952d-bfd61ad77bde" />

<img width="730" height="52" alt="image" src="https://github.com/user-attachments/assets/1f44b500-1811-4466-8681-449cc4c5bbb4" />

Look at the URL after the page refreshes


### 1. Login with Valid Credentials

* Enter valid username and password.
* Application redirects to the 2FA verification page (e.g., `/login2`).

<img width="465" height="312" alt="image" src="https://github.com/user-attachments/assets/b8ae8dad-c5dc-4dd7-a992-957b4ff24bc4" />

---

### 2. Analyze the Flow

* At this stage, the user is partially authenticated.
* A valid session is already created by the server.

---

### 3. Bypass 2FA

* Instead of entering the 2FA code, directly navigate to a protected endpoint such as:

```
/my-account
```

<img width="1045" height="666" alt="image" src="https://github.com/user-attachments/assets/0856ea2d-8587-41e4-8778-38f0c57807b3" />


---

### 4. Observation

* The application grants access without requiring the 2FA code.

✔️ This confirms that 2FA validation is not enforced on the server side.

---

## How It Works

The application:

* Creates a valid session after username and password verification.
* Fails to track or enforce whether 2FA has been completed.
* Allows access to protected resources based only on session presence.

---

## Root Cause

* Missing server-side validation of 2FA completion.
* Trusting session authentication without verifying full authentication flow.

---

## Impact

* Attackers can bypass 2FA protection.
* Unauthorized access to user accounts.
* Defeats the purpose of multi-factor authentication.

---

## Learning

Security features like 2FA must be enforced strictly on the server side. Even if implemented, improper validation can allow attackers to 
bypass the mechanism entirely.
