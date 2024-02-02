# Simple Admin Panel App - Cross-Site-Scripting - 2
+ *Exploit Title:* Simple Admin Panel App - Cross-Site-Scripting - 2
+ *Date:* 2024-02-02
+ *Exploit Author:* Burak Sevben
+ *Vendor Homepage:* https://code-projects.org/simple-admin-panel-in-php-with-source-code/
+ *Software Link:* https://download.code-projects.org/details/e651c111-a5f1-45f6-ab1b-5fbf972339af
+ *Version:* 1.0
+ *Tested on:* Kali Linux + PHP 8.2.12, Apache 2.4.58
+ *CVE:* Reported, waiting for CVE number

## Description:
Simple Admin Panel App is vulnerable to a cross-site scripting vulnerability because it fails to adequately sanitize user-supplied data. An attacker could exploit this issue to run arbitrary scripting code in an unsuspecting user's browser in the context of the affected site. This could allow an attacker to steal cookie-based authentication credentials and launch other attacks.


## Proof of Concept:
+ Go to http://localhost/admin_panel
+ Press the 'Add Size' button.
+ In the 'Size Number ' section, type this code: `"><img src=x onerror=alert("xss")>`
+ Press the 'Add' button.
+ XSS will be triggered.

![Ekran görüntüsü 2024-02-02 163034](https://github.com/BurakSevben/CVEs/assets/117217689/20b6ba01-4085-40c2-8ecd-d10c78647b74)


![Ekran görüntüsü 2024-02-02 163047](https://github.com/BurakSevben/CVEs/assets/117217689/9d8f6051-b3ac-42c0-815f-d9fabfabab12)
