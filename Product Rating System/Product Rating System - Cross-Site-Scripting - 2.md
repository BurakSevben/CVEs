# Product Rating System - Cross-Site-Scripting-2
+ *Exploit Title:* Product Rating System - Cross-Site-Scripting - 2
+ *Date:* 2024-15-03
+ *Exploit Author:* Burak Sevben
+ *Vendor Homepage:* https://www.sourcecodester.com/php/17238/product-reviewrating-system-using-php-and-mysql-source-code.html
+ *Software Link:* https://www.sourcecodester.com/download-code?nid=17238&title=Product+Review%2FRating+System+Using+PHP+and+MySQL+with+Source+Code
+ *Version:* 1.0
+ *Tested on:* Kali Linux + PHP 8.2.12, Apache 2.4.58
+ *CVE:* Reported, waiting for CVE number

## Description:
Product Rating System is vulnerable to a cross-site scripting vulnerability because it fails to adequately sanitize user-supplied data. An attacker could exploit this issue to run arbitrary scripting code in an unsuspecting user's browser in the context of the affected site. This could allow an attacker to steal cookie-based authentication credentials and launch other attacks.


## Proof of Concept:
+ Go to http://localhost/product-rating-system/
+ Press the 'Rate/Review This Product' button.
+ In the 'Comment' section, type this code: `<script>alert("Hello XSS")</script>`
+ Then press the 'Submit' button.
+ XSS will be triggered.

![comment kismi](https://github.com/BurakSevben/CVEs/assets/117217689/a99ca356-ad2a-437d-b73e-09f2f4ef3815)

![comment xss](https://github.com/BurakSevben/CVEs/assets/117217689/dea21785-1ec6-46a3-a54d-e4d323597c79)
