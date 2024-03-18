# QR Code Attendance System - Cross-Site-Scripting - 1
+ *Exploit Title:* QR Code Attendance System - Cross-Site-Scripting - 1
+ *Date:* 2024-14-03
+ *Exploit Author:* Burak Sevben
+ *Vendor Homepage:* https://www.sourcecodester.com/php/17242/qr-code-attendance-system-using-php-and-mysql-source-code.html
+ *Software Link:* https://www.sourcecodester.com/download-code?nid=17242&title=QR+Code+Attendance+System+Using+PHP+and+MySQL+with+Source+Code
+ *Version:* 1.0
+ *Tested on:* Kali Linux + PHP 8.2.12, Apache 2.4.58
+ *CVE:* Reported, waiting for CVE number

## Description:
QR Code Attendance System is vulnerable to a cross-site scripting vulnerability because it fails to adequately sanitize user-supplied data. An attacker could exploit this issue to run arbitrary scripting code in an unsuspecting user's browser in the context of the affected site. This could allow an attacker to steal cookie-based authentication credentials and launch other attacks.

## Proof of Concept:
+ Go to "http://localhost/qr-code-attendance-system/masterlist.php"
+ Press the 'Add Student' button.
+ In the 'Full Name' section, type this code: `"><img src=x onerror=alert(document.domain)>`
+ Then press the 'Generate QR Code' button.
+ XSS will be triggered.

![qr code xss 1](https://github.com/BurakSevben/CVEs/assets/117217689/f9bfe370-d5ed-4983-b112-6f0859b34959)

![qrcode xss 1 2](https://github.com/BurakSevben/CVEs/assets/117217689/5169be26-37e3-46a6-a7bd-0e160498dd9a)

