# CVE-2024-2553 -Product Rating System - Cross-Site-Scripting
+ *Exploit Title:* CVE-2024-2553 - Product Rating System - Cross-Site-Scripting
+ *Date:* 2024-15-03
+ *Exploit Author:* Burak Sevben
+ *Vendor Homepage:* https://www.sourcecodester.com/php/17238/product-reviewrating-system-using-php-and-mysql-source-code.html
+ *Software Link:* https://www.sourcecodester.com/download-code?nid=17238&title=Product+Review%2FRating+System+Using+PHP+and+MySQL+with+Source+Code
+ *Version:* 1.0
+ *Tested on:* Kali Linux + PHP 8.2.12, Apache 2.4.58
+ *CVE:* CVE-2024-2553
  
## Description:
Product Rating System is vulnerable to a cross-site scripting vulnerability because it fails to adequately sanitize user-supplied data. An attacker could exploit this issue to run arbitrary scripting code in an unsuspecting user's browser in the context of the affected site. This could allow an attacker to steal cookie-based authentication credentials and launch other attacks.


## Proof of Concept:
+ Go to http://localhost/product-rating-system/
+ Press the 'Rate/Review This Product' button.
+ In the 'Your Name' section, type this code: `"><img src=x onerror=alert(document.domain)>`
+ Then press the 'Submit' button.
+ XSS will be triggered.

![your name](https://github.com/BurakSevben/CVEs/assets/117217689/344dc5b9-88d9-42b5-91ec-8ab4ef0ff48c)


![your name xss](https://github.com/BurakSevben/CVEs/assets/117217689/fe50e9c6-b992-4cda-aa0f-15515156c52b)
