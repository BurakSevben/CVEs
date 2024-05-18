# Event Registration System - Cross-Site-Scripting - 1
+ *Exploit Title:* Event Registration System - Cross-Site-Scripting - 1
+ *Date:* 2024-17-04
+ *Exploit Author:* Burak Sevben
+ *Vendor Homepage:* https://www.sourcecodester.com/php/14884/event-registration-system-qr-code-php-free-source-code.html
+ *Software Link:* https://www.sourcecodester.com/download-code?nid=14884&title=Event+Registration+System+with+QR+Code+in+PHP+Free+Source+Code
+ *Version:* Latest
+ *Tested on:* Kali Linux + PHP 8.2.12, Apache 2.4.58
+ *CVE:* Reported, waiting for CVE number

## Description:
Event Registration System is vulnerable to a cross-site scripting vulnerability because it fails to adequately sanitize user-supplied data. An attacker could exploit this issue to run arbitrary scripting code in an unsuspecting user's browser in the context of the affected site. This could allow an attacker to steal cookie-based authentication credentials and launch other attacks.


## Proof of Concept:
+ Go to http://localhost/event/registrar/
+ Write a xss code in searchbar `'"><img src=x onerror=alert(1)>`
+ XSS will be triggered.

![Ekran görüntüsü 2024-05-18 234638](https://github.com/BurakSevben/CVEs/assets/117217689/cd64093f-34fa-4810-bded-7e13ff253917)


![Ekran görüntüsü 2024-05-18 234654](https://github.com/BurakSevben/CVEs/assets/117217689/ca8334fa-b7a7-400b-b66a-f5c28e2b6363)

