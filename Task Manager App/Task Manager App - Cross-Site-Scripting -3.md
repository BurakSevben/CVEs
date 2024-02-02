# Task Manager App - Cross-Site-Scripting -3
+ *Exploit Title:* Task Manager App - Cross-Site-Scripting -3
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
+ Go to http://localhost/TaskManager/Tasks.php
+ Click on Note Section
+ Write this payload : `"<script>alert(1)</script>`
+ Then click Submit button
+ XSS will be triggered.

![Ekran görüntüsü 2024-02-02 181900](https://github.com/BurakSevben/CVEs/assets/117217689/8684ec7d-64f6-4787-9972-c3347cd46504)

![Ekran görüntüsü 2024-02-02 182007](https://github.com/BurakSevben/CVEs/assets/117217689/a575303b-056d-4240-b63e-ff42e0475dde)
