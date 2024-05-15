# Budget Management App - Cross-Site-Scripting
+ *Exploit Title:* Budget Management App - Cross-Site-Scripting
+ *Date:* 2024-12-05
+ *Exploit Author:* Burak Sevben
+ *Vendor Homepage:* https://code-projects.org/budget-management-in-php-with-source-code/
+ *Software Link:* https://download.code-projects.org/details/1c10fce2-3260-479b-878f-b2107dec72f9
+ *Version:* 1.0
+ *Tested on:* Kali Linux + PHP 8.2.12, Apache 2.4.58
+ *CVE:* Reported, waiting for CVE number

## Description:
 Budget Management App is vulnerable to a cross-site scripting vulnerability because it fails to adequately sanitize user-supplied data. An attacker could exploit this issue to run arbitrary scripting code in an unsuspecting user's browser in the context of the affected site. This could allow an attacker to steal cookie-based authentication credentials and launch other attacks.


## Proof of Concept:
+ Go to http://localhost/PHP-Budget-Calculator/index.php
+ Fill in the Add expense section.
+ In the 'Budget Title' section, type this code: `"><img src=x onerror=alert(1)>`
+ Then click the 'Save' button.
+ XSS will be triggered.

![Ekran görüntüsü 2024-05-15 201734](https://github.com/BurakSevben/CVEs/assets/117217689/a3ca6b3c-7af5-4668-8465-a14fea038713)


![Ekran görüntüsü 2024-05-15 201751](https://github.com/BurakSevben/CVEs/assets/117217689/105c581b-d39f-423f-bc83-2bfb94d5068d)
