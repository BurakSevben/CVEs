# QR Code Bookmark System - Cross-Site-Scripting
+ *Exploit Title:* QR Code Bookmark System - Cross-Site-Scripting
+ *Date:* 2024-06-04
+ *Exploit Author:* Burak Sevben
+ *Vendor Homepage:* https://www.sourcecodester.com/php/17286/qr-code-bookmark-system-using-php-and-mysql-source-code.html
+ *Software Link:* https://www.sourcecodester.com/download-code?nid=17286&title=QR+Code+Bookmark+System+Using+PHP+and+MySQL+with+Source+Code
+ *Version:* 1.0
+ *Tested on:* Kali Linux + PHP 8.2.12, Apache 2.4.58
+ *CVE:* Reported, waiting for CVE number.
  
## Description:
QR Code Bookmark System is vulnerable to a cross-site scripting vulnerability because it fails to adequately sanitize user-supplied data. An attacker could exploit this issue to run arbitrary scripting code in an unsuspecting user's browser in the context of the affected site. This could allow an attacker to steal cookie-based authentication credentials and launch other attacks.


## Proof of Concept:
+ Go to localhost/qr-code-bookmark/
+ Press the 'Add Bookmark'
+ In the 'Bookmark Name' section, type this code: `"><img src=x onerror=alert(1)>`
+ XSS will be triggered.

![qr code bookmark xss poc](https://github.com/BurakSevben/CVEs/assets/117217689/7a24709d-bb4c-4f30-8665-6fc0e06da6bf)

![qrcode bookmark success](https://github.com/BurakSevben/CVEs/assets/117217689/13286ad3-d591-4d25-9ced-de542d73e524)
