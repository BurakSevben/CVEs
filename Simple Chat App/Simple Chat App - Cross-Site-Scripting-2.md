# Simple Chat App - Cross-Site-Scripting - 2
+ *Exploit Title:* Simple Chat App - Cross-Site-Scripting - 2
+ *Date:* 2024-12-05
+ *Exploit Author:* Burak Sevben
+ *Vendor Homepage:* https://code-projects.org/simple-chat-system-in-php-with-source-code/
+ *Software Link:* https://download.code-projects.org/details/55a2b6be-cd54-4114-8d04-2ba66d0bc11a
+ *Version:* 1.0
+ *Tested on:* Kali Linux + PHP 8.2.12, Apache 2.4.58
+ *CVE:* Reported, waiting for CVE number.
  
## Description:
Simple Chat App is vulnerable to a cross-site scripting vulnerability because it fails to adequately sanitize user-supplied data. An attacker could exploit this issue to run arbitrary scripting code in an unsuspecting user's browser in the context of the affected site. This could allow an attacker to steal cookie-based authentication credentials and launch other attacks.

## Proof of Concept:
+ Go to http://localhost/chat_project/chatpage.php
+ In the message section, write this code: `hello"><img src=x onerror=alert(1)>`
+ Then press the 'Send' button.
+ XSS will be triggered.

![Ekran görüntüsü 2024-05-15 045028](https://github.com/BurakSevben/CVEs/assets/117217689/c4cce473-e2fe-4ec5-bc89-233f93a9dd21)

![Ekran görüntüsü 2024-05-15 045040](https://github.com/BurakSevben/CVEs/assets/117217689/6514d512-3120-4060-8c71-5f385bbeadf5)
