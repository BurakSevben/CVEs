# To Do List App - Cross-Site-Scripting
+ *Exploit Title:* To Do List App - Cross-Site-Scripting
+ *Date:* 2024-26-03
+ *Exploit Author:* Burak Sevben
+ *Vendor Homepage:* https://www.sourcecodester.com/php/17259/todo-list-kanban-board-using-php-and-mysql-source-code.html
+ *Software Link:* https://www.sourcecodester.com/download-code?nid=17259&title=Todo+List+in+Kanban+Board+Using+PHP+and+MySQL+with+Source+Code
+ *Version:* 1.0
+ *Tested on:* Windows 11 Home + PHP 8.2.12, Apache 2.4.58
+ *CVE:* Reported, waiting for CVE number

## Description:
To Do List App  is vulnerable to a cross-site scripting vulnerability because it fails to adequately sanitize user-supplied data. An attacker could exploit this issue to run arbitrary scripting code in an unsuspecting user's browser in the context of the affected site. This could allow an attacker to steal cookie-based authentication credentials and launch other attacks.


## Proof of Concept:
+ Go to http://localhost/todo-list-in-kanban-board
+ Press the 'Add ToDo' button.
+ In the 'To Do' section, type this code: `<script>alert('XSS is easy?')</script>`
+ And press the 'Save Changes' button.
+ XSS will be triggered.

![to do list xss 1](https://github.com/BurakSevben/CVEs/assets/117217689/e6414365-d428-41af-948b-1e303adab4bb)

![todo list xss 2](https://github.com/BurakSevben/CVEs/assets/117217689/c2c092b7-5ab6-41a9-8756-bb939d5ce2ae)
