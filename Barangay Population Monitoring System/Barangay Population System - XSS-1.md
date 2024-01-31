# Barangay Population Monitoring System - Cross-Site-Scripting-1
+ *Exploit Title:* Barangay Population Monitoring System - Cross-Site-Scripting-1
+ *Date:* 2024-31-01
+ *Exploit Author:* Burak Sevben
+ *Vendor Homepage:* https://www.sourcecodester.com/php/17109/barangay-population-monitoring-system-using-php-mysql-and-chartjs-source-code.html
+ *Software Link:* https://www.sourcecodester.com/download-code?nid=17109&title=Barangay+Population+Monitoring+System+Using+PHP%2C+MYSQL+and+Chart.js+with+Source+Code
+ *Version:* 1.0
+ *Tested on:* Windows 11 Home + PHP 8.2.12, Apache 2.4.58
+ *CVE:* Reported, waiting for CVE number

## Description:
Barangay Population Monitoring System is vulnerable to a cross-site scripting vulnerability because it fails to adequately sanitize user-supplied data. An attacker could exploit this issue to run arbitrary scripting code in an unsuspecting user's browser in the context of the affected site. This could allow an attacker to steal cookie-based authentication credentials and launch other attacks.


## Proof of Concept:
+ Go to http://localhost/barangay-population-monitoring-system/masterlist.php 
+ Press the 'Add Resident' button.
+ In the 'Full Name' section, type this code: `"><img src=x onerror=alert(document.domain)>`
+ Fill in the Address and Contact number fields and press the 'Add' button.
+ XSS will be triggered.

![Ekran görüntüsü 2024-01-31 132436](https://github.com/BurakSevben/CVEs/assets/117217689/13f7abee-70b9-4b57-8cff-6f1b478ede5b)

![Ekran görüntüsü 2024-01-31 132915](https://github.com/BurakSevben/CVEs/assets/117217689/f6788eda-a62b-4fc5-ada9-eaaf020c1545)


