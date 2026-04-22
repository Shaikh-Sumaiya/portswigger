# Lab: Reflected XSS into HTML context with nothing encoded.

## Vulnerability
This lab demonstrates a Reflected Cross-Site Scripting (XSS) vulnerability where user input is reflected in the response without proper output encoding.

## Steps to Reproduce
1. Enter a normal input like:
   hello
2. Observe that the input is reflected in the page.

<img width="1259" height="552" alt="image" src="https://github.com/user-attachments/assets/4c96404c-c268-4a1d-99ab-a29611a3d61f" />

<img width="853" height="54" alt="image" src="https://github.com/user-attachments/assets/8b088861-de2c-4dfc-8fcb-44c020a47a5e" />

3. Now enter the payload:
   <script>alert(1)</script>

4. The script gets executed in the browser.

<img width="1127" height="233" alt="image" src="https://github.com/user-attachments/assets/b006e063-0653-4723-8ddd-eb37db90a63c" />

## Explanation
When a user enters input in the search bar, the value is sent to the server via a GET request and reflected back in the response.

Since the application does not encode or sanitize the input, any injected JavaScript is executed by the browser.

## Payload
<script>alert(1)</script>

## Impact
An attacker can execute arbitrary JavaScript in the victim’s browser, potentially leading to session hijacking, data theft, or phishing attacks.

## Learning
Reflected XSS occurs when user input is included in the response without proper output encoding, allowing execution of malicious scripts.
