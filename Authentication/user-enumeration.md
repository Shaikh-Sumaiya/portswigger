# Lab: Username enumeration via different responses

## Vulnerability

This lab demonstrates a username enumeration vulnerability where the application reveals whether a username exists based on differences 
in server responses.

## Explanation

The application responds differently when an invalid username is provided versus when a valid username with an incorrect password is 
submitted. These differences allow an attacker to identify valid usernames.

## Tools Used

* Burp Suite (Intruder)

## Steps to Reproduce

### 1. Initial Testing

* Attempted login with random credentials.
* Observed the response message: **"Invalid username"**.

<img width="423" height="230" alt="image" src="https://github.com/user-attachments/assets/1f2da417-b562-4c44-aa9b-e58b052049a2" />


### 2. Username Enumeration

* Intercepted the login request using Burp Suite.
* Sent the request to Intruder.
* Marked the username parameter as payload position.
* Supplied a list of usernames.

#### Observation:

* One response had a **different response length** compared to others.

✔️ This indicated a **valid username**


<img width="1269" height="351" alt="image" src="https://github.com/user-attachments/assets/3367b3df-98bf-4b9b-bf0a-8ee32b278f8b" />

Others show 'Invalid Username'
<img width="1268" height="572" alt="image" src="https://github.com/user-attachments/assets/2b548965-715b-4978-a811-725825799d08" />

But this one shows 'Incorrect Password', that means our username worked
<img width="1281" height="591" alt="image" src="https://github.com/user-attachments/assets/f35aa9bb-c001-454e-8f87-e9cec8b285e0" />


---

### 3. Password Brute Force

* Fixed the identified valid username.
* Sent the request to Intruder again.
* This time, marked the password parameter.
* Supplied a list of passwords.

<img width="597" height="570" alt="image" src="https://github.com/user-attachments/assets/b267a388-a2d0-483d-b266-1e9352071605" />


#### Observation:

* One response returned a **302 status code**.

✔️ This indicated a **successful login**

<img width="1272" height="310" alt="image" src="https://github.com/user-attachments/assets/d09ab027-46bd-4245-aca1-feef4884c06f" />


---

### 4. Final Step

* Used the valid username and password to log in successfully.

<img width="366" height="231" alt="image" src="https://github.com/user-attachments/assets/b6ec9ac6-89bb-4408-a7e7-f40a09ea7610" />


<img width="503" height="342" alt="image" src="https://github.com/user-attachments/assets/758964e9-6ee0-42e1-8abe-3fd2513a2bce" />

---

## How It Works

The application leaks information through:

* Different response messages
* Variations in response length
* Different HTTP status codes

These differences allow attackers to distinguish between valid and invalid usernames and eventually brute-force passwords.

---

## Impact

* Attackers can identify valid usernames.
* Reduces the complexity of brute-force attacks.
* Increases risk of account compromise.

---

## Learning

Even small differences in server responses (like length or status code) can leak sensitive information and enable attackers to perform 
username enumeration and targeted attacks.
