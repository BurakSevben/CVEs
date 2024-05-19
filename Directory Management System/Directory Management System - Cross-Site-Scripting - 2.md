# Directory Management System - Cross-Site-Scripting - 2
+ *Exploit Title:* Directory Management System - Cross-Site-Scripting - 2
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
+ Go to http://localhost/dms/admin/admin-profile.php
+ Write a xss code in searchbar. ex: `"><img src=x onerror=alert(1)>`
+ XSS will be triggered.

![Ekran görüntüsü 2024-05-19 192208](https://github.com/BurakSevben/CVEs/assets/117217689/96b28913-e091-49ed-90c8-c7503fea94ee)

![Ekran görüntüsü 2024-05-19 192228](https://github.com/BurakSevben/CVEs/assets/117217689/3744b31a-9b4c-41a3-9497-b82edcd991ef)


![Ekran görüntüsü 2024-05-19 192239](https://github.com/BurakSevben/CVEs/assets/117217689/b4d1cea7-6f9a-4638-b6ef-a8066ad11a50)

