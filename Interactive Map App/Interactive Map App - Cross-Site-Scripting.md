# Interactive Map App - Cross-Site-Scripting
+ *Exploit Title:* Interactive Map App - Cross-Site-Scripting
+ *Date:* 2024-10-05
+ *Exploit Author:* Burak Sevben
+ *Vendor Homepage:* https://www.sourcecodester.com/php/17354/interactive-map-markers-using-php-and-mysql-source-code.html
+ *Software Link:* https://www.sourcecodester.com/download-code?nid=17354&title=Interactive+Map+with+Marker+Using+PHP+and+MySQL+with+Source+Code
+ *Version:* 1.0
+ *Tested on:* Kali Linux + PHP 8.2.12, Apache 2.4.58
+ *CVE:* Reported, waiting for CVE number

## Description:
Interactive Map App is vulnerable to a cross-site scripting vulnerability because it fails to adequately sanitize user-supplied data. An attacker could exploit this issue to run arbitrary scripting code in an unsuspecting user's browser in the context of the affected site. This could allow an attacker to steal cookie-based authentication credentials and launch other attacks.


## Proof of Concept:
+ Go to http://localhost/interactive-map-marker/
+ Press the 'Add Marker' button.
+ In the 'Marker Name' section, type this code: `"><img src=x onerror=alert(window.location)>`
+ Press the 'Save Marker' button.
+ XSS will be triggered.

![intreactive map marker](https://github.com/BurakSevben/CVEs/assets/117217689/44c8b635-5bde-4435-88d9-b383c7f766cd)
