# Directory Management System - Cross-Site-Scripting - 1
+ *Exploit Title:* Directory Management System - Cross-Site-Scripting - 1
+ *Date:* 2024-17-04
+ *Exploit Author:* Burak Sevben
+ *Vendor Homepage:* https://phpgurukul.com/directory-management-system-using-php-and-mysql/
+ *Software Link:* https://phpgurukul.com/?sdm_process_download=1&download_id=9346
+ *Version:* 1.0
+ *Tested on:* Kali Linux + PHP 8.2.12, Apache 2.4.58
+ *CVE:* Reported, waiting for CVE number

## Description:
Directory Management System is vulnerable to a cross-site scripting vulnerability because it fails to adequately sanitize user-supplied data. An attacker could exploit this issue to run arbitrary scripting code in an unsuspecting user's browser in the context of the affected site. This could allow an attacker to steal cookie-based authentication credentials and launch other attacks.


## Proof of Concept:
+ Go to http://localhost/dms/admin/search-directory.php.
+ Write a xss code in searchbar. ex: `"><img src=x onerror=alert(2)>`
+ XSS will be triggered.

![Ekran görüntüsü 2024-05-19 192300](https://github.com/BurakSevben/CVEs/assets/117217689/82556ba0-50f8-40d1-8255-c33afb0bcd0d)

![Ekran görüntüsü 2024-05-19 192313](https://github.com/BurakSevben/CVEs/assets/117217689/7ec467e6-f2fe-4acf-b0fd-afc5048a1f0c)
