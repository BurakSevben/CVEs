# Online Book System - Cross-Site-Scripting
+ *Exploit Title:* Online Book System - Cross-Site-Scripting
+ *Date:* 2024-26-03
+ *Exploit Author:* Burak Sevben
+ *Vendor Homepage:* https://code-projects.org/online-book-system-in-php-css-js-and-mysql-free-download/
+ *Software Link:* https://download.code-projects.org/details/74c24d93-431e-4e31-acc1-802f14caef5c
+ *Version:* 1.0
+ *Tested on:* Kali Linux + PHP 8.2.12, Apache 2.4.58
+ *CVE:* Reported, waiting for CVE number.
  
## Description:
Online Book System  is vulnerable to a cross-site scripting vulnerability because it fails to adequately sanitize user-supplied data. An attacker could exploit this issue to run arbitrary scripting code in an unsuspecting user's browser in the context of the affected site. This could allow an attacker to steal cookie-based authentication credentials and launch other attacks.


## Proof of Concept:
+ Go to localhost/BookStore-master 1/index.php
+ Press the 'Any Product'
+ In the 'value' section, type this code: `<script>alert(document.domain)</script>`
+ XSS will be triggered.

![product value](https://github.com/BurakSevben/CVEs/assets/117217689/0620e0df-4c29-4b44-a2c9-9da2e0e3ac12)

![value xss](https://github.com/BurakSevben/CVEs/assets/117217689/55155df1-4a0c-4e19-b76f-ec15b7d5887a)
