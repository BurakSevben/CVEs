# Simple Admin Panel App - Cross-Site-Scripting - 1
+ *Exploit Title:* Simple Admin Panel App - Cross-Site-Scripting - 1
+ *Date:* 2024-02-02
+ *Exploit Author:* Burak Sevben
+ *Vendor Homepage:* https://code-projects.org/simple-admin-panel-in-php-with-source-code/
+ *Software Link:* https://download.code-projects.org/details/e651c111-a5f1-45f6-ab1b-5fbf972339af
+ *Version:* 1.0
+ *Tested on:* Windows 11 Home + PHP 8.2.12, Apache 2.4.58
+ *CVE:* Reported, waiting for CVE number

## Description:
Simple Admin Panel App is vulnerable to a cross-site scripting vulnerability because it fails to adequately sanitize user-supplied data. An attacker could exploit this issue to run arbitrary scripting code in an unsuspecting user's browser in the context of the affected site. This could allow an attacker to steal cookie-based authentication credentials and launch other attacks.


## Proof of Concept:
+ Go to http://localhost/admin_panel
+ Press the 'Add Category' button.
+ In the 'Category Name ' section, type this code: `"><img src=x onerror=alert(document.cookie)>`
+ Press the 'Add' button.
+ XSS will be triggered.

![Ekran görüntüsü 2024-02-02 162624](https://github.com/BurakSevben/CVEs/assets/117217689/bbd27b26-1e6c-432a-bfd8-e3760d5cf578)


![Ekran görüntüsü 2024-02-02 162643](https://github.com/BurakSevben/CVEs/assets/117217689/1c60c4b3-a579-436c-a2a7-d9a056753131)

