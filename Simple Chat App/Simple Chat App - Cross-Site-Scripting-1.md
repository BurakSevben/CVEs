# Simple Chat App - Cross-Site-Scripting-1
+ *Exploit Title:* Simple Chat App - Cross-Site-Scripting-1
+ *Date:* 2024-12-05
+ *Exploit Author:* Burak Sevben
+ *Vendor Homepage:* https://code-projects.org/simple-chat-system-in-php-with-source-code/
+ *Software Link:* https://download.code-projects.org/details/55a2b6be-cd54-4114-8d04-2ba66d0bc11a
+ *Version:* 1.0
+ *Tested on:* Kali Linux + PHP 8.2.12, Apache 2.4.58
+ *CVE:* Reported, waiting for CVE number.
  
## Description:
Simple Chat App 1.0 allows Cross-Site-Scripting(XSS) via the 'name' parameter at "http://localhost/chat_project/register.php/" . An attacker could exploit this issue to run arbitrary scripting code in an unsuspecting user's browser in the context of the affected site. This could allow an attacker to steal cookie-based authentication credentials and launch other attacks.

## Proof of Concept:
+ Go to http://localhost/chat_project/register.php
+ Fill out the form and register.
+ In the 'Name' section, write this code: `test"><img src=x onerror=alert(1923)>`
+ Then press the 'Sign Up' button.
+ XSS will be triggered.

![Ekran görüntüsü 2024-05-15 044951](https://github.com/BurakSevben/CVEs/assets/117217689/7c769158-7cbb-45a9-924f-d9b5ba3abc9e)

![Ekran görüntüsü 2024-05-15 045002](https://github.com/BurakSevben/CVEs/assets/117217689/62992a40-0ba5-4bc1-9370-8f228b4c0887)
